---
title: "Pagination in Vue 3"
date: 2021-08-30T14:10:42+01:00
draft: false
subtitle: "A simple guide to getting pagination right in Vue"
banner: https://www.plantcode.blog/me/banner.jpg
categories: -frontend
tags:
  - Vue
---

To configure pagination for a Vue app we first need to solve the problem of how we let our router index file read query parameters off the URL.

Consider a URL that looks like the following:

`https://plantcode.blog/posts?page=4`

To read our page listing inside our template, we can simply write:

{{< highlight html "linenos=tables,linenostart=1" >}}

 <h1>You are on page {{ $route.query.page }}</h1>
{{< / highlight >}}

{{< figure src="routequerypage.png" caption="Viewing the current page in the component template" >}}

Here we're using a [route object property](https://router.vuejs.org/api/#route-object-properties) to fetch us that data.

Note that to get access to the page number from a query string inside component code, we'd need to add `this`.

{{< highlight vue "linenos=tables,linenostart=1" >}}
computed: {
    page() {
     return parseInt(this.$route.query.page) || 1
    }
}
{{< / highlight >}}

If the page number is not within a query parameter, i.e. they don't come after the '?' in the URL, then they are treated as a route parameter and may be accessed differently. Note that here we are using `$route.params` instead of `$route.query` as we did earlier.

{{< highlight vue "linenos=tables,linenostart=1" >}}

 <h1>You are on page {{ $route.params.page }}</h1>
{{< / highlight >}}

For us to be able to use a page param it of course needs to be passed in as a dynamic segment of a route path. This is known as a **route parameter**. I've written about this in an earlier blog post about [dynamic routing in vue](../dynamic-routing-in-vue/). This means we'd likely have a view in our router path like:

{{< highlight javascript "linenos=tables,linenostart=1" >}}
const routes = [
...
{ path: '/events/:page', component: Events },
]
{{< / highlight >}}

Using a route parameter with params as props is preferable because it avoids that hard coupling between vue router and your component. To do this we specify `props: true` inside of our router configuration.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
const routes = [
...
{ path: '/events/:page', component: Events, props: true },
]
{{< / highlight >}}

This tells Vue router to send in our route parameters (the dynamic segment part of our route paths) as component props.

We then have the props object within our child component template being used by the template:

{{< highlight html "linenos=tables,linenostart=1" >}}
<template>

<h1>You are on page {{ page }}</h1>
</template>
<script>
    export default {
        props: ['page']
    }
</script>
{{< / highlight >}}

## Props Object Mode vs. Props Function Mode

### Configure a component from router file

Props Object Mode is basically specifying an object inside of our route object props and allowing the component template to render according to its configuration. This is about having a component that we want to configure at the route level.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
const routes = [
    {
        path: '/',
        name: 'Home',
        component: Home,
        props: { showExtra: true }
    }
]
{{< / highlight >}}

Since the v-if checks if `showExtra` is true, the div tag will render.

{{< highlight html "linenos=tables,hl_lines=4,linenostart=1" >}}
<template>
  <div class="home">
    <h1>This is a home page</h1>
    <div v-if="showExtra">Extra stuff</div>
  </div>
</template>
<script>
    export default {
    props: ["showExtra"]
    };
</script>
{{< / highlight >}}

### Transform Query Parameters

Sometimes we want to transform query parameters before sending it to our component as a prop. Props Function Mode is what we need when a query parameter needs to be matched with a component prop but they don't have the same name.

In our case let’s assume our URL is sending in e=true as a query parameter, while our component wants to receive showExtra=true using the same component in the above example. How could we do this transform?

