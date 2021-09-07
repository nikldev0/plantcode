---
title: "Looping in Vue"
date: 2021-09-06T22:20:15+01:00
draft: true
subtitle: "Or why it took me so long to properly understand v-for"
banner: https://www.plantcode.blog/me/banner.jpg
categories: -frontend
tags:
  - Vue
---

V-for is a relatively simple looping mechanism that has tripped me up more often than I'd like to admit.

A couple of core concepts to reiterate is that `v-for` the 'singular word' component of the for each loop *becomes* the prop that you pass down into a child element.

If an id doesn't exist for an object in store that we're looping from, we're using the automatically generated index of the object within the objects array as the key identifier. The `v-for` directive supports the index of the iterated item as an optional second argument. We're declaring this index and binding its value to the key of each iterated item.
