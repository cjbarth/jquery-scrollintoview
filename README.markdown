jQuery .scrollintoview() plugin <sup>(with :scrollable selector filter)</sup>
==
This plugin makes it easy to scroll any element on your page into view. It scrolls in a user friendly way using animation (speed can be configured) so users never loose track where they moved within the current page. Default browser functionality simply jumps to some element which may confuse users greatly. This kind of behaviour is considered bad user experience, since pages look different above and below the fold. This means that users may as well think they were redirected to a different site (since it looks different) or at least page within the same site.

How to use this plugin
--
Usage is very simple and straightforward:

```javascript
$("some_selector").scrollintoview();
```

Also supported is a function that will scroll to the top of the scrollable element: `scrolltotop`:

```javascript
$("some_selector").scrolltotop();
```

When both functions, aliases for camelCase are also supported: `scrollIntoView` and `scrollToTop`.

And that's it really. This is of course if we use defaults. It supports the same arguments as `jQuery.animate()`, but, if a single `options` object is passed in, other options, specific to this library can also be specified. In all cases, the jQuery defaults are used if they aren't specified.

* **direction** (default: "both"; ignored for `scrolltotop`) - scrolling can be performed in three different directions:
    * **x** or **horizontal**
    * **y** or **vertical**
    * **both** - scrolling will perform in both directions; since scrolling is performed only when element is actually out of view this simply means that scrolling may only perform in one direction even though you set it to scroll in both directions; *both* is therefore the most reliable scrolling option that will make sure your element will be visible
* **topOffset** (default: 0, ignored for `scrollintoview`) - when using the `scrolltotop` function, this will be added to the top to move the content down (or up) relative to the top of the scrollable element; this is ignored with `scrollintoview`
* **complete** function - this is the complete handler function that will get called when scrolling completes; it runs in context of scrollable element; this function will be called regardless whether scrolling will perform or not (when element already in view); *but* it won't get called when there's no scrollable element (context can't be determined)

```javascript
$("some_selector").scrollintoview({
    duration: 2500,
    direction: "vertical",
    complete: function() {
        // highlight the element so user's focus gets where it needs to be
    }
});
```

```javascript
$("some_selector").scrolltotop({
    duration: 2500,
    topOffset: 10,
    complete: function() {
        // highlight the element so user's focus gets where it needs to be
    }
});
```

How does this plugin solve the user experience issue
--
This plugin scrolls a particular element into view similar to browser built-in functionality (DOM's `scrollIntoView()` function), but works differently (and arguably more user friendly):

* it only scrolls to element when element is actually out of view; if element is in view (anywhere in visible document area), no scrolling will be performed, except for `scrolltotop` which will try scrolling no matter what to make sure the element is at the top;
* it scrolls using animation effects like any jQuery animation which can be specified just like the `jQuery.animate()` function; when scrolling is performed users know exactly they're not redirected anywhere, but actually see that they're simply moved somewhere else within the same page (as well as in which direction they moved);
* there's always the smallest amount of scrolling being applied; when element is above the visible document area it will be scrolled to the top of visible area; when element is below the visible are it will be scrolled to the bottom of visible area; this is the most consistent way of scrolling - when scrolling would always be to top it sometimes couldn't scroll an element to top when it was close to the bottom of scrollable container (thus scrolling would be unpredictable); for that reason, a separate `scolltotop` function is available for those that want this functionality;
* when element's size exceeds the size of visible document area its top-left corner is the one that will be scrolled to;

What about :scrollable selector filter
--
The good thing about this plugin is that you don't have to provide two elements: scrollable ancestor and element that will be scrolled into view. This plugin rather searches for the closest scrollable ancestor. By *scrollable* it means that its content exceeds its view area and that is also displays scrollbars (using `overflow:hidden` won't select element as scrollable). This checking is therefore implemented as a `:scrollable` selector filter that anyone can use.

```javascript
var scrollableDivs = $("div:scrollable");
```

This code would select all `DIV` elements that have scroll bars (at least one) and their content exceeds their visible area (at least in direction that can be scrolled using scroll bar).

[See blog post for details](http://erraticdev.blogspot.com/2011/02/jquery-scroll-into-view-plugin-with.html).