# Project data format

All data lives in `data/`. The website is generated from it (`npm run data`) – never hand-edit `web/src/data/site.json`.
- Add/change entries in JSON files via `python3 -I tools/zapis.py` (keeps the file format, see SKILL.md).
- After changes run `python3 -I tools/kontrola.py`.
- Filled-in sample data: `SKILL_DIR/template/ukazka/`.

## `osoby.py` – ancestors
Kekulé numbers: 1 = starting person; father of n = 2n, mother = 2n+1. Men have even numbers.
| number | who |
|---|---|
| 1 | starting person (woman: `sex="F"`) |
| 2, 3 | father, mother |
| 4–7 | grandparents (4, 5 father's parents; 6, 7 mother's) |
| 8–15 | great-grandparents = **8 lines** (`LINES`) |
| 16–31, 32–63 … | each generation doubles |

Example: father of ancestor 22 is 44, mother 45; child of 45 is 22 (45 // 2).

```python
P = {
 8: dict(n="Jan Novák", y="1890–1944", st="doc",
         f=["* 14. 2. 1890 Holice čp. 112", "kovář", "× 20. 6. 1919 Holice Josefa Pokorná", "† 1944 Holice"],
         r=["holice_krest_jan_novak_1890", "holice_snatek_jan_novak_josefa_pokorna_1919"],
         note="Proč nejistota / rozpor / odkud údaj"),
 9: dict(n="Josefa Pokorná", y="* 1895", st="doc", f=[...], r=[...]),
 33: dict(n="otec neuveden", st="unk", f=[], note="Nemanželský syn; otec v matrice není."),
}
```
| key | meaning |
|---|---|
| `n` | name (women by birth surname) |
| `y` | years for overviews: `"* 1895"`, `"1890–1944"`, `"asi 1720–1785"`, `"1680 – před 1740"`, `"† 1940"` (only death known); nothing known → `""` |
| `f` | facts: start with `*` (birth), `×` (marriage), `†` (death), or free text (trade, residence). The map finds place names in facts (`mista.json` → `re`). |
| `r` | record keys from `zaznamy.json` |
| `st` | status (details in `registers.md`): `doc` read in the register, `prob` probable, `fam` from the family (incl. a tree the family keeps itself, e.g. GEDCOM from their MyHeritage), `mh` someone else's online tree, `unk` |
| `note` | why uncertain / conflict / source of the claim |

Other structures in `osoby.py`:
- `LINES = {8: ("Novákové", "Kováři z Holic…"), …}` – numbers 8–15 only; line name = surname in plural.
- `SIDE` – siblings and other relatives: `dict(k=<ancestor number>, n="Josef Novák (1865–1920)", t="text", r=[keys])`.
- `ISSUES` – pairs `("Otázka", "Vyřešeno: … / Hypotéza: … / Otevřené: …")` → page „Co hledáme“ (resolved ones hidden).
  Goes on the website – no sensitive content (causes of death, family disputes) without consent (`privacy.md`).
- `HYPOTEZY` – `dict(rod=8, nadpis="…", text="…", polozky=[("jméno", "text")], zaznamy=[keys])` → the line's page.
- `SAME = {350: 140}` – ancestor appearing twice in the tree (alias → main number; the alias in `P` may have `f=["TÁŽ OSOBA jako 140"]`).

## `zaznamy.json` – records
```json
[{"klic": "holice_krest_jan_novak_1890", "svazek": "1a0b2337", "snimek": 57, "druh": "N",
  "datum": "14. 2. 1890", "nazev": "Křest Jana Nováka",
  "shrnuti": "Jan, syn Václava Nováka, kováře v Holicích čp. 112, a Barbory roz. Malé z Ostřetína.",
  "odkaz": "https://aron.vychodoceskearchivy.cz/apu/…/dao/…/file/…"}]
```
- `klic`: lowercase, no diacritics, `obec_druh_kdo_rok` (druh: krest, snatek, umrti, index, scitani…). Also the image file name.
- `druh`: N, O, Z, I (index), S (census), J (other source).
- `datum` in Czech form („14. 2. 1890“, „3/1867“, „1797“) – the website sorts by it.

## `svazky.json`
`{"1a0b2337": {"archiv": "SOA Zámrsk – Sbírka matrik…", "portal": "ARON", "svazek": "N Holice 1880–1900, sign. …",
"popis": "obce", "url": "odkaz na svazek"}}` – for ARON written by `aron_img.py <apu> svazek`.

## `prepisy.json`
`{"<klic>": {"orig": "[Datum] 14. února 1890\n[Dům] 112\n[Otec] …", "cs": "Český překlad celou větou…"}}`
- `orig`: faithful column-by-column transcription, `[Column name]` in square brackets, uncertain „(?)“, illegible „[…]“, keep abbreviations.
- `cs`: plain Czech translation for the family; explain old terms (domkář, podruh).

## `zvyrazneni.json`
`{"<klic>": {"boxes": [[x0, y0, x1, y1]], "status": "ok", "note": ""}}` – fractions of width/height of `vyrezy/<klic>.jpg`.
Written by `ramecek.py`. The box must enclose the whole record being read (not just the name).

## `mista.json`
`{"Holice": {"re": "Holic", "dotaz": "Holice, Česko", "lat": 50.066, "lon": 15.986}}` – managed by `mista.py`.
- `re` = regex searched in people's facts; use the word stem so inflection matches („Holic“ matches Holice and „v Holicích“).
- Watch for collisions: Hradec Králové × Nový Hradec Králové → `(?<!Nový )Hradec`; short names like `\bBuku?\b`.
- Hypothesis: `"hypoteza": true, "rod": 8`.
- OpenStreetMap can't find a place („Žár“, small hamlets) → pass the third parameter (query):
  `mista.py pridej "Žár" "Žár" "Žár, Zdíkov, okres Prachatice"`, or a query with a larger town/district.
- **Ambiguous names** (Podolí, Dub, Lhota): OpenStreetMap picks any village („Podolí, Česko“ = Podolí u Brna). Check the
  district in the last column of the output and query with a neighbouring village
  (`mista.py pridej "Podolí" "Podol" "Podolí, Odlochovice"`). The family knows local geography best – ask when unsure.

## `dokumenty.json` – family photos and documents on the website
`[{"img": "foto_svatba_1919", "popis": "Svatba Jana a Josefy, Holice 1919.", "osoby": [8, 9]}]`
Save the image via `vyrez.py dokumenty/<sken>.jpg foto_svatba_1919` (creates both `vyrezy/` and `zdroje/`).

## `web.json` – website settings
See `web.md`.

## Markdown notes
- `research_log.md` – table `| # | datum | zdroj | rozsah | hledáno | výsledek | citace |`; append new rows at the end,
  negative results too. Shown on the web as „Výzkumný deník“.
- `zajimavosti.md` – `## Nadpis` + text, links `[Jméno](/osoba/<slug>/)`. How to write: `stories.md`.
- `pribeh.md` – „Příběh <main line>“: frontmatter `title`, `lead` + `##` chapters; if the file doesn't exist the menu
  item is hidden. How to write: `stories.md`.
- `archivni_dotazy.md`, `udaje_od_rodiny.md` – free text.
- Notes for Claude inside markdown: write as `<!-- … -->` – they don't go on the web.

## Person links (slug)
Slug = name without diacritics + year, e.g. `jan-novak-1890`; on collision the person number is appended. Exact slugs after
`npm run data` in `web/src/data/site.json` (`people`). Renaming a person changes the slug – fix links in texts.
