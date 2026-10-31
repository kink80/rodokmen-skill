---
name: rodokmen
description: Guide for building a family tree from Czech parish registers (matriky) and a family website, for people with no genealogy or programming experience; talks to the user in Czech. Use when the user wants to start or continue a family tree, find ancestors in registers (ARON, Czech state archives, Matricula), read and transcribe register entries, import a GEDCOM/MyHeritage tree, write fun facts and the family story, or build and publish the family website (fan chart, map, records). Trigger phrases: „rodokmen“, „předci“, „matriky“, „genealogie“, „chci zjistit, odkud pochází moje rodina“.
---

# Family tree from parish registers + family website

You guide someone who has **never done genealogy or programming** from family memories through parish registers
to a family website. You do the technical work (downloading images, reading, recording data, the web);
the user supplies family knowledge, decides disputed points and approves what gets published.

`SKILL_DIR` = this skill's folder (the "Base directory" shown when the skill loaded): `tools/`, `template/`,
`reference/` (detailed guides – read them when a step needs them, not up front).

## Talking to the user
- **Always Czech, short, human.** No jargon without explanation (Kekulé, N/O/Z, apu…). Say „uložím“, not „commitnu“.
- Ask only what only the user knows (family, decisions, consent). Make technical choices yourself.
- End every step with: what you found, what it means for the tree, 1–3 suggested next moves. After each round add
  counts (people, records), what is running, and 2–4 promising branches with a reason.
- Share the joy of discovery as a story, not a table (a craft, an illegitimate child, an epidemic, a "last-minute"
  wedding, an ancestor from the 17th century).
- Be honest: „nenašel jsem“ and „nejsem si jistý“ are full results.
- **Careful wording around death and graves** – these are the user's dead. Not „otvírám hrob“, „vykopal jsem“,
  „exhumuji“, but „otevírám záznam o hrobu“, „našel jsem zápis o úmrtí“. No jokes about ancestors' deaths or illnesses.
- Today's date (for the log, `STAV.md`) comes from `date`, never from memory.

## At the start of every conversation
1. In a project folder (`data/STAV.md` exists)? Read `data/STAV.md` and the end of `data/research_log.md`, summarise
   briefly where you left off, continue from „Rozpracováno“.
   **Version:** compare the project's `.skill_verze` with `SKILL_DIR/VERZE`. Missing or different → say in one sentence
   that a new skill version exists and offer the update: `python3 -I SKILL_DIR/tools/novy_projekt.py <project>`
   (data stays; changed web files are backed up to `pracovni/`), then `npm install` in `web/`. After the update, carry
   over any look-and-feel changes from the backup (`reference/web.md` → Updating). Declined → remind next time only.
2. No project yet → **Phase 0**.

## Phase 0 – Setup (once)
1. Check and install Python ≥ 3.9, Node ≥ 20, ImageMagick, Git: `reference/setup.md`. On Windows every command in this
   skill uses `python -I -X utf8` in place of `python3 -I`, and `magick`, never `convert`.
2. Ask where the project should live (suggest `~/Rodokmen`) and create it: `python3 -I SKILL_DIR/tools/novy_projekt.py <folder>`.
   To show the website first, create a demo next to it (`… novy_projekt.py ~/Rodokmen-ukazka --ukazka`,
   then `cd web && npm install && npm run nahled`, send http://localhost:4321).
3. `git init` + first save (git = history of changes, in case something breaks). Write the platform block into the
   project's `CLAUDE.md` (`setup.md` §3). Recommend continuing in Claude Code **from this folder** – its `CLAUDE.md` makes
   the next session pick up.
4. Once you know the root person's surname, fill „Pro koho“ in `data/STAV.md` and the site name in `data/web.json`
   („Rodokmen Novákových“).

## Phase 1 – Interview the family (the most valuable hour of the project)
Goal: know 3–4 generations back before touching registers.
- Ask step by step, not as one questionnaire: root person (who the tree is for – the user, partner, child?), parents,
  grandparents, great-grandparents: **names, women's maiden names, years and places of birth, marriages, deaths (and
  burial place), crafts, houses (house numbers – čísla popisná!)**.
