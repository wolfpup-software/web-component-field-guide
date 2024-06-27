# Lifecycle

This article will review the "lifecycle" of a webcomponent as it engages with the DOM.

## First Render

A browser can parse and render web components on initial load.

This makes web components perfectly suited for SSR.

They do this by using a browser feature called `declarative shadow dom`.

```html
<my-element>
  <template shadowmode=closed>
    <p>Hai! :3</p>
    <p id="hydration-status">first render!</p>
  </template>
</my-element>
```

## On Load

Afterwards, the browser defines a custom element after the corresponding javascript file is fetched.

The class below defines a custom-element.

```js
class MyElement extends HTMLElement {}

customElements.define(MyElement, "my-element")
```

## Connecting to the DOM

Web components have three callbacks that 

```js
class MyElement extends HTMLElement {
  connectedCallback() {}

  disconnectedCallback() {}

  adoptedCallback() {}
}

```

## Reactivity

Web components have one callback that can instigate reactivity: `attributeChangedCallback`. It's called when an element's observed attributes change.

`Attributes` are probably the most standard way of crossing the OOP external / internal state boundary for elements.

```js
class MyElement extends HTMLElement {
  attributeChangedCallback(name, prevValue, currValue) {
    // a kind of state changed!
    // do anything you want!
  }
}
```

