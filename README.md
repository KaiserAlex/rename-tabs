# Rename Tabs

Rename the active browser tab title – persistently for the session. Works in Edge and Chrome without any installation or setup.

## Setup (one-time)

Create two bookmarks in your browser (right-click bookmarks bar → "Add page"):

### 1. Rename Tab

Set the URL to:

```
javascript:void((function(){var t=prompt('New tab title:',document.title);if(t){document.title=t;window.__forcedTitle=t;if(window.__titleObserver)window.__titleObserver.disconnect();window.__titleObserver=new MutationObserver(function(){if(document.title!==window.__forcedTitle)document.title=window.__forcedTitle});var el=document.querySelector('title');if(!el){el=document.createElement('title');document.head.appendChild(el)}window.__titleObserver.observe(el,{childList:true,characterData:true,subtree:true});window.__titleObserver.observe(document.head,{childList:true})}})())
```

### 2. Reset Tab Title

Set the URL to:

```
javascript:void((function(){if(window.__titleObserver){window.__titleObserver.disconnect();delete window.__titleObserver;delete window.__forcedTitle}fetch(location.href).then(function(r){return r.text()}).then(function(h){var m=h.match(/<title[^>]*>([^<]*)<\/title>/i);if(m)document.title=m[1]})})())
```

## Usage

| Bookmark | Action |
|----------|--------|
| **Rename Tab** | Click → enter new title → title is persistently set |
| **Reset Tab Title** | Click → original title is restored |

## How It Works

The **Rename Tab** bookmarklet injects a `MutationObserver` that watches the `<title>` element. Whenever the page tries to change the title, the observer immediately overrides it with your chosen title.

The **Reset Tab Title** bookmarklet disconnects the observer and fetches the current page's HTML to extract the original `<title>` – no page refresh needed.

The renamed title persists until:
- You click "Reset Tab Title"
- The tab is closed
- The page is refreshed (F5)
- The browser is restarted

## Limitations

- Requires a manual click (cannot be triggered via script)
- Does not work on browser-internal pages (`edge://`, `chrome://`, `about:`)

## System Requirements

- Microsoft Edge (Chromium-based) or Google Chrome
- No installation, no extensions, no modules required
