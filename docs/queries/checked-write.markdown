---
layout: page
title: "Docs: Query Reference : checked-write"
docs_tab: "qr"
---

checked-write
-------------

    ["checked-write", <read-query>, <expected-read-result>, <write-query>]
    => [true, <write-result>] | [false, <actual-read-result>]
    
    ["checked-write",
      ["count", "registrations", {"where": ["=", "person-id", 2]}],
      6,
      ["insert", "registrations", {"id": 13, "person-id": 2, "event-id": 4}]]
    => [true, 1] | [false, 7]

First, executes `read-query` and compares the result to `expected-read-result`. If the two are equal, then executes `write-query` and returns the result prefixed by `true`. If the actual and expected read results differ, does not execute `write-query` and instead returns the actual read result prefixed by `false`.

`read-query` must be one of the queries listed for [multi-read](/docs/queries/multi-read.html). `write-query` must be one of [insert](/docs/queries/insert.html), [update](/docs/queries/update.html), [delete](/docs/queries/delete.html), [create-index](/docs/queries/create-index.html), [drop-index](/docs/queries/drop-index.html), or multi-write.