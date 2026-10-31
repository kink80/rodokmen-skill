# Privacy, ethics and third-party services

## Living people
- On the website only name and birth year (`note="Žijící – údaje jen základní."`). No addresses, phone numbers, birth
  numbers (rodná čísla), health data, divorces, illegitimate children of living or recently deceased people without the family's consent.
- Sensitive matters from the recent past (suicides, imprisonment, illnesses): ask the user whether and how to mention them.
- Register data past the legal closure periods is public, but be considerate with it too (no needless ridicule).

## Before publishing the website – go through with the user
`python3 -I tools/kontrola.py` reports `CITLIVÉ` for texts from the last ~100 years (suicide, illness, divorce, debts,
illegitimate child…) that go on the web – in person notes, „Co hledáme“ (`ISSUES`), side relatives, fun facts and the story.
Record what the user approves for the web as a snippet in `web.json` → `citlive_schvaleno` (format: `web.md`).
Data from a family tree (GEDCOM) and from the family is often exactly like this – fine for research, on the web only with consent.

Checklist:
1. Living people on the website and what it says about them.
2. Family photos and documents – do the people in them (or their family) agree?
3. Login question and answer (`web.json` → `prihlaseni`) – set and hard enough? (`web.md`)
4. Contact on the website – does the user want their e-mail there?

Only then `npm run nasadit`.

## Paid and third-party services (MyHeritage, Ancestry, Geni, FamilySearch)
- Login, payments and subscriptions: **the user only**. Trials auto-renew as paid – remind the user when the trial ends and where to cancel.
- **No bulk browsing or downloading.** Read only the individual records you need, at human pace; better ask the user for an
  export (GEDCOM) or specific pages. (Real mistake: automated download of ~180 MyHeritage records got the account blocked
  „kvůli možnému zneužití“ – in the middle of a paid trial.)
- Data from these is status `mh` and is always verified in registers.

## Archive portals
Free and public, but servers have limited capacity: download only images you actually read, never whole volumes at once,
at most a few parallel requests. Always cite down to the image level (so it can be checked).

## Data in git and sharing
The project contains the family's personal data. If the user wants it on GitHub, **only as a private repo**.
