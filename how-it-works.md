---
layout: default
title: How it Works
permalink: /how-it-works
---

# How it Works

SiteHound does two things:

1. Make it really easy to **track key events** on your website based on the user visiting particular URLs or URL patterns.
2. Automatically **augment your tracking** with a bunch of useful properties for marketing and product engagement analysis.

## Tracking key events

SiteHound is designed around the observation that most key events useful for answering business-centric questions are linked to users viewing particular types of pages. For example:

- An e-commerce site should likely track when users view each of the following:
  - landing page
  - category page
  - product page
  - cart page
  - checkout page
  - order complete page
- For a SaaS business, relevant pages to track typically include:
  - landing pages 
  - blog articles
  - content marketing pages
  - trial/demo signup
  - product dashboard
  - subscription upgrade success

SiteHound makes it super-easy to track key events via a single configuration object that maps event names to URL patterns and/or `<body>` CSS classes, typically covering many or most required events in one easily comprehendible and maintainable location.

Here's an example:

{% highlight javascript %}
sitehound.track = {
  'Home': '/',
  'Blog': '/blog/*',
  'Category': ['/womens-clothes/', '/mens-clothes/', '/chidrens-clothes/'],
  'Product': '/product/*',
  'Cart': '/cart',
  'Order Complete': '/checkout/success/',
  'Checkout': '/checkout/*'
}
{% endhighlight %}

## Marketing and engagment data

Typical analytics tools such as Segment and Mixpanel don't do a great job of tracking all source attribution and behaviour data. This makes it hard/impossible to answer many key questions about campaign success and user behaviour.

SiteHound automatically augments your tracking with a bunch of useful properties on each user and event, including:

- First, mid and last-touch source attribution
- Landing page and initial referrer
- User login/logout and identification with automatic aliasing
- Session tracking with duration and session depth information for every event

## Tracking custom events

Tracking custom events and properties is straightforward via `identify()` and `track()`/`trackLink()`/`trackForm()` calls using a interface familiar to anyone who’s used Segment.com's [analytics.js](https://segment.com/docs/libraries/analytics.js).

## Implementation

SiteHound consists of a core Javascript library, plus a snippet used to load the core library. See [Getting Started]({{ site.url }}/getting-started) for how to implement SiteHound on your site.

Sitehound implements a pluggable javascript interface supporting Segment’s analytics.js and Mixpanel.js, with easy development of support for other platforms possible via creation of new adaptors.

---

**Next:** To get SiteHound up and running see **[Getting Started]({{ site.url }}/getting-started)**
