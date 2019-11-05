---
title: Get last item from List in F#
date: "2019-02-12T00:00:00.000Z"
template: "post"
category: "Programming"
tags: 
  - "Life"
draft: false
slug: "/posts/get-last-item-of-list-fsharp/"
description: "In C#, you use bang (!) to negate a boolean expression. Unfortunately, not in F#."
socialImage: "/media/42-line-bible.jpg"

---

In C#, you use bang (!) to negate a boolean expression. Unfortunately, not in F#. it's ["not"](https://stackoverflow.com/questions/239888/logical-negation-operator-in-f-equivalent)

List is a linked list in F#. Head is an element but Tail is a List. How can you get the last element? Anything like list.Last()?
Simple trick would be, if you don't consider performance, though ...

```fsharp
myList |> List.rev |> List.head
```

You can create a list of alphabets like

```fsharp
let list = ['a' .. 'z']
```

