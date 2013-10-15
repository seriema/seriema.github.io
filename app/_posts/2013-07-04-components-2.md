---
layout: post
title: Components part 2 - Basic JavaScript code structure for simple components
---

{% post_url 2013-07-03-components-1 %}

Last time we talked about the [central piece of JS]() for organizing JavaScript in a components based system. This time we'll take a look at how to structure the code for a typical component.

Typically a component needs to react to user input. So we have three parts: the DOM elements to work with, the event binding, and the functions containing the logic. During one year, 70+ components, and 2000 LOC, a pattern emerged and has been consistent.

{% highlight javascript %}
// 1. loop through component elements
// 2. grab local component elements
// 3. attach event handlers
// 4. do the logic
{% endhighlight %}

Let's say it's an AJAX form with a submit button.

{% highlight javascript %}
// 1. loop through component elements
$('.comp-form-ajax').each(function () {
    // 2. grab local component elements
    var $root = $(this);
    var $submitButton = $root.find('js-form-submit');

    // 3. attach event handlers
    $submitButton.click(submitForm);

    // 4. do the logic
    function submitForm() {
        ...
    }
});
{% endhighlight %}

There's a few things to note. First is the scoped search of the submit button. I prefer `$root.find('foo')` over `$('foo', $root)` because it flows better with more complex selectors such as `.not()` and `.has()`, but there are other ways. It's important that you only grab elements inside the current component.

The second is a naming convention for classes that are used by JS. They're prefixed with `js-` like: `js-form-submit`. This is to make it clear that a piece of HTML is used by JS so caution should be taken before changing or removing it. Which might otherwise be common for a graphical designer to do when finding classes in HTML that don't correspond to any styling in the CSS.

The third part is the fact that it's `js-form-submit` and not just `js-submit`. We should be safe from random `submit` classes because we scope our selectors with the component class-name, but with the content of the form being free and when we start reusing components inside components the risk increases. We want to minimize the risk of naming collisions so it's good to mix in a word or two from the component name, in this case `form`.

The fourth and final note is that we use function expressions (`function foo`) rather than statements (`var foo = function`). This is to enhance readability. It keeps all elements at the top, all event bindings after it, and that's usually what fits on one screen without scrolling. Functions are the bulk of the JS so I like to keep it a bit out of sight so I can quickly understand what is expected, rather than how it's done. The function expressions are hoisted to the top at runtime so binding to functions not yet written is fine.
