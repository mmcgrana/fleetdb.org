---
layout: page
title: "Docs : Client Libraries : Ruby"
docs_tab: "cl"
---

Ruby
----

A FleetDB Ruby client is available via Rubygems:

    $ sudo gem install fleet

Usage is simple:
    
    require "rubygems"
    require "fleet"
    
    client = Fleet.new("127.0.0.1", 3400)
    
    client.query(["ping"])
    #=> "pong"
    
    client.query(["select", "accounts", {"where" => ["=", "id", 2]}])
    #=> [{"id" => 2, "owner" => "Alice", "credits" => 150}]

The client will raise an exception in the case of an error:

    client.query(["bogus"])
    RuntimeError: Malformed query: unrecognized query type '"bogus"'

The source for this client is available [on Github](http://github.com/mmcgrana/fleet-rb).

