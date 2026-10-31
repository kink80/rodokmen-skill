# Helpers (subagents)

Use helpers for big rounds ("go through 5 volumes and find every Novák"). They cost a lot of credits, so first tell the
user what you plan and why, and get approval.

## Rules
- 2–4 helpers in parallel, each on a different branch/archive. One round ≈ 15–25 new ancestors.
- Run them on the cheaper model (Agent with `model: "sonnet"`): searching and first reading are fine there. Verification
  on the image and writing to `data/` stay with you (main model). Tell the user – it explains why credits drain slower.
- A helper writes only to `pracovni/pom_<branch>/`, **never to `data/`**. Don't delete helper output until you have used it.
- Only one helper can use the browser at a time – prefer tools that need no browser. ARON and Porta fontium work well.
- Don't send helpers to hard hands (Kurrent from the first half of the 19th century, faded scans), or give them the exact image.
- **Verify every helper reading yourself** before recording it as `doc`:
  `python3 -I tools/overit.py <image> x0,y0,x1,y1 pracovni/over` (fractions 0–1).
  Explain it to the user correctly: helpers don't read worse – it's the same model family. It is the four-eyes principle,
  and the helper lacks context (two families of the same name in the village, two Jans on one farm).

## Brief template (fill in and paste)
```
Task: find <persons/surname> in <archive>, volume <id/signature>, images <from–to> (years <…>).
Context: <known facts: parents, house number, spouse, dates; similar families to tell apart>.
Tools: run from <project dir>: <exact commands, e.g. python3 -I tools/aron_img.py <apu> <image> 0 pracovni/pom_<branch>/x.jpg>.
Platform: <paste the user's OS section from reference/setup.md>.
Rules: write only to pracovni/pom_<branch>/; never to data/. Download only images you read. Don't guess:
  uncertain reading "(?)", illegible "[…]". An index miss proves nothing.
Report for each find: record key (obec_druh_kdo_rok), image number, entry coordinates as fractions 0–1,
  transcription per column, your confidence.
Report for EVERY image you went through: what you actually read on it (year, first name on the page) – so missing or
  unloaded images show up.
Also report clearly related hits (rare family surname, same house, a name from the family's stories) – as leads, not proof.
```
