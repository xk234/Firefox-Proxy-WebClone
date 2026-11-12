# 🦊 Firefox (Web Edition)

A functional, web-based browser-in-a-browser clone built with vanilla HTML, CSS, and JavaScript. 

This project mimics the look and feel of a modern Firefox browser entirely within your own web browser. It utilizes multiple public CORS proxies and a `srcdoc` iframe injection to bypass standard cross-origin restrictions, allowing you to browse the web inside it.

## 📸 Screenshots

<img width="800" alt="image" src="https://github.com/user-attachments/assets/25f549eb-893a-4bf5-9a1e-21a553e9d9ed" />

### *The custom `about:home` start page featuring quick links and a Bing search bar.*

<hr>
<img width="888" alt="image" src="https://github.com/user-attachments/assets/077fb4b5-420e-4e29-923f-83c0f2f887e5" />

### *Browsing a live website with the URL bar, tab strip, and active bookmarks bar.*

<hr>
<img width="800" alt="image" src="https://github.com/user-attachments/assets/44f7f737-2a8d-4904-a07d-1e0c02e66a8d" />

### *The UI seamlessly transitions between Dark (default) and Light mode.*
<hr>

## ✨ Features

### 🖥️ User Interface (UI)
* **Authentic Firefox Theme:** Faithfully recreates the modern Firefox aesthetic with CSS variables managing layout, hover states, and spacing.
* **Dark/Light Mode:** Full theming support. Toggling the theme updates the browser chrome, menus, internal pages, and even dynamically inverts specific favicons (like GitHub) for visibility.
* **Tabbed Browsing:** Open, close, and switch between multiple tabs. Each tab maintains its own loading state, favicon, title, and independent Back/Forward navigation history.
* **Custom Start Page (`about:home`):** Features the Firefox logo, a Bing-powered omni-search box, and a grid of customizable shortcut tiles.
* **Interactive Toolbar:** * Working Back, Forward, Reload, and Home buttons.
    * URL bar with focus states, "secure" lock icon, and a loading spinner.
    * Toggleable Bookmarks bar underneath the main navigation.
    * A dropdown hamburger menu containing theme toggles and new tab options.
* **Status Bar:** A dedicated bottom bar showing "Loading..." or "Ready" states.

### ⚙️ Under the Hood (Technical Features)
* **Wisp Networking:** Navigation and proxied page `fetch()` calls use `libcurl.js` over a GX-derived pool of 14 configurable Wisp relays. The manager remembers the preferred/fastest relay, tracks per-host latency and failures, and cools down failed relay/host pairs.
* **`srcdoc` Injection & Wisp Bridge:** The browser fetches HTML over Wisp, injects a `<base>` tag, and renders it in a sandboxed `srcdoc` iframe. Link clicks, GET forms, and page `fetch()` requests are sent back to the parent browser for Wisp-backed handling.
* **Smart Omnibox (URL Normalizer):** Type a domain (e.g., `github.com`), a full URL, or just a search query. The script automatically detects the format and either prepends `https://` or routes the query to a Bing search.
* **Graceful Error Handling:** If the Wisp runtime or all eligible relays fail, the browser renders a custom, themed connection error page.

## 🚀 Getting Started

Since this is built with vanilla web technologies, there are no build steps, bundlers, or dependencies to install!

1.  **Download the repository:**
    
2.  **Open it:**
    Simply open `index.html` in your favorite modern web browser (Chrome, Edge, Firefox, Safari).
3.  **Start Browsing:**
    Use the Start Page shortcuts or type a URL/search query into the URL bar at the top!

## 📁 File Structure

* `index.html` - The core structure of the browser UI (toolbars, menus, and iframe containers).
* `style.css` - All styling, including CSS variables for theming, layout logic, and animations.
* `script.js` - The brains of the operation: tab management, history tracking, URL normalization, proxy fetching, and UI updating.

## ⚠️ Limitations & Known Issues

* **Relay and runtime availability:** The GX relay list is a historical default pool, not a guarantee that every relay is online today. `libcurl.js` 0.7.4 and its WASM file are loaded from pinned jsDelivr URLs; the browser therefore needs access to that CDN as well as at least one reachable Wisp relay. Use **Wisp Relay Settings** to pin a preferred relay or restore the GX pool.
* **Iframe Sandboxing:** Pages run in an opaque-origin sandbox. Link navigation, GET forms, and Fetch API calls are bridged through Wisp, but sites relying heavily on XMLHttpRequest, WebSockets, service workers, cross-window integrations, POST form navigation, or other browser APIs may still need additional compatibility work.
--
## 📜 License

This project is licensed under the [GNU General Public License v2.0](LICENSE). 
*Note: This is a clone and is not affiliated with Mozilla.*


## Wisp runtime and licensing

The GX-derived relay manager is in `wisp-manager.js`. It pins the upstream `libcurl.js` 0.7.4 browser runtime and WASM through jsDelivr rather than copying the 2 MB GX inline bundle into this repository. The upstream LGPL-3.0-or-later notice and full license are included in `THIRD_PARTY_NOTICES.md` and `LICENSES/libcurl.js-LICENSE`.

The public relay list is adapted from GX and should be treated as best-effort. A successful WebSocket handshake does not guarantee that a relay can proxy a real HTTPS request from this deployment origin.
