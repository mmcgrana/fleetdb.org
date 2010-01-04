---
layout: page
title: "Docs : Implementation"
docs_tab: "i"
---

FleetDB is implemented on the JVM, primarily in Clojure. This document assumes familiarity with Clojure, especially its [data structures](http://clojure.org/data_structures) and [sequences](http://clojure.org/sequences), and notion of [identity and state](http://clojure.org/state)

FleetDB stores all data in RAM in persistent data structures. Records are stored as maps of attribute names to their corresponding values. For example, a record in an `"accounts"` collection could be:

    {"id" 1 "owner" "Eve" "credits" 100}

Collections are stored as maps of record ids to records. For example a `"accounts"` record map might look like:
              
    {1 {"id" 1 "owner" "Eve" "credits" 100}
     2 {"id" 2 "owner" "Bob" "credits" 150}
     3 {"id" 3 "owner" "Dan" "credits" 55}}


Indexes are stored as sorted maps of the value of the indexed attributes to records. For example, an index map for an index on `"owner"` in the `"accounts"` collection would look like:

    {"Bob" {"id" 2 "owner" "Bob" "credits" 150}
     "Dan" {"id" 3 "owner" "Dan" "credits" 55}
     "Eve" {"id" 1 "owner" "Eve" "credits" 100}}

Each collection is grouped along with its indexes into a single map. For example, if the `"accounts"` collection has an index on `"owner"` and `"credits"`, then this map would look like:

    {:rmap <ids-to-accounts>
     :imap {"owner"   <owners-to-accounts>
            "credits" <credits-to-accounts}}

A database is then a map of collection names to these collection maps. If a database had `"branches"` and `"accounts"` collections it could look like:

    {"branches" {:rmap  <ids-to-branches>
                 :imap {"city"      <city-to-branches>
                        "employees" <employees-to-branches>}}
     "accounts" {:rmap <ids-to-accounts>
                 :imap {"owner"   <owners-to-accounts>
                        "credits" <credits-to-accounts>}}}

FleetDB manages the change of a database over time according to Clojure's model of state. The data structures above correspond to database *values*; these are immutable. A FleetDB server also maintains a single database *identity*. This identity assumes different *states* over time as the database is manipulated by users.

The separation of identity and state has important implications for the semantics and performance of the database under concurrent load. For example, to answer a read query, FleetDB dereferences the database identity to obtain the current database state - an immutable value - and computes the result of the query on that value. Read queries can therefore be answered in complete isolation, without blocking, and concurrently with any other queries.

Write queries are handled differently. First, write queries are queued so that they are executed serially. When a write query reaches the head of the write queue, the database identity is dereferenced to obtain the current state. The write query then executes as a function of that state, returning a query result and a new database value. The write query is then logged to the append-only database file, the new database value atomically swapped into the database identity as the current state, and finally the query result returned to the user.
