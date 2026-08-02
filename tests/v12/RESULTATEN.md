# Resultaten ronde v12

> **Nog niet ingevuld.** Dit is het lege formulier. Vul het tijdens en na de ronde in; laat niets staan als
> `<in te vullen>`. Een cel die je niet hebt gemeten krijgt **niet gemeten** — dat is een geldige uitkomst en
> het enige waar de bron iets aan heeft als je het niet hebt kunnen doen.

## Waartegen gemeten is

| | |
|---|---|
| Datum | `<in te vullen>` |
| Profiel | `<in te vullen>` — administratie: `<in te vullen>` |
| Volledig pad `installed_plugins.json` | `<in te vullen>` (is `CLAUDE_CONFIG_DIR` gezet?) |
| Doelrepo + commit van de verse checkout | verwacht `DaveKJohn/specialists-adoptietest` @ `f56a9e6` (3 commits, niet shallow, 0 tags) — gemeten `<in te vullen>` |
| README.md — bytes / regels | verwacht **1105** (git-blob, LF) / **1127** (op schijf, Windows met `core.autocrlf=true`) / **23** regels / **15** volgens `Measure-Object -Line` — gemeten `<in te vullen>` |
| `plugin.json` versie | verwacht **3.1.1** — gemeten `<in te vullen>` |
| `gitCommitSha` uit het record | `<in te vullen>` |
| Gelijk aan de `v3.1.1`-tag? | verwacht **ja** (`main` en de tag stonden op `4b1a74d` toen deze opdracht geschreven werd) — gemeten `<in te vullen>` |
| CLI-versie | `<in te vullen>` |
| Clone-stand (`rev-parse HEAD` in `marketplaces/`) | `<in te vullen>`; shallow? refspec? tags? |

## Stap 0 — is het profiel spoorloos?

> Denk aan de handmatige opruiming van `~/.claude/settings.json` **vóór** de eerste sessie. v11 kreeg rij 6
> nooit schoon omdat Claude Code's classifier dat bestand niet laat wijzigen.

| locatie | uitkomst |
|---|---|
| `installed_plugins.json` | `<in te vullen>` |
| `marketplaces/davekjohns-workshop/` | `<in te vullen>` |
| `cache/davekjohns-workshop/` | `<in te vullen>` |
| `data/specialists-davekjohns-workshop/` | `<in te vullen>` |
| `known_marketplaces.json` | `<in te vullen>` |
| `~/.claude/settings.json` | `<in te vullen>` |

## ⭐ De stappentabel

De kolommen v10 en v11 staan er al in. Vul alleen v12 in, plus de klasse en het issue bij elke afwijking.

### Vóór stap 1 — niet in enig document (v10)

| stap | v10 | v11 | v12 | klasse | issue |
|---|---|---|---|---|---|
| Claude Code installeren | 🔴 geblokkeerd | 🟢 groen (tekst) | `<>` | | #334 |
| `ExecutionPolicy` blokkeert elke `.ps1` | 🔴 geblokkeerd | 🟢 groen (tekst) | `<>` | | #334 |
| PATH + volledige herstart | 🟡 wrijving | 🟢 groen (tekst) | `<>` | | #334 |

> Zeg opnieuw eerlijk welk deel gemeten is en welk deel al stond. Zolang dit profiel Claude Code al heeft, is
> "groen (tekst)" het hoogst haalbare hier en betekent het niet meer dan dat.

### Test A — INSTALL tegen `QUICKSTART.md`

