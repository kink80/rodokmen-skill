# Fun facts and the family story

This is why the family reads the website. A table of names and dates hooks no one; „prapradědeček se narodil 267 dní
po svatbě“ or „rod žil sto let v jednom domě“ does. Don't leave it until the end of the research.

**Language:** fun facts (`data/zajimavosti.md`) and the story (`data/pribeh.md`) are written **in Czech** for the family.
Headings and phrases quoted below are Czech examples – keep that style.

## Fun facts (`zajimavosti`) – collect continuously
- Whenever you hit something unusual while reading a record, **immediately** add a short paragraph to `data/zajimavosti.md`
  and tell the user about it as a story.
- After every bigger round run `python3 -I tools/napady.py`: it scans the data and prints ideas (weddings shortly before
  a birth, a line in one house, marriages between relatives, longevity, epidemics…). Verify each idea in the record before writing it.

What tends to be interesting (all from one real family tree):
| Type | Example / how to write it |
|---|---|
| Last-minute weddings | table: wedding – child – after how many days (compute exactly); „přesná trefa“: born 267 days after the wedding – probably conceived on the wedding night |
| Illegitimate children, legitimation | „Jméno přežilo díky svobodné matce“ – surname inherited from the mother; legitimation note in the record margin |
| A hundred years in one house | same house number over 3–5 generations, who was born and died there |
| Ancestor appearing more than once | grandpa and grandma were distant relatives (`SAME`) – where the branches met |
| Unusual names | first name after a saint (and how registers garbled it) |
| Notable witnesses and godparents | councillor, village reeve (rychtář), parish priest, manorial official at a common craftsman's wedding |
| Dispensations | wedding with the bishop's permission (kinship, waived banns, Advent) |
| Trades in the family | tailors, gunsmiths, barber-surgeon (ranhojič), brewer… as one paragraph across all lines |
| Women's fates | widowed twice within ten years; a maid who married a widower 25 years older |
| Children orphaned early | mother died after childbirth, father in an epidemic – who raised the children |
| Epidemics and wars | cholera 1831/1855/1866, Austro-Prussian war 1866, Spanish flu 1918 (cause of death is in the record!) |
| Modern times | civil marriage at the registry office and court permission for an underage bride; moving to the borderlands after 1945 |
| Family lore vs. register | „rodina říkala Marie, ve skutečnosti Anna“; confirmed legend („babička byla ze statku“) |
| Lost beginning | where a line ends and why (burnt books, illegitimate child without a father) |
| Newspapers | clipping on an appointment, decoration, pension, obituary – add the image to `dokumenty.json` |

Style: heading `## Krátký a zvědavý` (short, curious), 2–5 sentences, concrete dates and places, person links `[Jméno](/osoba/<slug>/)`.
Put the best 2–4 also on the home page (`web.json` → `zajimavosti_na_uvodu`, see `web.md`).
Sensitive 20th-century matters (suicides, illnesses, divorces, illegitimate children of recently living people): only with
the user's consent – `privacy.md`. `kontrola.py` flags them.

## The main line's story
- **Main line** = the starting person's line (birth surname, usually line 8 in `LINES`). If the user wants another, set
  `web.json` → `pribeh.rod`.
- Title: **„Příběh <line name in genitive plural>“** (Příběh Novákových, Příběh Dvořáků). It has its own menu item and is the
  main thing on the home page.
- **When to write:** as soon as the main line has about 5 generations documented from registers. Don't wait until it's
  "done" – it can be extended later. `kontrola.py` reminds you. Offer it to the user even if they earlier said „až bude víc dat“.

**Procedure:**
1. Gather material: facts and notes of the main line's people, `SIDE`, fun facts, `udaje_od_rodiny.md`,
   family memoirs and documents, newspaper clippings.
2. Write a **draft in the chat** and get it approved (length, tone, what's in/out). Below the draft state what comes from
   registers vs. from the family, and what you left out as sensitive. Save only after approval.
3. Save to `data/pribeh.md`:
   ```markdown
   ---
   title: Příběh Novákových
   lead: Jedna věta, která chytí – od koho k komu příběh vede.
   ---

   ## Prolog v mlze (1700–1750)
   …
   ## Kováři z čísla 112 (1777–1874)
   …
   ```
4. Set `web.json` → `"pribeh": {"menu": "Příběh Novákových", "rod": 8, "tlacitko": "Přečíst příběh Novákových"}`
   and the home intro (`uvod.nadpis`, 2–3 teaser paragraphs, photo) – format in `web.md`. Without `uvod` the home page
   shows at least the story title and `lead`.

**Shape that works** (about 800–1500 words):
- Chapters by generation, from the oldest ancestor to today. Heading = the person or their role + years
  („Krejčí z Holic (asi 1756–1812)“, „Služka, která zachránila jméno“).
- Each chapter 1–3 short paragraphs: where they lived, livelihood, wedding, children, something human (a record, a witness,
  an accident, a move).
- Concrete: dates, places, house numbers, quotes from records in quotation marks. Person links `[Jméno](/osoba/<slug>/)`
  (slugs in `web/src/data/site.json`).
- Place names: archives and OpenStreetMap may write e.g. „Veliš“ while the family and today's official form is „Velíš“.
  Ask which form the family uses and keep it throughout.
- Honest: write unknowns as questions or conjectures („nejspíš“, „podle rodiny“). Never invent anything for flow.
- Final chapter about today: only names and relationships of living people, no details.
- Other lines get their space on line pages (description in `LINES`, `HYPOTEZY`) and in fun facts.
