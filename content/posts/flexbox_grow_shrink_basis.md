+++
title = 'Flexbox, around `flex-grow`, `flex-shrink` and `flex-basis`'
date = 2024-02-01T14:23:11+11:00
draft = false
summary = "Understanding the flex property"
tags = ['CSS', 'Flexbox']
+++


When I had my first look at flexbox, I was curious about the meaning of the following:

```css
flex: 1 3 400px;
```

What do these numbers do? To get an answer, we have to look at `flex-grow`, `flex-shrink` and `flex-basis`. Let's start with the following example:

```css
.container {
  display:flex;
}

.box1 {
  flex-basis:400px;
}

.box2 {
  flex-basis:400px;
}
```

![Two boxes with flex-basis](/images/flexbox/flexbox_basis.png)

As you can see, the boxes have the same size, 400 pixels wide. There is also empty unused space on the right of the boxes. `flex-basis` is here to state that ideally, the boxes should be 400 pixels wide. Now let's add `flex-grow` to the boxes:

```css
.container {
  display:flex;
}

.box1 {
  flex-basis:400px;
  flex-grow:2;
}

.box2 {
  flex-basis:400px;
  flex-grow:1;
}
```

This results in the following:

![Two boxes with flex-basis and flex-grow](/images/flexbox/flexbox_using_grow.png)

The boxes now fill the available space. With `flex-grow:2`, the first box then gets twice as much available space as the second box with `flex-grow:1`.

On the other hand, `flex-shrink` will dictate how the boxes shrink when there is little space available. Let's add `flex-shrink` to the boxes:

```css
.container {
  display:flex;
}

.box1 {
  flex-basis:400px;
  flex-grow:2;
  flex-shrink:2;
}

.box2 {
  flex-basis:400px;
  flex-grow:1;
}
```

When shrinking the bowser window, we get:

![One box with flex-shrink](/images/flexbox/flexbox_using_shrink.png)

This time, when there is little room available, the first box is shrinking itsel twice as much as the second box.

Finally, we should point out that these three different properties can be combined into a single one, `flex`. The following is equivalent to the previous example:

```css
.container {
  display:flex;
}

.box1 {
  flex:2 2 400px;
}

.box2 {
  flex:1 1 400px;
}
```

Here, the first number is `flex-grow`, the second is `flex-shrink` and the third is `flex-basis`.