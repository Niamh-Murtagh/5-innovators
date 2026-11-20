# Local Library Book Club: launch site

A single-page launch site for a paid, six-week public-library book club.

**The pitch:** six Wednesday evenings, one librarian-chosen list, then you're free.

The page has two halves in one `index.html`:

1. **Landing page:** hero, three reasons, a sample Wednesday, sourced reader quotes and stats, and an FAQ.
2. **"Find your cohort" qualifier:** five multiple-choice questions that recommend one of three streams.

| Stream | Covers |
|---|---|
| **Page-turners** | Crime, thriller and mystery |
| **Big worlds** | Sci-fi, fantasy and romantasy |
| **Real life** | Narrative non-fiction, essays and memoir |

No frameworks and no build step. One HTML file with inline CSS and JS. The only external dependency is Google Fonts (Inter); without it the page falls back to system fonts.

## Run it locally

Open `index.html` in a browser. It works from `file://`, so no server is needed.

## Deploy to GitHub Pages

1. Keep `index.html` in the repo root.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then select your main branch and the `/ (root)` folder.
4. Save. The site is served at `https://<your-username>.github.io/<repo-name>/`.

## Before you launch

Replace the placeholders. Everything on the page reads from one config object near the bottom of `index.html`:

```js
var CONFIG = {
  LIBRARY: "[Library name]",
  PRICE_TEXT: "€[price]",
  START_DATE: "[start date]",
  BOOKING_URL: "#",
  CONTACT_URL: "#"
};
```

| Key | Used for |
|---|---|
| `LIBRARY` | Wordmark, hero, sample block, footer, browser tab title |
| `PRICE_TEXT` | Hero meta line, FAQ answer, every quiz result (include the € sign) |
| `START_DATE` | Hero meta line |
| `BOOKING_URL` | Primary result button. The stream is appended as `?stream=page-turners`, `big-worlds` or `real-life` (or `&stream=…` if the URL already has a `?`) |
| `CONTACT_URL` | Footer link, plus the secondary button on the Big worlds and Real life results |

Two things the config does **not** change. Edit them by hand in `<head>`:

- The static `<title>` and the `og:` / `twitter:` tags still say `[Library name]`.
- The `og:image` and `twitter:image` tags are commented-out placeholders. Add a 1200×630 image, set the absolute URL, and uncomment them.

## How the qualifier scores

Each answer adds points to Page-turners (P), Big worlds (B) or Real life (R). Question 4 scores nothing; it picks the personalised line in the result.

| Q | Question | Scoring |
|---|---|---|
| 1 | What do you want from a book right now? | gripped → P2 B1 · carried away → B2 P1 · learn something true → R2 |
| 2 | How do you read best? | big gulps → P2 · long sinks → B2 · in pieces → R2 |
| 3 | Where do your next reads come from? | TikTok/BookTok/Goodreads → B2 P1 · friends and family → P2 · newspapers/podcasts/lists → R2 P1 · struggle to find anything → P1 B1 R1 |
| 4 | What puts you off book clubs? | no points; five answers, each with its own result line |
| 5 | Which would you pick up first? | detective → P2 · kingdom at war → B2 · true account → R2 |

The highest total wins. On a tie, the stream chosen in Q5 wins if it is in the tie. Otherwise the tied stream that scored most on Q1 wins. If that is still tied, Page-turners wins.

The questions, scoring, result copy and objection lines live in the `QUESTIONS`, `OBJECTIONS` and `TIERS` constants in the script.

## Design notes

- **Theme:** dark-first, with a sun/moon toggle in the header. The choice is saved in `localStorage` and applied before first paint. The light theme is a full `html.light { … }` override of the CSS variables.
- **Type:** headlines use a system serif stack (Georgia first). Body text is Inter.
- **Signature element:** the Six-Lamp Rail. One lamp per Wednesday, ending in a short bar. The same rail is the quiz progress indicator: lamps 1–5 light as you answer, and lamp 6 lights on your result.
- **Background:** a 60px grid and two slowly orbiting orbs. The orbs are masked so they only appear outside the 880px content column.
- **Accessibility:** radio inputs with keyboard support, focus moved to each new question and to the result, and `prefers-reduced-motion` respected (lamps appear lit and static, orbs stop).

## What the research supports (and what it doesn't)

All copy and statistics on the page come from the market-research brief. Sources are linked in the page footer:

- [The Reading Agency via World Book Night, 2025](https://worldbooknight.org/news/nation-of-interrupted-readers)
- [The Guardian via Yahoo, 2024](https://uk.style.yahoo.com/more-third-uk-adults-given-141113221.html)
- [RTÉ Culture, 2026](https://www.rte.ie/culture/2026/0225/1559546-why-you-should-pick-up-a-book-for-ireland-reads-day)
- NielsenIQ: [UK & Ireland 2025](https://nielseniq.com/global/en/insights/commentary/2026/bestsellers-and-trends-uk-and-ireland-2025/), [UK Book Consumer 2025](https://nielseniq.com/global/en/insights/commentary/2026/the-uk-book-consumer-2025), [The age of social reading](https://nielseniq.com/global/en/insights/commentary/2025/the-age-of-social-reading)
- Mumsnet threads: [2025](https://www.mumsnet.com/talk/what_were_reading/5449834-book-club-are-you-in-one-if-so-what-does-it-cost-if-anything-and-what-do-you-likedislike-about-it), [2024](https://www.mumsnet.com/talk/relationships/5090613-billy-no-mates-at-the-book-group-wwyd?page=10)

Things to know before relying on it:

- **The named benchmarks aren't paid cohort clubs.** Faber Members is free to join (paid events were £16/£11 in 2026). Books Are My Bag is a bookshop campaign. A current Penguin Live book-club offer could not be verified. The closest real analogues are the dlr Libraries clubs, Little Bird Book Club (Dublin, often €12 per session with wine) and Faber's paid events.
- **Pricing:** €60–€120 for six sessions is €10–€20 per session. The best-supported zone is about **€72–€90**; €120 is above any in-person club benchmark the research found.
- **The "Real life" stream is the least certain.** It rests on Irish non-fiction sales data and on not finding an in-person evening non-fiction club in the listings checked. Its "Tell us what you'd want to read" button is there partly to test demand.
- **The reader quotes are about book clubs in general**, not testimonials for this one. The page says so.
- **No Irish-sourced pain-point quotes** were found; the five verbatims are UK sources.
- **Older data:** the 17% "guidance on choosing books" figure is from 2024, outside the 24-month window.

## Decisions still open

- Final price, within €60–€120.
- Whether the foyer drink is included in the price.
- Who leads the discussion each week. The copy says only "your cohort talks", so nothing is promised.
- Whether the reading list is published before booking. One quiz line ("the list is published up front") assumes it is.
- The library name, start date, booking link and contact link.

## Testing

The page was checked in headless Chrome: all 108 scoring paths matched an independent reference of the rubric, all 15 stream and objection-line combinations rendered, theme persistence worked across reloads, and there was no horizontal overflow at widths from 360px to 1600px. `window.__cohort` is exposed for testing; delete that last line of the script if you don't want it.

## Licence

Not yet chosen. Add one before making the repo public.