| stap | v10 | v11 | v12 | klasse | issue |
|---|---|---|---|---|---|
| A0 — het document vinden | 🟡 wrijving | 🟢 groen | `<>` | | #338 |
| A1 — het settings-fragment | 🟡 wrijving | 🟢 groen | `<>` | | #335 |
| **A2 regel 1 — `marketplace update`** | 🔴 **geblokkeerd** | ⚪ niet gemeten | `<>` | | **#329** |
| A2 regel 2 — `install --scope project` | 🟢 groen | 🟢 groen | `<>` | | |
| A2 — `settings.json` herschreven | 🟡 wrijving | 🟢 groen | `<>` | | #336 |
| A2 extra — install tegen `project` vs `local` | niet gemeten | 🟢 groen | `<>` | | #325 |
| A3 — de `projectPath`-verificatiequery | 🟢 groen | 🟢 groen | `<>` | | |
| **A3 — het nieuwe `payload`-veld** | — | — | `<>` | | **#355** |
| A3 extra — de tagvergelijking | 🔴 geblokkeerd | 🟡 wrijving | `<>` | | #322 |
| **A4 — de verwachte `Done:`-regel** | 🟢 groen | 🟡 wrijving | `<>` | | **#358** |
| **A4 — de hook-stub in het settings-voorstel** | — | — | `<>` | | **#363** |
| A4 — de roster-hook ná een schone bootstrap | 🟡 wrijving | 🟢 groen | `<>` | | #333 |
| A4 — de output van de bootstrap zelf | 🟡 wrijving | 🟢 groen | `<>` | | #337.4 |
| A4 — regeleindes + slotregel | 🟡 wrijving | 🟢 groen | `<>` | | #337.2 |
| **A5 — de afzendercheck** | 🟢 groen | 🟡 wrijving | `<>` | | **#361** |
| A′ — het `@`-importpad | 🟡 wrijving | 🟢 groen | `<>` | | #330 |
| **A6 — de `RELEASE.md`-kaart leesbaar** | — | — | `<>` | | **#368** |

### Test B — UNINSTALL tegen `UNINSTALL.md`

