---
layout: page
title: "Docs : Client Libraries : Clojure"
docs_tab: "cl"
---

Clojure
-------

A Clojure client library is included with the FleetDB distribution: `fleetdb.client`. 

    => (use '(fleetdb [client :as c]))
    => (def client (client/connect "127.0.0.1" 3400))
    
    => (c/query client [:ping])
    "pong"
    
    => (c/query client [:select :accounts {:where [:= :id 2]}])
    [{:id 2 :owner "Alice" :credits 150}]
    
    => (c/close client)

A Clojure client library is included with the FleetDB distribution: `fleetdb.client`. 

    => (use '(fleetdb [client :as c]))
    => (def client (client/connect "127.0.0.1" 3400))
    
    => (c/query client ["ping"])
    "pong"
    
    => (c/query client ["select" "accounts" {"where" ["=" "id" 2]}])
    [{"id" 2 "owner" "Alice" "credits" 150}]
    
    => (c/close client)