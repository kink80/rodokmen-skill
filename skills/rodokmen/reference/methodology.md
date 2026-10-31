<!-- Digest of an AI-assisted research report "Professional Genealogy in the Czech Lands & Central Europe —
Operating Playbook", dated 2026-10-09. Author unknown. Not covered by the repo's MIT licence. If you are the author
and object, contact sarsonj@gmail.com. Verify URLs, periods and prices before relying on them. -->

# Methodology – sources, access, linkage, brick walls

1. Sources: per territory, search-mode map, timeline, access periods, military
2. Method: workflow, linkage, disambiguation, failure modes, proof, HTR/LLM accuracy
3. Brick walls: substitutes in priority order

Czech portals and tools: reference/archives.md. Reading registers, record types, palaeography, glossary: reference/registers.md.

## 1 Sources

Images are online; name search mostly is not. Use indexes to find entries, then read the image. A negative index result proves little.

### Czech lands

| Record class | Years | Holder | Online |
|---|---|---|---|
| Matriky (parish registers; Protestant after 1781) | c. 1600s–1949 | 8 regional archives, fond "Sbírka matrik" | images |
| Jewish registers | 1784–1949 | Národní archiv (NA) | images |
| Gruntovní knihy (estate land books), urbáře, soupisy poddaných | 15th c.–1850 | SOA, fond "Velkostatek X" | SOA Plzeň Porta fontium: practically all; SOA Praha eBadatelna: ~900 vols of 26 estates, rest reading room; ZA Opava; SOA Třeboň (unverified: complete); SOA Hradec Králové in progress; MZA via ARON (unverified) |
| Court land books | 1871–1950/64 | katastrální úřad / archive | offline |
| Soupis poddaných podle víry (subjects by faith) | 1651 | NA | edition PDFs per kraj, no name index |
| Berní rula (tax roll) | 1654 | NA | printed surname index (Červený 2003) |
| Lánové rejstříky 1669–79 (Moravia); Tereziánský katastr 1713–57; Josefský katastr 1785–89 | – | NA / MZA | unverified |
| Censuses | 1857–1910, 1921, 1930 | SOkA / MZA / AMB | partial images |
| Police registrations, Prague | 1850–1914/18 | NA | name index |
| Stabilní katastr indicative sketches | 1824–43 | ČÚZK | images |
| Kmenové listy (service sheets) | cohorts 1865–1910 | VÚA–VHA Praha | request |
| Pozůstalostní spisy (probate), sirotčí knihy (orphan books), svatební smlouvy (marriage contracts) | 17th c.–1950 | SOkA (okresní soud), estate fonds | mostly offline |
| Kroniky (chronicles) | 19th–20th c. | SOkA | partly |
| Newspapers | 1800s– | Kramerius, ANNO (AT) | full text, OCR varies |

Berní rula lists only taxpayers with property – no podruzi (lodgers), no čeleď (servants). The 1651 list gives whole families with ages but is incomplete.

### Other territories

| | Key sources | Notes |
|---|---|---|
| SK | Cirkevné matriky 1592–1895 in Štátne archívy; FamilySearch "Slovakia, Church and Synagogue Books, 1592-1935" | partial index, mainly baptisms, "incomplete". Civil registers from 1895-10-01 at matričný úrad. Use Hungarian place names (Helységnévtár). |
| AT | Matricula (data.matricula-online.eu): Catholic Altmatriken (pre-1939) of all dioceses, Protestant registers; Burgenland: matriken.at | images, no index, free. Vienna Meldeunterlagen ~1904–1975 at WStLA: written request, €35 per started half hour even if nothing found; 19th-c. Meldezettel largely destroyed (FamilySearch "Wiener Meldezettel 1850–1896" – unresolved). |
| PL | Metryki in diocesan archives / AP; USC civil books → AP; Geneteka (geneteka.genealodzy.pl) | Geneteka very uneven (weak in Lower Silesia). Link, do not scrape. |
| DE | Standesamt from 1876; Heimatortskartei Sudetendeutsche (HOK Regensburg), Lastenausgleich, Gemeindeseelenlisten at Bundesarchiv Bayreuth (B 530) | HOK: residence 1939-09-01 + postwar address; written request. |
| HU | State civil registers from 1895-10-01, duplicates to 1980 at MNL county archives | church archives set own periods. |

### Search-mode map, Czech lands

N = name index, I = images online, O = offline, – = none.

| Class | 1650–1780 | 1780–1850 | 1850–1900 | 1900–1950 |
|---|---|---|---|---|
| Matriky | I | I | I | I to closure, then O |
| Estate land books | I (Plzeň, Opava, Třeboň), else O | same | – | – |
| Court land books | – | – | O (from 1871) | O |
| 1651 list / 1654 roll | N (printed) | – | – | – |
| Censuses | – | – | I partial | I/O |
| Police registrations | – | – | N (Prague) | N to 1914/18, then O |
| Military | O (Musterlisten) | O (GB Vienna) | O (VHA) | 1887–98 cohorts lost |
| SK church books | I | I / partial N | I / partial N | O |
| AT Matriken | I | I | I | I to 1938, then O |

