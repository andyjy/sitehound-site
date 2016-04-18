---
layout: default
title: API Reference
permalink: /api-reference
---

# API Reference

Refer to the [Getting Started]({{ site.url }}/getting-started) guide for the basics on how to implement SiteHound on your site.

## What to track

{% highlight javascript %}
// names and paths of key pages we want to track
// paths can be simple string matches, arrays, or regular expressions        
sitehound.trackPages = {};

// track "Viewed Unidentified Page" event for all pages not covered above
sitehound.trackAllPages = false;

// use .page to manually set the type of the current page based on your own logic
// rather than via .trackPages
sitehound.page = null;
{% endhighlight %}


## Traits (properties)

Use these properties to configure Segment’s traits (aka properties) to pass through with tracking for pages, the user, and custom events.

{% highlight javascript %}
// traits to pass with all future events, and also set on the current user
sitehound.globalTraits = {};

// traits that we only want to pass with events on the current pageview
sitehound.thisPageTraits = {};

// traits to pass only with any calls to track the current page
sitehound.pageTraits = {};
{% endhighlight %}

Set `sitehound.globalTraits` to an object of key-value pairs to set global traits on all subsequent events in this and future sessions (as well as set as properties on the current user if any).

`thisPageTraits` is similar to `globalTraits`, but will only be set for events during the current pageview. (These will not be set on the user, or passed with events on subsequent pages.)

Set `pageTraits` to an object of key-value pairs to track on any _Pageview_ event we may trigger on the current page (as configured by `sitehound.trackPages` above). (These will not be set on the user, or passed with any other events on this or future pages.)



## Indentifying the current user

{% highlight javascript %}
// pass in a user ID where you have one, and the library will take care of the rest
// (calls to alias() / identify() will be made as appropriate)
sitehound.userId = undefined;

// set if you want the absence of a userId on this page to trigger a logout event
sitehound.detectLogout = undefined;
{% endhighlight %}

Tracking user logins/logouts with standard javascript libraries such as Segment and Mixpanel is one of the most common areas for mistakes, driven by the intricacies required in the calls to `identify()` and `alias()` under various circumstances. SiteHound removes all the complexity, enabling simple user tracking without the headaches.


### userId

First, track user logins by setting `sitehound.userId` on the page immediately after a user signs up or logs in. SiteHound will detect the user was previously anonymous, and handle tracking the Login event and associating all events for the current and subsequent pages with the ID provided.

If this is the first time the user has logged in, the library automatically handles aliasing the user with their previous anonymous ID so you have complete history of the trail from when the user first landed on your site through to their logged-in identity, along with all their source attribution data.

It’s recommended you set the `userId` property on every page while the user is logged in (this ensures you don’t miss any one-off capture of a login). However this is not necessary, and the user will remain logged in until you specifically pass a `userId` of `null` or use `detectLogout`, below. This conversion comes in particularly handy when your site contains a mixture of static and dynamic pages.


### detectLogout

Set `detectLogout` to true if you wish to change from the default behaviour so that the user is logged out on the current page if you haven’t also passed in a value for `userId`.


_Tip: if you know your pages will display either a “sign in” or “sign out” link depending on the user’s login state, you can track logouts easily without any backend changes to your site using something like the following:_  
`// log out the user if the current page doesn't contain our sign out link
sitehound.detectLogout = !document.getElementById('sign-out');`

In summary:

- pass a `userId` to log the user in
- The user will be logged out when you set `userId` to null, or leave `userId` unset while setting `detectLogout` to true.


### userTraits

{% highlight javascript %}
// traits to set on the user (which will also be passed with all future events)
// similar to globalTraits, but these will be set with the prefix "User "
// so they can be easily identified
sitehound.userTraits = {};
{% endhighlight %}

Any properties you set on `sitehound.userTraits` will be set as global traits on the user and all their subsequent events.

`userTraits` is essentially the same as `globalTraits`, but to ensure user properties are easily identifiable as such within your analytics tool, user traits are prefixed with “User“ to distinguish them from other traits that may be set on specific events. This means e.g. you won’t confuse “User type” with any other “Type” properties you may use to annotate particular events.


## Adding custom logic, including properties & events based on the page type

{% highlight javascript %}
.detectPage()
.ready(function)
{% endhighlight %}

