---
layout: page
title: "Docs: Query Reference : list-indexes"
docs_tab: "qr"
---

list-indexes
------------

    [:list-indexes <collection>]
    <index-specs>

    [:list-indexes :people]
    [:name :age]

Returns a vector of index specs indicating all indexes on the given 'collection'.

The elements of 'index-specs' are of the same form as the 'index-spec' values given for [create-index](/docs/queries/create-index.html) and [drop-index](/docs/queries/drop-index.html) queries.
