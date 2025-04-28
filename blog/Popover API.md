---
title: Say Hello to the Popover API
menu_order: 2
post_status: publish
featured_image: _images/post-background.png
taxonomy:
  category:
    - javascript
  post_tag:
    - javascript
    - popover
    - ui
    - frontend
---

<div class="toc" markdown="1">

## On this Page
* [What's the Popover API?](#whats-the-popover-api)
* [A Simple Popover in Action](#a-simple-popover-in-action)
* [Understanding the `popover` Property](#understanding-the-popover-property)
    * [`auto`: The Smart Popover](#auto-the-smart-popover)
    * [`hint`: The Gentle Hint (Experimental)](#hint-the-gentle-hint-experimental)
    * [`manual`: Full Control Popovers](#manual-full-control-popovers)
* [Feature Detection: Knowing if Your Browser Understands](#feature-detection-knowing-if-your-browser-understands)
* [Setting Up Popovers with JavaScript](#setting-up-popovers-with-javascript)
* [Wrapping Up: Build Better Interactions, Natively!](#wrapping-up-build-better-interactions-natively)

</div>

<div class="guru-main" markdown="1">

## Say Hello to the Popover API: Your New Best Friend for Interactive Elements!

Hey web developers! 👋 Remember those times you spent wrestling with JavaScript to create accessible and well-behaved popovers, tooltips, or select menus? Well, get ready for some good news! The **Popover API** has officially landed in all major browsers, making it **Baseline Newly available** as of January 27, 2025!


### What's the Popover API?

The Popover API is a built-in browser feature that provides a standardized way to create various interactive popover elements commonly used in web applications. Think about those little info boxes that appear when you hover over something, or dropdown menus, or even notification panels. Previously, you had to rely heavily on JavaScript to handle their behavior (like showing, hiding, accessibility, and positioning). This new API brings all that power directly to the browser, often with just a few lines of HTML!

### A Simple Popover in Action

Let's dive right into a basic example to see how easy it is:

```html
<button popovertarget="my-popover">Open Popover</button>

<div id="my-popover" popover>
  <p>I am a popover with more information. Hit <kbd>esc</kbd> or click away to close me.</p>
</div>
````

That's it! With just these few lines of HTML, you get a popover that:

  * Opens when you click the "Open Popover" button.
  * Can be closed by pressing the `Esc` key or clicking outside the popover.

Pretty neat, huh? The browser handles all the underlying magic for you, ensuring accessibility and proper behavior.

### Understanding the `popover` Property

The key to this magic lies in the `popover` HTML attribute and the corresponding `popover` property in JavaScript. This property tells the browser how the element should behave as a popover. It accepts a few different values:

#### `auto`: The Smart Popover

When you set `popover="auto"`, you get a "light-dismissible" popover. This means:

  * It can be closed by user interaction outside the popover (like clicking elsewhere).
  * It can usually be closed by pressing the `Esc` key.
  * Typically, only one `auto` popover can be shown at a time. If you open another one, the first one will automatically close (unless they are nested).

Think of this as the go-to option for most common popover scenarios like tooltips or simple info panels.

#### `hint`: The Gentle Hint (Experimental)

The `hint` value is currently experimental. Popovers with `popover="hint"` have different behavior:

  * They don't automatically close other `auto` popovers when they appear.
  * They will close other `hint` popovers.
  * They are also light-dismissible and respond to close requests.
  * They are often intended to be shown and hidden based on non-click JavaScript events like `mouseover` or `focus`.

Keep an eye on the evolution of this value as it might bring interesting possibilities for less intrusive UI elements.

#### `manual`: Full Control Popovers

If you need more control over when and how your popover appears and disappears, `popover="manual"` is your friend.

  * These popovers are **not** light-dismissible.
  * They don't automatically close when other popovers are shown.
  * You need to explicitly show and hide them using JavaScript or declarative buttons.
  * You can have multiple `manual` popovers open simultaneously.

This is useful for elements like custom dropdown menus or persistent panels that shouldn't close automatically.

### Feature Detection: Knowing if Your Browser Understands

Before you start heavily relying on the Popover API, it's always a good practice to check if the user's browser supports it. You can do this easily with JavaScript:

```javascript
function supportsPopover() {
  return HTMLElement.prototype.hasOwnProperty("popover");
}

if (supportsPopover()) {
  console.log("Your browser supports the Popover API!");
  // You can now safely use popover features
} else {
  console.log("The Popover API is not supported in this browser.");
  // Provide a fallback or alternative functionality
}
```

This simple function checks if the `popover` property exists on the `HTMLElement` prototype, which indicates browser support.

### Setting Up Popovers with JavaScript

While you can create basic popovers using just HTML attributes, you can also manipulate them dynamically using JavaScript. Here's an example of how you might set up a popover and a button to toggle its visibility:

```javascript
const popover = document.getElementById("my-dynamic-popover");
const toggleBtn = document.getElementById("dynamic-toggle-btn");
const popoverSupported = supportsPopover();

if (popoverSupported) {
  popover.popover = "auto"; // Or "manual" depending on your needs
  toggleBtn.popoverTargetElement = popover;
  toggleBtn.popoverTargetAction = "toggle"; // 'show', 'hide', or 'toggle'
} else {
  console.log("Popover API not supported. Implementing fallback.");
  // Implement your own popover logic here
}
```

In this code:

  * We get references to our popover element and the button.
  * We check for Popover API support.
  * If supported, we set the `popover` property on the popover element.
  * We use `popoverTargetElement` on the button to link it to the popover.
  * `popoverTargetAction` tells the button what to do with the popover ("toggle" will show it if hidden and hide it if shown).

### Wrapping Up: Build Better Interactions, Natively!

The Popover API is a game-changer for creating accessible and interactive web elements. By bringing this functionality directly into the browser, it simplifies development, improves accessibility, and often reduces the amount of JavaScript you need to write. So, go ahead, explore the possibilities, and start building better user experiences with the native power of the Popover API!

Master the Code, Be the Guru\!

</div\>

