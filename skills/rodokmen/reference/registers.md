# Reading parish registers (matriky) – practical guide

## Book types
| Code | Book | Contents | Why it matters |
|---|---|---|---|
| **N** | births/baptisms (narození, křty), *Liber natorum / baptisatorum*, *Geburtsbuch* | birth and baptism date, house (čp. = číslo popisné, house number), child, parents (from ~1784 also their parents and residence), godparents, midwife | sex, legitimacy, mother's parents → next generation |
| **O** | marriages (oddaní, sňatky), *Liber copulatorum*, *Trauungsbuch* | date, house, groom and bride (age, status, religion), their parents, witnesses | **best first target** – opens two families at once |
| **Z** | deaths (zemřelí), *Liber defunctorum*, *Sterbebuch* | date of death and burial, house, name, age, cause | age → estimated birth year; status ("widow of…") |
| **I** | index (rejstřík) to N/O/Z | name → date / page | fast lookup; **indexes are often incomplete** – a negative index result proves nothing |

Other sources: censuses (sčítání lidu, 1857–1921; households with birth dates), soupis poddaných 1651 (serf list, whole
families with ages), berní rula 1654 (tax roll, heads of farms only), gruntovní/pozemkové knihy (land books – who owned
which house; link generations via the house), military records, domovské listy (certificates of domicile), parte
(death notices). See archives.md and methodology.md → Sources.

## Closure periods (Act 301/2000 Sb.)
A volume goes to the archive (and thus online) only once its **last** entry is older than: births **100 years**,
marriages **75**, deaths **30**. Younger books stay at the registry office (matriční úřad) of the municipality with
extended powers / authorised office; a direct relative can get an extract (request + proof of kinship). Record it in
`archivni_dotazy.md`.

## Search workflow
1. **Village → parish.** Small villages belonged to a parish in a neighbouring village; the volume description lists
   all villages of the parish. Search the village within volume descriptions (`aron_search.py "Obec"`), not just titles –
   and search by village, not only by the parish named in a later record: the village may have kept its own register
   (filiální kostel = filial church, lokálie = local chaplaincy) even if the mother parish issued the certificate.
   A village split between two parishes has two book series, each only for "its" house numbers.
2. **Volume by year and type.** Watch for overlaps: one book for several villages, separate books per village.
   **Druhopisy** (duplicates kept for the deanery or authorities, e.g. "T" volumes in ZA Opava) often hold N, O and Z of
   one year together – useful when the original is missing, illegible or huge.
3. **Index first, then the book.** An index sorted by letter and year gives page/folio; estimate the image number
   (image ≈ folio × 1 to 2) and fine-tune.
   - Older indexes (18th c.) sometimes point 1–4 pages off – if the entry isn't on the stated page, browse around it.
   - The image–page offset shifts within a book (inserted leaves, missing pages) – read the page number on the preview and recompute.
   - For old parishes first look for a **cumulative index** spanning decades (e.g. N/O/Z 1636–1746) – it gets you to the 17th century fast.
   - Indexes often follow **German spelling**: look up Czech names starting with O and V also under W (Otava = Wotawa, Vávra = Wawra).
   - Multi-village books are often ordered by village with the index at the end.
   - Large parishes have indexes as **separate volumes** (e.g. a marriage index covering 300 years) – find those first.
   - **Register missing** (not online or lost)? Try the index – often kept separately and survived where the book didn't.
     An index entry documents date, place and father; record it as `prob` (a copy, not the original) and retry the register later.
4. **No index: browse chronologically.** Estimate position from the years on the first and last images (bisection).
   Old books bound from **annual extracts** (Extractus, Consignatio, 18th c.) may be out of order with years missing;
   the year is often only in the extract heading. Check the year headings before concluding "the entry isn't there".
   **Uncertain date** (digits from a later note): first go through all possible years in the home parish, only then
   neighbouring parishes. (Real mistake: "9?" read as 97, five neighbouring parishes searched; the marriage was at home five years earlier.)
5. **Read the whole entry**, then look at the surroundings (twins, other children of the same parents).
6. **Marriage usually took place in the bride's parish** (her residence) – search there first, then the groom's.
   Stuck? A child's baptism entry sometimes has the parents' marriage date and place added (Prague ~1880–1890) – verify it in the marriage book.
7. **Family missing from the Catholic register? Try Protestant or Jewish.** After the Patent of Toleration (1781)
   Protestant congregations (a. v. = Lutheran, h. v. = Reformed) kept their own registers, one congregation covering dozens
   of villages; in ARON they have signatures "R …" and `aron_search.py` finds them by village. Common in East Bohemia
   (Rychnovsko, Dobrušsko, Opočensko). Jewish registers: see archives.md.
   For 1920–1945 also try **civil registers** (civilní matriky) of people without religion at district offices (tested at SOA Třeboň, see archives.md).

## Reading the handwriting
- Kurrent index: download full resolution and cut the page into quarters (name column + dates):
  `magick f.jpg -crop 1250x1650+X+Y +repage ctvrt.jpg`. **Added-in lines** are easily misread (Franziska × Heinrich,
  page 131 × 151) – always verify an index entry in the register.
