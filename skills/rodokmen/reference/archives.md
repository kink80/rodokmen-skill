# Archive portals and how to get images from them

Which archive holds which region's matriky (parish registers), the tool for each portal, its quirks and limits,
plus non-register sources (police registrations, newspapers, maps, graves, WWII, DNA).

Registers are online at eight state archives – pick by **where the ancestors lived** (today's kraje (regions) roughly match):

| Archive | Region | Register portal | Tested with the skill |
|---|---|---|---|
| SOA Zámrsk | East Bohemia (Hradec Králové + Pardubice regions) | ARON – aron.vychodoceskearchivy.cz | **yes – ready tools** |
| SOA Praha | Central Bohemia | eBadatelna – ebadatelna.soapraha.cz | **yes – `ebadatelna.py`** (also urbáře and gruntovní knihy) |
| SOA Plzeň | West Bohemia | Porta fontium – portafontium.eu | **yes – `portafontium.py`** |
| SOA Třeboň | South Bohemia | digi.ceskearchivy.cz | **yes – `trebon.py`** |
| SOA Litoměřice | North Bohemia | Digitální badatelna – digi.soalitomerice.cz | **partly – `soal_zoomify.py`** (find the book in a browser) |
| AHMP | City of Prague | katalog.ahmp.cz (Pragapublica) | **yes – `ahmp_search.sh`, `ahmp_scan.py`** |
| MZA Brno | South Moravia, part of Vysočina | Acta Publica – www.mza.cz/actapublica | partly – browser only (see below) |
| ZA Opava | North Moravia and Silesia, Olomouc and Litovel areas | Digitální badatelna – digi.archives.cz/da/ | **partly – `soal_zoomify.py`** (browser required, user solves CAPTCHA) |
| Národní archiv (National Archives) | Jewish registers (HBMa), census lists; Prague police registrations 1850–1914 | badatelna.eu; digi.nacr.cz/prihlasky2 | registrations **yes – `policie_prihlasky.py`** |

Verify URLs of untested portals on first use (portals change). Border regions and outside CZ: methodology.md → Other territories
(Matricula for Austria/Germany, Geneteka for Poland, FamilySearch for Slovakia) and below.

General rules for every portal:
- Be kind to servers: download only images you actually read; never bulk-download whole volumes.
- Rate-limit warnings below (Litoměřice, Opava, FamilySearch, Geneteka) must be copied into helper briefs (helpers.md).
- Faded/microfilm images: read a contrast-boosted copy
  `magick x.jpg -colorspace Gray -contrast-stretch 2%x30% x_k.jpg`; crop from that copy and note it in the transcription.
- Anything only available in the reading room (badatelna): record in `data/archivni_dotazy.md` (archive, fonds,
  inventory number, what to look for).

## ARON (SOA Zámrsk) – ready tools
- **Find volumes:** `python3 -I tools/aron_search.py "Holice"` → lines `apu_id | signatura, years | type, villages`.
  Description contains book type (NAROZENÝCH / ODDANÝCH / ZEMŘELÝCH = births/marriages/deaths, index) and all villages of the farnost (parish).
- **Volume info:** `python3 -I tools/aron_img.py <apu_id> info` → title and image count.
- **Download image:** `python3 -I tools/aron_img.py <apu_id> <snímek> 1 pracovni/holice_n_57.jpg`
  (level 1 = half resolution, for orientation; 0 = full, for reading and cropping).
  Part of a page at full res: `… <snímek> 0 pracovni/detail.jpg 0.0,0.4,1.0,0.7`.
- **Image link** (field `odkaz` in `zaznamy.json`): `python3 -I tools/aron_img.py <apu_id> link <snímek>`.
- **Volume into data:** `python3 -I tools/aron_img.py <apu_id> svazek` (key = first 8 chars of apu_id → field `svazek` in the record).
- ARON also has other fonds (census, some gruntovní knihy (land books), chronicles): `aron_search.py "Obec" "název fondu"`.
- Same-name villages are common (Vinice u Hořic × Vinice u Velké Jesenice). Results are sorted by parish; narrow with
  comma-separated words that must all appear in the description: `aron_search.py "Vinice,Hořice"` (or search a neighbouring/parish village).
- For old parishes look first for **summary indexes** (description „index NAROZENÝCH…“ spanning many years) – often back to the 17th century.
- Village not found? Try the name **without qualifier** („Lhota“ not „Horní Lhota“) and the old name. Pre-~1784 registers
  are often in the neighbouring town's parish; parish indexes have per-village sections („Copulati Gross Lhota“).

## SOA Třeboň (digi.ceskearchivy.cz) – `tools/trebon.py`, no browser
- `trebon.py misto "Obec"` → place id. Tool strips diacritics and spaces (portal only knows „RatiborskeHory“).
  Portal always returns exactly one place – **check the printed name and district**: for same-name villages it may return
  another (Buk → Bukovka), for towns a district or synagogue (Jindřichův Hradec → district 1949–1960). Then find the
  correct id on the portal and use it. Small hamlets aren't places at all (Malá Chyška) – search the parish or neighbouring
  village (Chyšky) and look for the hamlet in book descriptions.
- Occasional HTTP 500 = portal outage; tool retries. If it persists, try later.
- `trebon.py svazky <id|"Obec">` → books of the place (book id, type, years, villages).
- `trebon.py rejstrik <id_knihy> <písmeno>` → **volunteer name index** of the book with image numbers – huge time saver.
  Not every book has one; empty result = no index, not "person absent".
- `trebon.py snimek <id_knihy> <snímek> pracovni/x.jpg [nahled]`, `trebon.py svazek <id_knihy>` (into `svazky.json`).
- Permalink: `https://digi.ceskearchivy.cz/<id_knihy>/<snímek>`.
- Some parishes keep per-village books not listed under the place (Vacov → Žár) – search by the parish's book titles.
- **Civil registers (1920–1945) of people without religion** are in district office books (e.g. Tábor, book 12304)
  with a volunteer index (`trebon.py rejstrik 12304 N`). Good route when the parish register is silent. They don't show
  in `svazky "Obec"` (they belong to the district) – find the book id on the portal.

## SOA Plzeň (Porta fontium, portafontium.eu) – `tools/portafontium.py`, no browser
- `portafontium.py fara <fara>` → parish books: kniha | villages | years and types (`*` births, `oo` marriages, `+` deaths, `i` index) | description.
  `<fara>` = Roman Catholic parish name („Dolní Bělá“, „Mýto“ – tool strips diacritics) or its URL slug (`dolni-bela` =
  `…/contents/register/soap-pn/cirkev-rimskokatolicka/dolni-bela`).
- Tool does not map village → parish. Find the parish via the portal's register search (field Místo, in browser) or in
  neighbouring parishes' book descriptions. Without a browser: `https://www.portafontium.eu/searching/register?search_api_views_fulltext_1=<obec>`
  lists links `/register/soap-pn/<kniha>` – use `<kniha>` (e.g. `kdyne-29`) directly in `portafontium.py info`.
- `portafontium.py info <kniha>` (`dolni-bela-17`) → signatura, type, cover text and **contents** – for books written per
  village it lists which folios hold which village („Ves X z let 1862–1901 = fol. 121–200“).
- `portafontium.py snimek <kniha> <snímek> pracovni/x.jpg [nahled] [x0,y0,x1,y1]` → whole page at full res in one
  request (IIIF); `nahled` = 1/4. Image number = thumbnail order in the viewer; files carry the folio number
  (`dolni-bela-17_0951` = fol. 95), so in continuously foliated books **image ≈ folio**.
- `link <kniha> <snímek>` → permalink; `svazek <kniha>` → `svazky.json` (key `pf_<kniha>`).
- Without a browser User-Agent the portal returns 403 (tool sends one). No name search – indexes exist only inside books
  (`*i`, `ooi`, `+i` in the parish book list).

## SOA Praha (eBadatelna, ebadatelna.soapraha.cz) – `tools/ebadatelna.py`, no browser
- `ebadatelna.py hledat "Obec"` → full locality names („Lhota - obec X - okres Y“); `matriky "Obec"` (or full name)
  → matrikaId | kniha | type | years | parish | image count. Lists options for same-name villages.
- `ebadatelna.py info <matrikaId>` → book contents incl. **page layout per village** (e.g. „1–48 Ves A, 49–82 Ves B,
  1–30 Ves C …“ – each village may have its own pagination). For such books don't estimate the image from the year;
  use that village's pages (page number at top of image). They are ordered by village, not chronologically – if there is
  an index, start with it.
- `ebadatelna.py snimek <matrikaId> <snímek> pracovni/x.jpg [nahled] [x0,y0,x1,y1]`; `link`, `svazek` (key `ebad_<id>`).
- **Archiválie** (archival items: urbáře (urbaria), gruntovní and pozemkové knihy (land books) of estates – bridge across the 17th century):
  `a-info <archivalieId>`, `a-snimek`, `a-link`, `a-svazek` (key `ebad_a_<id>`). Tool doesn't search them – find on the
  portal in a browser; id is in the URL `…/pages/ArchivaliePage/archivalieId/<id>`.
- Older images are often microfilm or heavily faded – use the contrast command above.
- Portal always serves the whole image (no tiles); tool makes preview/crop locally with ImageMagick (`magick`).
  Daily maintenance around 4:44 (~2 minutes).

## AHMP Praha – `tools/ahmp_search.sh`, `tools/ahmp_scan.py`
- `bash tools/ahmp_search.sh "Smíchov"` → volumes of the register collection: description | parish | (XID, id, image count).
- `python3 -I tools/ahmp_scan.py <XID> <snímek> pracovni/x.jpg [1] [x0,y0,x1,y1]` → image (1 = half resolution).
- Permalink `https://katalog.ahmp.cz/pragapublica/permalink?xid=<XID>&scan=<N>`.
- New portal digi.ahmp.cz has bot protection – don't use it.
- Prague registers 1890–1940 mostly **have no indexes** and there are many parishes – without exact date and parish
  (from a death/marriage record or the family) searching is very expensive. Try police registrations (below) first.

## SOA Litoměřice (North Bohemia) – `tools/soal_zoomify.py`
- Digitální badatelna digi.soalitomerice.cz has an automatic entry check (~15 s "computing") → find book and image in a
  browser, then in network traffic find `…/zoomify/cz/archives/CZ-214000010/matriky/<cesta>/ImageProperties.xml`.
- Download with the tool: `python3 -I tools/soal_zoomify.py <cesta> 0 pracovni/x.jpg` (0 = full, 1 = half resolution).
  Image server has no rate limit and needs no cookies.
- **WARNING: after ~100 fast requests to application pages (image viewer, result detail) the archive blocks access for
  30 minutes.** Use the browser only to locate the book and image path, then only the tool.

## ZA Opava (North Moravia, Silesia, Olomouc, Litovel) – `tools/soal_zoomify.py`
- Border trap: villages around Litovel and Olomouc belong to **ZA Opava**, not MZA Brno.
- **Impossible without a browser.** `https://digi.archives.cz/da/` (old matriky.archives.cz just redirects) has bot
  protection – curl and WebFetch can't even reach search. Needs a connected browser (Claude in Chrome – user connects it
  with `/chrome` – or Playwright); otherwise the user finds the book and saves images.
  Entry check passes by itself, but search often ends in a **CAPTCHA: the user solves it** in the browser window, never you.
