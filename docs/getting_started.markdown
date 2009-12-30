---
layout: page
title: "Docs : Getting Started"
docs_tab: "gs"
---

First, download FleetDB:

    $ wget http://fleetdb.org/files/fleetdb-all.jar

Then to start a server:

    $ java -cp fleetdb-all.jar fleetdb.server -f demo.fdb -i text

FleetDB will store the database data in the file `demo.fdb`. Since we specified the `text` protocol, you can use `telnet(1)` to send queries:

    $ telnet 127.0.0.1 3400

At the telnet prompt, type `[:ping]` and hit return. This will send a ping query to the server, which should respond with `"pong"`:

    [:ping]
    :pong

A FleetDB database consists of collections of records. Collections are created automatically as needed. For example, the following inserts a record into a new `:accounts` collection:

    [:insert :accounts {:id 1 :owner "Eve" :credits 100}]
    1

The return value `1` indicates that 1 record was been added.

To insert multiple records:

    [:insert :accounts [{:id 2 :owner "Bob" :credits 150}
                        {:id 3 :owner "Dan" :credits 50}
                        {:id 4 :owner "Amy" :credits 1000 :vip true}]]
    3

Records can have arbitrary attribute names and values that are numbers, keywords, strings, booleans, or nil. The only requirement is that each record have a unique `:id`.

You can use `:select` to retrieve records. To get a single record by `:id`:

    [:select :accounts {:where [:= :id 2]}]
    [{:id 2 :owner "Bob", :credits 150}]

You can also select several records by id:

    [:select :accounts {:where [:in :id [1 3]]}]
    [{:id 1 :owner "Eve" :credits 100}
     {:id 3 :owner "Dan" :credits 50}]

`:select` queries can find records by other criteria as well. For example, to find accounts with at least 150 credits:

    [:select :accounts {:where [:>= :credits 150]}]
    [{:id 2 :owner "Bob" :credits 150}
     {:id 4 :owner "Amy" :credits 1000 :vip true}]

Or use the `:limit` and `:sort` options to find the 2 smallest accounts:

    [:select :accounts {:order [:credits :asc] :limit 2}]
    [{:id 3 :owner "Dan" :credits 50}
     {:id 1 :owner "Eve" :credits 100}]

To see what all of our records look like, issue a `:select` query without any conditions:

    [:select :accounts]
    [{:id 1 :owner "Eve" :credits 100}
     {:id 2 :owner "Bob" :credits 150}
     {:id 3 :owner "Dan" :credits 55}
     {:id 4 :owner "Amy" :credits 1000 :vip true}]

Count queries are formed similarly to select queries:

    [:count :accounts {:where [:= :vip true]}]
    1

As are delete queries:

    [:delete :accounts {:where [:= :id 3]}]
    1

You can update records like this:

    [:update :accounts {:credits 55} {:where [:= :owner "Dan"]}]
    1

This update merges the map `{:credits 55}` into those records matching the criteria `[:= :owner "Dan"]`.

In the current database, finding records by `:owner` requires scanning through all records and testing each one. We can verify this by asking FleetDB to explain its query plan for such a select query:

    [:explain [:select :accounts {:where [:= :owner "Eve"]}]]
    [:filter [:= :owner "Eve"]
      [:record-scan :accounts]]

If you search by `:owner` often, you may want to add and index to speed query execution:

    [:create-index :accounts :owner]
    1

Now FleetDB will use this index to answer queries that depend on the `:owner` attribute:

    [:explain [:select :accounts {:where [:= :owner "Eve"]}]]
    [:index-lookup [:accounts :owner "Eve"]]

FleetDB offers several queries for reading and writing a database that are accessed concurrently by other users. For example, if you want the result of multiple read queries to reflect a single snapshot of the database, it may not be safe to issue them individually. Instead you can compose read queries using the `:multi-read` query. Thus the following returns the names of the largest and smallest accounts according to a single snapshot of the database:

    [:multi-read
      [[:select :accounts {:order [:credits :desc] :limit 1 :only :owner}]
       [:select :accounts {:order [:credits :asc]  :limit 1 :only :owner}]]]
    [["Amy"] ["Eve"]]

Similarly you may want to execute multiple write queries atomically. Thus if you wanted to move 5 credits from "Amy" to "Bob":

    [:multi-write
      [[:update :accounts {:credits 995} {:where [:= :owner "Amy"]}]
       [:update :accounts {:credits 105} {:where [:= :owner "Eve"]}]]]
    [1 1]

Finally, FleetDB provides an optimistic concurrency control query `:checked-write`. This query first executes a read query to ensure that it corresponds to an expected result before executing a write query. You can think of it as a generalized compare and swap. Thus to safely deduct 5 credits from Eve's account, which previously had 105 credits:

    [:checked-write
      [:select :accounts {:where [:= :owner "Eve"] :only :credits}]
      [105]
      [:update :accounts {:credits 100} {:where [:= :owner "Eve"]}]]

The response if the write succeeds:

    [true 1]

Or if it fails, for example because the actual checked balance was 120:

    [false [120]]

Several housekeeping queries are available. To see all of the indexes on a collection:

    [:list-indexes :accounts]
    [:owner]

And to drop one of these indexes:

    [:drop-index :accounts :owner]
    1

To show all collections:

    [:list-collections]
    [:accounts]

To remove a collection, drop its indexes (which we did above) and delete all of its records:

    [:delete :accounts]
    3

    [:list-collections]
    []

You now know the basics of FleetDB! Check out the [Client Libraries](/docs/clients.html), [Query Reference](/docs/queries.html), and  [Server CLI](/docs/server_cli.html) pages to learn more.
