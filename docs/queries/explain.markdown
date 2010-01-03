---
layout: page
title: "Docs: Query Reference : explain"
docs_tab: "qr"
---

explain
-------

    ["explain", <query>]
    => <query-plan>
    
    ["explain", ["select", "people"]]
    => ["collection-scan", "people"]
    
    ["explain", ["select", "people", {"where": ["=", "name", "Bob"], "order": ["age", "asc"]}]]
    => ["sort", ["age", "asc"],
         ["index-lookup", ["people", "name", "Bob"]]]

Returns the query plan that will used by the database to execute the given `query`.

`query` must be on of `select`, `count`, `update`, or `delete`.

