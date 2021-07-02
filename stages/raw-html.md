# How to use Hubzity tracking with stages

Hubzity provides tracking code that can report to the campaign post-click events. These events can represent different stages of a sales funnel, different event on a page and so on.

## Storing Click Session

The first step when tracking is to store the click id in a local cookie so we can use it when we need to track post click events.

Just paste the following code in your html:

```html
<script type="text/javascript">
/* Hubzity tracking code start */
(function() {
    "use strict";

    var head = document.getElementsByTagName("head")[0];
    var trackingScript = document.createElement("script");

    trackingScript.setAttribute("async", true);
    trackingScript.setAttribute("src", "//static.rtbaxs.io/tracking.min.js?ts=" + new Date().getTime());

    trackingScript.addEventListener("load", function () {
        window.trackingStoreSession();
        window.trackingConversion("0");
    });

    trackingScript.addEventListener("error", function () {
        console.log('Failed to load Hubzity tracking SDK');
    });

    head.appendChild(trackingScript);
})();
/* Hubzity tracking code end */
</script>
```

## Conversion Stages

The post-click event tracker can track up to 10 events (0-9) reported by you in your html page as you see fit. As noted, these can represent different stages in your conversion funnel or different events on the landing page.

Conversion tracking usually happens when user performs some actions on your page.

Consider the following scenario. You have a product subscription process and within this process user needs to fill a registration form. Let's imagine your conversion process consists of 3 stages:

1. Stage 0 - user opens the registration page.
2. Stage 1 - user fills the registration form.
2. Stage 2 - user clicks 'Subscribe' button.

To enable conversion tracking attach the following code to the event handlers that on your page that reflect user actions (your webmaster or site developer should know how handle it). 

### Example 1. Conversion stage is bound to a non-link (`<a href="/">`) element click

HTML:
```html
<button class="btn-subscribe" onclick="subscribe();">Subscribe now</button>
```

JS
```js
$('.btn-subscribe').on('click', function() {
    /* Hubzity conversion stage tracking start */
    var CONVERSION_STAGE = "2";
    window.trackingConversion(CONVERSION_STAGE);
    /* Hubzity conversion stage tracking end */
});
```

### Example 2. Conversion stage is bound to a link (`<a href="/">`) element click

HTML:
```html
<a class="link-subscribe" href="/subscribe">Subscribe now</a>
```

JS:
```js
$('.link-subscribe').on('click', function(e) {
    /* Hubzity conversion stage tracking start */
    e.preventDefault();
    var CONVERSION_STAGE = "2";
    window.trackingConversion(CONVERSION_STAGE, false, function() {
        document.location = e.target.href;
    });
    /* Hubzity conversion stage tracking end */
});
```

**IMPORTANT:** please use this example for link elements (`<a>`). By default on click event link element starts navigation to a different page. In order to let tracking code run correctly we add a little delay to give the code a chance finish its job before browser opens a new page.

To track other stages you just need to change **CONVERSION_STAGE** variable value in quotes to a different stage e.g. **"1"**.

Please note: the code mentioned in **Storing Click Session** section **MUST BE** also added to the page where you want to track conversions.