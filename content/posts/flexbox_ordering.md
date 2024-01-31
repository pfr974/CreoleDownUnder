+++
title = 'Flexbox, around ordering'
date = 2024-01-31T15:15:56+11:00
draft = false
summary = "Bringing some order"
tags = ['CSS', 'Flexbox']
+++

Looking at our initial css, we have:
```css
/* Colours for each box */
.box1 { background:#1abc9c;}
.box2 { background:#3498db;}
.box3 { background:#9b59b6;}
.box4 { background:#34495e;}
.box5 { background:#f1c40f;}
.box6 { background:#e67e22;}
.box7 { background:#e74c3c;}
.box8 { background:#bdc3c7;}
.box9 { background:#2ecc71;}
.box10 { background:#16a085;}

/* We start writing out flexbox here. The above is just page setup */

.container {
  display: flex;
}

.box {
  flex:1;
}
```
Our page looks like this:

![flexbox1](/images/flexbox/ordering_start.png)

Now, if we had the following lines in our css file:
```css
.box3 {
  order:3;
}
```

We do get:

![flexbox2](/images/flexbox/adding_order.png)

What happened? We do have 10 boxes and by default, they have `order: 0`. So by adding `order: 3` to `box3`, we are telling the browser to put it at the third positionm after all of the other boxes with `order: 0`. Since we only have one box with a specified order, we would have obtained the same result if we had `order: 1` or `order: 2`. However, if now we change our css to:
```css  
.box3 {
  order:1;
}

.box7 {
  order:2;
}
```

We get:

![flexbox3](/images/flexbox/playing_order.png)

Now, for our last example, what happens if we have `order: -1`? We get:

![flexbox4](/images/flexbox/negative_order.png)

Coming from a world where `array[-1]` refers to the last element of the array, we would think it's weird to have box3 moved to the front. However, we have to remember that by default, all boxes have `order: 0`. So, by setting `order: -1` to `box3`, we are telling the browser to put it before all of the other boxes with `order: 0`, so at the front.