- Search the village, filter Matriky (icon, then the magnifier again). Results include civil registers (hundreds of
  volumes per village) – filter „Datum od – do“ (by volume start, e.g. 1680–1873) to keep only church ones. Books are
  often ordered by village with an index at the end. Next to parish books there are often **druhopisy** (duplicate
  copies; „T“ in signatura, see registers.md).
- **Save the volume list and image IDs immediately** to `pracovni/<svazek>/` – the session expires after a few hours and
  search results live only in it. On the volume detail page: entity
  `document.querySelector('[entityuuid]').getAttribute('entityuuid')` (for volumes stored in another archive it is not
  UUID-shaped – don't regex for it); image IDs are the values of `select.getScanSelect option` (form „N%20uuid“).
  Open an image any time without searching:
  `/da/showZoomifySeparate?imageNumberSeparate=N&myEntityUuid=<entita>&myImageUuid=<uuid>`.
- Image URL: `fetch('/da/showZoomify/<volume id from URL>/<part of option.value after „%20“>')` returns HTML with the
  path `…/images/<folder>/<uuid>.jpg`. Paths differ per volume (`…/CZ-217000010/NAD-165/dao/images/…`,
  `…/zao/matriky/dao/images/…`) and the extension may be `.JPG` – always pass the **full URL from the HTML**:
  `python3 -I tools/soal_zoomify.py "<adresa>" 2 pracovni/x.jpg` (2 = quarter res, to orient by page headers;
  0 = full only for the found record). Image server images.archives.cz has no rate limit and needs no cookies.
- **WARNING – blocking** (application pages only, not images): fast queries → CAPTCHA, „operace vyhodnoceny jako
  nestandardní“ or „Access blocked“ for 30 minutes; after a few dozen queries per day showZoomify returns „Server
  Exception“ (daily limit – continue next day). Rules: 3–6 s pause between pages, batches up to ~80 pages, no fast
  `fetch` right after a search, don't page through result pages in a row. Run longer loops in the browser in the
  background, store results in `window` or `localStorage` and collect them in parts (the JavaScript tool times out at ~45 s).

## MZA Brno (South Moravia, Acta Publica) – no tool
- Portal `https://www.mza.cz/actapublica/`, no login. actapublica.eu is often blocked by Cloudflare („Sorry, you have
  been blocked“ – per IP, even in a normal browser); if mza.cz blocks too, ask the user to turn off VPN or try another network.
- Find the book in a browser by village or parish. Images are IIIF: find the image URL in network traffic and download
  with `curl` (Python gets HTTP 426). Then follow "Other portals" below.

## Prague police registrations 1850–1914 (Národní archiv) – `tools/policie_prihlasky.py`
- Cards of Prague families: head of family, wife with birth year and place, children with years, addresses, sometimes
  marriage place. Excellent when the family lived in Prague and the parish is unknown (Prague registers lack indexes).
- `python3 -I tools/policie_prihlasky.py Kolar [Jan] [1850] [1870]` – surname without diacritics (prefix match), given
  name and birth years optional. Without a browser only the first 20 results come back – search as narrowly as possible.
  Download a card: `policie_prihlasky.py karta <pořadí> pracovni/karta.jpg Kolar [Jan] [1850] [1870]` (same search;
  rank above 20 works too) – prints citation and permalink (`index.php?action=link&ref=czarch:CZ-100000010:874&karton=K&folium=F`).
- **Does not cover Smíchov, Vinohrady, Žižkov, Karlín and other suburbs before 1922** (separate towns then). Cards don't state religion.

## Matricula (Austria, Bavaria, Poland…; data.matricula-online.eu) – `tools/matricula_*.sh`
- No login. Book list: `…/de/oesterreich/<diecéze>/<farnost>/?page=N`; image `…/<id>/?pg=<snímek>` (also the permalink).
- Image URL contains `csrf` and `ctrl` computed by JavaScript → needs a browser (Playwright / Claude in Chrome): open the
  book, click a page (`.docview-pagelink`), take the image URL from `performance.getEntriesByType('resource')` and run
  `bash tools/matricula_dl.sh "<url>" pracovni/x.jpg`. Several pages: file of lines „<snímek> <url>“ → `matricula_batch.sh`.
- Index without a browser: the book page HTML has a `labels` list with image captions („04-Index Taufe_0001“).
- Military registers (e.g. Graz) are not there – they are in the Kriegsarchiv in Vienna (offline).

## Poland – Geneteka (geneteka.genealodzy.pl)
- Free volunteer indexes of baptisms, marriages, deaths by parish; use the web UI (voivodeship → parish → surname).
  The index is a lead – scans are often not online (parish or diocesan archive); request the record there.
- **Parish membership changed**: a village could belong to a neighbouring parish (Lipowa → Radziechowy until 1896) – search there too.
- No bulk downloads: PTG states „linkujemy, nie pobieramy danych“ ("we link, we don't harvest") and after dozens of fast
  queries the server returns unfiltered data. Batch queries with pauses and verify results.
- Single query without a browser:
  `https://geneteka.genealodzy.pl/api/getAct.php?draw=1&start=0&length=10&bdm=B&rid=B&w=12sl&search_lastname=<příjmení>&search_name=&search_lastname2=&search_name2=&from_date=1850&to_date=1900&exac=1`
  (`bdm`/`rid` B baptisms, S marriages, D deaths; `w` voivodeship, e.g. 12sl Silesian, 08op Opole) → JSON.
  Trap: `recordsTotal` > 0 with empty `data` – try surname only without given name and a narrower year range. Pagination repeats rows.
- Some Polish parishes' scans are also on Matricula (above) or FamilySearch (below).

## FamilySearch (via the user's logged-in browser)
- Login needed even for index search. Czech registers are barely indexed (Slovakia is, mainly baptisms).
- Use only when the user has it open and offers it. From the page context call the JSON API
  `https://www.familysearch.org/service/search/hr/v2/personas?q.surname=…&q.givenName=…&q.fatherGivenName=…&count=20`
  (`fetch(…, {credentials:'include'})`); results in `entries[].content.gedcomx`.
- Useful for register indexes and graves (BillionGraves, Find a Grave). Result is a lead – verify in the register.
- Don't change the user's open tabs (restore the original URL after searching). Never bypass Cloudflare pages – ask the
  user to open them and send a screenshot.
- Film images (DGS): list via `POST /search/filmdatainfo/image-data` with `{"type":"film-data","args":{"dgsNum":"00…","state":{"i":"0"},"locale":"cs"}}`
  (`images[]` = ARK). Image: `https://www.familysearch.org/service/records/storage/deepzoomcloud/dz/v1/<ARK 3:1:…>/$dist`
  (full res), small thumbnails `…/image_files/8/0_0.jpg`. **After ~1000 fast requests: HTTP 429** and a block for tens
  of minutes – fetch only specific images, a few per second at most, never crawl whole books.

## Graves and cemeteries
- Burial places come from the family interview (Phase 1, SKILL.md). Headstone photos (names, dates, maiden names) are a
  valuable source mainly for the 20th century, where registers are not yet accessible.
- Grave registers: cemetery administration (who is buried, who pays the lease – often a living descendant); many towns
  have online grave search. Internationally BillionGraves and Find a Grave.

## Newspapers and yearbooks – `tools/anno.py`, `tools/anno_snap.py`, `tools/kramerius.py`
**Worth it for:** state and manorial officials, judges, teachers, priests, officers, military doctors, notaries,
postmasters, mayors, entrepreneurs – papers printed appointments, transfers, decorations, pensions, bankruptcies,
marriage announcements and parte (obituary notices), roughly from 1860. This tells **where the person lived when** →
where to look for children's baptisms, marriage, death; also when a person is "missing" from expected registers (moved
with the job). **Rarely:** village farmers and craftsmen.
- ANNO (Austrian newspapers 1700–1950, incl. Prager Zeitung, Wiener Zeitung):
  `python3 -I tools/anno.py '"Johann Nowak"' 1860 1915 [stran] [--snip]` – names **in German** („Johann Nowak“,
  „Wenzel Dworschak“). Crop the hit: `tools/anno_snap.py <docId> "<dotaz>" pracovni/x.jpg`.
  ALEX and annoshow are behind Cloudflare – don't bypass.
