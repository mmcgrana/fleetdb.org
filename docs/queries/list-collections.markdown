---
layout: page
title: "Docs: Query Reference : list-collections"
docs_tab: "qr"
---

list-collections
----------------

    ["list-collections"]
    => <collections>
    
    ["list-collections"]
    => ["people", "friendships"]

Returns a vector of names of non-empty collections in the database. A collection is defined to be non-empty if it has a positive number of records or a positive number of indexes.
