---
layout: page
title: "Docs: Query Reference : udpate"
docs_tab: "qr"
---

update
------

    [:update <collection> <update-map> <find-options>?]
    <count>
     
    [:update :people {:vip true} {"where": ["=" :name "Bob"]}]
    1

Updates records in the `collection` by merging `update-map` into any records that match `find-options`. Returns the number of updated records.

If an attribute is present in both `update-map` and a matching record, the updated record assumes the value from the `update-map`. If an attribute is valued with `nil` in the update map, that attribute will not be present in the updated record.

`find-options` are of the same form as for [select](/docs/queries/select.html) queries, except that the `"only":` option is not recognized.