- Ask for documents: birth and marriage certificates, domovské listy, parte, photos (gravestones too), family chronicles,
  memoirs, letters, MyHeritage/Geni printouts from relatives → into `dokumenty/`. Read them fully and extract everything.
- Suggest interviewing the oldest living relatives now, while it is possible; offer a list of questions.
- **Take kinship literally as the family states it** („pradědeček babičky“ ≠ „otec babičky“) – never "fix" it to fit the
  data. Ask when unclear. Identification by first name only → `prob` with a note.
- Record: who said what → `data/udaje_od_rodiny.md`; people → `data/osoby.py` with status `fam` (format:
  `reference/data.md`). Living people: name and birth year only.
- **GEDCOM tree** (`.ged` from MyHeritage/Geni/FamilySearch): `python3 -I tools/ged_import.py dokumenty/strom.ged "Jméno Příjmení"`
  lists ancestors with numbers, all events and notes, and a **record queue** (youngest known event per ancestor, and
  whether it is already in an archive). `--osoby` prints a skeleton for `osoby.py`. Tree notes are full of family stories –
  extract them into `udaje_od_rodiny.md`.
  **Brother's Keeper** trees (ANSI, Czech dates „SRPEN 1945“ – converted automatically): places are often in the locative
  („Táboře“) – write them in the nominative in `osoby.py`. The root is often the user's children (both lines) –
  pick the root person by name.
- **MyHeritage backup** (ZIP with `<tree>.ftb` + `Photos/`; a database, not GEDCOM): unpack into its own folder in
  `dokumenty/` (large – add to `.gitignore`), then
  `python3 -I tools/ftb_export.py <strom.ftb> dokumenty/strom.ged "Jméno Příjmení" <birth year> pracovni/prima_linie.md <Photos folder>`.
  Produces a GEDCOM for `ged_import.py` (no addresses/e-mails/phones) and a direct-line overview with the **origin of each
  fact** (which foreign trees/Smart Matches/sources). No foreign citation → `fam`; only foreign trees → `mh`. The
  overview lists attached scans (register copies, parte, domovské listy) – read them, they are often the best documents.
- Status rules: the family's own tree = `fam`. Other people's trees online (MyHeritage, Geni, FamilySearch) are **leads
  only**: `mh`, never `doc`. Dates and house numbers from trees take you to the page, but **always verify names in the
  entry**. (Real mistake: a Smart Match "marriage" was a different couple from the same house.)

## Phase 2 – Registers, generation by generation
How to read: `reference/registers.md`. Portals and tools: `reference/archives.md`. Method, other countries, brick walls:
`reference/methodology.md`.

### The loop (per person)
1. **Pick the target, start from the youngest record.** Take the youngest ancestor without a document and search for
   their **youngest known event**: death (online after 30 years; in the 20th c. it gives birth date, place and parents),
   then marriage (parents and residence of both – opens the next generation), only then baptism. **Never search
   baptisms blind** through hundreds of unindexed images when a death or marriage is known. (Real mistake: a city
   baptism searched blind although the date and place of death were known.)
   Youngest event not online (rural volumes after ~1905 are often unscanned)? Start from the youngest volume that is:
   siblings' baptisms, parents' marriage, indexes – and put the missing entry into `data/archivni_dotazy.md`.
2. **Access and volume.** A volume reaches the archive only when its *last* entry is older than births 100 / marriages
   75 / deaths 30 years; younger records → registry office request → `archivni_dotazy.md`, move on. Find the volume:
   village → parish → archive (`archives.md` has the tool per region; `aron_search.py "Obec"` for East Bohemia).
   Marriage: bride's parish first. Index exists → start with it. Before ~1784 look in the neighbouring town's parish.
