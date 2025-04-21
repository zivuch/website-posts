---
title: Deep Cloning Objects in JavaScript
menu_order: 1
post_status: publish
featured_image: _images/post-background.png
taxonomy:
  category:
    - javascript
  post_tag:
    - javascript
    - clone
---

<div class="toc" markdown="1" >

## On this Post
* [The Need for Deep Cloning](#the-need-for-deep-cloning)
* [The Pitfalls of Shallow Cloning](#the-pitfalls-of-shallow-cloning)
* [JSON.stringify and JSON.parse: A Clever but Flawed Approach](#jsonstringify-and-jsonparse-a-clever-but-flawed-approach)
* [Enter `structuredClone`: The Modern Solution](#enter-structuredclone-the-modern-solution)
* [`structuredClone` in Action](#structuredclone-in-action)
* [What `structuredClone` Can't Do](#what-structuredclone-cant-do)
* [Browser and Runtime Support](#browser-and-runtime-support)
* [Goodbye Libraries, Hello Native!](#goodbye-libraries-hello-native)
* [Conclusion](#conclusion)

</div>

<div class="main" markdown="1">

## Deep Cloning Objects in JavaScript: Embrace `structuredClone`!

Hey there, fellow JavaScript enthusiasts! Have you ever found yourself in a situation where you needed to make a true, independent copy of an object in JavaScript? A copy where changes to the new object don't mysteriously alter the original? If so, you're in the right place!



### The Need for Deep Cloning

A common pattern in JavaScript is to create an *immutable clone* of an object. This is incredibly useful when you want to modify a copy without affecting the original data. Imagine you're working with user settings – you might want to create a temporary copy, let the user make changes, and only update the original settings if they hit "save."

### The Pitfalls of Shallow Cloning

For creating copies, you might have seen or even used the spread operator (`...`) like this:

```javascript
const originalObj = { a: 1, b: 2 };
const newObj = { ...originalObj };

newObj.b = 3;
console.log(originalObj); // Output: { a: 1, b: 2 }
```

This works great for simple, flat objects! However, things get tricky when you have nested objects:

```javascript
const originalObj = { deep: { a: 1, b: 2 } };
const newObj = { ...originalObj };

newObj.deep.b = 3;
console.log(originalObj); // Output: { deep: { a: 1, b: 3 } }
```


As you can see, the `deep` property is still a *reference* to the same object in both `originalObj` and `newObj`. Modifying it in `newObj` inadvertently changes it in `originalObj` too. This is called a **shallow clone**.

### `JSON.stringify` and `JSON.parse`: A Clever but Flawed Approach

To overcome this, developers often resorted to a clever trick using JSON:

```javascript
const originalObj = { deep: { a: 1, b: 2 } };
const newObj = JSON.parse(JSON.stringify(originalObj));

newObj.deep.b = 3;
console.log(originalObj); // Output: { deep: { a: 1, b: 2 } }
```

This works for deeply nested objects\! By converting the object to a JSON string and then parsing it back, you create a completely new object in memory. However, this method has significant drawbacks:

  * **Loss of Data Types:** `JSON.stringify()` calls `.toString()` on many properties. `Date` objects become strings:

    ```javascript
    const originalObj = { date: new Date() };
    const newObj = JSON.parse(JSON.stringify(originalObj));
    console.log(newObj.date); // Output: "2025-04-15T18:54:00.000Z" (or similar string)
    ```

  * **Empty Objects for Complex Types:** `Set` and `Map` objects are converted to empty plain objects:

    ```javascript
    const originalObj = {
      set: new Set([1, 2, 3]),
      map: new Map([["a", 1], ["b", 2]]),
    };
    const newObj = JSON.parse(JSON.stringify(originalObj));
    console.log(newObj.set); // Output: {}
    console.log(newObj.map); // Output: {}
    ```



### Enter `structuredClone`: The Modern Solution

Say hello to `structuredClone()`\! This built-in JavaScript function provides a robust and straightforward way to create deep copies of objects. It handles various data types correctly, making your cloning tasks much simpler and more reliable.

```javascript
// Good - clones everything deeply and correctly!
const cloned = structuredClone(obj);
```

### `structuredClone` in Action

Let's see `structuredClone()` in action with the examples we looked at earlier:

  * **Deeply Nested Objects:**

    ```javascript
    const originalObj = { deep: { a: 1, b: 2 } };
    const newObj = structuredClone(originalObj);

    newObj.deep.b = 3;
    console.log(originalObj); // Output: { deep: { a: 1, b: 2 } }
    ```

    No more accidental modifications\!

  * **Handling Dates, Sets, and Maps:**

    ```javascript
    const originalObj = {
      date: new Date(),
      set: new Set([1, 2, 3]),
      map: new Map([["a", 1], ["b", 2]]),
    };
    const newObj = structuredClone(originalObj);

    console.log(newObj.date instanceof Date); // Output: true
    console.log(newObj.set instanceof Set);   // Output: true
    console.log(newObj.map instanceof Map);   // Output: true
    console.log([...newObj.set]);           // Output: [ 1, 2, 3 ]
    console.log(newObj.map.get("a"));        // Output: 1
    ```

    `structuredClone()` preserves the original data types\!



### What `structuredClone` Can't Do

While `structuredClone()` is fantastic, it's important to know its limitations:

  * **Functions:** Functions cannot be cloned and will throw a `DataCloneError`.
  * **DOM Nodes:** Attempting to clone DOM nodes will also result in a `DataCloneError`.
  * **Property Descriptors, Setters, and Getters:** These are not cloned. For example, with a getter, only the resulting value is cloned, not the getter function itself.
  * **Object Prototypes:** The prototype chain is not duplicated. An object cloned from a custom class will lose its `instanceof` relationship with that class.

### Browser and Runtime Support

The great news is that `structuredClone()` is supported in all major modern browsers, as well as Node.js and Deno\! This means you can confidently use it in your projects without relying on external libraries in most cases.

### Goodbye Libraries, Hello Native\!

For a long time, libraries like Lodash with its `_.cloneDeep()` function were the go-to solution for deep cloning. While these libraries are powerful, importing an entire utility library just for deep cloning can sometimes feel overkill, especially when browsers now offer a native solution. `structuredClone()` provides a performant and built-in alternative for most deep cloning needs.

### Conclusion

`structuredClone()` is a powerful and welcome addition to the JavaScript language. It offers a clean, efficient, and reliable way to perform deep cloning of objects, handling complex data types correctly without the pitfalls of the `JSON.stringify`/`JSON.parse` trick. While it has some limitations, for the vast majority of deep cloning scenarios, `structuredClone()` is the modern and recommended approach. So, go ahead and embrace the power of native deep cloning in your JavaScript projects\!

For a more in-depth look at the nuances, specific edge cases and compatibility details of `structuredClone()`, be sure to check out: [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone)

Master the Code, Be the Guru!

</div>
