---
layout: page
title: "Docs : Client Libraries : Clojure"
docs_tab: "cl"
---

## Clojure

The FleetDB Clojure client `fleetdb-client` is available via is available via [Clojars](http://clojars.org); see the [project page](http://clojars.org/fleetdb-client) for instructions on installing with Leiningen or Maven.

Usage of the library is simple:
 
    => (use 'fleetdb.client)
    => (def client (connect))
    
    => (query client ["ping"])
    "pong"

    => (query client ["select" "accounts" {"where" ["=" "id" 2]}])
    [{"id" 2 "owner" "Alice" "credits" 150}]
    
Keywords can be used in queries, as they are converted to strings before being sent to the server:

    => (query client [:select :accounts {:where [:= :id 2]}])
    [{"id" 2 "owner" "Alice" "credits" 150}]

The client can also be used as a function that executes the given query:

    => (client [:ping])

The client will raise an exception in the case of an error:

    => (client ["bogus"])
    java.lang.Exception: Malformed query: unrecognized query type '"bogus"'

You can optionally specify a host and port other than the default `"127.0.0.1"` and `3400`:

    (def client (connect {:host "68.127.150.103" :port 3401}))

The source for this client is available [on Github](http://github.com/mmcgrana/fleetdb-client).