- Kramerius NK ČR (Czech newspapers, yearbooks, directories): `python3 -I tools/kramerius.py 'text_ocr:"Jan Novák" AND datum_begin:[1900 TO 1910]'`.
- Military schematisms: full text on archive.org (e.g. `militrschematis13kriegoog` = 1850).
- **Risks:** names repeat and full text is noisy („Winter“ also = winter, „Novák“ also = newcomer). Identify only when
  several facts match (residence, job consistent with the register, chronology). Source `druh` is „J“ (other source) –
  a lead and story material, not a substitute for a register.
- Clippings are great for the website: save to `dokumenty/noviny/`, run through `vyrez.py`, add to `dokumenty.json`.
- Order: registers first; newspapers when the trail breaks because of moving, or for officials/soldiers when we want the story.

## Other portals – general procedure
No ready tools. Do this and record the result in `STAV.md` so you know next time:
1. Open the portal in a browser (Claude in Chrome / Playwright if available; else ask the user). Find the fonds
   „Sbírka matrik“ / „Matriky“ and the volume by village and years.
2. **Image:** most viewers have a download button – save to `pracovni/`. If not, check network traffic for where
   tiles/images load from (IIIF, DeepZoom, Zoomify) and fetch the whole image that way – one at a time, only images you
   read. Last resort: ask the user to save the image into `pracovni/`.
