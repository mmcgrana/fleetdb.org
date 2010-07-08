---
layout: page
title: "Docs: Query Reference : drop-collection"
docs_tab: "qr"
---

drop-collection
----------

    ["drop-collection", <collection>]
    => <0|1>
    
    ["drop-collection", "people"]
    => 1
    
Ensures that the `collection` does not exist. Returns 1 if the collection existed and was removed or 0 if it did not exist.
