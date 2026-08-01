# Resultaten ronde v11

> **Gedraaid op 1 augustus 2026, in modus A′.** De consument heeft de uitvoering aan een sessie
> overgedragen, dus dit is **AI-geassisteerde adoptie**, niet een handmatige test A. Het document benoemt
> die modus zelf als niet-in-scope voor een handmatige meting; hij staat hier zoals v10 dat ook meldde.

## Waartegen gemeten is

| | |
|---|---|
| Datum | 1 augustus 2026 |
| Profiel | `davek_onn` — administratie: `C:\Users\davek_onn\.claude\` |
| Volledig pad `installed_plugins.json` | `C:\Users\davek_onn\.claude\plugins\installed_plugins.json` (`CLAUDE_CONFIG_DIR` niet gezet) |
| Doelrepo + commit van de verse checkout | `DaveKJohn/specialists-adoptietest` @ `f6c0470b4fcc3b7be92b562600bf9894fa67fdce` (2 commits, niet shallow, 0 tags) |
| README.md — bytes / regels | verwacht **1105 / 15** — gemeten **1127 / 23** op schijf, **1105** in de git-blob. Verschil = CRLF: `core.autocrlf=true`, 22 regeleindes. "15" klopt alleen als leegregels niet meetellen |
| `plugin.json` versie | verwacht **3.1.0** — gemeten **3.1.0** |
| `gitCommitSha` uit het record | `6feac75860ef0ad6c1cd9bd05740dbea80912136` |
| Gelijk aan de `v3.1.0`-tag? | **ja.** `v3.1.0` is een *annotated* tag (`7c08ff3`) die dereferencet naar `6feac75`; upstream `main` stond óók op `6feac75`. Venster was nog open |
| CLI-versie | `2.1.220` |
| Clone-stand (`rev-parse HEAD` in `marketplaces/`) | `6feac75860ef0ad6c1cd9bd05740dbea80912136`; shallow **ja**, refspec `main`-only, **1 tag: `v3.1.0`** |

## Stap 0a — issue #350

De vraag: zijn de twee `claude plugin`-commando's van stap 1 overbodig?

Gedraaid zoals de QUICKSTART het zelf voorschrijft (r. 269-278): twee keys schrijven, restart, restart,
dan de zes locaties en de skill-lijst lezen **zonder één `claude plugin`-commando**.

| locatie / meting | uitkomst |
|---|---|
| `installed_plugins.json` na twee herstarts | start 1: bestand aangemaakt maar **leeg** (`plugins: {}`). start 2: **volledig project-scoped record**, `version 3.1.0`, `gitCommitSha 6feac75`, juiste `projectPath` |
| de andere vijf locaties | marketplace **opnieuw geregistreerd** door start 1 (nieuwe `lastUpdated`), clone **teruggezet**; `cache/` **bleef afwezig** — ook na drie starts; `data/` afwezig |
| **skill-lijst** — zit `specialists-init` erin? | **nee**, na start 1, 2 én 3. Geen specialisten-subagents, geen afzenderregel, geen hook-output |
| **Conclusie** | **De twee commando's zijn NIET overbodig.** Een sessiestart registreert de marketplace (#329's mechanisme) én schrijft een compleet, correct record (#327's mechanisme) — voor het eerst als één keten en zonder handmatige `marketplace add`. Maar hij vult **de cache niet**, en dus laadt er niets. `marketplace update` + `install` zijn wat de **payload** halen. Dat is de schakel die de twee losse metingen nooit samen hadden |

**Nieuwe bevinding hierbij:** het record dat een sessiestart schrijft wijst naar een `installPath`
(`…\cache\davekjohns-workshop\specialists\3.1.0`) die **niet bestaat** — de hele cache-map bestond niet. De
verificatiequery van het document gaf desondanks een perfecte regel. Zie *Nieuwe bevindingen*, N1.

## Stap 0 — is het profiel spoorloos?

Gemeten in een kale PowerShell, vóór enige nieuwe sessiestart. Een metersessie liep al (A′-artefact).

| locatie | uitkomst |
|---|---|
| `installed_plugins.json` | afwezig ✅ |
| `marketplaces/davekjohns-workshop/` | afwezig ✅ |
| `cache/davekjohns-workshop/` | afwezig ✅ |
| `data/specialists-davekjohns-workshop/` | afwezig ✅ |
| `known_marketplaces.json` | schoon ✅ |
| `~/.claude/settings.json` | ❌ **`extraKnownMarketplaces` bleef staan** — de classifier van Claude Code weigert wijziging van dit bestand, via zowel PowerShell als de Edit-tool. Er is niet omheen gewerkt. **Dit is de eerste bevinding van de ronde in de zin van stap 0b**, maar met een harnas- in plaats van een documentoorzaak |

**Gevolg voor de ronde:** #350's experiment (voorgeschreven door het document) registreert de marketplace
opnieuw. Daarmee was de nulstand van **#329** — *een profiel waar die marketplace nooit heeft bestaan* —
weg vóórdat A2 gedraaid kon worden. De consument heeft besloten: **#329 gaat als niet gemeten.** De twee
metingen sluiten elkaar op één profiel uit; de opdracht zet 0a vóór A2, maar 0a vernietigt A2's nulstand.

## ⭐ De stappentabel

### Vóór stap 1 — niet in enig document (v10)

| stap | v10 | v11 | klasse | issue |
|---|---|---|---|---|
| Claude Code installeren | 🔴 geblokkeerd | 🟢 groen (tekst) | — | #334 |
| `ExecutionPolicy` blokkeert elke `.ps1` | 🔴 geblokkeerd | 🟢 groen (tekst) | — | #334 |
| PATH + volledige herstart | 🟡 wrijving | 🟢 groen (tekst) | — | #334 |

> **Eerlijk over dit blok: alleen de tekst is getoetst, niet de werking.** Dit profiel had Claude Code, PATH
> en `ExecutionPolicy` al lang staan. De drie groenen zeggen dat de sectie bestaat, compleet is en
> draaibare commandoblokken afdrukt — niet dat ze op een kaal profiel opnieuw gemeten zijn.

### Test A — INSTALL tegen `QUICKSTART.md`

| stap | v10 | v11 | klasse | issue |
|---|---|---|---|---|
| A0 — het document vinden | 🟡 wrijving | 🟢 groen | — | #338 |
| A1 — het settings-fragment | 🟡 wrijving | 🟢 groen | — | #335 |
| **A2 regel 1 — `marketplace update`** | 🔴 **geblokkeerd** | ⚪ niet gemeten | — | #329 |
| A2 regel 2 — `install --scope project` | 🟢 groen | 🟢 groen | — | |
| A2 — `settings.json` herschreven | 🟡 wrijving | 🟢 groen | — | #336 |
| A2 extra — install tegen `project` vs `local` | niet gemeten | 🟢 groen | — | #325 |
| A3 — de `projectPath`-verificatiequery | 🟢 groen | 🟢 groen | — | |
| **A3 extra — de tagvergelijking** | 🔴 **geblokkeerd** | 🟡 wrijving | **3 + 1** | #322 |
| A4 — `specialists-init`, de aantallen | 🟢 groen | 🟡 wrijving | **1** | #337.3 |
| A4 — de roster-hook ná een schone bootstrap | 🟡 wrijving (19 errors) | 🟢 groen | — | #333 |
| A4 — de output van de bootstrap zelf | 🟡 wrijving | 🟢 groen | — | #337.4 |
| A4 — regeleindes + slotregel | 🟡 wrijving | 🟢 groen | — | #337.2 |
| A5 — Chris neemt het woord | 🟢 groen | 🟡 wrijving | **4** | |
| A′ — het `@`-importpad | 🟡 wrijving | 🟢 groen | — | #330 |

### Test B — UNINSTALL tegen `UNINSTALL.md`

| stap | v10 | v11 | klasse | issue |
|---|---|---|---|---|
| B0 — het document vinden | 🟡 wrijving | 🟢 groen | — | #338 |
| B1a — pre-flight 1 met zijn filter | 🟢 groen | 🟢 groen | — | |
| B1b — succesgeval geeft exit 1 | 🟡 wrijving | 🟢 groen | — | #332 |
| B1c — pre-flight 2 meet de index | 🟡 wrijving | 🟢 groen | — | #332 |
| B1d — safety-net commit te smal | 🟡 wrijving | 🟢 groen | — | #332 |
| B1h — de `<plugin>`-placeholder | 🟡 wrijving | 🟢 groen | — | #330 |
| B2a/b — preview, en preview = apply | 🟢 groen | 🟢 groen | — | |
| B2c — lege `scripts\lib\` blijft staan | 🟡 wrijving | 🟢 groen | — | #331 |
| B2d — `[FREE]` terwijl prose overleeft | 🟡 wrijving | 🟡 wrijving | **1** | #331 |
| B3a — `uninstall --scope project` | 🟢 groen | 🟢 groen | — | |
| B3b — de ongedocumenteerde `.orphaned_at` | 🟡 wrijving | 🟢 groen | — | #337.5 |
| B4a — de sleutels uit `settings.json` | 🟢 groen | 🟢 groen | — | |
| B4b — `marketplace remove` | 🟢 groen | 🟡 wrijving | **2** | #339 |
| **B4c — stap 3 verwijdert het document** | 🔴 **geblokkeerd** | 🟢 groen | — | #328 |
| B4d — permission-key overleeft | 🟡 wrijving | ⚪ niet gemeten | — | #337.6 |
| B5 — de cache met de hand opruimen | 🟢 groen | 🟢 groen | — | |
| **B5b — stap 4's audit is weg** | 🔴 **geblokkeerd** | 🟢 groen | — | #328 |
| B6 — record geschreven, sessie inert | 🟡 wrijving | 🟢 groen | — | #327 |
| B6 — de padloze `user`-vorm | niet opgetreden | ⚪ niet gemeten | — | #323 |
| B6 — remedie leesbaar uit de sessie | niet gemeten | ⚪ niet gemeten | — | #324 |
| — de stap-0-tabel na test B | — | 🟡 wrijving | **2** | N3 |

**Totalen v10: 5 geblokkeerd, 18 wrijving, 14 groen.**
**Totalen v11:** _geblokkeerd:_ **0** · _wrijving:_ **7** · _groen:_ **26** · _niet gemeten:_ **5**

**Alle vijf blokkades van v10 zijn weg.** Twee ervan (#328 tweemaal) zijn aantoonbaar gerepareerd, één
(#322) is naar wrijving gezakt, één (#329) is niet gemeten omdat de nulstand ervoor weg was, en de twee
pre-stap-1-blokkades zijn tekstueel gedicht maar niet op een kaal profiel herhaald.

## De zeventien reparaties

| # | Wat er gerepareerd is | Waar je het ziet | Uitkomst | Toelichting |
|---|---|---|---|---|
| #322 | tagvergelijking vervangen door iets uitvoerbaars | A3 extra | **geverifieerd, met één onjuistheid** | `rev-parse HEAD` en de `gh api …/tags --jq …commit.sha`-route werken en gaven `6feac75`, gelijk aan het record. `fatal: ambiguous argument` staat benoemd als **verwachte** derde uitkomst. Twee claims kloppen (shallow ✓, refspec ✓), één niet: *"`main` only, **no tags**"* — mijn verse clone draagt **1 tag (`v3.1.0`)**, en de oude `rev-list`-route werkte daardoor toevallig wél. Staat bovendien in `SKILL.md`, niet in de QUICKSTART: een lezer ziet het pas bij A4 |
| #323 | de padloze degradatie wordt gemeld | B6 | **niet gemeten** | De padloze `user`-vorm trad niet op. De sessiestart schreef een correct `project`-record met juist pad, dus er was geen verkeerde vorm om als `[RECORD-SHAPE]` te melden. Vereist een fixture die die vorm uitlokt |
| #324 | het remedie is uit de sessie-output leesbaar | B6 | **niet gemeten** | Er was geen hook-output om een remedie in te lezen: de plugin laadde niet, dus er liep geen hook. Dat is precies het gedrag dat het document voorspelt, niet de klacht die #324 was. Vereist een sessie waarin de hooks wél draaien mét een verkeerd gevormd record |
| #325 | trigger versmald naar een scope-mismatch | A2 extra | **geverifieerd** | Install tegen bestaand `project`-record → `already installed`, **1** regel, geen zwerfrecord. Met een `local`-record erbij → **2** regels, tweede leest `local`. Na `uninstall --scope local` weer 1 |
| #327 | "record aanwezig, sessie inert" heeft een naam | B6 | **geverifieerd** | Eén sessiestart met alleen een enable-key + geregistreerde marketplace schreef een **vers, volledig project-scoped record** (`installedAt 20:49:09`, juiste sha) terwijl de sessie **niets laadde** en er **geen hook-output** was. De uninstall heelde zichzelf. Geverifieerd via de skill-lijst, niet via het record. Randvoorwaarde scherper dan gedocumenteerd: mét de marketplace verwijderd schreef dezelfde staat **niets** |
| #328 | stapvolgorde: document en audit overleven | B4c, B5b | **geverifieerd** | Na stap 2 bestonden `UNINSTALL.md` (in de clone) én `teardown.ps1` (in de cache) nog. Stap 4 zegt eerlijk dat de audit met stap 2 weg is en dat je de output uit stap 1 moet hebben bewaard; stap 5 haalt de clone als **laatste** weg. Beide v10-blokkades zijn hiermee weg |
| #329 | het eerste commando is geen doodlopend spoor | A2 | **niet gemeten** | De nulstand (*marketplace heeft hier nooit bestaan*) was door 0a al vernietigd — 0a's eigen voorgeschreven experiment registreert hem opnieuw. Besluit van de consument. Wél waar: `marketplace update` liep op dit profiel door (exit 0), en de registratie staat in het document nu **vóór** het commando |
| #330 | importpad + `<plugin>`-placeholder | A′, B1h | **geverifieerd** | Het import in `SPECIALISTS.md` r. 9 wijst naar `marketplaces/…/personas/01-01-persona.md` — de **clone**, niet de `cache/` — en het document legt uit waarom dat bewust is. De `<plugin>`-placeholder is uitgelegd als het `installPath`-veld, met een query die het pad meeprint en een expliciete waarschuwing de `SPECIALISTS.md`-route **niet** te gebruiken. Zonder gokken vindbaar |
| #331 | `[KEEP]` bij overlevende prose, lege map weg | B2c, B2d | **geverifieerd, op één telling na** | `scripts\lib\` en `scripts\` zijn **weg**, geen lege mappen. De twee prose-regels worden expliciet als **`[KEEP]`** gerapporteerd, met een `[note]` die zegt dat dit alles is wat er in `CLAUDE.md` overblijft — precies wat ik meet (4 regels: kop, leegregel, 2× prose). **Maar:** `Summary: 30 item(s) to remove, **0 kept**` terwijl er 2 `[KEEP]`-regels staan. Zie N2 |
| #332 | pre-flight meet commits, niet de index | B1b, B1c, B1d | **geverifieerd** | Met 25 bestanden gestaged en **0** commits toegevoegd: het nieuwe `git ls-tree … HEAD` gaf **0** regels, de oude `git ls-files`-variant **20** — exact het valse groen van v10, side-by-side. Commando 1 gaf exit **1** zonder output, zoals gedocumenteerd. De safety-net-commit dekt nu `CLAUDE.md`, `.claude` én `scripts`. De `fatal: … HEAD`-uitkomst is benoemd maar niet meetbaar hier (repo heeft 2 commits) |
| #333 | roster-hook leest de seam, één `[ROSTER-PENDING]` | A4 | **geverifieerd** | v10: **19** `[ERROR]`-regels naar `CLAUDE.md`. v11: **0** `[ERROR]`, **precies 1** `[ROSTER-PENDING]`, die `.claude/specialists/SPECIALISTS.md` noemt en zelf zegt *"Nothing is broken and nothing has drifted"*. `scripts/repo-config.ps1:42` zet `$script:RosterPath = '.claude/specialists/SPECIALISTS.md'`; de enige `CLAUDE.md`-vermeldingen daar zijn commentaar dat de oude fout uitlegt |
| #334 | prerequisites vóór stap 1 | vóór A1 | **geverifieerd (alleen tekst)** | `Before you start` bestaat met alle drie, en met één draaibaar commando voor de `ExecutionPolicy`. PATH/editor-herstart staat erbij, eerlijk afgebakend als artefact van de meetroute. **Niet opnieuw gemeten op een kaal profiel** — dit profiel had alles al |
| #335 | settings-fragment is geldige JSON | A1 | **geverifieerd, empirisch** | Fence-label is `json` (niet `jsonc`), blok heeft omsluitende accolades, en letterlijk geknipt en in een leeg bestand geplakt **parseert het**, met beide keys. Tweede helft ook: een aparte blockquote onderscheidt `.claude/` in de repo van `~/.claude/`, met *"create it if it is not there"* |
| #336 | byte-identiek-claim versmald | A2 | **geverifieerd, empirisch** | SHA256 vóór `F694FB44…EFA8` (224 b) → ná `EB8834F7…5E4A` (246 b): **niet byte-identiek**, zoals het document nu belooft. Beide voorspelde verschuivingen kloppen: `enabledPlugins` gaat naar voren, en het geneste `source`-object wordt uitgeklapt. Inhoud equivalent, geen gedragsverandering. **Dit levert het hashpaar dat het document zelf als ontbrekend bewijs benoemt** |
| #337 | zes papercuts | A4, B3b, B4d | **vijf geverifieerd, één niet gemeten, één stuk** | **.2** geverifieerd — alles LF (0 CRLF) en `CLAUDE.md` mist zijn slotregel, ook op Windows; onbenoemd extraatje: `settings.suggested.jsonc` mist die óók. **.3 nog stuk** — zie N4. **.4** geverifieerd — *"the orchestrator import"* enkelvoud, en de regel zegt *"**NOT gitignored in this repo**"* in plaats van *"gitignored in many repos"*. **.5** geverifieerd — `.orphaned_at` verscheen (13 bytes), precies zoals voorspeld. **.6 niet gemeten** — stap 3 benoemt de `permissions`-entry, maar die trad niet op: ik heb `settings.suggested.jsonc` nooit overgenomen, dus de voorwaarde deed zich niet voor |
| #338 | de documenten zijn vindbaar | A0, B0 | **geverifieerd** | De root-README draagt in de **tweede alinea** een blockquote met links naar Quickstart **én** UNINSTALL, met de reden en de issueverwijzing erbij. Via die route **één beslissing** vóór stap 1, tegen v10's tweederde-pagina. De QUICKSTART waarschuwt bovendien zelf voor de `WebFetch`-hallucinatie en geeft twee werkende omwegen |
| #339 | `marketplace remove` — vraag beantwoord en tot regel gemaakt | B4b, B5 | **geverifieerd, met een onbenoemd restant** | De regel klopt exact: clone **weg**, cache **blijft** (1.120.897 bytes), `marketplace list` noemt hem niet meer. De absolute bytegetallen wijken af (4,5 MB vs 2,9 MB clone), maar het document zegt zelf dat de **regel** telt en niet de twee getallen — correct voorbehoud. **Onbenoemd:** het commando laat een lege `extraKnownMarketplaces: {}` achter in `~/.claude/settings.json`. Zie N3 |

**Score: 13 geverifieerd · 3 niet gemeten (#323, #324, #329) · 1 gemengd (#337: 4 van 6 groen, 1 stuk, 1 niet gemeten).**

## Nieuwe bevindingen

| issue | klasse | stap | één regel |
|---|---|---|---|
| N1 | **1** | 0a | Het record dat een sessiestart schrijft noemt een `installPath` die niet bestaat — de hele cache-map bestond niet — en de verificatiequery van het document geeft desondanks een perfecte `project 3.1.0 <juiste sha>`-regel. Scherper dan #327 beschrijft: dáár zat de payload nog in de cache, hier is er géén payload |
| N2 | **1** | B2 | `Summary: 30 item(s) to remove, **0 kept**` terwijl de teardown twee `[KEEP]`-regels afdrukt en de `[note]` van *"2 line(s)"* spreekt. De teller spreekt de markers tegen, in precies de reparatie (#331) die overlevende prose zichtbaar moest maken |
| N3 | **2** | B4b | `marketplace remove` laat een lege `extraKnownMarketplaces: {}` achter in `~/.claude/settings.json`. Het document benoemt het spiegelbeeld hiervan (`enabledPlugins: {}` na uninstall) wél, dit niet. Gevolg: rij 6 van de stap-0-tabel is na een volledige teardown nooit letterlijk leeg |
| N4 | **1** | A4 | De afgedrukte verwachte slotregel van `specialists-init` zegt `0 script-scaffold(s) created, 2 already present`; een **verse** repo geeft `2 created, 0 already present`. De voorbeeldregel is opgenomen in een repo waar die scripts al stonden — terwijl #337.3's hele doel was een verse lezer getallen te geven om tegen af te zetten. Het document dekt alleen de andere richting (*"if a count is lower than this…"*) |
| N5 | **1** | B3 | Bij een scopeloze `uninstall` voorspelt de QUICKSTART de melding *"Plugin `specialists` is not installed at scope user"*. CLI `2.1.220` geeft iets anders: *"is enabled at project scope (.claude/settings.json, shared with your team). To disable just for you: `claude plugin disable … --scope local`"*. De CLI noemt dus **niet** de vlag van het document, en wie zijn suggestie volgt doet een `disable --scope local` in plaats van de bedoelde `uninstall --scope project` |
| N6 | **3/4** | 0b | Het ijkpunt *"1105 bytes / 15 regels"* is het git-zijdige LF-getal. Een Windows-werkkopie meet **1127 / 23**. Het document noemt de conventie niet en waarschuwt tegelijk uitdrukkelijk om afwijkende getallen niet als restant van een vorige ronde te lezen — de lezer die 1127 ziet heeft geen manier om te weten welk getal juist is |
| N7 | **4** | A5 | De QUICKSTART belooft *"a sender header such as `🧭 Chris — intake & routing`"*. Wat verscheen was **"This one is for Rebecca (Research Specialist)"** — Chris routeert wel degelijk, maar de vorm wijkt af van het voorbeeld, dus een lezer die op die vorm controleert vindt hem niet |
| N8 | **2** | B4/na | De `[KEEP]`-prose in `CLAUDE.md` blijft na de teardown beweren dat de repo *"is governed by **Claude Specialists**"* en verwijst naar `specialists-init`. Een latere sessie leest dat als geldende governance terwijl niets meer laadt — twee verse sessies merkten die tegenspraak zelf op. Het document zegt dát de prose blijft, niet dat ze een volgende sessie misleidt |
| N9 | **2** | A4 | `settings.suggested.jsonc` verwijst naar een Stop-hook `scripts/maintenance/lint-changed-hook.ps1` die de bootstrap niet aanmaakt en die in deze repo niet bestaat |
| N10 | **—** (harnas, niet de bron) | 0b | Claude Code's eigen classifier weigert wijziging van `~/.claude/settings.json`, via zowel PowerShell als de Edit-tool. Daardoor kon rij 6 van de stap-0-tabel niet worden schoongemaakt en kon #329's nulstand niet worden herbouwd. **Geen documentfout** — hier vastgelegd omdat het de meting bepaalde |

## Wat deze ronde niet bewees

- **De teardown-regressie in twee cycli.** Dat is de bezette-consumertest; v11 is de verse. `life-hub` is niet aangeraakt.
- **Een vers apparaat.** Zelfde machine, CLI (`2.1.220`), git en OS. Dit was een verse **gebruiker**.
- **De poorten en checks bij de bron.** Van buiten onzichtbaar, zoals in v8 t/m v10.
- **Een handmatige test A van begin tot eind.** Dit is **A′** — de uitvoering is aan een sessie overgedragen.
- **De aantallen-sweep door de documentatie.** Alleen de getallen op het gelopen pad zijn nageteld.
- **Het #283-CRLF-artefact.** Deze repo heeft geen `.gitignore` en er is er geen gebouwd.
- **Wat er onderweg bij kwam:**
  - **#329** — nulstand vernietigd door 0a's eigen voorgeschreven experiment.
  - **#323 en #324** — de fixtures die die twee uitlokken traden niet op; zie de tabel.
  - **#337.6** — `settings.suggested.jsonc` is nooit overgenomen, dus de `permissions`-entry ontstond niet.
  - **Het pre-stap-1-blok (#334)** — alleen tekstueel getoetst; dit profiel had Claude Code, PATH en `ExecutionPolicy` al.
  - **De skill-wrapper van `specialists-init`** — de headless sessie kon `bootstrap.ps1` niet draaien (geneste PowerShell geblokkeerd) en weigerde correct output te verzinnen; het **script** is direct gedraaid, de **skill** dus niet.
  - **Interactieve TUI-sessies** — alle sessiestarts waren `claude -p` (headless). Dat skills en hooks daar identiek laden is een aanname; de eerste afwezige afzenderregel bleek inderdaad een artefact van mijn eigen prompt.
  - **De `fatal: … HEAD`-uitkomst van pre-flight 2** — benoemd in het document, niet meetbaar in een repo met 2 commits.

## Het profiel daarna

**Opgeruimd**, op één punt na dat ik niet kon opruimen.

| locatie | staat |
|---|---|
| `installed_plugins.json` | afwezig ✅ |
| `marketplaces/davekjohns-workshop/` | afwezig ✅ |
| `cache/davekjohns-workshop/` | afwezig ✅ (met de hand, via het commando uit stap 5) |
| `data/specialists-davekjohns-workshop/` | afwezig ✅ |
| `known_marketplaces.json` | schoon ✅ |
| `~/.claude/settings.json` | ⚠️ `{"extraKnownMarketplaces": {}, "theme": "dark"}` — de **declaratie** is weg (door `marketplace remove` zelf), de **lege sleutel** blijft. Zie N3 en N10 |

De **repo** staat exact terug op de fixture: `git status` leeg, één bestand (`README.md`), HEAD `f6c0470`.

De volledige profielstaat van vóór de ronde (v10's fixture: record `3.0.9`/`6a9ef82`, clone, cache,
`known_marketplaces`- en `settings`-keys) is geverifieerd geback-upt in `%TEMP%\v11bk` — 287 + 62
bestanden plus 3 JSON's — mocht #350 die fixture terug nodig hebben.

**Voor de volgende ronde:** dit is een bruikbare nulstand op vijf van zes rijen. Rij 6 vraagt één
handmatige ingreep die een sessie met deze guard niet kan doen: `extraKnownMarketplaces` uit
`~/.claude/settings.json` halen.