3. **Permalink** to the image („trvalý odkaz“/permalink button, else volume URL + image number) goes into `odkaz` in the record.
4. Add the volume to `data/svazky.json` by hand: `"klic": {"archiv": "SOA Třeboň", "portal": "digi.ceskearchivy.cz",
   "svazek": "N Volary 1784–1820, sign. …", "popis": "obce…", "url": "<odkaz na svazek>"}`.
5. Continue as with ARON: `vyrez.py`, `ramecek.py`, transcription, persons, log.
6. Once the procedure works, write a small tool for the portal modelled on `trebon.py` (browserless if possible),
   record the procedure in `STAV.md` and the finding in `poznamky_pro_skill.md` (for the skill maintainer).

## Older sources (17th–18th century)
- **Soupis poddaných podle víry 1651** (list of subjects by faith) – whole families with ages, by kraj (region); National
  Archives edition as PDF (nacr.cz, search „Soupis poddaných podle víry“ + region). PDF full text; surnames often without
  diacritics / different spelling (Šaršoun → „Šaroun“, long ſ read as f). Villages are listed within the panství
  (manor/estate); note the edition page.
- **Berní rula 1654** (tax roll) – only landholders; editions and indexes in libraries (Kramerius/DNNT).
- **Gruntovní (pozemkové) knihy** (land books) – house transfers father → son; only partly online (SOA Plzeň almost all,
  SOA Praha some, others mostly reading room). Fonds „Velkostatek <panství>“.
