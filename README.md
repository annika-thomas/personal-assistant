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
| `teeth cleaning every 6 months book a month ahead` | Schedule, on a cycle |
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

## Appointments that come back around

A cleaning every six months needs booking a month out, and the thing that
actually fails is nobody telling you to phone. So the reminder is for the
booking, not the appointment — the date looks after itself once it exists.

Give an appointment a **Comes around** interval and a **Remind me to book**
lead time and it runs a cycle:

1. It sits quietly under *Comes around again*, showing when the next one is due
   and the day the nudging starts. It asks for nothing until then.
2. On that day it moves to *Ring them now*, on the Schedule and on Today.
3. You call, put the date in, and it's an ordinary booked appointment.
4. You go, tick it off — and instead of being filed away it restarts from the
   date you actually went, quiet again until the next lead time comes round.

Ticking never finishes a recurring appointment, it only rolls it forward.
Deleting it is how you stop the cycle.

A repeat of two months or more that names an appointment is read as one of
these; anything shorter stays a chore, so "clean the bathroom every 2 weeks"
and "teeth cleaning every 6 months" go to different places.

## Getting it to reach you

A static page can't push a notification — that needs a server. So it hands off
to the thing already in your pocket that can. The calendar icon on an
appointment, or **Settings → Export everything dated**, writes an `.ics` your
calendar app will take:

- the appointment itself, with alarms a day before and two hours before
- a separate all-day **"Ring to book: …"** entry on the day the booking window
  opens, alarmed for 9am
- birthdays, as yearly repeats alarmed the evening before

Past appointments are left out, and re-exporting replaces entries rather than
piling them up (each carries a stable UID).

## Adding things by voice

Opening the page with `?add=` and some text files it on the spot, then scrubs
the URL so a refresh can't add it twice. Multi-line text opens the importer
instead. The address is in Settings, with a copy button.

### The iOS Shortcut

Three actions, in the Shortcuts app:

1. **Dictate Text** — in its options set *Stop Listening: After Pause*.
2. **URL Encode**, with *Dictated Text* as its input. This is what keeps an
   ampersand or a hash from cutting the sentence short.
3. **Open URLs** — type
   `https://annika-thomas.github.io/personal-assistant/?add=` into the field
   and insert the *URL Encoded Text* variable right after the `=`.

Rename the shortcut to **Add to Off My Plate** — on iOS the name *is* the Siri
phrase, so "Hey Siri, add to Off My Plate" runs it, listens, and files what you
said. Swap Dictate Text for **Ask for Input** if you would rather type.

It opens the app to do it, because the page has to be running to record
anything; a silent add needs a server.

**Check where it lands.** iOS may keep a home-screen web app's storage separate
from Safari's, and this app keeps everything in local storage. Run the shortcut
once, then open the app the way you normally do and see whether the item is
there. If it isn't, pick one home — use it in Safari, or open the shortcut's URL
from inside the installed app — or it's the point at which a small backend
starts earning its keep.

## Birthdays

A person can carry a birthday and a lead time (a week, two weeks, a month).
The nudge lands early enough to do something about it, the same idea as the
booking lead, and one tap drops "Gift for <name>" onto the Buy list under
Gifts, with the date in the note.

## Putting something off

The clock icon on a task, chore, person or unbooked appointment defers it —
tomorrow, the weekend, next week, a fortnight. It disappears from Today and
from the overdue groups, and waits in *Put off for now* at the foot of its
list with the date it comes back. The real due date never changes, so nothing
is quietly rewritten to make the screen look tidier.

## The bin

Deleting puts something in **Recently deleted** rather than ending it. The
undo toast is still there for the immediate mis-tap; the bin is for noticing a
week later. Reach it from Settings.

Each entry says which list it came from, when it went, and how long it has
left — 30 days, after which it clears itself on the next load so the bin never
becomes another list to manage. *Put it back* returns it to where it came
from, minting a fresh id if that one has since been reused, so a restore can
never overwrite something newer. *Delete for good* and *Empty it now* both
ask first, and neither can be undone.

The bin is included in backups.

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

## Layout

The page scrolls one way only. `touch-action: pan-y pinch-zoom` on the body
rules out sideways drags, `overflow-x: clip` on the root catches anything that
would otherwise widen the page (`clip` rather than `hidden`, which would make
them a scroll container and break the sticky header), and long unbroken words
wrap rather than push.

On a phone the eight sections sit in a 4×2 grid rather than a scrolling strip —
a scrollable row under the input was what let the whole app be dragged
sideways. Pinned to the top are the add box and that grid, about 20% of the
screen; the wordmark and the search, paste and settings icons scroll away with
the content.

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
