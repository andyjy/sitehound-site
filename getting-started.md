---
layout: default
title: Getting Started
permalink: /getting-started
---

# Getting Started

Follow the steps below to install and configure the SiteHound javascript on your website. Yay!

1. Drop the SiteHound tag into your website templates - or via a tag manager.
2. Use a few lines of Javascript to specify the key events you want to track, based on your site’s URL structure, `<body>` CSS classes, or your own custom script.
3. Analyse your data - including all SiteHound events and properties - via Mixpanel or any tool you have connected via Segment.


## If you already have tracking set up

SiteHound is compatible with your existing tracking - installing the library should have no adverse effects, and will automatically enhance your existing events with additional properties in addition to the events it tracks automatically.

## Installing SiteHound

To install SiteHound, you just need to add some Javascript to the `<head>` of your website.

If you use a tag manager, you can paste the scripts below into a custom HTML tag.

You can use the same scripts (snippets and configuration) on all pages of your site.

### 1 - Add the Segment.com snippet

SiteHound relies on the Segment.com snippet to send data to your analytics tooling, so ensure that's included first:

- **Copy/paste the snippet** from the Segment.com web interface setup page.
  - _The snippet looks like the example below._
  - _Ensure the API key for the correct project is filled out in the call to `analytics.load()`._
- **Remove the call to `analytics.page()`** that’s included within the Segment snippet by default, as well as any other calls to `analytics.page()`.
  - _SiteHound will take care of recording page views._

{% highlight html %}
<script type="text/javascript">
!function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","page","once","off","on"];analytics.factory=function(t){return function(){var e=Array.prototype.slice.call(arguments);e.unshift(t);analytics.push(e);return analytics}};for(var t=0;t<analytics.methods.length;t++){var e=analytics.methods[t];analytics[e]=analytics.factory(e)}analytics.load=function(t){var e=document.createElement("script");e.type="text/javascript";e.async=!0;e.src=("https:"===document.location.protocol?"https://":"http://")+"cdn.segment.com/analytics.js/v1/"+t+"/analytics.min.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(e,n)};analytics.SNIPPET_VERSION="3.1.0"; 
analytics.load("<YOUR SEGMENT KEY HERE>"); 
}}(); 
</script>
{% endhighlight %}

### 2 - Add the SiteHound snippet

Copy and paste the below into your site after the Segment.com snippet. Simples. 👌

{% highlight html %}
<script type="text/javascript">
!function(){var t=window.sitehound=window.sitehound||{},n=("https:"===document.location.protocol?"https://":"http://")+"andyyoung.github.io/sitehound/sitehound-min.js";if(!t.sniff){if(t.invoked){var e="SiteHound snippet included twice";return window.console&&console.error&&console.error(e),void t.trackDebugWarn(e)}t.invoked=!0,t.queue=[];for(var r=["doNotTrack","identify","identifyOnce","ready","track","trackAndCount","trackLink","trackForm","trackOnce","trackDebugInfo","trackDebugWarn","trackError"],o=function(n){return function(){var e=Array.prototype.slice.call(arguments);return e.unshift(n),t.queue.push(e),t}},i=0;i<r.length;i++){var a=r[i];t[a]=o(a)}t.sniff=t.done=function(){t.isDone=!0},t.SNIPPET_VERSION="1.3",t.load=function(e){t.adaptor=e;var r=document.createElement("script");r.type="text/javascript",r.async=!0,r.src=n+"?snippet_ver="+t.SNIPPET_VERSION;var o=document.getElementsByTagName("script")[0];o.parentNode.insertBefore(r,o)}}}();
sitehound.load("segment");
</script>
{% endhighlight %}

### 3. Add config for your site & trigger the tracking when done

- You can use the same config Javascript on all pages - just paste into your HTML template after the snippets above.
- See the [API reference]({{ site.url }}/api-reference) for details of all config options.

{% highlight html %}
<script type="text/javascript">
// add rules for key pages we wish to track
sitehound.trackPages = {
  'Home': '/',
  // use arrays match any of multiple criteria:
  'Page type': [
    '/string/matches/full/url/path',
    /^\/regex\/matches\/multiple\/url\/paths\//,
    '.css_class_on_html_body_element'
  ]
  // NB. use URL matching where possible, since this can also be applied to referrers
};

// track "Viewed Unidentified Page" event for all pages not covered above
sitehound.trackAllPages = true;

// enable logging informational messages to the console
sitehound.logToConsole = true;

// after all config, trigger the tracking for this page
sitehound.sniff();
</script>
{% endhighlight %}

### 4 - Use methods to track your custom events

- This passes events through to Segment, ensuring they have all the appropriate traits added by the library.
- `identifyOnce`, `trackOnce`  and `trackAndCount`  provide some useful additional functionality.

{% highlight javascript %}
// track an event, a la analytics.track()
sitehound.track(name, [traits]);

// as per track(), but count the number of occurences in a property
sitehound.trackAndCount(name, [traits]);

// as per track(), but only track this event once per session
sitehound.trackOnce(name, [traits]);

// as per analytics.trackLink(), wrapped to pass relevant traits for this page
sitehound.trackLink(link_elements, event_name, [traits]);

// as per analytics.trackForm(), wrapped to pass relevant traits for this page
sitehound.trackForm(form_elements, event_name, [traits]);
{% endhighlight %}

### Meta-tracking: tracking info, warnings and errors about our tracking

If useful, you can make use of the following methods to track things that go wrong or unexpected events for further investigation:

{% highlight javascript %}
sitehound.trackDebugInfo(message);
sitehound.trackDebugWarn(message);
sitehound.trackError(Error error);
{% endhighlight %}

---

**Next:** Refer to the **[API Reference]({{ site.url }}/api-reference)** for full documentation, or the **[User Guide]({{ site.url }}/user-guide)** for how to work with the data SiteHound collects.
