# Resultaten ronde v13

> **Dit is een formulier, nog niet een uitkomst.** Elke `VUL-IN` wordt tijdens de ronde vervangen door een
> meting. Een rij die niet gemeten is krijgt **niet gemeten** — niet groen, en ook niet stuk. De kolommen v10,
> v11 en v12 staan er al in en worden **niet** aangepast: zij zijn de vergelijking, en het enige dat laat zien
> of het pad begaanbaarder is geworden in plaats van alleen anders.

## Waartegen gemeten is

| | |
|---|---|
| Datum | VUL-IN |
| Profiel | VUL-IN — administratie: VUL-IN |
| Volledig pad `installed_plugins.json` | VUL-IN (`CLAUDE_CONFIG_DIR` gezet? VUL-IN) |
| Doelrepo + commit van de verse checkout | verwacht `DaveKJohn/specialists-adoptietest` @ `50ec727` (4 commits, niet shallow, 0 tags, enige tracked file `README.md`) — gemeten: VUL-IN |
| README.md — bytes / regels | verwacht 1105 blob / 1127 op schijf / 22 terminated / 23 posities / 15 volgens `Measure-Object -Line` — gemeten: VUL-IN |
| `plugin.json` versie | verwacht 3.1.2 — gemeten VUL-IN |
| `gitCommitSha` uit het record | VUL-IN |
| Gelijk aan de `v3.1.2`-tag? | **verwacht: nee.** `main` lag op 2 aug 18:10 UTC op `ad84ef9`, drie commits vóór de release; tag-object `647ac1e` peelt naar `101d597`. Gemeten: VUL-IN |
| CLI-versie | VUL-IN — git VUL-IN, PS VUL-IN, Windows VUL-IN |
| Clone-stand (`rev-parse HEAD` in `marketplaces/`) | VUL-IN — shallow? VUL-IN, refspec VUL-IN, tags: VUL-IN |

## Stap 0 — is het profiel spoorloos?

| locatie | uitkomst |
|---|---|
| `installed_plugins.json` | VUL-IN |
| `marketplaces/davekjohns-workshop/` | VUL-IN |
| `cache/davekjohns-workshop/` | VUL-IN |
| `data/specialists-davekjohns-workshop/` | VUL-IN |
| `known_marketplaces.json` | VUL-IN |
| `~/.claude/settings.json` | VUL-IN |

**Was de handmatige opruiming nodig?** VUL-IN. (v12 ruimde het profiel na de ronde volledig op, dus verwacht:
nee. Meet het, neem het niet aan.)

## ⭐ De stappentabel

### Vóór stap 1 — niet in enig document (v10)

| stap | v10 | v11 | v12 | v13 | klasse | issue |
|---|---|---|---|---|---|---|
| Claude Code installeren | 🔴 geblokkeerd | 🟢 groen (tekst) | 🟢 groen (tekst) | VUL-IN | | #334 |
| `ExecutionPolicy` blokkeert elke `.ps1` | 🔴 geblokkeerd | 🟢 groen (tekst) | 🟢 groen (tekst) | VUL-IN | | #334 |
| PATH + volledige herstart | 🟡 wrijving | 🟢 groen (tekst) | 🟢 groen (tekst) | VUL-IN | | #334 |

**Zeg eerlijk wat hier gemeten is.** In v10 t/m v12 stond alles al; "groen (tekst)" is dan het hoogst haalbare.

### Test A — INSTALL tegen `QUICKSTART.md`

