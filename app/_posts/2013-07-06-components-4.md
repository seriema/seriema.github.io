---
layout: post
title: Components part 4 - Loading components through AJAX
---

Sometimes we don't have all the HTML we need up front. Parts of the page is loaded through AJAX and that HTML might contain components.

Our current approach is to load all components once, at document ready, but the any new HTML is ignored.

A simple solution is to attach your own "document ready" startup code using jQuerys eventsystem

{% highlight javascript %}
$('.comp-form-ajax').on('init.comp', function() {
var $root = $(this);
    // etc as usual
});
{% endhighlight %}

This creates a little problem. Nothing will happen automatically on page load! You could attach to the doc.ready event, but its implementation is a bit special so it doesn't support our next step. Which is running the registration code on newly recieved components.

{% highlight javascript %}
$.getHtml(url, data, $placeholder);
$placeholder.trigger('comp-init');
{% endhighlight %}


This can be easily wrapped into a helper method.
