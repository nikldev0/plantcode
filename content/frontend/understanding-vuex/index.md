---
title: "Understanding Vuex"
date: 2021-09-01T21:03:34+01:00
draft: false
subtitle: "The quest for a single source of truth"
banner: https://plantcode.blog/frontend/understanding-vuex/understanding-vuex.png
categories: -frontend
tags:
  - Vue
---

State is the data that your components depend on and render. The aim of Vuex is to provide a single source of truth for this state and the ability to change it in predictable, organised patterns. This entails each component having direct access to a global state rather than its own version of state.

The global state is reactive, meaning when one component updates the state other components that are using that data get notified and automatically receive the new value.

## Vuex Terminology

First consider the basic structure of a Vue instance:

{{< highlight javascript "linenos=tables,linenostart=1" >}}
const app = createApp({
data: {
...
},
methods: {
...
},
computed: {

        }
    })

{{< / highlight >}}

While the Vue instance has data, Vuex store has state. Both are reactive. Vue instance methods can, among other things, update our data. Vuex actions can update our state. We only pass in updates to our state via mutations however. Mutations are used to commit and track state changes. **Mutations shouldn't be used to modify state directly but should be dispatched via actions**. Just as the instance has computed properties which can access our instance data, the Vuex store has getters which can access our state.

🧠 *For better scalability, some projects have individual Vuex components in separate files.*

{{< highlight javascript "linenos=tables,linenostart=1" >}}
const store = createStore({
 state: {
     ...
 },
 mutations: {
     ...
 }
 actions: {
     ...
 },
 getters: {
     ...
 }
})
{{< / highlight >}}

The 'context' object contains all the properties of our Vuex store which allows us to 'commit' mutations. The first mutation we're committing is to change the loading status by calling the mutation name, and then setting it to loading.

Do not put state directly in our data as this can cause reactivity issues, although bear in mind that the way to access the global store is through this.$store.

## Dynamic Mutations

Payloads are used for dynamic mutations. This is a second parameter that the mutation can accept. This is how basic mutations work:

First we want to post our event to our mock database with JSON Server. This means we need to add a new post request to our EventService.js file.

{{< highlight javascript "linenos=tables, linenostart=1" >}}
  postEvent(event) {
    return apiClient.post('/events/' + event)
  }
{{< / highlight >}}

This method takes in the event and *posts* it to our events endpoint.

We can then use our postEvent method within our EventCreate component. We'll do so **within the onSubmit method**, after importing EventService into this component.

{{< highlight javascript "linenos=tables, linenostart=1" >}}
  methods: {
    onSubmit() {
      const event = {
        ...this.event,
        id: uuidv4(),
        organizer: this.$store.state.user
      }
      EventService.postEvent(event)
        .then(() => {
          this.$store.commit('ADD_EVENT', event)
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
{{< / highlight >}}

Notice how this has two parameters, one for accepting the mutation and the other for an event. This event parameter is the dynamic payload which will be determined by user input. For the first parameter within the postEvent 'this.$store.commit('ADD_EVENT', event)', we're telling it to commit the mutation that we've specified (ADD_EVENT), and then we're passing in the event that we want to add.

We're also using the [NPM uuid library](https://www.npmjs.com/package/uuid) which allows us to set random, cryptographically strong values. We can use this to set a unique id for every onSubmit request so each submission has a unique id number.

Commit is just Vuex syntax that means we are calling our mutation, which *commits* us to a new state within our app. The mutation name in all caps, is a convention that helps outline code that is causing state changes in your app.

Then we add the mutation in our src\store\index.js file:

{{< highlight javascript "linenos=tables,hl_lines=8-12, linenostart=1" >}}
import { createStore } from 'vuex'

export default createStore({
  state: {
    user: 'Nikhil K',
    events: []
  },
  mutations: {
    ADD_EVENT(state, event) {
      state.events.push(event)
    }
  },
  actions: {},
  modules: {}
})
{{< / highlight >}}

As you can see, our ADD_EVENT mutation takes in two arguments: the Vuex state itself, and the event that we want to push onto our new events array within that state. That’s as simple as it is. We’ve just written the code that we can call to add events to our state.

## Changing state through action dispatch

The previous workflow is not very good practice. It is recommended by Core Vue Team Members to [always wrap your mutations within actions](https://next.vuex.vuejs.org/guide/actions.html).

Actions allow us to wrap some business logic around our mutations that we either commit or not depending on what happens with that business logic. The action is like expressing the intent for something to happen for some state change to be made within our app.

A single action can commit multiple mutations so it's more powerful. First we add our action to our store/index.js file under our mutations:

{{< highlight javascript "linenos=tables, linenostart=1" >}}
  actions: {
    // using the context object
    createEvent({ commit }, event) {
      EventService.postEvent(event)
        .then(() => {
          commit('ADD_EVENT', event)
        })
        .catch(err => {
          console.log(err)
        })
    }
  },
{{< / highlight >}}

The context object gives us the ability to run commits on our Vuex store. Notice this means we don't have to add this.$store.commit and can just write commit('ADD_EVENT', event) -- this is because we are within the store and don't need access to it as a global object which is what `this.$store` is doing for us. We're also performing the API call directly in our action so when we dispatch is all we need is a single line of code.

The action dispatch (dispatch is when we actually call the action) can happen in the SFC (single file component) that renders the relevant template. The 'onSubmit' function that we've got in our particular case matches the async function we use to dispatch our action.

We simply add the following line:

{{< highlight javascript "linenos=tables, linenostart=1" >}}
 this.$store.dispatch('createEvent', event)
{{< / highlight >}}

Remember the action simply calls the mutation, it's the mutation that does the work of changing state.

When we call our action we refer to the name of the action, as the payload that eventually gets passed in as a payload for the mutation within.

{{< highlight javascript "linenos=tables,hl_lines=8, linenostart=1" >}}
methods: {
    onSubmit() {
      const event = {
        ...this.event,
        id: uuidv4(),
        organizer: this.$store.state.user
      }
      this.$store.dispatch('createEvent', event)
    }
  }
{{< / highlight >}}

One of the benefits of Vuex is that the data that comes with the store does not live specifically in any particular component, which is why we delete that data property and call the global state object in a computed method -- we use a computed method because it allows us to maintain reactivity.
