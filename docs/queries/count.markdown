---
layout: page
title: "Docs: Query Reference : count"
docs_tab: "qr"
---

count
-----

    ["count", <collection>, <find-options>?]
    => <count>
    
    ["count", "people"]
    => 25
    
    ["count", "people", {"where": [">", "age", 30]}]
    => 14

Returns the number of records in the `collection` that match the `find-options`. 

`find-options` are of the same form as for [select](/docs/queries/select.html) queries, except that the `only` option is not recognized.
