+++
title = 'Creating Elements with JavaScript'
date = 2024-02-09T07:36:32+11:00
draft = false
summary = "Adding elements 101"
tags = ['HTML', 'JavaScript']
+++

The following personal notes are based on the [Udemy course by Maximilian Schwarzmüller](https://www.udemy.com/course/javascript-the-complete-guide-2020-beginner-advanced/).

At the start, we have a simple html page obtained from [a previous post](/posts/styling_dom_elements/):

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>DOM Interaction</title>
    <style>
      .goldenrod-bg {
        background-color: goldenrod;
        color: black;
      }

      .visible {
        display: block;
      }

      .invisible {
        display: none;
      }
    </style>
    <script src="app.js" defer></script>
  </head>
  <body>
    <header><h1 id="main-title">Dive into the DOM! Styling DOM Elements</h1></header>
    <section class="goldenrod-bg visible">
      <ul>
        <li class="list-item">Item 1</li>
        <li class="list-item">Item 2</li>
        <li class="list-item">Item 3</li>
      </ul>
    </section>
    <button id="toggle-visibility">Toggle visibility</button>
    <input type="text" value="default text" />
  </body>
</html>
```

As a reminder, `app.js` is:

```javascript
const ul = document.body.firstElementChild.nextElementSibling;
const firstLi = ul.firstElementChild;

console.log(firstLi);

const section = document.querySelector('section');
// section.className=''

const button = document.getElementById('toggle-visibility');

button.addEventListener('click', () => {
  /* if (section.className === 'goldenrod-bg visible') {
    section.className = 'goldenrod-bg invisible'
  } else {
    section.className = 'goldenrod-bg visible';
  } */
  section.classList.toggle('visible');
  section.classList.toggle('invisible');
});
```

Let's now see how to add elements to the DOM via JavaScript.

## Adding Elements with `innerHTML`:

At the moment, the `section` element contains an unordered list:

![Initial state](/images/javascript/adding_element_js_innerhtml_start.png)

Let's open our browser's console and type the following:

```javascript
section.innerHTML = '<h2>Some new content</h2>';
``` 

This will replace the unordered list with an `h2` element:

![After using innerHTML](/images/javascript/adding_element_js_innerhtml_h2.png)

But there is "Adding" into the title of this section, so let's do this instead and add a new list element `li` to the `ul` of `section`:

```javascript
const list = document.querySelector('ul');
list.innerHTML = list.innerHTML + '<li>Item 4</li>';
```

As expected we get a new list item showing up:

![After adding a new list item](/images/javascript/adding_element_js_innerhtml_li.png)

However, while this is not really noticeable here, we have replaced the entire content of the `ul` with a new one. For a more complex page, this could lead to performance issues. What we would like to do is to add a new `li` to the `ul` without replacing the entire content. `insertAdjacentHTML` can help us with that.

## Adding Elements with `insertAdjacentHTML`:

The `insertAdjacentHTML` method allows us to insert a string of HTML into the DOM at a specified position. The method takes two parameters: the position and the string of HTML to be inserted. According to the official documentation, the `position` parameter can be one of the following:
- `beforebegin`: Before the element itself.
- `afterbegin`: Just inside the element, before its first child.
- `beforeend`: Just inside the element, after its last child.
- `afterend`: After the element itself.

Going back to our example, we could add a new `li` to the `ul` of `section` via the following:

```javascript
list.insertAdjacentHTML('beforeend', '<li>Item 4</li>');
```

This hasn't replaced the content of the `ul` like `innerHTML` did. Performance wise, there is not really that much of a change for this simple example but for more complex pages, you can start to see why this is useful.

## Adding Elements with `createElement` and `appendChild`:

Finally, while `insertAdjacentHTML` is useful, it does not give us the liberty of deciding what to do with the newly created element. Here, we only have one `ul` on our page so there is not a lot of thinking required but what it would be better if we could first create a new element and then decide where to place it. `createElement` and `appendChild` are then there for us:

```javascript
const list = document.querySelector('ul');
newListItem = document.createElement('li');
newlistItem.textContent = 'Item 4';
list.appendChild(newListItem);
```

This achieves the same result as the previous subsection. Again, not much of a difference here and we actually typed more lines but for a more complex page, this gives us more liberty over what to do with a new element.