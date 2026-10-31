# Setup and OS pitfalls

Single source for installation and shell/OS traps. Paste the section for the user's OS into every helper brief
(`helpers.md`).

## 1. Check the toolchain (Phase 0)
| Tool | Check | Needed |
|---|---|---|
| Python | `python3 --version` (Windows: `python --version`, else `py -3 --version`) | ≥ 3.9 |
| Node | `node --version` | ≥ 20 |
| ImageMagick | `magick -version` | 7; outside Windows the tools fall back to v6 `convert` themselves |
| Git | `git --version` | any |

Let the user confirm every system install. Give interactive commands to the user prefixed with `!` so they run them.
- **macOS:** `brew install python node imagemagick git`.
- **Windows:** always use winget with the full flags, otherwise it dies on the msstore prompt
  („Error reading input in prompt“):
  `winget install --id <pkg> --source winget --accept-source-agreements --accept-package-agreements`
  Packages: `Python.Python.3.12` (add `--scope user`), `OpenJS.NodeJS.LTS`, `ImageMagick.ImageMagick`, `Git.Git`.
  New installs reach `PATH` only after restarting Claude Code. Until then call Python by full path
  `"$LOCALAPPDATA/Programs/Python/Python312/python.exe"` and prefix ImageMagick commands with
  `export PATH="/c/Program Files/ImageMagick-7…:$PATH"` (exact folder from `C:\Program Files`).

## 2. Windows: which Python, and HTTPS
- `python3` is often a dead Microsoft Store alias („Python nebyl nalezen“). Use `python` (or `py -3`).
- Check **which** Python it is (`which -a python`) – it may be bundled with another app (Inkscape, GIMP…) and lack certificates.
- Verify HTTPS: `python -I -c "import urllib.request; urllib.request.urlopen('https://aron.vychodoceskearchivy.cz')"`.
  `CERTIFICATE_VERIFY_FAILED` → install the normal Python via winget and use it by full path.

## 3. Write the platform rules into the project
Right after creating the project, add to its `CLAUDE.md` a short "Platform" block with the command forms below
(Windows: `python -I -X utf8`, `magick`, Git Bash for `.sh`), so later sessions and helpers use them.

## 4. Pitfalls – macOS (zsh)
- `for a in "x y z"; do set -- $a` does not split words in zsh (use `${=a}`); `ls F*` with no match is an error
  („no matches found“); `echo ====` is an error (`=` at word start). Run complex loops via `bash -c '…'`.
- ImageMagick: pass file lists as separate arguments, not one space-joined variable. `-annotate` and `montage` labels need
  a font: `-font /System/Library/Fonts/Supplemental/Arial.ttf` (otherwise „unable to read font“).

## 5. Pitfalls – Windows (Git Bash)
- Python: always `python -I -X utf8 tools/<tool>.py` in place of `python3 -I` (or `py -3 -I -X utf8`). Without `-X utf8`
  pipes and redirects use cp1250: Czech breaks and one-off scripts crash on „→“, „≈“. Skill tools switch to UTF-8 themselves,
  your own scripts don't. `-I` ignores `PYTHONUTF8`. Redirect output to files in Git Bash – Windows PowerShell 5.1 `>`
  writes UTF-16.
- ImageMagick is `magick`, **never `convert`** (that is the Windows FAT→NTFS disk converter).
  Font: `-font C:/Windows/Fonts/arial.ttf`.
- Command line limit is 32 767 chars – hundreds of tiles in one `magick` call fail (WinError 206); assemble row by row.
- Run `.sh` tools via Git Bash: `bash tools/…`.
- Don't write long one-off Python as a heredoc (`python - <<EOF`) – Git Bash sometimes fails with „unexpected EOF“.
  Write it to `pracovni/x.py` and run it.
- Filenames are case-insensitive (`o_29.jpg` = `O_29.jpg`): tell preview and full-resolution apart by prefix
  (`nahled_`, `plne_`).
- Write data files as UTF-8 **without BOM** (PowerShell 5.1 `-Encoding UTF8` adds a BOM and `osoby.py` then fails to load).
- `!` commands may run in Windows PowerShell 5.1, where `&&` doesn't work and `npx` is blocked by Execution Policy –
  ask the user to open PowerShell 7 or Command Prompt instead.

## 6. All platforms
- Never edit data with `sed -i` or a script that rewrites a whole file: JSON via `tools/zapis.py`, `osoby.py` and markdown
  with the file-edit tool (SKILL.md → Recording rules).
- Keep downloaded pages in `pracovni/<volume>/` and check before re-downloading.