| stap | v10 | v11 | v12 | klasse | issue |
|---|---|---|---|---|---|
| B0 — het document vinden | 🟡 wrijving | 🟢 groen | `<>` | | #338 |
| B1a — pre-flight 1 met zijn filter | 🟢 groen | 🟢 groen | `<>` | | |
| B1b — succesgeval geeft exit 1 | 🟡 wrijving | 🟢 groen | `<>` | | #332 |
| B1c — pre-flight 2 meet commits | 🟡 wrijving | 🟢 groen | `<>` | | #332 |
| B1d — safety-net commit te smal | 🟡 wrijving | 🟢 groen | `<>` | | #332 |
| B1h — de `<plugin>`-placeholder | 🟡 wrijving | 🟢 groen | `<>` | | #330 |
| B2a/b — preview, en preview = apply | 🟢 groen | 🟢 groen | `<>` | | |
| B2c — lege `scripts\lib\` blijft staan | 🟡 wrijving | 🟢 groen | `<>` | | #331 |
| **B2d — het `kept`-getal naast de markers** | — | 🟡 wrijving | `<>` | | **#356** |
| **B2e — wat het overgebleven proza dóét** | — | — | `<>` | | **#362** |
| B3a — `uninstall --scope project` | 🟢 groen | 🟢 groen | `<>` | | |
| **B3c — de voorspelde CLI-melding** | — | — | `<>` | | **#359** |
| B3b — de ongedocumenteerde `.orphaned_at` | 🟡 wrijving | 🟢 groen | `<>` | | #337.5 |
| B4a — de sleutels uit `settings.json` | 🟢 groen | 🟢 groen | `<>` | | |
| **B4b — de lege `extraKnownMarketplaces`** | 🟢 groen | 🟡 wrijving | `<>` | | **#357** |
| B4c — stap 3 verwijdert het document | 🔴 geblokkeerd | 🟢 groen | `<>` | | #328 |
| B4d — permission-key overleeft | 🟡 wrijving | ⚪ niet gemeten | `<>` | | #337.6 |
| B5 — de cache met de hand opruimen | 🟢 groen | 🟢 groen | `<>` | | |
| B5b — stap 4's audit is weg | 🔴 geblokkeerd | 🟢 groen | `<>` | | #328 |
| B6 — record geschreven, sessie inert | 🟡 wrijving | 🟢 groen | `<>` | | #327 |
| B6 — de padloze `user`-vorm | niet opgetreden | ⚪ niet gemeten | `<>` | | #323 |
| B6 — remedie leesbaar uit de sessie | niet gemeten | ⚪ niet gemeten | `<>` | | #324 |
| — de stap-0-tabel na test B | — | 🟡 wrijving | `<>` | | N3 |

**Totalen v10: 5 geblokkeerd, 18 wrijving, 14 groen.**
**Totalen v11: 0 geblokkeerd, 7 wrijving, 26 groen, 5 niet gemeten.**
**Totalen v12:** _geblokkeerd:_ `<>` · _wrijving:_ `<>` · _groen:_ `<>` · _niet gemeten:_ `<>`

## De negen reparaties van `v3.1.1`

Elke reparatie krijgt precies één woord: **geverifieerd**, **niet gemeten**, of **nog stuk**.

| # | Wat er gerepareerd is | Waar je het ziet | Uitkomst | Toelichting |
|---|---|---|---|---|
| #355 | de verificatiequery controleert of `installPath` bestaat | A3 | `<>` | |
| #356 | de teardown-samenvatting telt zijn eigen `[KEEP]`-regels | B2d | `<>` | |
| #357 | stap 5 noemt de lege `extraKnownMarketplaces` | B4b | `<>` | |
| #358 | de verwachte `Done:`-regel is die van een verse repo | A4 | `<>` | |
| #359 | de voorspelde CLI-melding, en de suggestie die je niet moet volgen | B3c | `<>` | |
| #360 | een bytegetal noemt zijn line-ending-conventie | stap 0 | `<>` | *(in de papieren van deze ronde zelf)* |
| #361 | de afzendercheck vraagt naar de invariant | A5 | `<>` | |
| #362 | het achtergebleven proza is niet inert, en er staat een remedie | B2e | `<>` | |
| #363 | de hook-stub is zichtbaar een placeholder, plus slotregel | A4 | `<>` | |

Consumentzichtbaar meegekomen met dezelfde release:

| # | Wat | Waar | Uitkomst | Toelichting |
|---|---|---|---|---|
| #368 | de mojibake uit de `RELEASE.md`-kaart | A6 | `<>` | 145 beschadigde reeksen in de `v3.1.0`-kaart |

Aan de bronkant en hier **niet** meetbaar: de zelf-committende fold (#369) en de poort op voorbeeld-output
(#370). Noem ze in het rapport als buiten scope, zodat "niet gemeten" niet als "niet gerepareerd" leest.

## De vier metingen die v11 niet kon doen

Dit is de eigenlijke opbrengst van v12. Alle vier krijgen ze een expliciete uitkomst, ook als die opnieuw
"niet gemeten" is — dan met de reden erbij.

| # | Wat | Waarom v11 het niet kon | Uitkomst v12 |
|---|---|---|---|
| #329 | `marketplace update` op een profiel waar die marketplace nooit bestond | stap 0a herregistreerde de marketplace en vernietigde de voorwaarde | `<>` |
| #323 | de padloze `user`-vorm wordt als `[RECORD-SHAPE]` gemeld | die vorm trad niet op | `<>` |
| #324 | het remedie is uit de sessie-output leesbaar | er laadde geen plugin, dus geen hook, dus geen output | `<>` |
| #337.6 | een `permissions`-entry die naar de plugin-map wijst | de voorwaarde deed zich niet voor | `<>` |

## Nieuwe bevindingen

| # | Klasse | Stap | Wat | Issue |
|---|---|---|---|---|
| N1 | `<>` | `<>` | `<>` | `<>` |

## Scope — lees dit vóór je iets hierboven als groen leest

- Was dit **A′** (uitvoering overgedragen aan een sessie) of een hand-gelopen test A? `<in te vullen>`
- Waren de sessiestarts headless (`claude -p`) of interactief? `<in te vullen>`
- Is de skill-wrapper van `specialists-init` echt gebruikt, of is `bootstrap.ps1` direct gedraaid? `<in te vullen>`
- Welk deel van het pre-stap-1-blok is opnieuw gemeten en welk deel stond al? `<in te vullen>`
- Wat is er met het profiel gedaan na afloop? `<in te vullen>`
