---
layout: page
title: "Docs: Query Reference : create-index"
docs_tab: "qr"
---

create-index
------------

    [:create-index <collection> <index-spec>]
    <0|1>
  
    [:create-index <collection> :name]
    1
    
    [:create-index <collection> [:name [:age :desc]]]
    1
    
Ensures that the index described by `index-spec` exists on the given collection. Returns 1 if such an index is created or 0 if it already exists.

`index-spec` may indicate an index on 1 attribute or on multiple attributes.

    <attribute>
    
    [<attribute>|[<attribute> <:asc|desc>]+]

If an order is not given for an attribute in a multi-attribute index, it is assumed to be ascending.

The following are examples of `index-specs`:
    
    :name
    
    [:name [:age :desc]]
    
    [[:name :desc] [:age :asc]]
