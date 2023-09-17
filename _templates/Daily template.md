---
title: Daily Note
aliases: 
tags: 
date created: <% tp.file.creation_date(format: string = "YYYY-MM-DD HH:mm") %>
date modified: <% tp.file.creation_date(format: string = "YYYY-MM-DD HH:mm") %>
---
<% tp.frontmatter["date created"] = "test" %>
<% tp.date.now("YYYY-MM-DD", -1) %>


## Tasks

- [ ] Today review


## GOAL


## TIL
```dataview
list from "_posts"
WHERE file.cday = this.file.cday
```