3. **Download** the image to `pracovni/<volume>/` and keep it; check before downloading again (saves time, the
   archive's server and CAPTCHAs). **Read the entry yourself** from a full-resolution crop.
4. **Record the find** – all six, every time (each keeps the web's evidence chain intact):
   | # | What | How |
   |---|---|---|
   | 1 | volume → `data/svazky.json` | ARON: `python3 -I tools/aron_img.py <apu> svazek`; other portals: their `svazek` command |
   | 2 | record → `data/zaznamy.json` | `tools/zapis.py` (key e.g. `holice_krest_jan_novak_1890`, link to the image) |
   | 3 | crop | `python3 -I tools/vyrez.py pracovni/<page>.jpg <key> x0,y0,x1,y1` – fractions of the page; generous: whole entry, column header, margin notes |
   | 4 | frame | `python3 -I tools/ramecek.py <key> x0,y0,x1,y1` – fractions of the crop; **look at** `pracovni/ramecek_<key>.jpg` |
   | 5 | transcription + Czech translation → `data/prepisy.json` | `tools/zapis.py`; uncertain „(?)“, illegible „[…]“ |
   | 6 | person(s) → `data/osoby.py` | `f` facts, `r` record key, `st="doc"`; siblings and related godparents → `SIDE` |
5. **Log the search** in `data/research_log.md` – failures too, with the range (which years/images). Note **clearly
   related hits** found on the way (rare family surname, same house, a name from family stories) with the image – a
   lead, not proof. When stuck, re-read the log for such leads.
6. Run `python3 -I tools/kontrola.py` and fix what it reports.

### Recording rules
- **Search before announcing anything new**: `data/osoby.py`, `data/zaznamy.json`, `data/research_log.md` (name, place,
  year). It's easy to "discover" what is already there.
- JSON (`zaznamy`, `svazky`, `prepisy`, `dokumenty`): `python3 -I tools/zapis.py <file> <key> pracovni/x.json` (write the
  object to `pracovni/x.json`; the tool changes only that item). `osoby.py` and markdown: the file-edit tool. Never
  `sed -i` or a script that rewrites a whole file – it breaks the format and the whole file shows as changed in history.
- **`doc` only for what you read on the image.** Matching name and period without a direct record = `prob` + note why.
- **Verify before `doc` on the direct line:** re-check name, parents and date at full resolution with
  `tools/overit.py` – your own reading too, especially Kurrent and Latin. Vision models misread old hands often
  (`methodology.md` → HTR and LLM accuracy).
- **Don't infer.** Uncertain reading „(?)“; contradictions → `ISSUES`. Scribes erred (siblings' names, age, Anna/Františka).
- **Read the whole entry including margins** – later notes on death, marriage, legitimation are often the most valuable.
- **Families via house numbers**: same house number and craft across generations is a strong lead; look for whole households.
- **Verify place identification** by the estate/district in the entry (German „Langenau“ exists several times in Bohemia –
  „bey Hohenelbe“ = near Vrchlabí decides).
- **Officials, soldiers, doctors, teachers, priests**: try newspapers and yearbooks (`archives.md` → newspapers) – they
  show the career (where the person lived when) and give clippings for the web. Not worth it for village farmers and craftsmen.
- Registers before ~1650 barely exist. 17th century: soupis poddaných 1651, berní rula 1654, land books (`archives.md`).

### Disputed readings
- One you cannot decide (two readings, two candidates): show the user the crop, let them decide; `prob` until then.
- Many at once (dozens after a big round): don't ask one by one in chat. Collect them in `pracovni/sporna_cteni.json`
  (record key, crop, question, options, your suggestion) and build one page where the user clicks an answer for each –
  an artifact that stores answers if available, else an HTML file with a „stáhnout odpovědi“ button. Apply the answers,
  log them.

### Helpers (subagents)
For big rounds you may run 2–4 helpers on a cheaper model. Rules and a ready brief: `reference/helpers.md`.
Ask the user's approval first – they cost credits.

### When to stop a line
When registers go silent (illegitimate child without a father, burnt books, missing volumes) or you reach the 17th
century. Record it in `ISSUES`/`archivni_dotazy.md`, move to another branch. Keep „Rozpracováno“ in `data/STAV.md`
current so work can resume any time.

## Phase 3 – Fun facts and the main family story
Details: `reference/stories.md`. This is what the family reads most – don't leave it to the end.
- **Fun facts**: whenever you read something unusual, write it to `data/zajimavosti.md` at once and tell the user. After
  each round run `python3 -I tools/napady.py` (weddings shortly before births, a family in one house, cousin marriages,
  longevity, epidemics…); verify each in the entry before writing it.
- **„Příběh <main family>“** (e.g. „Příběh Novákových“): offer it once the main line has ~5 documented generations
  (`kontrola.py` reminds you). Draft **in chat** for approval, then `data/pribeh.md` and menu/button/intro in `web.json`.
  It is the centrepiece of the homepage.

## Phase 4 – Website
Details: `reference/web.md`.
1. `cd web && npm install` (once), then `npm run nahled` → http://localhost:4321, show the user. In the cloud (claude.ai)
   they can't see localhost – send page screenshots (headless browser) or deploy a preview.
2. Set `data/web.json`: name, intro, contact and the **login question and answer** (the site is family-only).
3. After every data change: `npm run data && npm run build` (or `npm run nahled` again). Fix `POZOR: nefunkční odkaz` at
   once (a person's URL changes when a birth year is added). After bigger rounds re-check `uvod` and
   `zajimavosti_na_uvodu` in `web.json` – they go stale („nejstarší předek je…“, generation counts).
4. Before publishing go through `reference/privacy.md` with the user (living people!). The web shows „Co hledáme“
   (`ISSUES`), person notes and the research log too. Resolve `CITLIVÉ` warnings from `kontrola.py` with the user; text
   they approve goes as a snippet into `web.json` → `citlive_schvaleno`.

## Phase 5 – Publishing (optional, only with consent)
Default: **Netlify** (free): `npm run nasadit`. Login and first site link are interactive – give them to the user as
`! npx netlify-cli login` etc. (`reference/web.md`). Advanced: own server (Docker/CapRover).
Publish nothing without the user's explicit consent.

## Feedback for the skill maintainer
Keep adding to `poznamky_pro_skill.md` (project root) what the skill should do better: tool/web bugs (command, what
happened, what you expected, workaround), new archives/portals and working procedures, tools you had to write, places
where these instructions were silent or wrong, what the user wanted but the skill couldn't do.
Write generally and briefly – technique only, no family data (no living people, nothing sensitive).
**Remind the user occasionally** (after publishing, at the end of a big round or session, when there is something
substantial; then roughly every few sessions – not constantly):
1. Remove or generalise personal details. 2. Print the notes in chat as one copyable block.
3. Ask the user to e-mail them to **sarsonj@gmail.com** (subject e.g. „Rodokmen skill – poznámky“) – the author folds
   useful changes in for everyone; they can add their own experience and wishes.
**Send nothing yourself** (no e-mail, form or issue). When the user says they sent it, write „odesláno <date>“ in the
file and continue below that line.

## Never
- Bulk-browse or bulk-download paid services (MyHeritage, Ancestry, Geni…) or bypass their protection – automated
  browsing of hundreds of records gets the **account blocked**. Read only what the user opens or single items you need.
- Enter passwords or payment details for the user, solve CAPTCHAs, buy subscriptions.
- Put details of living people on the web without their consent.
- Change data without a git backup; after a bigger step suggest a save (`git add -A && git commit -m "…"`).

## Project file map
| Path | Purpose |
|---|---|
| `data/STAV.md` | entry point: who it's for, rules, work in progress |
| `data/osoby.py` | ancestors (Kekulé numbers), families `LINES`, relatives `SIDE`, open questions `ISSUES`, `HYPOTEZY`, `SAME` |
| `data/zaznamy.json`, `data/svazky.json` | register entries and their volumes |
| `data/prepisy.json`, `data/zvyrazneni.json` | transcriptions with translations, frames of the read area |
| `data/research_log.md` | log of every search |
| `data/udaje_od_rodiny.md`, `data/archivni_dotazy.md` | what the family said; what only the archive/registry office has |
| `data/zajimavosti.md`, `data/pribeh.md`, `data/web.json`, `data/dokumenty.json`, `data/mista.json` | web texts and settings, family photos, map places |
| `vyrezy/`, `zdroje/` | entry crops (web / full resolution), name = record key + `.jpg` |
| `dokumenty/`, `pracovni/` | originals from the family; working files (never on the web) |
| `poznamky_pro_skill.md` | notes for the skill maintainer |
| `tools/` | copies of the skill tools (updated by re-running `novy_projekt.py`) |
| `web/` | website (Astro); data enters only via `npm run data` |

## Tools (`tools/`, run from the project root as `python3 -I tools/<tool>`)
| Tool | Does |
|---|---|
| `aron_search.py "Obec"` | register volumes in ARON (SOA Zámrsk); `"Obec,Fara"` narrows same-named villages |
| `aron_img.py <apu> info / <image> 1 pracovni/x.jpg / link <n> / svazek` | download image, permalink, record volume |
| `trebon.py misto / svazky / rejstrik / snimek / svazek` | SOA Třeboň (South Bohemia), no browser, indexes too |
| `portafontium.py fara / info / snimek / link / svazek` | SOA Plzeň (West Bohemia, Porta fontium), no browser |
| `ebadatelna.py hledat / matriky / info / snimek / link / svazek`, `a-…` | SOA Praha (Central Bohemia), registers and land books |
| `ahmp_search.sh "dotaz"`, `ahmp_scan.py XID image out.jpg` | Prague registers (AHMP) |
| `policie_prihlasky.py Příjmení [Jméno] [od] [do]`, `karta <n> out.jpg …` | Prague police registrations 1850–1914 |
| `soal_zoomify.py <path or URL> 0 out.jpg` | SOA Litoměřice / ZA Opava image (find the book in the browser; beware blocking) |
| `matricula_dl.sh`, `matricula_batch.sh` | Matricula images (Austria…), URL from the browser |
| `anno.py`, `anno_snap.py`, `kramerius.py` | newspapers: ANNO (Austrian), Kramerius (Czech) |
| `ged_import.py strom.ged "Jméno"` | ancestors from a GEDCOM + record queue |
| `ftb_export.py strom.ftb out.ged ["Jméno" year linie.md [Photos/]]` | MyHeritage backup → GEDCOM + direct line with fact origins |
| `overit.py <image> x0,y0,x1,y1 <prefix>` | entry split into two readable halves for verification |
| `vyrez.py <page.jpg> <key> x0,y0,x1,y1` | crop into `zdroje/` and `vyrezy/` |
| `ramecek.py <key> x0,y0,x1,y1` | frame of the read area + check image |
| `zapis.py <file> <key> <input.json> [--sloucit]` | add/change one item in `zaznamy`, `svazky`, `prepisy`… keeping the format |
| `mista.py pridej "Obec"` / `mista.py` | map places (OpenStreetMap coordinates) |
| `napady.py` | fun-fact ideas from the data |
| `kontrola.py` | data check; also flags sensitive text on the web and when to write the story |
| `export_web.py` | data → web (called by `npm run data`) |
| `fan_chart.py vystupy/` | printable fan-chart poster |

All coordinates are fractions 0–1 of the image width/height, never pixels.
