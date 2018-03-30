---
layout: post
title: "[HL-20] A bitty note of Views on Backbone.js"
categories:
    - frontend
tags:
    - backbonejs
published: true
---

<!-- Outline:
1. Issue of setting el
2. Different view rendering patterns -->

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Issue of setting 'el'](#issue-of-setting-el)
- [Two ways of rendering views](#two-ways-of-rendering-views)
- [Further reading](#further-reading)

<!-- /TOC -->

I recently started to use Backbone.js in my frontend work. I just want to write
down  two important issues I meet when using Backbone.js.

# Issue of setting 'el'

'el' is the DOM element where the view will render. If you define it when
creating or initializing a view, you use 'this.$el' to refer an element in the
existing DOM tree. So all your operations change the **REAL** DOM tree.

However, what if we don't set 'el' when creating a view?  The answer is that it
creates a new element '<div></div>' in memory, which has **NOT** been inserted
into the existing DOM tree.

This causes a big trouble. Why? Because your added element,

~~~ js
this.$el.html('your_added_element')
// or
this.$el.append('your_added_element')
~~~
cannot be found in the existing DOM tree:

~~~ js
$('your_added_element') // => undefined
~~~

You have to search your added element by writing:

~~~ js
$('your_added_element', this.$el)
~~~

Thus, you have two ways to create a view in Backbone.js:

1.   Modify the element in the existing DOM tree.
2.   Create a new element '<div></div>'. You can also create a new tag by specifying 'tagName', such as 'li'.

# Two ways of rendering views

Based on above discussions, we have two ways to render views:

Changing the existing DOM tree:

~~~ js
let view = new View({el: 'your_selector', model: model})
view.render() // change the DOM tree
~~~

Not changing the existing DOM tree and all operations happen in a newly  created
element in memory:

~~~ js
let subview = new View({model: model})
this.$el.find('your_selector').html(subview.render().el) // inject
// Or
this.$el.append(subview.render().el) // append
~~~

I prefer the second way: initialzing and injecting. However, a small trouble
is that sometimes an empty '<div></div>' has to be wrapped.

# Further reading

<!-- Mention two nice posts.

.html calls .empty first, unbinds subviews' events
append: shouldn't control layout in JS, should be controlled in templates.
setElement, calls delegateEvents to rebind events

Only render subviews,  -->

There are two nice posts about how to render subviews efficiently and correctly in Backbone.js.

* [Rendering Views in Backbone.js Isn’t Always Simple](https://ianstormtaylor.com/rendering-views-in-backbonejs-isnt-always-simple)
* [Assigning Backbone Subviews Made Even Cleaner](https://ianstormtaylor.com/assigning-backbone-subviews-made-even-cleaner)
