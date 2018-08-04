---
title: Beedle Docs
---
# Beedle

Beedle is a tiny little library to help you manage state across your application. Inspired by great libraries like Vuex and Redux, Beedle creates a central store that enables you to both better control and cascade state across your application.

This library was initially created as a prototype for [this article on CSS-Tricks](https://css-tricks.com/build-a-state-management-system-with-vanilla-javascript/), where you learn how to build a state management system from scratch with vanilla JavaScript.



## Not a Redux replacement

Beedle is inspired by libraries like Redux, but certainly isn't designed to replace it. Beedle is aimed more at tiny little applications or where a development team might be looking to create the smallest possible footprint with their JavaScript.



## Getting started

You can pull Beedle down via [npm](http://npmjs.com) or take a [zip of this repository](https://github.com/hankchizljaw/beedle/archive/master.zip). The rest of this guide assumes you've used npm.

### 1) Install

Run `npm install beedle` in your project directory.



### 2) Create a `store` instance

First up, import it into your JavaScript:

```JavaScript
import Store from 'beedle';
```



Once you've got that you should create some `actions`, `mutations` and some initial state:

```javascript
const actions = {
    saySomething(context, payload) {
        context.commit('setMessage', payload);
    }
};

const mutations = {
    setMessage(state, payload) {
        state.message = payload;

        return state;
    }
};

const initialState = {
    message: 'Hello, world'
};
```

Once you've got those setup, you can create a `Store` instance like this:

```javascript
const storeInstance = new Store({
    actions,
    mutations,
    initialState
});
```

### 3) Use in your app

Let's say you've got a text box that you type a message into. When the content is changed, it could dispatch a new message to your store:

```javascript

// Grab the textearea and dispatch the action on 'input'
const textElement = document.querySelector('textarea');

textElement.addEventListener('input', () => {

    // Dispatch the action, which will subsequently pass this message to the mutation
    // which in turn, updates the store's state
    storeInstance.dispatch('saySomething', textElement.value.trim());
});
```

### 4) Listen for changes

Beedle uses the Pub/Sub pattern to transmit changes. Let's attach the message to a DOM element:

```javascript
// Grab the text element and attach it to the stateChange event
const messageElement = document.querySelector('.js-message-element');
const { state } = storeInstance;

// This fires every time the state updates
storeInstance.events.subscribe('stateChange', () => {
    messageElement.innerText = state.message;
});
```

Head over to the [demo](https://github.com/hankchizljaw/beedle/tree/v0.3.0/demo) to see this in action 🚀

## Acknowledgements 

Thanks to [Eli Fitch](https://twitter.com/EliFitch/) for giving me the idea to call this Beedle. This matches my preference to call my little projects names based on Zelda. [Here's Beedle from Zelda](https://zelda.gamepedia.com/Beedle).

Thanks to the incredible people who maintain projects such as [Redux](http://redux.js.org), [Vuex](http://vuex.vuejs.org) and [MobX](http://mobx.js.org) et. al. Thanks for making our lives easier and for inspiring this project.