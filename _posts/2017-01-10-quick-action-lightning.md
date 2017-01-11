---
layout: post
title: "One-Click Actions in Lightning"
description: ""
keywords: "salesforce"
---

Back in the olden days, to take a "one-click action", we would often write a Visualforce Page with an `action` right on the page element. That meant when a custom button for the page is clicked, it'd run the controller action and we could immediately redirect (either back to the detail page, or somewhere else).

In Lightning though, we're looking for a different interaction. The NPSP has a "refresh naming" button on opportunity that works the old fashioned way. The [OPP_OpportunityNamingBTN](https://github.com/SalesforceFoundation/Cumulus/blob/5dfe2f1d8f4c35665be9e0dcf9f0be0a18a8ebab/src/pages/OPP_OpportunityNamingBTN.page) page runs the page action and then reloads the record home. This means one-click, but a lot of page reloads and waiting!

<blockquote class="imgur-embed-pub" lang="en" data-id="WAAdnc2"><a href="//imgur.com/WAAdnc2"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

So, what's the interaction to look for? I'm drawing inspiration from the Sales Path component. With Sales Path, the user can click the "Mark Stage as Complete" button, and if there are no validation errors or required fields, the record will update and throw a toast.

<blockquote class="imgur-embed-pub" lang="en" data-id="a/ySTLl"><a href="//imgur.com/ySTLl"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

So, what is a Lightning Developer to do? We implement the force:lightningQuickActionWithoutHeader interface, and build a [Lightning Component Quick Action](https://andyinthecloud.com/2016/08/21/winter17-using-a-lightning-component-from-an-action/).

Now it's not the most beautiful trick, but we can fire the force:closeQuickAction event as soon as possible on initialize, and then do our stuff:

```
({
    doInit: function(component,event,helper) {
        window.setTimeout(
            $A.getCallback(function() {
                if (component.isValid()) {
                    helper.closeAction();
                }
            }), 0
        );
        helper.act('c.generateJELs',{recordId: component.get("v.recordId")}, function(resp) {
            helper.toast(component,"Success","Missing records were generated.");
            helper.refreshView(component);   
            
        }, function(resp) {
            helper.toast(component,"Failure","Missing records logs were not generated.");
            helper.refreshView(component);   
            
        });
    }
})
```

<blockquote class="imgur-embed-pub" lang="en" data-id="a/uyntz"><a href="//imgur.com/uyntz"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>

And like that, with one click, we have a nice success toast and no full page reloads! I've asked around the Salesforce dev community about if there's a better method than just closing the pane right away. It's a bit ugly, so I would like something better!
