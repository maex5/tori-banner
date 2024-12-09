# Dynamic Banner for Tori.fi
The `index.html` contains a dynamic banner with the following features:
- Gets the targeting parameters to the banner from the URL
- Updates content based on targeting parameters
- Shows content and video starts playing when 50% in view (with IntersectionObserver and threshold: 0.5)
- Has two different links, both are tracked with GAM clickurl

## Explanation of Parameters

| Parameter               | Description                                      | Example Value       |
|-------------------------|--------------------------------------------------|---------------------|
| `%%PATTERN:obj_price%%` | Price of the item in euros (without cents), populated from GAM targeting. | `199`               |
| `%%PATTERN:search_query%%` | The search term used, populated from GAM targeting. | `iphone`            |
| `%%CLICK_URL_ESC%%`     | A GAM macro that dynamically inserts the click URL. | `https://example.com` |
| `%%CACHEBUSTER%%`       | Ensures the URL is unique on every request to prevent caching issues. | `12345`             |

## Testing Locally
1. Use [Live Server - Visual Studio Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer).
2. Open `index.html` with Live Server.
3. Add this query string to the URL:

```
?clickurl=https://example.com&cachebuster=12345&obj_price=199
```
**NOTE**: `clickurl` won't redirect the same way as it will in GAM's production environment

## Add to GAM as a Third-Party Tag
```html
<iframe
    src="https://maex5.github.io/tori-banner/?clickurl=%%CLICK_URL_ESC%%&obj_price=%%PATTERN:obj_price%%&cachebuster=%%CACHEBUSTER%%"
    width="160"
    height="600"
    frameborder="0"
    scrolling="no"
    style="border: none;">
</iframe>
```

## GAM preview
Open product page and add this after URL
```
?google_preview=vI-7Ep_rtuMYgvPbugYwgo-RwgaIAYCAgJDAjbSHkQE&iu=117157013&gdfp_req=1&lineItemId=6865737029&creativeId=138500404553
```

If you want to test search results, add this instead to the chrome dev console:
```
const params = new URLSearchParams(window.location.search);
params.set("google_preview", "vI-7Ep_rtuMYgvPbugYwgo-RwgaIAYCAgJDAjbSHkQE");
params.set("iu", "117157013");
params.set("gdfp_req", "1");
params.set("lineItemId", "6865737029");
params.set("creativeId", "138500404553");
window.history.replaceState({}, "", `${window.location.pathname}?${params}`);
window.googletag?.pubads()?.refresh();
```
