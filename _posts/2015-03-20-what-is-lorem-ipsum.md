---
layout: post
title: "Best Practices for Advanced Lightning Apps"
description: "a webinar about complicated Lightning Experience Apps"
keywords: "salesforce"
---

> Jon Belo leads us through a small section of the DreamHouse app, a component design for a list view card, and a use for the Utility Bar.

The webinar [was recorded](https://www.youtube.com/watch?v=9G3TyoB0_vQ&feature=youtu.be) and I watched that recording over lunch.

Some things that stood out:
* Lightning Data Service won't count toward SOQL Limits!
* You can use the utility bar to cache data.
* setBackground() on actions is worth evaluating. Start with it on.
* Don't be a chatty component to save on SOQL calls.
* Using service components or provider components is great!
* Build an extensible component with a helper action for your callServer action. Uh...duh...why didn't I do that before?

I liked how they split the components into categories:
* service components
* helper components
* building block (UI) components
* experience (app builder) components.

Jon also split up what should be configured in LAB attributes, and when to add a cMDT record to your config. The App Builder attributes for the list view had things like "how many records per page", "should we paginate", "title", and "theme". It also had the ID of a metadata record with the query, the actions, etc..

