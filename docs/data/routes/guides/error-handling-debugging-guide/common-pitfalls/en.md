---
name: common-pitfalls
routeTemplate: ./data/component-templates/guide.yml
title: Common Pitfalls
---

## Node Proxy & calling external APIs 

Avoid proxying external API calls through Node.

If you are using the Headless Node Proxy topology, and your front-end code needs to make a call for something other than rendering JSS components (ex. Sitecore Services Client or a custom API), add this path to `config.pathRewriteExcludeRoutes` value in your [headless proxy configuration](https://github.com/Sitecore/jss/blob/dev/samples/node-headless-ssr-proxy/config.js).

These ‘ignored’ paths are reverse proxied to Sitecore instead of being treated as pages to attempt to render with Layout Service.

---

## Tracking identity during AJAX calls
When using fetch() to ajax data from the server, cookies are not sent by default, and cookies need to be sent to tell the server the identity of the current user. Use the `init` parameter to make fetch send cookies.

Reference: developer.mozilla.org - fetch Parameters

```javascript
fetch(url, {
  credentials: 'same-origin'
});
```

---

## Gotchas of "Hybrid Headless"
Most data necessary to render a page in JSS can be obtained by making calls to the Layout Service API. However, some data doesn't flow over the API because it's dynamic and processed on-the-fly.
- media URLs for media content populated by Content Authors
- visitor identification, which enables tracking and personalization
- dictionary values for translations in multi-lingual apps

> To see the complete list of routes that flow directly to the Sitecore host, check `config.pathRewriteExcludeRoutes` value in your [headless proxy configuration](https://github.com/Sitecore/jss/blob/dev/samples/node-headless-ssr-proxy/config.js).

### Sample scenario
Possible scenarios where these differences from a completely headless system would need to be accounted for include:
- wanting to host the Sitecore Content Delivery server behind an API Management Gateway
- wanting to host the front-end portion of a JSS application in a CDN

---

## Placeholder naming conventions
Multiple placeholders with same name within same JSS component are not allowed.