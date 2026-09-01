# freelancegighub

The GigScout sales page and post-payment hand-off page. Static — no build step,
no dependencies. The bot itself lives in
[zaddywebbuilds/gigscout](https://github.com/zaddywebbuilds/gigscout).

```
index.html       the sales page
thank-you.html   where Paystack redirects after a successful payment
assets/          hero video, poster frame, screenshots
```

## Two links to set before this goes live

**1. `index.html` — the checkout link.** Near the top:

```html
<script>window.GIGSCOUT_CHECKOUT = '';</script>
```

Set it to your Paystack payment page link. Every buy button reads from it.
While it is empty the buttons scroll to the price card instead of going
nowhere, so a half-configured page still behaves.

**2. `thank-you.html` — the bot username.** Near the top:

```html
<script>window.GIGSCOUT_BOT = 'FreelancegighubBot';</script>
```

## How the two pages connect

Paystack redirects to `thank-you.html?reference=...` after a successful
payment. That page turns the reference into a Telegram deep link —
`https://t.me/<bot>?start=<reference>` — so tapping the button makes Telegram
send `/start <reference>` to the bot on the buyer's behalf. The bot then checks
that reference against Paystack before unlocking anything.

Nothing here is a secret. The reference is only useful to whoever already paid,
and it is the server-side verification that makes it access, not the URL. So
this repo can stay public and the pages can be hosted anywhere static.

Set your Paystack payment page's success/redirect URL to wherever you host
`thank-you.html`.

### On the bot handle

The sales page deliberately never names the bot, so nobody browsing the pitch
goes looking for it directly. `thank-you.html` has to name it — that is the
page that builds the link — and it must be publicly reachable for Paystack to
redirect to it, so the handle is discoverable to anyone who looks.

That is fine, because the handle was never the lock. Telegram bot usernames are
searchable, and every paying subscriber knows it anyway. The actual gate is on
`/start`: with no valid Paystack reference it refuses and shows the pay link,
whoever is asking. Keeping the name off the sales page reduces idle curiosity;
the paywall is what stops freeloading.

## Hosting

Any static host. For GitHub Pages: Settings → Pages → deploy from `master`,
root. `.nojekyll` is already here so the build does not touch the files.

## A note on the screenshots

`assets/` contains untouched screenshots of real public LinkedIn posts and of
real GigScout alerts, used as evidence. They carry real names and profile
photos. That is the point — mockups would not be proof — but it is worth
knowing what is in the repo, since it is public.