{{< highlight javascript "linenos=tables,hl_lines=6,linenostart=1" >}}
const routes = [
  {
    path: "/",
    name: "Home",
    component: Home,
    props: route => ({ showExtra: route.query.e }),
  },
{{< / highlight >}}

We're creating a new object and setting the showExtra property equal to `route.query.e`, and this anonymous function could also be written like:

{{< highlight javascript "linenos=tables,linenostart=1" >}}
props: route => {
  return { showExtra: route.query.e }
}
{{< / highlight >}}

It's useful to outline this view for when we want to achieve more complex transformations inside of the prop function.

## Building Pagination

The first step to building pagination is to modify our API call to take in two query parameters that come preconfigured from our JSON Server endpoint. In this post I'm using [this endpoint from a Vue Mastery course](https://my-json-server.typicode.com/Code-Pop/Touring-Vue-Router) which has the functionality built-in to paginate.

I then amend the getEvents function from my EventService.js file (which initialises axios and handles api calls from one place) to take in `_limit` for setting the number of items per page, and `_page` to indicate what page we're on.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
getEvents(perPage, page) {
  return apiClient.get('/events?_limit=' + perPage + '&_page=' + page)
},
{{< / highlight >}}

We then want to parse the URL's query parameters for page value and set the current page from the router. This matches the 'page' query parameter to 'page' our component prop using the Prop Function Mode.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
{
  path: '/',
  name: 'EventList',
  component: EventList,
  props: route => ({ page: parseInt(route.query.page) || 1 })
},
{{< / highlight >}}

EventList.vue, which is the component we're paginating and where we have the created lifecycle hook that calls our API function getEvents() from EventService.js, will need to have its API function call modified.

But first we need to pass in the page prop that we've configured earlier for this component from our router index file.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
name: 'EventList',
props: ['page'], // <---- receive the param as a prop, the current page
components: {
  EventCard
},
{{< / highlight >}}

Then in our API function, we fill in the parameters for how many items we want per page for `perPage` and then allow it to pass in the existing prop value with `this.page`.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
created() {
  EventService.getEvents(2, this.page) // <---- 2 events per page, and current page
    .then(response => {
      this.events = response.data
    })
    .catch(error => {
      console.log(error)
    })
},
{{< / highlight >}}

When we run our app instance and append /?page=1 and so on to our root, we get 2 card items for each page, with new items depending on which page we're on. We can then add our pagination links to change the page prop value and use `query:` to specify the previous and next page by subtracting and adding 1. A v-if conditional is also in place so that the previous page link will not appear on the first page.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
    <router-link
      :to="{ name: 'EventList', query: { page: page - 1 } }"
      rel="prev"
      v-if="page != 1"
      >Prev Page</router-link
    >
    <router-link
      :to="{ name: 'EventList', query: { page: page + 1 } }"
      rel="next"
      >Next Page</router-link
    >
    ...
{{< / highlight >}}

We run into a problem here which is that reloading a component with a change of query parameters is not successful. This means `created()` is not getting called again. We can fix this simply by watching the reactive properties for changes. We do this by wrapping the our API call inside a [watchEffect](https://v3.vuejs.org/guide/reactivity-computed-watchers.html#watcheffect) method. This is a new method in Vue 3 which just watches for reactive property changes and reruns the appropriate code if anything changes.

{{< highlight javascript "linenos=tables,linenostart=1" >}}
import { watchEffect } from 'vue' // <--- Have to import watchEffect
...
export default {
...
created() {
    watchEffect(() => {
      this.events = null
      EventService.getEvents(2, this.page)
        .then(response => {
          this.events = response.data
          this.totalEvents = response.headers['x-total-count']
        })
        .catch(error => {
          console.log(error)
        })
    })
  },
  ...
{{< / highlight >}}

Right under watchEffect() we perform a reset with `this.events = null`. This is so when we load another page the current list of events is removed so the user knows that it’s loading.

### Checking for the last page

This is a fairly simple process, the JSON server endpoint sends us headers with total events from the API call. This is from the x-total-count header, meaning our code can be rewritten as follows:

{{< highlight javascript "linenos=tables,linenostart=1" >}}
<template>
  <div class="events">
    ...

    <router-link
      :to="{ name: 'EventList', query: { page: page + 1 } }"
      rel="next"
      v-if="hasNextPage"
      >Next Page</router-link
    >
  </div>
</template>

<script>
  ...
  data() {
    return {
      events: null,
      totalEvents: 0, // <--- Added this to store totalEvents
    }
  },
  created() {
	  watchEffect(() => {
      EventService.getEvents(2, this.page)
        .then(response => {
          this.events = response.data
          this.totalEvents = response.headers['x-total-count']  // <--- Store it
        })
        .catch(error => {
          console.log(error)
        })
	   }
  },
  computed: {
    hasNextPage() {
      // First, calculate total pages
      var totalPages = Math.ceil(this.totalEvents / 2) // 2 is events per page

      // Then check to see if the current page is less than the total pages.
      return this.page < totalPages
    }
  }
}
</script>
{{< / highlight >}}

Note that the router-link is v-if conditional on the existence the hasNextPage() computed property triggering, which will only return if the current page is less than the total number of pages.

### References
- https://www.vuemastery.com/courses/touring-vue-router
