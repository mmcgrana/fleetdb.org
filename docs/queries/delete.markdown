---
layout: page
title: "Docs: Query Reference : delete"
docs_tab: "qr"
---

delete
------

    [:delete <collection> <find-options>?]
    <count>
    
    [:delete :people]
    25
    
    [:delete :people {:where [:= :name "Bob"]}]
    1
    
Removes all records from the `collection` that match the `find-options`. Returns the number of deleted records.

`find-options` are of the same form as for [select](/doc/queries/select.html) queries, except that the `:only` option is not recognized.