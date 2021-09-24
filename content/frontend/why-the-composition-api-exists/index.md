---
title: "Why the Composition API Exists"
date: 2021-09-08T11:18:51+01:00
draft: false
subtitle: "Vue 3's most powerful new feature"
banner: https://www.plantcode.blog/me/banner.jpg
categories: -frontend
tags:
  - Vue
---

Some of the biggest limitations you may face when building a Vue app using the Options API is that code that belongs together logically is split up across multiple options (data, methods, computed).

This also makes re-using logic across components tricky and cumbersome and it wouldn't always make it obvious where this logic is coming from.

The Composition API merges the data, methods and computed into one setup method. With this method you can still add to your component config object and use new features to manage it all and solve problems.

When we use the v-bind, the double quotation marks interprets eveything inside it as an expression, so to put a string within that expression we use single quotes.