### Timeline

- 1563 Trent orders registers; in Bohemia few survive before c. 1620–1650 (Slovakia from 1587).
- 1651 Soupis poddaných; 1654 Berní rula; 1740–1820 Musterlisten.
- 1770/71 house numbering – čp. becomes a linkage key; watch renumbering.
- 1781 Toleranzpatent; non-Catholics still often entered by the Catholic priest to 1849.
- 1784 Josephine patent: separate N/O/Z (birth/marriage/death) books with columns; parishes reorganised 1784–90 (lokálie).
- 1787 Jewish surname adoption. 1820 Grundbuchblätter replace Musterlisten.
- 1850 estates end; land/orphan/probate records move to soudní okres (court district) fonds. 1871 court land books.
- 1895 Hungarian (incl. SK) civil registration. 1939 Austrian Standesamt. 1950 state matriky in Czechoslovakia (unverified).

### Access periods (years: births / marriages / deaths)

| | Law | B / M / D | Counted from |
|---|---|---|---|
| CZ | zákon 301/2000 Sb. | 100 / 75 / 30 | **last entry in the volume**; earlier: matriční úřad, close kin with proof |
| SK | zákon 154/1994 Z. z. § 5(3) | 100 (transfer, all three) | last entry; earlier: matričný úrad |
| AT | PStG 2013 § 52(5) | 100 / 75 / 30, if no living person concerned | entry (statute) vs last processing (Diözesanarchiv Wien) – conflict |
| DE | PStG § 5(5) | 110 / 80 / 30 | register year |
| PL | Prawo o aktach stanu cywilnego 2014 | 100 / 80 / 80 | closing of the book; transfer to AP within 2 years |
| HU | 2010. évi I. tv. 93/A (njt.hu) | 110 / 86 / 30 | year after creation; MNL pages quoting older rules are outdated |

- A Czech book 1890–1935 stays closed until 2035, even for 1890 entries: order an extract from the matriční úřad or find an earlier-closing duplicate/index volume.
- SK: a 1960 death is still at the registry office (open in CZ/AT/DE/HU).
- Austrian church books after 1938 carry no right of inspection – use the Standesamt.

### Military

- Grundbuchblätter (service sheets, 1820–1918) of Czech-domiciled men, cohorts 1865–1900, stayed in successor states; most destroyed. Cohorts 1865–1886 survive (VHA Praha), earlier fragmentary, 1887–1898 largely shredded. Cohorts ~1820–1864 at ÖStA/KA Vienna, partly on FamilySearch.
- WWI generation: Verlustlisten (casualty lists), Kriegsarchiv loss card indexes (~40 million cards, written request), parish records.
- Musterlisten (1740–1820) and military matriky (1633–1938, 100-year block) are searched by regiment – find it first via Wrede's recruitment-district table. Catalogue: archivinformationssystem.at. Files are >90 % German; Kurrent required.

## 2 Method

### Workflow

1. One person, one event, one hypothesis.
2. Locality guide first: obec → fara/lokálie (before/after 1784–90) → panství (estate, to 1850) → after 1850 soudní okres → archive, fond, book with years. Note German/Hungarian/Polish names, renumbering, regiment. Gazetteers: Retrospektivní / Historický lexikon obcí, oefr.at, Helységnévtár. Use the parish as of the event year.
3. List every record class for that place and period with search mode and access.
4. Extract every field: witnesses, godparents, house number, midwife, marginal notes.
5. Read the whole book for surname and house number, not index hits; extract whole households.
6. Correlate on a timeline: house × person × event × status term; test conflicts.
7. Proof argument for any conclusion resting on indirect evidence.
8. Log every search with scope. "Not in Z book 1784–1820, every entry read" is evidence; "not in index" is not.
9. Write to the tree only with an image-level citation (archive, fond, book, folio, image, permalink, date); conflicts as notes (reference/data.md).

### Linkage techniques

