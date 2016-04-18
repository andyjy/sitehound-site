---
layout: default
title: User Guide
permalink: /user-guide
---

# User Guide

Once you have SiteHound installed and sending data to your analytics platform of choice, you can take advantage of the events and properties tracked by SiteHound to answer many questions about your user behaviour and product performance.

In addition to events that you configure or track manually, SiteHound tracks the following events and properties for you:

Properties:

- Attribution - all for first touch and last touch.
  - UTM Source, Medium, Campaign, Term and Content
  - `Landing Page` and `Landing Page Type` 
  - `Initial Referrer`, `Initial Referrer Domain` 
  - `Page Type` and `Referrer Type` 
  - (Multi-channel attribution can be analysed by looking at the last-touch properties across multiple successive `Landing Page Viewed` events - huzzah!)
- Behaviour:
  - `First seen`, `Days since first seen` and `Session count` 
  - `Days since signup` 
  - `Session started` and `Minutes since session started` 
  - `Pageviews this session` 
- User:
  - `User email domain` 
  - Tidy handling of your own custom user properties
- Integration with other marketing and analytics tools:
  - Automatic setting of appropriate source/medium for clicks from Adwords and Yesware
  - `Fullstory URL` 

Events:

- `Landing Page Viewed` 
- `Login`/`Logout` 


## Global event properties

These properties are present on every event tracked by SiteHound.

### Attribution properties

The attribution properties cover the UTM properties (for inbound links you’ve tagged to determine where the user came from), Landing Page and Initial Referrer:

- `UTM Source`  - e.g. “Google”, “Facebook”
- `UTM Medium` - e.g. “email”, “cpc”, “cpm”
- `UTM Campaign` - e.g. “My Campaign”
- `UTM Term` - e.g. “”
- `UTM Content` - e.g. “”
- `Landing Page` 
- `Landing Page Type` 
- `Initial Referrer` 
- `Initial Referrer Domain` 

All the attribution properties are included twice, suffixed with `[first touch]` and `[last touch]` - corresponding to the user’s first ever and most recent visits. (For a first-time visitor, the first touch and last touch properties will therefore be the same.)

The UTM properties pick up the standard UTM values as you’d expect. (For a guide to tracking your visitors using UTM tags, see XXX.)

`Landing Page` is the URL of the page the visitor landed on when they started the session. This could be any URL on your site, not just pages you may classically think of as “landing pages” - the key point is that this is where the visitor arrived. 


  Tip: Analysing landing page URLs can often provide a good proxy for determining the source of different visitors in the absence of full UTM tagging.

`Landing Page Type` is the type of the page the user landed on, as determined by the rules you’ve used to configure SiteHound. You can use this to analyse in more general terms where your users are landing, or to compare the behaviour of users who land on the home page versus those who land directly on product or category pages, for example.

`Initial Referrer` is the referring URL that the visitor came from in order to reach the landing page. This will be set where present, but be aware the referrer is not available in many situations - in particular when the user comes from a page served over HTTPS but your site is not served securely.

`Initial Referrer Domain` is useful for grouping many different refereeing URLs from the same site, e.g. a directory, news site or search engine.

### Behaviour properties

These properties track information about the user’s behaviour in visiting and browsing the site:


- `First Seen` - the date/time of the visitor’s first session.
- `Days Since First Seen` - whole number of days elapsed since the visitor’s first session.
- `Session Started` - the date/time the current session started.
- `Minutes Since Session Start`- whole number of minutes elapsed during the current session
- `Session Count` - how many sessions the user has returned for in total (including the current session).
- `Pageviews This Session` - how many pages the user has viewed in this session (including the current page).

Example usage:

- Use the `Days Since First Seen` and/or `Session Count` properties on a key event - e.g. sign up or purchase action - to analyse how long it takes users to decide to engage.
- Use `First Seen` to cohort/group users based on when they first visited your site.
- Use `Minutes Since Session Start` and/or `Pageviews This Session` properties events to build funnels revealing how long users stay around, with the % drop-off between significant thresholds, or to determine whether taking key actions (e.g. add to cart) is correlated with spending more time on your site and better understand what drives conversion decisions.

### Other

These properties are also present on every event tracked by SiteHound: 


- `Page Type` - the type of the current page, as determined by the rules you used to configure SiteHound.
- `Referrer Type` - for pages where the user came from another page on your site, this will contain the type of the referring page as determined by the rules you used to configure SiteHound. (Note that this can only be populated using rules based on the page URL - rules using CSS selectors will not be picked up.)


##  User properties

- `User Name` 
- `User Email` 
- `User Email Domain` 

These properties are present on every event tracked by SiteHound once a user has logged in, and are registered as global user properties with the underlying tracking library (Segment, Mixpanel) so are also present on all events subsequently tracked directly via the library.

`User Email Domain` is automatically populated if you provide the user’s email, and can be used for segmenting/bucketing users by company (to the extent they use their work email addresses).


  Tip: If you don’t wish to track user email address (potentially as part of your policy for Personally Identifiable Information [PII]), you may wish to set `userTraits['Email Domain']` yourself manually using something like the following:
  `sitehound.userTraits['Email Domain'] = email.match(/[^@]*$/)[0];` 


## Events

SiteHound automatically tracks the following events:

- `Viewed Landing Page` 
- `Login` 
- `Logout` 
- `Tracking Debug` 
- `Tracking Error` 

Additionally, SiteHound tracks various `Viewed XXX Page` events according to the rules you have configured.


## Examples

The following examples illustrate how to use the data collected by SiteHound to answer various questions about you users:

    [ ] Funnels
    [ ] Conversion rate by channel
      [ ] two-step funnel, segemented by xxx
    [ ] Identify landing pages; use landing pages to determine source attribution information
    [ ] Compare first vs last touch attribution
      [ ] Last non-direct?
      [ ] Last paid?
    [ ] Multi-channel attribution using multiple landing page events + last-touch params
    [ ] Session engagement analysis - session duration; # pageviews
    [ ] Repeat visit analysis - # visits before conversion; Duration between first seen and conversion (Days since first seen)