| stap | v10 | v11 | v12 | v13 | klasse | issue |
|---|---|---|---|---|---|---|
| A0 — het document vinden | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #338 |
| A1 — het settings-fragment | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #335 |
| A2 regel 1 — `marketplace update` | 🔴 geblokkeerd | ⚪ niet gemeten | 🟢 groen | VUL-IN | | #329 |
| A2 regel 2 — `install --scope project` | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| A2 — de lege `Available marketplaces`-lijst | — | — | 🟡 wrijving (klein) | VUL-IN | | nieuw in v12 |
| A2 — `settings.json` herschreven | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #336 |
| A2 extra — install tegen `project` vs `local` | niet gemeten | 🟢 groen | 🟢 groen | VUL-IN | | #325 |
| A3 — de `projectPath`-verificatiequery | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| A3 — het `payload`-veld | — | — | 🟢 groen | VUL-IN | | #355 |
| **A3 extra — de tagvergelijking** | 🔴 geblokkeerd | 🟡 wrijving | 🟡 wrijving | **VUL-IN** | | #322 · **#372** |
| **A4 — de skill-wrapper `/specialists-init`** | — | ⚪ niet gemeten | ⚪ niet gemeten | **VUL-IN** | | nieuw in v13 |
| A4 — de verwachte `Done:`-regel | 🟢 groen | 🟡 wrijving | 🟢 groen | VUL-IN | | #358 |
| A4 — de hook-stub in het settings-voorstel | — | — | 🟢 groen | VUL-IN | | #363 |
| A4 — de roster-hook ná een schone bootstrap | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #333 |
| A4 — de output van de bootstrap zelf | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #337.4 |
| A4 — regeleindes + slotregel | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #337.2 |
| A5 — de afzendercheck | 🟢 groen | 🟡 wrijving | 🟢 groen | VUL-IN | | #361 |
| A′ — het `@`-importpad | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #330 |
| A6 — de `RELEASE.md`-kaart leesbaar | — | — | 🟢 groen | VUL-IN | | #368 |

### Test B — UNINSTALL tegen `UNINSTALL.md`

