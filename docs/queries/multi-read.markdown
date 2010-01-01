---
layout: page
title: "Docs: Query Reference : multi-read"
docs_tab: "qr"
---

multi-read
----------

    ["multi-read", [<read-query>+]]
    => [<read-result>+]
    
    ["multi-read", 
      [["count", "people", {"where": ["=", "age", 30]}],
       ["count", "people", {"where": ["=", "name", "Bob"]}]]]
    => [4 1]

Executes the given read queries on a single snapshot of the database, returning a vector of their respective results.

Each `read-query` must be either a [select](/docs/queries/select.html), [count](/docs/queries/count.html), [explain](/docs/queries/explain.html), [list-collections](/docs/queries/list-collections.html), [list-indexes](/docs/queries/list-indexes.html), or multi-read. The queries may operate on different collections.