You can call `detectPage()` to return the type of the current page as determined by applying the rules in your configuration. You can make use of the current page type in your code to help determine which additional properties you may wish to track on different pages - for example product IDs or category names.

`detectPage()` is only available once the SiteHound library has loaded and initialized, so you should wrap your code in a function and pass the function as an argument to `ready()` to have it executed once detection of the current page is available.

Your custom code can conditionally set key/value pairs under `pageTraits` to annotate just the event to track detection of the current page, or `thisPageTraits` to annotate all events tracked on the current page view.

Recommended practice is to use your backend code to output HTML that initializes a global javascript object with any relevant context, then read that back and sent relevant properties to SiteHound as above.


## Tracking single-page web apps

{% highlight javascript %}
detectURLChange: true
detectHashChange: false 
{% endhighlight %}

SiteHound is designed around the concept of mapping URLs to key events, and by default will trigger new page detection if your app updates the window location. By default hash changes are excluded, since the hash is more commonly used for it’s traditional purpose that doesn’t apply a new 


## Excluding test data

You probably don’t need to update these, but if they’re here in case you do.

{% highlight javascript %}
// domains to not track
sitehound.domainsIgnore = ['localhost'];

// track when the host is an IP address? (e.g. dev machines..)
sitehound.domainsIgnoreIPAddress = true;

// subdomains to not track
sitehound.domainsIgnoreSubdomains = ['staging', 'test'];

// disable tracking for the current browser
.doNotTrack()

{% endhighlight %}

By default, SiteHound will not track any data if the current page is served from `localhost`, an IP address, or `staging` or `test` subdomains. You can use these properties to override or customise these behaviours if desired.

Calling `sitehound.doNotTrack()` will prevent any data or events being logged by SiteHound for this and any future sessions in the current browser (unless the user clears cookies).


## Session timeout

{% highlight javascript %}
// customize the timeout period before we consider the user has started a new session
// (even if we still have the session cookie because the browser has remained open)
sitehound.sessionTimeout = 30; // minutes
{% endhighlight %}

By default, SiteHound reboots the session-tracking properties (`Session count`, `Session started`, `Minutes since session started`, `Pageviews this session`) if the user doesn’t view any pages for 30 minutes. You can set the `sessionTimeout` parameter to an integer number of minutes to customise this duration.


## Debugging / troubleshooting

{% highlight javascript %}
// enable logging informational messages to the console
sitehound.logToConsole = false;
{% endhighlight %}

Set `sitehound.logToConsole = true` to enable logging of informational output to the browser console that you can use for debugging/troubleshooting.

### Untracked landing pages

{% highlight javascript %}
// Pass the URLs of any landing pages without tracking code here in order to 
// record them as the original landing page when the user clicks through
sitehound.trackReferrerLandingPages = [];
{% endhighlight %}

If you have pages on your site that don’t contain the SiteHound tracking code, and your users may land on them before any tracked pages, SiteHound won’t be able to detect the true landing pages. If this is the case, you can pass an array of relative URLs to `trackReferrerLandingPages` and SiteHound will backfill a best attempt at the actual landing page events and properties using the referrer for any users who subsequently click directly through to a tracked page. For example:

{% highlight javascript %}
sitehound.trackReferrerLandingPages = ['/terms.html', '/privacy.html', '/untracked_landing_page.html'];
{% endhighlight %}

By default the library will detect if this happens, track a warning
"Tracking Debug" event with details, and not tracking a "Viewed Landing Page" event
for that session.

## Adaptors: sending your data where you want

SiteHound sends all events and data through to your analytics platform of choice via a simple Javascript adaptor interface. By default it looks for and attaches to Segment’s analytics.js, but you can specify to send directly to Mixpanel by specifying the Mixpanel adaptor in the load() call on each page load as follows:

{% highlight javascript %}
// your config here
// ..
// load SiteHound and select the Mixpanel adaptor
sitehound.load('mixpanel');
{% endhighlight %}

If you wish to send your data directly to other analytics services without using Segment, creating a new adaptor for your service of choice should be straightforward. Create your own Javascript class following the naming convention `SiteHound_Adaptor_Mylibrary` and swap in the name into the call to `sitehound.load('mylibrary')` - the code for the Segment analytics.js adaptor `SiteHound_Adaptor_Segment` demonstrates the necessary interface you need to implement.

---

**Next:** Refer to the **[User Guide]({{ site.url }}/user-guide)** for how to work with the data SiteHound collects.
