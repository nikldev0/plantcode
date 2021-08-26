---
title: "Dynamic Routing in Vue"
date: 2021-08-26T17:18:23+01:00
draft: false
subtitle: "Making Vue router malleable"
banner: https://www.plantcode.blog/me/banner.jpg
categories: -frontend
tags:
  - Vue
---

Dynamic routing is needed in Vue when you want to add to, remove from or otherwise change routes on the fly while the application is already running. This is particularly useful when you want a dynamic segment of a URL that mutates according to - for example - user, id or page number.

Dynamic routing involves understanding how props work.

Props are custom attributes for passing data from a parent to a child component. If you liken a prop to a funnel, then you need to understand that it's the child component that holds the funnel, so it's the child component that will have the prop object. Allow me to explain.

In the `index.js` file where I've configured my routes I have the params of id at the end of the events path. The colon here is similar to the [v-bind shorthand](https://v3.vuejs.org/guide/class-and-style.html#object-syntax) and allows us to respond to URLs matching the specified pattern.

{{< highlight typescript "linenos=tables,linenostart=1" >}}
const routes = [
//...some routes here...
{
path: '/events/:id',
props: true,
name: 'EventDetails',
component: EventDetails
},
{{< / highlight >}}

This means when a user lands on a path with `/events/123` or `/events/456`, they will be served the same route but with a variable parameter which gets updated with the id of whichever event is currently displayed on that route. In another usecase this could've been `:user` or `:orderNumber`.

The dynamic id is fed into the component as a prop, which allows, for example, the api endpoint to be adjusted accordingly for that specific page. Note that we also set the boolean `props` to be true to give the component that will be displaying the information to access this dynamic segment parameters as a prop.

If we were to set a router-link wrapped around a card which determines the value for the page depending on the id, it would look something like this:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
<router-link :to="{ name: 'EventDetails', params: { id: event.id } }">
{{< / highlight >}}

Here the id param is being determined by event.id. This particular component has `event` as a prop funneling in data from a parent component for each event from a list.

A particular event's id is looped into the id parameter of the dynamic route - meaning we get a URL with an id that matches the id of the event, and therefore unique pages for each event.

In this project, it's actually another component (EventDetails) which handles the templating for each EventCard. This means that the card links to EventDetails and the route's path is appended with the event's id in that component.

Below we have the contents of the script tag within our EventDetails component:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
<script>
import EventService from '@/services/EventService.js'
export default {
    props: {
    id: {
      type: String
    }
  },
    data() {
    return {
        event: null
    }
  },
  created() {
    EventService.getEvent(this.id)
    .then(response => {
    this.event = response.data
    })
    .catch(error => {
    console.log(error)
    })
  }
}
</script>
{{< / highlight >}}

First, we're importing EventService - which is the home of our axios configuration and what makes API calls neater for us. We're then feeding the id param into the EventDetails component as a prop because this is where the individual event information gets configured.
When we say getEvent(this.id), we’re referring to the newly added id prop. When EventDetails is routed to and thus created, it now makes a request for the event with the id that is found in the dynamic parameter of the route’s path.

You can consult the documentation if you'd like to [learn more about the route object](https://router.vuejs.org/api/#the-route-object) and its properties.