- **House number** – same čp. = same holding; check renumbering against the indicative sketch.
- **House name (vulgo, jméno po chalupě)** – survives change of holder, may pass to a son-in-law; not a surname.
- **FAN clusters** – recurring godparent couples mark one family; godparents were often fixed kmotrovské families, not kin.
- **Status terms** – sedlák → výměnkář (retired holder) in the same house = farmer retired; domkář ≠ sedlák of same name.
- **Land transfer** – "Václav prodává synu Janovi … dědické díly bratru Matějovi, sestře Anně" names parent, child, siblings: direct filiation. Payouts were often entered decades later; date each.
- **Orphan books** – orphans' ages → birth windows. **Marriage contracts** – name the bride's father and village. **Probate after 1850** – names heirs elsewhere.
- **Age at death** – tolerance ±5 years for ages ≥60, ±2 for children; old ages are inflated.
- **Naming** – first son after a grandfather; a reused name means an earlier sibling died. Weak alone.
- **Name forms** – -ová / -owa; Joannes/Ioannes; Hungarian surname-first; Kováč → Kovács, Šimek → Schimek, Bytča = Nagybiccse. Search all forms.
- **Dates** – feast-day dating ("feria 2da post Dominicam Laetare" = Monday after 4th Sunday of Lent): perpetual calendar. 7bris–Xbris = Sep–Dec.
- **Marginal legitimation** – "legitimiert durch nachfolgende Ehe" identifies the father; may sit in another book.
- **Domovská příslušnost / Heimatrecht** (right of domicile) leads to the home parish; ≠ birthplace.

### Disambiguation

Merge same-name persons only on **two independent identifiers** (house number, both parents, spouse, godparent cluster, exact birth date), each from a different record and informant.
- Same-name cousins: house number alone fails; require parents plus spouse.
- Remarried widows: maiden name plus her father's name in both marriage entries.
- Step-families: a výměnek beneficiary may be a stepparent; the land book says.
- Foundlings: foundling-hospital number plus wet-nurse's parish. Soldiers: regiment plus Heimatrecht.

### Failure modes (cue → fix)

- Children 10 months apart to "one" couple → two men merged; check both marriages.
- Illegitimate child's father assumed → marginal note, later marriage, court file.
- Index or transcript cited as the record → cite the image.
- Village missing from expected book → parish reorganisation; locality guide.
- Same family, different čp. → renumbering. Surname "changes" on succession → house name.
- Baptism not found from age at death → widen window; orphan book.
- Marriage not in bride's parish (usual, not always) → banns notes, land book.
- Joannes ≠ Joachim; -ová taken as another family; Hungarian place name not mapped.
- "Not yet transferred" → volume closure; registry office.
- Godparent read as kin; výměnkář read as father → FAN; land transfer.
- Pre-1787 Jewish patronymics → Familiantenbücher. Convert → conversion note.
- Protestant before 1849 → Catholic register. Old Catholic after 1877 → WStLA duplicates.
- Vienna death → Vienna parish, Totenbeschauprotokoll. Soldier's child missing → military matriky.
- US-anglicised name → passenger list, naturalisation, CGSI.
- Two sources agree because one copied the other → identify the original.

### Proof skeleton

Question → answer with confidence → sources searched incl. negatives → direct evidence cited to image → indirect evidence with its inference → conflicts and why one source wins (original vs derivative, informant, time from event) → other same-name candidates and why rejected → conclusion and what would overturn it.

### HTR and LLM accuracy

No tool has published accuracy on Czech or Latin parish entries. Transkribus Kurrent models report 4.5–6.9 % validation CER – fine for search, not for unattended name extraction, as errors concentrate in names. General LLM vision is far worse: Crosilla, Klic & Colavizza (2025, arXiv 2503.15195) measured GPT-4o at 33.5–80.2 % CER on non-English historical handwriting vs 1.75 % on modern English. FamilySearch Full-Text Search does not list Czech, Latin, German or Polish among its main languages and fails on columnar forms. So every HTR or LLM transcript – your own reading included – is a finding aid: verify names and dates against the image before recording them. Pilot 20–30 entries before bulk extraction (reference/helpers.md).

## 3 Brick walls

- **Pre-1784** → combined registers read whole → estate land books → orphan books / marriage contracts → Tereziánský katastr → 1651 / 1654. Link by holding. Realistic floor: c. 1650.
- **Register lost** → duplicate/index volumes, land books, conscription lists, Musterlisten.
- **Illegitimate birth** → marginal legitimation → mother's later marriage → godparents → court file → land book.
- **Migrant to Vienna** → Vienna parish → Meldeunterlagen (1904–75) → Verlassenschaft (probate) → Heimatrecht place. Weak 1850–1904.
- **Migrant to USA** → passenger lists → naturalisation → church/fraternal (CGSI) → home-parish marginal notes. Use the original village spelling.
- **Soldier** → regiment → Musterlisten / GB / military matriky → Verlustlisten (see Military).
- **Foundling** → foundling-hospital records → wet-nurse parish.
- **Sealed 20th-c. register** → registry office with proof of kinship; Standesamt; HOK; police registrations.
- **Sudeten German after 1945** → HOK → Lastenausgleich → Gemeindeseelenlisten; link by 1939 residence.

Societies: Familia Austria, CompGen, PTG, CGSI. DNA is GDPR special-category data (reference/privacy.md).
