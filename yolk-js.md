# Yolk's Javascript Tracker

Collect and stream data from your Javascript data sources.

## 🚀 Setup

### 1. Add the Javascript snippet to your data source, updating the options (org, apiKey, and env).

```
<script>
!function(e){if(t)t.options=e||{},t.start instanceof Function?(console.warn("yolk already loaded - starting anyway"),t.start()):console.error("Yolk disabled: `yolk` Object exists but is the wrong type");else{var t={functionQueue:[],options:e||{}};window.yolk=t;var n=["event","context","identify","alias","view","track"];function o(e,n){if(null==e)console.error("missing function name in snippet enqueue() - queued function ignored");else{var o={fn:e,args:n};t.functionQueue.push(o)}}for(let e=0;e<n.length;e++){var a=n[e];t[n[e]]=function(){o(a,Array.prototype.slice.call(arguments))}}var i=document.createElement("script"),c=t.options.yolkJsUrl||"https://cdn.yolkdata.com/yolk.js";i.type="text/javascript",i.async=!0,i.src=c,i.addEventListener("load",function(){t.start()}),document.getElementsByTagName("head")[0].appendChild(i)}}({
    org:"cake",
    write_key:"3b11be58-af28-4c3d-86c2-fbaef90f4f85",
    endpoint:"https://api.cake.yolkanalytics.com/track",
    // env: "production",
    // logLevel: "debug"
}); </script>
```

### 2. Instrument Tracking

#### Track events

```
yolk.event("event name", {
  "properties":{
    "any":"properties",
    "you_would":"like to track"
  }
});
```

#### Identify users

```
yolk.alias("alias",{
  "properties":{
    "vid":"3dd34",
    "orgid":"54t66",
  }
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