| stap | v10 | v11 | v12 | v13 | klasse | issue |
|---|---|---|---|---|---|---|
| B0 — het document vinden | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #338 |
| B1a — pre-flight 1 met zijn filter | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| B1b — succesgeval geeft exit 1 | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #332 |
| B1c — pre-flight 2 meet commits | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #332 |
| B1d — safety-net commit te smal | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #332 |
| B1h — de `<plugin>`-placeholder | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #330 |
| B2a/b — preview, en preview = apply | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| B2c — lege `scripts\lib\` blijft staan | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #331 |
| B2d — het `kept`-getal naast de markers | — | 🟡 wrijving | 🟢 groen | VUL-IN | | #356 |
| B2e — wat het overgebleven proza dóét | — | — | 🟢 groen | VUL-IN | | #362 |
| B2f — de fixture-README niet als werk-item | — | — | 🟢 groen | VUL-IN | | stap 0 |
| B3a — `uninstall --scope project` | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| B3c — de voorspelde CLI-melding | — | — | 🟢 groen | VUL-IN | | #359 |
| B3b — de ongedocumenteerde `.orphaned_at` | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #337.5 |
| B4a — de sleutels uit `settings.json` | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| **B4b — de lege `extraKnownMarketplaces`** | 🟢 groen | 🟡 wrijving | 🟡 wrijving | **VUL-IN** | | #357 · **#374** |
| B4c — stap 3 verwijdert het document | 🔴 geblokkeerd | 🟢 groen | 🟢 groen | VUL-IN | | #328 |
| B4d — permission-key overleeft | 🟡 wrijving | ⚪ niet gemeten | ⚪ niet gemeten | VUL-IN | | #337.6 |
| B5 — de cache met de hand opruimen | 🟢 groen | 🟢 groen | 🟢 groen | VUL-IN | | |
| **B5b — wanneer stap 4's audit verdwijnt** | 🔴 geblokkeerd | 🟢 groen | 🟡 wrijving | **VUL-IN** | | #328 · **#373** |
| B6 — record geschreven, sessie inert | 🟡 wrijving | 🟢 groen | 🟢 groen | VUL-IN | | #327 |
| B6 — de padloze `user`-vorm | niet opgetreden | ⚪ niet gemeten | ⚪ niet gemeten | VUL-IN | | #323 |
| B6 — remedie leesbaar uit de sessie | niet gemeten | ⚪ niet gemeten | 🟢 groen | VUL-IN | | #324 |
| — de stap-0-tabel na test B | — | 🟡 wrijving | 🟢 groen | VUL-IN | | N3(v11) |

**Totalen v10: 5 geblokkeerd, 18 wrijving, 14 groen.**
**Totalen v11: 0 geblokkeerd, 7 wrijving, 26 groen, 5 niet gemeten.**
**Totalen v12: 0 geblokkeerd, 3 wrijving, 38 groen, 2 niet gemeten.**
**Totalen v13: VUL-IN.**

## De vier reparaties van `v3.1.2`

| # | Wat er gerepareerd is | Waar | Uitkomst | Toelichting |
|---|---|---|---|---|
| #371 | de ijkpunt-tabel wordt gegenereerd, met beide regelconventies als eigen rij | stap 0 | VUL-IN | VUL-IN — klopt elke rij tegen jouw eigen meting? |
| #372 | de clone draagt wél tags, en een geannoteerde tag moet met `^{}` gepeeld worden | A3 | VUL-IN | VUL-IN — is de valse *"no tags"*-clausule weg, staat de derde uitkomst er, en kwam je op de juiste reden uit? |
| #373 | `teardown.ps1` overleeft stap 2; stap 4 biedt de her-run uit de cache | B1 / B5b | VUL-IN | VUL-IN — bestaan de twee bestanden na stap 2 aantoonbaar nog? |
| #374 | de lege-sleutel-waarschuwing is voorwaardelijk; de byte-claim is een reeks | B4b | VUL-IN | VUL-IN — mag een volgende ronde een volledig schone stap-0-tabel als normaal rapporteren? |

**Niet gemeten ≠ niet gerepareerd.** Buiten scope, aan de bronkant: de craft-regel van #378 en poort 16 van
#379.

## De metingen die blijven liggen

| # | Wat | Hoe lang al | Uitkomst v13 |
|---|---|---|---|
| #323 | de padloze `user`-vorm als `[RECORD-SHAPE]` | v10, v11, v12 niet opgetreden | VUL-IN |
| #337.6 | een `permissions`-entry die naar de plugin-map wijst | v11, v12 voorwaarde deed zich niet voor | VUL-IN |
| — | de **skill-wrapper** `/specialists-init` interactief | nog nooit gemeten (v11 + v12 draaiden `bootstrap.ps1` direct) | VUL-IN |

Bij elk: **is de voorwaarde uitgebleven, of is de meting mislukt?** Dat is niet hetzelfde, en de bron kan er
alleen iets mee als het onderscheid er staat. Bij de wrapper hoort er ook bij: **harnasgrens of bevinding over
de plugin?**

## Nieuwe bevindingen

| # | Klasse | Stap | Wat | Issue |
|---|---|---|---|---|
| N1 | VUL-IN | VUL-IN | VUL-IN | VUL-IN |

## Scope — lees dit vóór je iets hierboven als groen leest

- **Was dit A′?** VUL-IN — is de uitvoering aan een sessie overgedragen (dan meet je AI-geassisteerde adoptie,
  zoals v10 t/m v12) of hand-gelopen?
- **Sessiestarts headless of interactief?** VUL-IN. Headless output van de session-start-hooks komt in de
  context van het model en niet op stdout, dus zeg hoe je de `[ERROR]`/`[ROSTER-PENDING]`-telling hebt gedaan.
- **Skill-wrapper of `bootstrap.ps1` direct?** VUL-IN — en als het de wrapper niet werd: waar liep hij vast?
- **Welk deel van het pre-stap-1-blok is opnieuw gemeten?** VUL-IN.
- **Wat is er met het profiel gedaan?** VUL-IN — volledig opgeruimd, of half? Een half opgeruimd profiel is
  voor v14 geen nulstand.

## Nog één ding, voor wie v14 schrijft

VUL-IN — de les die deze ronde opleverde, in één alinea, met de bevinding erbij waar hij uit komt.
