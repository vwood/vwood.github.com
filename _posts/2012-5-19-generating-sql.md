---
layout: post
title: Generating SQL
tags: [databases, compilers]
published: false
---

{{ page.title }}
================
<p class="meta">19th May 2012</p>

Generating SQL from another query that gives us MapReduce or other semantics.

Generating SQL for different databases

Compile to SQL to show the execution plan

Optimise for different databases - A composition of tiny optimisers so each database is just a composition of different optimisers

Take *all* the SQL queries, so we can suggest sub-indexes and so on

Allow for diffs between execution plans

Replace joins with views (perhaps anon. views)
