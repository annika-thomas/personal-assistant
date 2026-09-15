# Off My Plate

A personal assistant for the non-work side of life: the things you remember at
random and want out of your head. Errands, appointments you still have to book,
recurring chores, people you meant to call, projects that quietly stalled.

One static page, no build step, no account, no server.

**Live:** https://annika-thomas.github.io/personal-assistant/

## Turning on hosting

GitHub Pages isn't on for this repo yet. One-time setup:

1. **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: **`claude/personal-assistant-app-7qdpw8`**, folder **`/ (root)`**
4. Save. The site is live a minute or so later at the URL above.

Every push to that branch redeploys it.

## Where your data lives

In your browser, in `localStorage`, under the key `offmyplate.v2`. Nothing is
uploaded and there is no database — which has two consequences worth knowing:

- **The page is public, your data isn't.** Anyone with the URL can open the app,
  but they get their own empty copy. Your items never leave your device.
- **Devices don't sync.** Your phone and your laptop keep separate lists.
  Settings → *Save a backup* writes a JSON file; *Open a backup* reads one back
  in on the other device. Clearing site data for github.io wipes it, so take a
  backup before you do.

## Installing it like an app

Open the URL and add it to your home screen — Share → Add to Home Screen on
iPhone, the install button in the address bar on Android or a laptop. It opens
full-screen and works with no signal: `sw.js` caches the page itself (never your
data).

## How things get filed

Type into the one box at the top. The app reads the line and routes it, showing
the destination before you commit — a dropdown overrides it.

| You type | Where it goes |
| --- | --- |
| `dentist tues 2pm` | Schedule, booked, next Tuesday at 2:00 PM |
| `schedule teeth cleaning` | Schedule, flagged *not booked* |
| `buy crispy shallots` | Buy → Groceries, as "Crispy shallots" |
| `hiking boots` | Buy → want, no rush |
| `change address` | Do |
| `renew registration friday` | Do, due Friday |
| `chore: laundry every week` | Chores, due every 7 days |
| `keep in touch with Grandma every 2 weeks` | People, nudges after 14 days |
| `plan desk setup` | Projects |
| `clean teeth myself?` | Projects → someday (a question mark means an idea) |
| `nice bandaids` | Buy → Pharmacy & care |
| `auto fat feeders` | Inbox — it won't guess |

Anything it can't confidently place lands in the **Inbox** with one-tap buttons
to file it, rather than being dropped in the wrong list.

When it does get something wrong, the pencil icon on any row opens the editor
with a **List** field at the top: pick a different one and the form rebuilds for
it, carrying the name, notes and any date across. Nothing moves until you save.

Dates it understands: `today`, `tonight`, `tomorrow`, a weekday name
(`tues`, `next friday`), `in 3 days`, `sep 20`, `20 sep`, `9/20`, `next week`.
Times: `2pm`, `2:30pm`, `14:00`, `noon`. Recurrence: `every week`,
`every 3 days`, `every other week`, `daily`, `monthly`.

## Pasting a whole list

The clipboard icon in the top bar (or pasting anything multi-line into the add
box) opens the importer: one item per line, straight out of Notes. It shows
where every line is headed, lets you change any of them and untick the ones you
don't want, and adds nothing until you confirm. Leading bullets, dashes and
numbering are stripped.

## The design rule

The Today screen is capped on purpose. At most three chores surface a day, no
matter how many are due, with a note saying how many are waiting. Tasks cap at
six, people at two, stalled projects at one. Long lists are one tap away when
you want them; the home screen never becomes a wall.

## Files

| | |
| --- | --- |
| `index.html` | The whole app — markup, styles and logic in one file |
| `sw.js` | Service worker, caches the page shell for offline use |
| `manifest.webmanifest` | Makes it installable as a home-screen app |
| `icon.svg`, `icon-*.png`, `apple-touch-icon.png` | App icons |

No dependencies and no build. Edit `index.html`, push, done. To work on it
locally, open the file directly or run `python3 -m http.server` in this folder
(the service worker only registers over HTTPS, which is fine — everything else
works either way).
