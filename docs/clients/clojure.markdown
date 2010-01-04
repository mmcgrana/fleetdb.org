---
layout: page
title: "Docs : Client Libraries : Clojure"
docs_tab: "cl"
---

## Clojure

A FleetDB Clojure client is available in the namespace `fleetdb.client`.

If you want to incorporate this client into a project managed by Maven or [Leiningen](http://github.com/technomancy/leiningen), you can find the necessary Maven artifact information on the client's [Clojars project page](http://clojars.org/fleetdb-client).

If you just want to get started right away with at the REPL:

    $ wget http://github.com/mmcgrana/fleetdb-client/downloads/fleetdb-client-standalone.jar
    $ java -cp fleetdb-client-standalone.jar clojure.contrib.repl_ln

Using the library is simple:
 
    (use 'fleetdb.client)
    (def client (connect))
    
    (query client ["ping"])
    => "pong"

    (query client ["select" "accounts" {"where" ["=" "id" 2]}])
    => [{"id" 2 "owner" "Alice" "credits" 150}]
    
Keywords can be used in queries, as they are converted to strings before being sent to the server:

    (query client [:select :accounts {:where [:= :id 2]}])
    => [{"id" 2 "owner" "Alice" "credits" 150}]

The client can also be used as a function that executes the given query:

    (client [:ping])

The client will raise an exception in the case of an error:

    (client ["bogus"])
    java.lang.Exception: Malformed query: unrecognized query type '"bogus"'

You can optionally specify a host and port other than the default `"127.0.0.1"` and `3400`:

    (def client (connect {:host "68.127.150.103" :port 3401}))

The source for this client is available [on Github](http://github.com/mmcgrana/fleetdb-client).
