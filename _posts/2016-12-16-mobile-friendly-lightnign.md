---
layout: post
title: "Mobile Friendly Lighting"
description: "first steps to making my component lightning aware"
keywords: "salesforce"
---

> Converting a LEX component to SF1!

The app I'm working on right now has a custom data entry component that's exposed inside a `ui:tab` on a Record Home flexipage. This works great for their desktop work, but this component doesn't appear in the Salesforce1 mobile app, because custom record homes don't work there.

After communicating with a Salesforce1 PM, I was given two options:

* wrap the component in a Lightning Out app, put that in a Visualforce Page, and expose that in the app as a Mobile Card (appears on the "Related" Tab)
* make a version of the component to render as a (quick) Action. 

Unfortunately SF1 and LEX share the same action configs. So, I put the quick action as the last one, this makes it somewhat easy to get to on SF1, and somewhat buried in LEX. Seperate action sets would be great here.

Since it's inevitable that the component will get used as an action on the desktop by SOMEONE -- I'm taking this opportunity to redesign the interaction model a little bit for the interface to be more consistent with actions. Initially I started by trying to make the components the same and discover their environments (Desktop vs. Mobile). At this stage, UI components are detecting their size (i.e. radioPicklist and the note entry box are checking the `$Browser` global value provider).

However, I am going to move the bulk of logic out of the global action that's currently in App Builder into a body component. Then, the App Builder component will consist of that body component and the save button. The Action component will consist of a header, the body component, and the action footer. The UI components will continue to detect the form factor where appropriate, but the experience components will be able to tell the building block component additional details too. This is because there are a bunch of UI components that I just want to be smart enough to size appropriately, but there is also one UI component that I want to move from a table based view to a card based view on mobile.

Most of this is just simple refactoring and wrapping, and will all be able to be done in markup!
