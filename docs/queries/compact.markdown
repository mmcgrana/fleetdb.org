---
layout: page
title: "Docs: Query Reference : compact"
docs_tab: "qr"
---

compact
-------

    ["compact"]
    => <true|false>

Request asynchronous, non-blocking compaction of the database log file. Returns immediately, with `false` if compaction was already underway or `true` if it was not and therefore started.

The status and result of compactions can be found with the [info](/docs/queries/info.html) command.
