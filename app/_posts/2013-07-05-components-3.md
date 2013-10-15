---
layout: post
title: Components part 3 - Having instance-unique data in the components
---

[Last time]() we used an AJAX form as an example. But what if we want to send the data to different URL's bases on where the component is used? We can't keep the URL stored inside the JS file that's for certain, so how then?

Easy. We just pass in the data through HTML5 data-attributes.

{% highlight html %}
<form ... data-url="/userRegistration"> ... </form>
{% endhighlight %}

Grabbing it is as easy as grabbing DOM elements.

{% highlight javascript %}
$('.comp-form-ajax').each(function() {
    var $root = $(this);
    var url = $root.data('url');
    ...
});
{% endhighlight %}

Now we have the data separate from the logic and the components are much more reusable.

(Honestly, in this exact case I chose to use `action` instead of `data-url` in our project. So it's more semantically correct and works as a backup in case the JS can't load correctly.)
