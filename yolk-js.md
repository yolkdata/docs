# Yolk's Javascript Tracker SDK

Collect and stream data from your Javascript data sources. This SDK is an implementation of Segment's open source analytics.js.

## 🚀 Setup

### 1. Add the Javascript snippet to your data source, updating the options (org, apiKey, and env).

```
<script>!function(e){if(n)n.options=e||{},n.start instanceof Function?(console.warn("yolk already loaded - starting anyway"),n.start()):console.error("Yolk disabled: `yolk` Object exists but is the wrong type");else{var n={functionQueue:[],options:e||{}};window.yolk=n;var t=["event","context","identify","alias","view","track"];function o(e,t){if(null==e)console.error("missing function name in snippet enqueue() - queued function ignored");else{var o={fn:e,args:t};n.functionQueue.push(o)}}for(let e=0;e<t.length;e++){var i=t[e];n[t[e]]=function(){o(i,Array.prototype.slice.call(arguments))}}var r=document.createElement("script"),a=https://cdn.yolkdata.dev/public/yolk/yolk-tracker/yolk-tracker-0.0.0-3cb0721.js;r.type="text/javascript",r.async=!0,r.src=a,r.addEventListener("load",function(){n.start()}),document.getElementsByTagName("head")[0].appendChild(r)}}(
  {
    org: "<YOUR_ORG_NAME>",
    apiKey: "<YOUR_YOLK_API_KEY>",
    env: "development"
  }
);</script>
```

### 2. Instrument Tracking

#### Track events

```
yolk.event("event name", {
  "any":"properties",
  "you_would":"like to track"
});
```

#### Identify users

```
yolk.identify({
  "vid":"3dd34",
  "orgid":"54t66",
});
```

## Other Info

#### Javascript Compatibility Note

The fragment cannot be processed by Babel since its directly embedded in HTML files. To support IE11 we can only use language features from ECMAScript 5 and below.

#### How the SDK loads

The Javascript snippet is a self-executing function to load Yolk asynchronously. The snippet declares the public interface of the `yolk` Object and will queue any calls to the defined methods while the javascript is loading to avoid having to wait for `yolk.js` during page load while still allowing calls to `yolk.*()` methods to succeed.

The snippet asynchronously loads the `yolk.js` library by inserting `<script>` tags into the DOM. For production use we recommend you specify one of:
  * `version` - load a specific version of `yolk.js` from the Yolk CDN
  * `yolkJsUrl` - load `yolk.js` directly from this URL, overriding any other setting. For repeatable builds, be sure to embed a version number inside the filename

If both of these options are missing, the snippet will attempt to load `/yolk.js` which is useful to support local testing of the `yolk.js` library itself.