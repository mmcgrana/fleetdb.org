---
layout: page
title: "Docs: Query Reference : insert"
docs_tab: "qr"
---

insert
------

    ["insert", <collection>, <record>|<records>]
    => <count>
    
    ["insert", "people", {"id": 1, "name": "Bob"}]
    => 1
    
    ["insert", "people", [{"id": 1, "name": "Bob"}, {"id": 2, "name": "Amy"}]]
    => 2

Inserts `record` or `records` into the `collection`. `record` is a map, `records` a vector of maps.

Returns the number of records added.

It is an error to add a record that has no id, or that has the same id as an existing record.