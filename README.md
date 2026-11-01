# Calm Feed

**Instagram on the web with only the accounts you follow.** Calm Feed is a small userscript that runs on instagram.com in Safari on iPhone (or any desktop browser). It:

- opens your **Following** feed, so you see only accounts you follow, newest first
- hides the **Reels** tab and sends you back to your feed if you land on Reels
- sends the **Explore** grid to the search page, so you can still look people up
- hides **"Suggested for you"** posts and accounts in the feed

It keeps your DMs, Stories, profiles, and reels someone sent you directly.

> Calm Feed is not affiliated with, endorsed by, or connected to Instagram or Meta.

## Privacy

Calm Feed only hides and redirects things on the page you're looking at. It makes **no network requests** and never reads your password, messages, or cookies. You log in on Instagram's own website in Safari, the same as without it. The whole thing is one readable file: [`calm-feed.user.js`](calm-feed.user.js).

## Install on iPhone

1. Install **Userscripts** from the App Store. It's free and [open source](https://github.com/quoid/userscripts).
2. Turn it on: **Settings → Apps → Safari → Extensions → Userscripts**. Switch it on and set **instagram.com** to **Allow**. (On older iOS versions it's **Settings → Safari → Extensions**.)
3. In Safari, open this link:
   **https://raw.githubusercontent.com/lmxproject/calm-feed/main/calm-feed.user.js**
4. Tap the **page menu icon** next to the address bar (it looks like **ᴀA** or a puzzle piece) → **Userscripts** → **Install**.
5. Go to **instagram.com** in Safari and log in.

### Make it feel like an app

1. On instagram.com in Safari, tap **Share → Add to Home Screen**.
2. Switch **"Open as Web App" off**, then tap **Add**.

This step matters: Safari extensions **don't run inside Home Screen web apps**. With "Open as Web App" off, the icon opens a normal Safari tab, where Calm Feed works.

Optional: to stop yourself from opening the real Instagram app, set a 1-minute limit for it in **Settings → Screen Time → App Limits**.

## Install on a computer

Install [Violentmonkey](https://violentmonkey.github.io/) or [Tampermonkey](https://www.tampermonkey.net/) in your browser, open the link in step 3 above, and click **Install**.

## Settings

Open `calm-feed.user.js` and change `true` to `false` at the top for anything you want to keep:

```js
const SETTINGS = {
  followingFeed: true, // open the "Following" feed instead of the algorithmic home feed
  hideReels: true,     // hide the Reels tab and leave the Reels page
  hideExplore: true,   // leave the Explore grid for the search page (search keeps working)
  hideSuggested: true, // hide "Suggested for you" posts and accounts in the feed
};
```

**Instagram not in English?** Suggested posts are found by their label text. Add the exact text you see in your language to `SUGGESTED_LABELS`.

## Updates

Calm Feed checks this repository for new versions (`@updateURL`). When Instagram changes its website and something stops working, a fix pushed here reaches your phone automatically. To get it sooner, open the Userscripts extension in Safari and tap **Update**.

## Known limits

- **Safari only on iPhone.** It can't change the Instagram app; no app on the App Store can.
- **No push notifications**, because you're using the website.
- **Instagram changes its site often.** Something may break now and then until the script is updated.
- **The Following feed address** (`/?variant=following`) isn't documented by Instagram. If Instagram ignores it, Calm Feed stops trying after a few attempts, so the page never keeps reloading. You'll then see the normal feed with suggested posts hidden.

## How it works

- **Redirects.** Instagram's website changes pages without reloading, so the script checks the address a few times a second. If you're on `/reels/` or the Explore grid, it sends you on. If Instagram keeps undoing a redirect, the script gives up on that redirect for the rest of the tab.
- **Hiding.** Reels links are hidden by their link address, which changes less often than Instagram's styling. Suggested posts are found by their label. The script then hides that post, or the block around the label that sits between posts, but never anything that contains your normal posts.

## Testing

Open `test/test.html` through a local web server and check that every test passes:

```bash
python3 -m http.server 8765
```

Then visit http://localhost:8765/test/test.html.

## License

[MIT](LICENSE)
