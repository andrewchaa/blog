---
title: Cursor-based Pagination
date: "2021-07-29T00:00:00.000Z"
template: "post"
category: "Development"
tags: api
draft: false
slug: "/posts/2021-07-28/cursor-based-pagination/"
description: "Cursor based pagination"
socialImage: "/media/42-line-bible.jpg"
---

After using a classical pagination of total counts, current page, next and previous, we (the dev team I belong to) settled on a new cursor-based strategy with some similar principles to Relay.

Requests that support this new pagination will take two new fields:

* cursor: an opaque string value
* limit: a maximum number of items you want returned for the page

Responses will leverage the existing response metadata and include a new field:

* next_cursor: an opaque string value, or an empty string if there are no more results

```json
"pagination": {
    "next_cursor": "Njk2",
    "limit": 25
}
```

The main reason of the change was to support large dataset. We have tens of thousands incoming transactions and it could soon become impractical to count the total results and page numbers. We shifted our focus on doing one thing well: forward pagination through an entire dataset.


