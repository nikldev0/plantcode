---
title: "Using Vue Devtools Well"
date: 2021-10-12T17:47:50+01:00
draft: false
subtitle: "Know your way around the new browser extension"
banner: https://www.plantcode.blog/me/banner.jpg
categories: -frontend
tags:
  - Vue
---

The new Vue Devtools extension for Firefox and Chrome makes it easier to edit and manipulate Vue applications at runtime. This makes for a better debugging experience as you can update component data on the fly using *only*  the browser's devtools rather than going back and forth to your code editor.

Once you've downloaded the [Vue extension from the Chrome web store](https://chrome.google.com/webstore/detail/vuejs-devtools/ljjemllljcmogpfapbkkighbhhppjdbg), or the [xpi file for Firefox](https://github.com/vuejs/devtools/releases) you'll be able to see a new tab popup along the top of your devtools window. If you're unable to see this, clicking the double chevron icon should expand the selection.

{{< figure src="devtools-vue-tab.png" caption="Accessing the new Vue tab in DevTools" >}}

{{< figure src="inspector.png" caption="Multi-app view and Inspector icon" >}}

If you have mounted multiple app instances on the same page you can toggle between them using the multi-app dropdown. The compass icon is our element inspector, and is the main tool we use to navigate through our app structure within DevTools.

If you inspect an element using this panel you will notice on the top right-hand-side we have three buttons pertaining to the selected element like so:

{{< figure src="Inspector-tab.png" caption="Inspector tab with 'Scroll to Component', 'Show Render Code' and 'Inspect DOM' buttons respectively" >}}

The first icon scrolls to the component on the page, the second icon reveals the code the browser uses to render the component, and the third icon allows you to see the Vue component as it's written and rendered in the DOM.

If you'd like a reminder of what component is rendering something on the page I like using this crosshair icon (the first one of the right) next to the Components dropdown.

{{< figure src="crosshair.png" caption="Crosshair icon to select component in the page" >}}

Beside a component you'll notice what's called a performance pill. This indicates the time it took to render or patch. If you hover over the pill, a tooltip will appear breaking down the timings.

{{< figure src="performance-pill.png" caption="Component performance time" >}}

Vue 3 Devtools can be fully integrated with [external plugins](https://devtools.vuejs.org/plugin/plugins-guide.html) and comes with a couple of plugins built in. These include the plugins for the Router and Vuex. You can click on the components dropdown to toggle between these plugin views.

{{< figure src="plugins.png" caption="Components dropdown" >}}

For example, the router plugin view will show you a list of current available routes.

The Vuex panel shows you the current condition of state and, for example, what/how getters are retrieving from it. You can also rewind mutations that have occurred upon render or after a v-on directive has been triggered.

Finally, next to the Inspector compass icon you'll notice the timeline icon. This is the events timeline which displays color-coded events as they're happening in real time. Clicking on an event dot will display more information about it. Here you will also find custom events emitted by a component with all the payload information.

If you need to narrow down the amount of event information in the panel you can click on Select layers icon to filter your results.

{{< figure src="tl-layers.png" caption="Toggle layers on and off so you're only tracking what you need to be" >}}

The final thing to note is that on custom components you have a view in editor icon (far right). This opens up the code responsible for that custom component in your code editor! Note that this won't work for automated components created by Vue such as the RouterView component.

{{< figure src="editor.png" caption="View in Editor icon" >}}
