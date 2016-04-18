---
layout: default
title: How it Works
permalink: /how-it-works
---

# How it Works

Typical analytics tools such as Segment and Mixpanel don't do a great job of tracking all source attribution and behaviour data. This makes it hard/impossible to answer many key questions about campaign success and user behaviour.

SiteHound is designed around the observation that most key events useful for answering business-centric questions are linked to users viewing particular types of pages. For example:

- An e-commerce site should likely track when users view a:
  - landing page
  - category page
  - product page
  - cart page
  - checkout page
  - order complete page
- For a SaaS business relevant page types to track typically include:
  - landing pages 
  - blog articles
  - content marketing pages
  - trial/demo signup
  - product dashboard
  - subscription upgrade success

### Tracking key events

SiteHound therefore makes it super-easy to track key events via a single configuration object that maps event names to URL patterns and/or `<body>` CSS classes, typically covering many or most required events in one easily comprehendible and maintainable location.

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


### Tracking custom events

Tracking custom events and properties is straightforward via `identify()` and `track()`/`trackLink()`/`trackForm()` calls using a interface familiar to anyone who’s used Segment.com's analytics.js.

Additionally, SiteHound augments this basic event tracking with a plethora of useful additional data and events about your users and behaviour that will soon become invaluable to how you answer analytics questions with your platform of choice.

## Features

### Tracking

- Landing pages and initial referrer
- First, mid and last-touch source attribution
- User login/logout and identification with automatic aliasing
- Session tracking with duration and session depth information for every event
- Augments your existing tracking and any events you track manually

### Implementation

- Pluggable javascript interface supporting Segment’s analytics.js and Mixpanel.js, with easy development of support for other platforms possible via an adaptor interface.

---

**Next:** To get SiteHound up and running see **[Getting Started]({{ site.url }}/getting-started)**