- Language: until ~1780 Latin or Czech, then mostly German (Kurrent), from ~1860 often Czech, Czech in the 20th century.
- Czech names in German/Latin: Johann/Joannes = Jan, Wenzel/Wenceslaus = Václav, Joseph = Josef, Franz/Franciscus = František,
  Maria = Marie, Anna, Katharina = Kateřina, Barbara = Barbora, Magdalena = Majdalena/Magdaléna, Theresia = Terezie,
  Ludmilla = Lidmila, Dorothea = Dorota, Apollonia = Apolena, Georg/Georgius = Jiří, Matthias = Matěj, Adalbert = Vojtěch.
- Surnames are spelled phonetically and drift (Šaršon/Scharschon/Šaršoun, Kroupa/Kraupa). Search all variants.
- Female surnames in German lack "-ová" (Maria Novak = Marie Nováková).
- Village names have old forms, e.g. with a prefixed "V" ("Volešnice" = Olešnice); try those when searching.
- Kurrent: long "ſ" looks like "f", "e" like "n", "h" has a downward loop. When unsure, compare with the same letter
  elsewhere on the page in the same hand – also for an "odd" surname (final "k" written almost like "r": "-ek" looks like "-er").
  A word split across a line end ("Urlau-" / "ber") continues at the start of the next line.
- Dates are often given as feast days (*Dominica*, *festum S. Joannis*) – compute them when unclear.

### Glossary (Latin / German → Czech)
| term | Czech | English |
|---|---|---|
| *natus / geboren*, *baptisatus / getauft* | narozen, pokřtěn | born, baptised |
| *copulati / getraut*, *sponsus / Bräutigam*, *sponsa / Braut* | oddáni, ženich, nevěsta | married, groom, bride |
| *mortuus, sepultus / gestorben, begraben* | zemřel, pohřben | died, buried |
| *legitimus / ehelich*, *illegitimus / unehelich* | manželský, nemanželský | legitimate, illegitimate |
| *viduus, vidua / Witwer, Witwe* | vdovec, vdova | widower, widow |
| *filius, filia / Sohn, Tochter* | syn, dcera | son, daughter |
| *patrini / Pathen*, *testes / Zeugen*, *obstetrix / Hebamme* | kmotři, svědci, porodní bába | godparents, witnesses, midwife |
| *agricola, rusticus / Bauer* | sedlák, rolník | farmer, peasant |
| *Häusler*, *casarius* | domkář | cottager |
| *Inwohner / inquilinus*, *podruh* | podruh (bydlel v cizím domě) | lodger (lived in someone else's house) |
| *Gärtner* (in a village) | zahradník = malý usedlík, ne nutně pěstitel | smallholder, not necessarily a gardener |
| *Inleute*, *Taglöhner* | podruzi, nádeníci | lodgers, day labourers |
| *Bürger*, *civis* | měšťan | burgher, townsman |
| *Schneider / sartor*, *Schuhmacher / sutor*, *Weber / textor*, *Schmied / faber*, *Müller / molitor* | krejčí, švec, tkadlec, kovář, mlynář | tailor, shoemaker, weaver, smith, miller |
| *N.* / *No.* / *Haus Nr.* | číslo domu (popisné) | house number (čp.) |
| *ex*, *aus*, *von* | z (obce) | from (village) |
| *Lungensucht*, *Abzehrung* | souchotiny (TBC) | consumption (TB) |
| *Cholera*, *Blattern* | cholera, neštovice | cholera, smallpox |
| *Fraisen* | křeče (časté u kojenců) | convulsions (common in infants) |

## Estimating years
- Age at death/marriage is often rounded by ±2–5 years. Write "asi 1756" (about 1756) and search the baptism in a wider window.
  Even an exact birth date in a marriage entry may have the wrong month: if the baptism isn't there, go through the whole
  year ±1 and the mother's parish (children were often born at her parents').
- Bride typically 18–28, groom 22–32; children every 1.5–3 years.
- Groom marked "vdovec" (widower) = look for the first marriage and its children.

## Data status codes
| status | when |
|---|---|
| `doc` | read directly on the image of an entry that unambiguously identifies the person |
| `prob` | strong match (name, period, place, house, trade) but no direct entry; always add a `note` why |
| `fam` | from the family / a family document |
| `mh` | only from someone else's online tree |
| `unk` | only the position in the tree is known |

## Typical pitfalls (from real research)
- Scribe got the grandfather's name wrong (Josef instead of Václav) – multiple entries of the same family decide.
- Child registered under a different given name than the family used (Františka → "Anna" at home).
- Marriage a few months before or after the birth – not an error, a "last-minute wedding"; an interesting detail for the website.
- Illegitimate child carries the mother's surname; father missing or added later (legitimation in the margin).
- Same ancestor twice in the tree (distant relatives married) – `SAME` in the data.
- Index found nothing ≠ entry doesn't exist. Record exactly what you searched.
- Helpers (subagents) and OCR/AI misread historical handwriting – always verify yourself (see helpers.md, methodology.md → Failure modes).

## Verification and crops
- Verify a reading: `python3 -I tools/overit.py <image> x0,y0,x1,y1 pracovni/over [v]` (fractions 0–1, not pixels) –
  splits the entry into two halves scaled to 1900 px (a double page is otherwise illegible).
- Website crop (`vyrez.py`): crop only the **read entry** (with the column header if useful), never the whole double page –
  it would be illegibly small on the website. The full page is in `zdroje/` and on the portal via link.

Shell, ImageMagick, Python and Windows/macOS pitfalls (include them in helper briefs): see setup.md.
