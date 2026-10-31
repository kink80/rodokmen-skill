# Family website – build, settings, publishing

Static site (Astro + Pagefind search); data comes from `data/` via `tools/export_web.py`.
Pages: home (teaser, statistics, fun facts, map, lines, fan chart), story, fun facts, lines, line page, map
(OpenStreetMap), full-screen fan chart with search, list of ancestors, records (crop, highlight box, transcription
and translation), person page, families, „Co hledáme“, research log, search.

## Commands (in `web/`)
| command | does |
|---|---|
| `npm install` | once at the start (downloads libraries, ~1 minute) |
| `npm run data` | converts data for the web (always after data changes; `scripts/data.mjs` finds a working Python 3) |
| `npm run build` | builds the site into `dist/` |
| `npm run nahled` | data + build + preview at http://localhost:4321 (Ctrl+C stops) |
| `npm run dev` | dev mode while editing the look |
| `npm run nasadit` | data + build + upload to Netlify |

Run the preview in the background and send the user the link. Check the look yourself (browser screenshot, if available).

## `data/web.json`
```json
{
 "nazev": "Rodokmen Novákových",
 "popis": "Popis pro vyhledávače a sdílení (web je ale noindex).",
 "zapati": "Dohledáno v digitalizovaných matrikách SOA Zámrsk (portál ARON)",
 "kontakt": {"jmeno": "Petrovi", "email": "petr@example.cz"},
 "prihlaseni": {"uvod": "Tenhle web je jen pro rodinu.", "otazka": "Jak se jmenoval pes u babičky v Holicích?", "odpoved": "Alík"},
 "uvod": {"nadpis": "Lákavý nadpis", "odstavce": ["2–3 věty, které chytí"], "foto": "klic_obrazku_ve_vyrezech", "foto_popis": "…"},
 "pribeh": {"menu": "Příběh Nováků", "rod": 8, "tlacitko": "Přečíst příběh Nováků"},
 "zajimavosti_na_uvodu": [{"nadpis": "Nejstarší předek", "text": "Kovář Jan, asi 1825."}]
}
```
- The template `web.json` is empty – set `nazev` as soon as you know the starting person's surname (`kontrola.py` reports sample leftovers).
- `kontakt.jmeno` is in the Czech **dative** („Napište Petrovi na …“).
- **`prihlaseni` (login):** a question only the family can answer (not easily found on social media).
  - Answer compared ignoring diacritics and case. Server and browser cookie store only its SHA-256 hash, never the answer itself.
    A short or common answer (a name, a village) can still be guessed from the hash by trial – tell the user not to reuse it as a password anywhere.
  - Without an answer, deployment (`npm run nasadit`, `deploy-caprover`) fails; an open site only explicitly via `"otevreny": true` in `prihlaseni`.
  - Light protection against casual visitors, not bank-grade security – sensitive things don't belong on the web.
- `uvod.foto` = image key in `vyrezy/` (save via `vyrez.py`). Without `uvod`, the home page shows at least the story title and `lead`.
- `pribeh`: name `menu` and `tlacitko` after the main line („Příběh Novákových“); `rod` = line number (8–15). See `stories.md`.
- `zajimavosti_na_uvodu`: best 2–4 fun facts for the home page (`stories.md`).
- `citlive_schvaleno`: list of text snippets the user approved for the web (e.g. `["oběsil se ve stodole"]`) – `kontrola.py`
  then no longer reports them as `CITLIVÉ`. The snippet must contain the reported word; if the text changes, it is reported again.
  Approval process: `privacy.md`.

## Publishing on Netlify (default, free)
Before the first deploy go through the checklist in `privacy.md`.
1. User creates an account at netlify.com (Google/GitHub login is enough) – they do it themselves.
2. CLI login (opens browser) – give the user: `! cd web && npx -y netlify-cli@latest login`.
   If `!` doesn't work (cloud environment): `npx -y netlify-cli@latest login --request "Rodokmen"` prints a link and a ticket
   number – user opens the link and approves, then `npx -y netlify-cli@latest login --check <ticket>`.
3. Create the site and link the folder (interactive, once) – give the user:
   `! cd web && npx -y netlify-cli@latest sites:create` (picks team and name → `https://<name>.netlify.app`),
   or `npx netlify-cli link` if the site already exists. If the user is already logged in you can create it non-interactively
   yourself: `cd web && npx -y netlify-cli@latest sites:create --name <name> --account-slug <team>`.
   **Windows:** if `!` runs in Windows PowerShell 5.1 (`&&` doesn't work and Execution Policy blocks `npx`), have the user open
   PowerShell 7 or Command Prompt, go to the `web` folder and run `npx -y netlify-cli@latest login` (and `sites:create`).
4. Deploy: `cd web && npm run nasadit` (you may run it; prints the site URL).
5. Verify: open the URL **in a private/incognito window** → the family login question must appear; after the correct answer, the site.
   If a Netlify login appears instead (401, „Login Redirect“), the site inherited the team's „Team protection“.
   User turns it off in site admin: Project configuration → Access & security → Visitor access. The family question then
   protects the site – re-check in a private window that it appears.
   Login is handled by the Netlify Edge Function `web/netlify/edge-functions/brana.js` (generated by the export).

Later updates = just `npm run nasadit`. Custom domain can be set in Netlify admin.

**Never use „Netlify Drop“** (drag-and-drop folder in the browser): it doesn't support Edge Functions → site without login.

## Preview in Claude desktop
The project has `.claude/launch.json` – Claude desktop preview runs `npm --prefix web run dev` (port 4321). Export data first
(`cd web && npm run data`). If the preview doesn't start (e.g. Windows, where `npm` is `npm.cmd`), set in `launch.json`
`"runtimeExecutable": "node"` and `"runtimeArgs": ["web/node_modules/astro/bin/astro.mjs", "dev", "--root", "web"]`
(Astro 7's launcher is `astro.mjs`, not `astro.js`). The server runs only on this computer – don't add `--host`.

## Own server (advanced)
- Docker: `web/Dockerfile` (Astro build → nginx with `nginx.conf` from `nginx.sablona.conf`). Always `npm run data` before building the image.
- CapRover: `npm run deploy-caprover` (needs the `caprover` CLI installed and logged in).

## Customising the look
Styles `web/src/styles/global.css` (colours in `:root`), layout `web/src/layouts/Base.astro`, pages `web/src/pages/`.
Line colours: `tools/export_web.py` (`LINE_COLORS`) and `tools/fan_chart.py` (`BASE`). Change only at the user's request;
afterwards check the site on mobile and desktop.

## Fan chart as a poster
`python3 -I tools/fan_chart.py vystupy/` → SVG and HTML for printing (plus PNG if Google Chrome is installed; on Windows also Microsoft Edge).

## Updating an older project
1. Re-run `python3 -I SKILL_DIR/tools/novy_projekt.py <project folder>`. Data (`data/`, crops) stay; `tools/` is overwritten,
   and so are files in `web/` that differ from the new template – old versions are backed up to
   `pracovni/zaloha_web_<datum>/` and listed.
2. If you customised the look with the user (styles, pages), port the changes from the backup (diff the files) and check the site.
3. In `web/`: `npm install` and `npm run data`. `export_web.py` rejects an old login template with a prompt to update.
