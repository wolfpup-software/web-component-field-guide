# Lifecycle

This article will review the "lifecycle" of a webcomponent as it engages with the DOM.

## First Render

A browser can parse and render web components on initial page load.

This makes web components well suited for server side rendering.

Web components accomplish this with the `declarative shadow dom`.

A `declarative shadow dom` can be defined by creating a `<template>` inside a _yet to be defined_ custom element with the attribute `shadowmode=closed`.

```html
<my-element>
  <template shadowmode=closed>
    hello, declarative shadow dom!
  </template>
</my-element>
```

## On Load

The browser will define a custom element after the corresponding javascript file is JIT'd.

The class below defines a custom-element and checks if a declarative shadow dom has already been defined.

Otherwise it creates a shadow dom

```js
class MyElement extends HTMLElement {
  constructor() {
    super();
    let internals = this.attachInternals();
    if (internals.shadowRoot) {
      // shadow dom exists!
      // add events and stuff!
    } else {
      // shadow dom does not exist so make one!
      let shadow_root = this.attachShadow({mode: "open"})
    }
  }
}

customElements.define(MyElement, "my-element")
```

That's all you need to define a web component!

It won't do much but it will exist!

## Connecting to the DOM

Web components have three callbacks used to tell a component it's relationship with a document:

```js
class MyElement extends HTMLElement {
  connectedCallback() {
    // added to the DOM
  }

  disconnectedCallback() {
    // removed from the DOM
  }

  adoptedCallback() {
    // added to a different DOM (a different window or context)
  }
}

```

## Reactivity

Web components have one reactive callback with external state: `attributeChangedCallback`. It signals when an element's observed attributes change.

`Attributes` are probably the most standard way of crossing the OOP external / internal state boundary for elements.

```js
class MyElement extends HTMLElement {
  attributeChangedCallback(name, prevValue, currValue) {
    // a kind of state changed!
    // do anything you want!
  }
}
```

## Reactivity without querying

Web components are simply dom elements.

After an event or message is recieved, an "update" or "render" function can be called on an element.

And that's reactivity!

WOOOOFF!

### Events

If a component needs to react to external state without being directly coupled to external contexts, the browser provides several kinds of message queues.

Consider using `events` to pass scoped javascript objects through the DOM.

### postMessage

If events don't work for you, consider using the `BlahBlah.postMessage` APIs so send serializable data across different contexts.
- `Window.postMessage(...)` to send serializable stuff across a document
- `BroadcastChannel.postMessage(...)` to send serializable stuff across windows and tabs and iframes and workers
- `MessageChannelEvent.ports[N].postMessage(...)` to send serial data between ports
- `Worker.postMessage(...)` to send serializable stuff to worker threads
