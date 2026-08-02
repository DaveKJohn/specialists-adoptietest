# Meetlog v13 — ruwe metingen, in volgorde van meten

Meter: Claude (deze sessie). Consument: Dave. Datum: 2 augustus 2026.

## Stap 0 — profiel

Gemeten 2026-08-02 18:20:41Z, in PowerShell 5.1 via de tool-laag van een **al lopende** Claude Code-sessie
(zie caveat 1 onderaan).

- Profiel: `dave\davek_onn`, `USERPROFILE=C:\Users\davek_onn`
- `CLAUDE_CONFIG_DIR`: **niet gezet** (leeg)
- Administratie: `C:\Users\davek_onn\.claude\` (bestaat)
- Volledig pad `installed_plugins.json`: `C:\Users\davek_onn\.claude\plugins\installed_plugins.json`

| locatie | uitkomst |
|---|---|
| `~/.claude/plugins/installed_plugins.json` | **afwezig** (bestand bestaat niet) |
| `~/.claude/plugins/marketplaces/davekjohns-workshop/` | **afwezig** |
| `~/.claude/plugins/cache/davekjohns-workshop/` | **afwezig** (map `cache/` bestaat en is leeg) |
| `~/.claude/plugins/data/specialists-davekjohns-workshop/` | **afwezig** (map `data/` bestaat en is leeg) |
| `~/.claude/plugins/known_marketplaces.json` | **afwezig** (bestand bestaat niet) |
| `~/.claude/settings.json` | **afwezig** (bestand bestaat niet, dus geen `enabledPlugins`/`extraKnownMarketplaces`) |

Alle zes afwezig → nulstand gehaald. Handmatige opruiming was **niet** nodig.

**Afwijking t.o.v. wat de opdracht voorspelde (geen bevinding tegen de plugin, wel noteren).** De opdracht
verwachtte de v12-eindtoestand: `installed_plugins.json` met 35 bytes `{"version": 2, "plugins": {}}` en
`known_marketplaces.json` met 2 bytes `{}`. Beide bestanden bestaan nu **helemaal niet meer**. Naast de
plugin-mappen staat `.last_inuse_sweep` met inhoud `2026-08-02T12:09:37.806Z` — Claude Code heeft die dag zelf
een sweep gedraaid. Beide toestanden vallen onder *"afwezig, of zonder enig record"*, dus de rij is groen; het
verschil zit in wie het weghaalde.

**Wel aanwezig, relevant voor A2:** `~/.claude/plugins/marketplaces/claude-plugins-official/` bestaat, terwijl
`known_marketplaces.json` afwezig is. Dit profiel is dus niet *"écht maagdelijk"* in de zin van de
v12-observatie bij A2 — de `Available marketplaces:`-lijst kan hier gevuld zijn zonder dat er een record is.

## Stap 0 — de ijkpunt-tabel van #371

Gemeten op de bestaande werkkopie `C:\Users\davek_onn\Documents\DaveKJohn\specialists-adoptietest`
(zie caveat 2: niet opnieuw gekloond, wél aantoonbaar identiek aan een verse clone).

| measure | verwacht | gemeten | commando |
|---|---|---|---|
| size, repo side | 1105 | **1105** | `git cat-file -s HEAD:README.md` |
| size, on disk | 1127 | **1127** | `(Get-Item README.md).Length` |
| size delta | 22 | **22** | 1127 − 1105 |
| lines, terminated | 22 | **22** | `(Get-Content README.md).Count` / `grep -c ''` |
| line positions | 23 | **23** | 22 + 1 (bestand eindigt op terminator) |
| lines per `Measure-Object -Line` | 15 | **15** | `(Get-Content README.md \| Measure-Object -Line).Lines` |
| commits op HEAD | 4 | **4** | `git rev-list --count HEAD` |
| HEAD | `50ec727` | **`50ec7278eae9ac23aa3ff8ac1b3e47e134ddaa97`** | `git rev-parse HEAD` |

Verder: **0 tags**, `.git/shallow` **afwezig**, `git status --porcelain -uall --ignored` **leeg**,
`README.md` de **enige** tracked file (geen `.claude/`, geen `CLAUDE.md`), `core.autocrlf=true`,
refspec `+refs/heads/*:refs/remotes/origin/*`, remote `https://github.com/DaveKJohn/specialists-adoptietest.git`.

**Acht van de acht rijen kloppen.** #371 → geverifieerd op stap 0.

## Omgeving

| | |
|---|---|
| Claude Code | 2.1.220 |
| git | 2.49.0.windows.1 |
| PowerShell | 5.1.26100.8875 |
| Windows | 10.0.26200 (Windows 11 Pro) |

## Twee caveats bij stap 0 — vooraf vastgelegd, niet achteraf

**Caveat 1 — de meting is niet vóór de eerste sessie gedaan.** De opdracht schrijft voor: meet in een kale
PowerShell *vóórdat* de eerste Claude Code-sessie start, want een sessiestart schrijft een ontbrekend record
zelf (#327). Deze ronde is begonnen dóór een Claude Code-sessie te starten in de fixture-repo; de meting is
dus tijdens die sessie gedaan, via haar tool-laag. **Wat dat wel en niet aantast:** alle zes rijen zijn
alsnog afwezig, inclusief `installed_plugins.json` — dus als deze sessiestart een record had geschreven, was
dat hier zichtbaar geweest. De #327-val heeft bovendien een `enabledPlugins`-sleutel nodig en die bestaat niet.
De nulstand staat, maar de *volgorde-eigenschap* van de meting is deze ronde niet bewezen.

**Caveat 2 — de werkkopie is niet weggegooid en opnieuw gekloond.** De opdracht schrijft `Remove-Item -Recurse
-Force` + `git clone` voor. Dat kan niet vanuit een sessie waarvan de werkmap díe repo is. In plaats daarvan is
gemeten of de bestaande werkkopie van een verse clone te onderscheiden is: zelfde commit, 4 commits, niet
shallow, 0 tags, `git status` met `-uall --ignored` volledig leeg, één tracked file. Op elk meetbaar punt
identiek aan een verse clone.
