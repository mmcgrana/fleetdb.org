---
layout: page
title: "Docs: Query Reference : multi-write"
docs_tab: "qr"
---

multi-write
-----------

    ["multi-write", [<query>+]]
    [<query-result>+]
    
    ["multi-write",
      ["select", "people", {"where": ["=", "counted", false]}],
      ["update", "people", {"counted": true}, {"where": ["=", "counted", false]}]]
    => [[{"id": 1, "name": "Bob"}, {"id": 2, "name": "Amy"}] 2]
    
Executes the given queries atomically and in order on the current snapshot of the database. Unlike [multi-read](/docs/queries/multi-read.html), this query allows write queries as well as read queries.
