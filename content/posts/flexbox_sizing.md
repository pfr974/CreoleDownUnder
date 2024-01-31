+++
title = 'Flexbox, around sizing'
date = 2024-02-01T10:14:41+11:00
draft = false
summary = "Understanding Flexbox sizing"
tags = ['CSS', 'Flexbox']
+++

We begin with the following: 

![flexbox1](/images/flexbox/flexbox_sizing_demo_start.png)

if we add the following lines to our starting CSS file:

```css
.box {
  flex:1;
}

.box3 {
  flex:2;
}
```

We then get the following:

![flexbox2](/images/flexbox/flexbox_sizing_demo_1.png)

This is saying that initially, all the boxes should be the same size when there is enough space. By adding a `flex:2` property for `box3`, we are overwriting the `flex:1` property set for all the boxes and saying that `box3` should be double the size of the others **if there is enough space.**