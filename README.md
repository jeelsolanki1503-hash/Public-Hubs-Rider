# Hub Ride — mobile prototype (sections 1 + 2)

A static, mobile-only HTML/CSS/JS prototype of the Hub Ride rider flow for
Mumbai T2. All visuals come from the Hub design system documented under
[`docs/`](docs/).

## Run locally

Any static server works. Simplest:

```bash
cd "D:/PDL/public hubs/riders app/Smart Home/riders flow"
python -m http.server 5173
```

Then open <http://localhost:5173/>.

> Opening `index.html` with the `file://` protocol will not work: the ES
> module `js/app.js` only loads over `http(s)://`. Always use a server.

## Deploy to GitHub Pages

1. Push the folder to a repo (`main` branch).
2. In the repo's **Settings → Pages**, pick **Deploy from branch**, branch
   `main`, folder `/ (root)`.
3. Open `https://<user>.github.io/<repo>/`.

Rules the layout already honours:
- All paths are relative (`./css/base.css`, `./js/app.js`).
- `.nojekyll` is present so Pages serves everything as-is.
- `404.html` is a byte-for-byte copy of `index.html`, so deep-link refreshes
  still boot the app (reserved for future hash routing).
- Case-sensitive filenames throughout (Pages runs Linux).
- No build step. No bundler. No `node_modules`.

## File layout

```
index.html          App shell + every rider screen as a <section>.
404.html            Copy of index.html (deep-link fallback).
.nojekyll           Empty file; disables Jekyll processing on Pages.
css/
  variables.css     Design-system tokens (colour, radii, sizes, motion).
  animations.css    @keyframes used across the app.
  base.css          Reset, phone frame, status bar, home indicator, screen stack.
  components.css    Buttons, inputs, chips, options, cards, sheets, dialogs,
                    OS pop-up, toast, spinner, timeline, tab bar, banners.
  screens.css       Screen-specific layout (lock screen, bay bar, map,
                    big plate, code tiles, ETA badge, rating stars).
js/
  app.js            One IIFE: router, auto-advance table, timers, delegated
                    click handler, stepper + toggle + star behaviours.
docs/
  README.md         Overview of the Figma-source docs.
  design-system.md  Every token, component, state and rule.
  rider-flow.md     All 70 Figma rider screens with verbatim copy.
```

## What's built in this prototype

**Section 1 — Sign up and lock a ride (before landing)**
A1 Welcome · A2 Choose language · A3 Mobile number · A4 Enter code ·
A5 Your name · A6 Location permission (OS pop-up) · A7 Save your Home ·
B1 Home (first time) · B2 Your flight · B3 Ride home (book) ·
B3a Where to? · B3b Other ways home (sheet) · B4 Lock your ride (sheet) ·
B4a What's included (sheet) · B4b Ride promise (sheet) · B5 Paying ·
B6 Ride locked · B7 Your ride (scheduled) · B7a Share your ride (sheet) ·
B8 Home (ride booked).

**Section 2 — Landing to her door (first ride)**
C1 Landed: notification (dark lock screen) · C2 Welcome to Mumbai ·
C3 Finding your auto · C4 Your driver · C5 Walk to Bay 12 ·
C5a Send a photo (sheet) · C6 Meet at Bay 12 · C7 On the trip ·
C8 All bags out? (sheet, no scrim close) · C9 Rate your ride.

**Live behaviour**
- A4: resend-code countdown (0:24 → "Resend code")
- B5 → B6: 2.4 s auto-advance after "Pay ₹420 and lock"
- C3 → C4: 3 s spinner, then driver card
- C5: free-wait ring counts down from 4:12
- C6 → C7: 5 s hold on the big plate, then trip screen
- C7 → C8 sheet: 6 s after the trip starts
- C9 "Done" loops back to B1

**Dead-ends on purpose**
Buttons for the edge cases this build hasn't shipped yet (SOS, cancel,
wrong-auto, bags-delayed, no-auto-free, skip-driver, extra-money,
lost-bag, report-a-problem) show a short toast noting the flow comes in a
later batch, so no button is dead.

## Not built yet

Driver app · edge cases X0–X20 · M-tabs (Trips/Help/Profile beyond stubs)
· No-app lane (W1–W4) · Pool screens (B3p / B3pn / B3pi / B3pw / B4p) ·
A7b (confirm Home) · B3mi (why a minimum fare) · B3w (other ways out of
T2) · X16 (short-trip minimum fare) · X19 (bags don't fit).

## Theme

Light by default. The system also carries a dark palette (see
`css/variables.css`) that activates whenever the viewer's OS prefers dark;
no in-app toggle, per brief.

## No prototype panel

Per brief, there is no dev panel. All flow starts come from the real
screens in order.
