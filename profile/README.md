<div align="center">

<img src="https://avatars.githubusercontent.com/u/328288197?v=4" width="96" alt="OurEvenet" />

# OurEvenet

**Invitations that work on a phone, in a chat, one-handed.**

We build digital invitations for Sri Lankan weddings and engagements —
static sites that carry the RSVP, the running order and the calendar
handoff, with no server to rent and nothing to keep running after the day.

<a href="https://github.com/OurEvenet/He-She"><img alt="hosting: GitHub Pages" src="https://img.shields.io/badge/hosting-GitHub%20Pages-121013?logo=github&logoColor=white"></a>
<img alt="build step: none" src="https://img.shields.io/badge/build%20step-none-2ea44f">
<img alt="vanilla ES modules" src="https://img.shields.io/badge/JavaScript-vanilla%20ES%20modules-f7df1e?logo=javascript&logoColor=black">
<img alt="accessibility: measured" src="https://img.shields.io/badge/accessibility-measured%2C%20not%20eyeballed-4c1">

</div>

---

## Why this exists

An invitation is read a handful of times, by people who are mostly not
technical, mostly on a phone, mostly from a forwarded WhatsApp message. It
has to be correct on the first open — the right time in the reader's own
timezone, a reply that actually reaches the couple, directions that dial and
navigate — and then it has to stop mattering.

That rules out most of the modern web stack. Our sites are plain HTML, CSS
and vanilla ES modules: push them to a branch, turn on GitHub Pages, and they
work. No framework, no bundler, no database, no subscription that lapses two
years later and takes the invitation with it.

---

## What's here

| Repository | What it is | Live |
| --- | --- | --- |
| **[He-She](https://github.com/OurEvenet/He-She)** | The invitation platform. Seven complete designs sharing one content file, with RSVP, `.ics` and Google Calendar handoff, a form-based editor, and a Python toolchain for photographs, icons and audits. | [Preview](https://ourevenet.github.io/He-She/) |
| **[Gayathri-Ashen](https://github.com/OurEvenet/Gayathri-Ashen)** | A real engagement invitation built on the same approach — a single self-contained page, sent to actual guests. | [View](https://ourevenet.github.io/Gayathri-Ashen/) |
| **[.github](https://github.com/OurEvenet/.github)** | This profile and the organisation's shared community files. | — |

---

## How a site is put together

```
data/wedding.json     ← every word, date, address and phone number
        │
        ├── index.html + theme1…theme6.html   seven designs, markup only
        │
        └── assets/js/modules/                the parts a design must not reimplement
              dates.js       wall-clock time at the venue → correct UTC, anywhere
              calendar.js    .ics with real VALARMs, Google links, Calendar API
              rsvp.js        validation, submission, calendar handoff
              render.js      builds every section from the JSON
              images.js      lazy loading, blur-up, no layout shift
```

Change a time once and all seven designs have it. A design supplies markup and
a palette and nothing else — so a new design **cannot** get the times wrong or
break the reply form, because it does not implement either.

---

## What we hold ourselves to

**Times are converted, not typed twice.** Times are written as local
wall-clock time at the venue, the way a person reads an invitation. The page
converts to UTC with `Intl`, so a guest whose phone is set to London or
Melbourne still sees the ceremony at the right Colombo hour, and their
calendar entry lands correctly.

**Contrast is measured, not eyeballed.** `tools/check-pages.py` walks every
design as a browser renders it and measures every line of text against the
ground actually behind it — over a thousand of them — plus sideways scroll at
320px, tap targets under 44px, and console errors. Text over a photograph is
read out of the rendered pixels, because there is no single colour to score
against.

**Private by default.** Invitations carry a family's names, a house-full of
phone numbers, and a date and place they will all be at. Pages ship
`noindex, nofollow` unless a couple deliberately turns it off, guest-list CSVs
never enter the repository, and imported photographs are rebuilt from raw
pixels so no EXIF, GPS or camera serial survives the trip.

**Built for a thumb.** A dock in the thumb zone with *Reply*, *Directions* and
*Call*; nothing tappable under 44px; form fields never under 16px, the size
below which iOS zooms the page mid-form; edge-to-edge paint with every edge
padded back out of the notch.

**Accessible as shipped.** Skip links, visible keyboard focus, labelled
controls, live error messaging at the field it concerns, the running order as
a real ordered list, decoration kept out of the tab order, and
`prefers-reduced-motion` honoured everywhere.

**Nothing that can go stale.** No service worker, because offline support
means a cache that can serve last week's times after you have corrected them.
Fonts are self-hosted, so no CDN can break the page. Image URLs carry a hash
of the file's own content, so a replaced photograph can never be served from
an old cache.

---

## Start your own

```bash
git clone https://github.com/OurEvenet/He-She.git my-invitation
cd my-invitation
python3 -m http.server 8000
```

Then open `http://localhost:8000/themes.html` to see all seven designs side by
side, and `http://localhost:8000/editor.html` to fill in the details — the
editor gives every field a proper control, shows you the real timezone
conversion under each start time, and flags the things that bite quietly
(an event ending before it starts, a gallery entry pointing at a missing
file, an unset form endpoint).

Push to `main`, enable **Settings → Pages → Deploy from a branch**, and set
`meta.url` to the address Pages gives you. Before you send the link, work
through [`docs/BEFORE-YOU-SEND.md`](https://github.com/OurEvenet/He-She/blob/main/docs/BEFORE-YOU-SEND.md)
— twenty minutes on a phone, and every item on it is there because it is a
thing that goes wrong silently.

Full documentation lives in the
[He-She README](https://github.com/OurEvenet/He-She#readme).

---

## Built with

Plain HTML · CSS custom properties · vanilla ES modules · GitHub Pages ·
Python 3 for the build-time tooling (Pillow, NumPy, Playwright) ·
Fraunces & Karla, self-hosted under the SIL Open Font Licence

---

## Contributing

Issues and pull requests are welcome on any repository — an eighth design, a
bug that only shows up on one phone, or a line of documentation that misled
you. New designs are deliberately cheap to add: a markup file, a palette, and
a page, with the arithmetic and the reply form inherited.

If you are reporting something that goes wrong on a device, the device, the
browser and the screen width are the three things we will ask for first.

<div align="center">
<sub>Built in Sri Lanka 🇱🇰 · <a href="https://github.com/OurEvenet">github.com/OurEvenet</a></sub>
</div>
