---
layout: page
title: "Docs: Query Reference : drop-index"
docs_tab: "qr"
---

drop-index
----------

    ["drop-index", <collection>, <index-spec>]
    => <0|1>
    
    ["drop-index", "people", "name"]
    => 1
    
Ensures that the index described by `index-spec` does not exist on the given collection. Returns 1 if the index existed and was removed or 0 if it did not exist.

`index-spec` is the value as was given when the index was created with [create-index](/docs/queries/create-index.html).