- **Stabilní katastr maps (1824–43)** – where the house with a given čp. (house number) stood: archivnimapy.cuzk.cz (ČÚZK archive).
  **Císařské otisky** (imperial copies) partly lack colours and house numbers. Better: **indikační skici** (indication
  sketches) – house numbers and owner names on plots („N. 13“ = belongs to house čp. 13). House without plots outside
  the village = domkář (cottager) without fields. The owner of the house itself is usually only in the písemný operát
  (written records; archive, not online). Map crops belong on the website with the family.
- **Today's address** by čp.: RÚIAN (ČÚZK, layer AdresniMisto). Careful: old town centres often kept their čp. since the
  19th century, but suburbs and villages renumbered. Good for a „procházka po stopách předků“ (walk in the ancestors'
  footsteps). nahlizenidokn.cuzk.gov.cz also finds the building by village + čp. (to verify an address from a document);
  the owner is behind a CAPTCHA and is data about a living person – don't look it up.

## WWII victims
- **Arolsen Archives** (collections.arolsen-archives.org): documents on prisoners, forced labourers, displaced persons.
  Search in a browser; the user must accept the entry declaration.
- **Mauthausen – Raum der Namen** (raumdernamen.mauthausen-memorial.org): prisoners who died in Mauthausen, with birth
  and death date and place. Sensitive – on the website only with the family's consent (privacy.md).

## Surnames today and DNA
- **Surname distribution today**: kdejsme.cz (bearer counts per district). Quickly shows whether a surname is rare and
  where its "nests" are. When looking for distant relatives, exclude the user's own family.
- **DNA** (only if the user wants it – their decision): autosomal test (MyHeritage, Ancestry) finds cousins to ~5th
  degree. **Y-DNA** (FamilyTreeDNA, Y-37 to Big Y) passes father-to-son unchanged: verifies whether two lines of the same
  surname share an ancestor; for an illegitimate child a match with another surname can point to the father.
  Contact with living relatives always only through the user.
