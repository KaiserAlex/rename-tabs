# Rename Edge Tab

Rename the active browser tab title – persistently for the session. Works in Edge and Chrome without any installation or setup.

## Setup (one-time)

1. Create a new bookmark in your browser (Ctrl+D or right-click bookmarks bar → "Add page")
2. Set the name to: `Rename Tab`
3. Set the URL to the following code:

```
javascript:void((function(){var t=prompt('New tab title:',document.title);if(t){document.title=t;window.__forcedTitle=t;if(window.__titleObserver)window.__titleObserver.disconnect();window.__titleObserver=new MutationObserver(function(){if(document.title!==window.__forcedTitle)document.title=window.__forcedTitle});var el=document.querySelector('title');if(!el){el=document.createElement('title');document.head.appendChild(el)}window.__titleObserver.observe(el,{childList:true,characterData:true,subtree:true});window.__titleObserver.observe(document.head,{childList:true})}})())
```

## Usage

- Click the bookmark → enter new title → done
- The title persists even if the page tries to change it
- To reset: refresh the page (F5)

## How It Works

The bookmarklet injects a `MutationObserver` that watches the `<title>` element. Whenever the page tries to change the title, the observer immediately overrides it with your chosen title.

The title persists until:
- The tab is closed
- The page is refreshed (F5)
- The browser is restarted

## Limitations

- Requires a manual click (cannot be triggered via script)
- Does not work on browser-internal pages (`edge://`, `chrome://`, `about:`)

## System Requirements

- Microsoft Edge (Chromium-based) or Google Chrome
- No installation, no extensions, no modules required
