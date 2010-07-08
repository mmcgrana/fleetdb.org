---
layout: page
title: "Docs: Query Reference : select"
docs_tab: "qr"
---

select
------

    ["select", <collection>, <find-options>?]
    => <records>
    
    ["select", "people"]
    => [{"id": 1, "name": "Bob"}, {"id": 2, "name": "Amy"} ...]
    
    ["select", "people", {"where": ["=", "name", "Bob"]}]
    => [{"id": 1, "name": "Bob"}]
    
    ["select", "people", {"limit": 2, "only": ["id", "name"]}]
    => [[1, "Bob"], [2, "Amy"]]
    
    ["select", "people", {"order": ["name", "asc"], "limit": 1, "only": "name"}]
    => ["Bob"]

Retrieve records from the `collection`, optionally qualified by the `find-options`.

`find-options` is a map of options that describe the search parameters. Recognized options are `"where"`, `"order"`, `"offset"`, `"limit"`, `"only"`, and `"distinct"`.

`offset` and `limit` are integer-valued options that have the same behavior as the SQL operators of the same name.

`only` is given as a vector of attribute names or as a single attribute name. If it is a vector, returned records are represented as vectors of attribute values in the same order. If `only` is a single attribute, returned records are represented by the corresponding value.

`distinct` is an optional boolean value that, when `true`, prevents duplicate results from being returned. Note that duplicates are determined according to the record values that would be returned according to projection by the `only` option, not according to the `id`'s nor to the unprojected records.

`order` specifies the sort order in which the records should be retrieved. Records can be ordered by one attribute or by multiple attributes:

    [<attribute>, <"asc"|"desc">]
    
    [[<attribute>, <"asc"|"desc">]+]

The following are examples of `order` option values:
    
    ["name", "asc"]
    
    [["name", "asc"], ["age", "desc"]]

If `order` is not given, the ordering of the records in the result is unspecified.

`where` describes the criteria by which records should be filtered:

    ["=",      <attribute>, <value>]
    ["!=",     <attribute>, <value>]
    ["<",      <attribute>, <value>]
    ["<=",     <attribute>, <value>]
    [">",      <attribute>, <value>]
    [">=",     <attribute>, <value>]
    ["in",     <attribute>, [<value>+]]
    ["not-in", <attribute>, [<value>+]]
    ["><",     <attribute>, [<low-value>, <high-value>]]
    [">=<",    <attribute>, [<low-value>, <high-value>]]
    ["><=",    <attribute>, [<low-value>, <high-value>]]
    [">=<=",   <attribute>, [<low-value>, <high-value>]]
    ["or",     <where-condition>+]
    ["and",    <where-condition>+]

The following are examples of `where` conditions:

    ["=", "name", "Bob"]
    
    ["in", "name", ["Bob", "Amy"]]
    
    [">=<", "age", [30, 40]]
    
    ["and", ["in", "name", ["Bob", "Amy"]], [">=<", "age", [30, 40]]]

If no `find-options` are given, `select` returns all records in the collection.
