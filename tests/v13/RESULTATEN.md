# Resultaten ronde v13

> **Uitgevoerd 2 augustus 2026.** De kolommen v10, v11 en v12 zijn ongewijzigd overgenomen: zij zijn de
> vergelijking. De ruwe metingen staan in `MEETLOG.md` (stap 0), `TESTA.md` en `TESTB.md` naast dit bestand.

## Waartegen gemeten is

| | |
|---|---|
| Datum | 2 augustus 2026, 18:19–19:20 UTC |
| Profiel | `dave\davek_onn` — administratie: `C:\Users\davek_onn\.claude\` |
| Volledig pad `installed_plugins.json` | `C:\Users\davek_onn\.claude\plugins\installed_plugins.json` (`CLAUDE_CONFIG_DIR` gezet? **nee**, leeg) |
| Doelrepo + commit van de verse checkout | `DaveKJohn/specialists-adoptietest` @ **`50ec727`** — 4 commits, niet shallow, 0 tags, enige tracked file `README.md`. **Gemeten: klopt op alle punten** |
| README.md — bytes / regels | **1105 blob / 1127 op schijf / 22 terminated / 23 posities / 15 volgens `Measure-Object -Line`** — alle vijf gemeten en gelijk |
| `plugin.json` versie | **3.1.2** |
| `gitCommitSha` uit het record | **`ad84ef97906d6e68023a3834cd8fcf56aedbaf70`** |
| Gelijk aan de `v3.1.2`-tag? | **Nee, zoals verwacht.** `gh api …/tags` gaf `101d597…` voor `v3.1.2`; het record staat op `ad84ef9` = `main`, drie commits ná de release |
| CLI-versie | **2.1.220** — git 2.49.0.windows.1, PS 5.1.26100.8875, Windows 10.0.26200 (11 Pro) |
| Clone-stand (`rev-parse HEAD` in `marketplaces/`) | **`ad84ef9`** — shallow? **ja** (depth 1), refspec `+refs/heads/main:refs/remotes/origin/main`, tags: **0** |

## Stap 0 — is het profiel spoorloos?

| locatie | uitkomst |
|---|---|
| `installed_plugins.json` | **afwezig** (bestand bestond niet) |
| `marketplaces/davekjohns-workshop/` | **afwezig** |
| `cache/davekjohns-workshop/` | **afwezig** (`cache/` bestond, leeg) |
| `data/specialists-davekjohns-workshop/` | **afwezig** (`data/` bestond, leeg) |
| `known_marketplaces.json` | **afwezig** |
| `~/.claude/settings.json` | **afwezig** |

**Was de handmatige opruiming nodig?** **Nee** — alle zes vanzelf schoon. Wel een afwijking van wat de
opdracht voorspelde: die verwachtte de v12-eindtoestand (`installed_plugins.json` 35 bytes,
`known_marketplaces.json` 2 bytes). Beide bestanden **bestonden helemaal niet meer**; naast de plugin-mappen
stond `.last_inuse_sweep` met `2026-08-02T12:09:37.806Z`, dus Claude Code had die dag zelf een sweep
gedraaid. Beide toestanden vallen onder *"afwezig, of zonder enig record"* — de rij is groen, het verschil
zit in wie het weghaalde.

**Twee caveats, vooraf vastgelegd.** (1) De meting is **niet** vóór de eerste sessie gedaan maar via de
tool-laag van een lopende sessie — alle zes rijen waren alsnog afwezig, dus als die sessiestart een record
had geschreven was dat zichtbaar geweest; de *volgorde-eigenschap* is deze ronde niet bewezen. (2) De
werkkopie is niet weggegooid en opnieuw gekloond (kan niet vanuit een sessie waarvan die repo de werkmap is);
in plaats daarvan is aangetoond dat ze op elk meetbaar punt van een verse clone niet te onderscheiden was.

## ⭐ De stappentabel

### Vóór stap 1 — niet in enig document (v10)

| stap | v10 | v11 | v12 | v13 | klasse | issue |
|---|---|---|---|---|---|---|
| Claude Code installeren | 🔴 geblokkeerd | 🟢 groen (tekst) | 🟢 groen (tekst) | 🟢 groen (tekst) | | #334 |
| `ExecutionPolicy` blokkeert elke `.ps1` | 🔴 geblokkeerd | 🟢 groen (tekst) | 🟢 groen (tekst) | 🟢 groen (tekst) | | #334 |
| PATH + volledige herstart | 🟡 wrijving | 🟢 groen (tekst) | 🟢 groen (tekst) | 🟢 groen (tekst) | | #334 |

**Zeg eerlijk wat hier gemeten is.** Alle drie stonden al: Claude Code draait (`2.1.220`), de
`ExecutionPolicy` is ruim genoeg (elk `.ps1` in deze ronde liep zonder `PSSecurityException`, inclusief
`claude.ps1` zelf), PATH + editor-herstart niet opnieuw gemeten. **"Groen (tekst)" is het hoogst haalbare**,
net als in v10 t/m v12.

### Test A — INSTALL tegen `QUICKSTART.md`

| stap | v10 | v11 | v12 | v13 | klasse | issue |
|---|---|---|---|---|---|---|
| A0 — het document vinden | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #338 |
| A1 — het settings-fragment | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #335 |
| A2 regel 1 — `marketplace update` | 🔴 geblokkeerd | ⚪ niet gemeten | 🟢 groen | 🟢 groen | | #329 |
| A2 regel 2 — `install --scope project` | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| A2 — de lege `Available marketplaces`-lijst | — | — | 🟡 wrijving (klein) | 🟡 wrijving (klein) | 1 | nieuw in v12 |
| A2 — `settings.json` herschreven | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #336 |
| A2 extra — install tegen `project` vs `local` | niet gemeten | 🟢 groen | 🟢 groen | 🟢 groen | | #325 |
| A3 — de `projectPath`-verificatiequery | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| A3 — het `payload`-veld | — | — | 🟢 groen | 🟢 groen | | #355 |
| **A3 extra — de tagvergelijking** | 🔴 geblokkeerd | 🟡 wrijving | 🟡 wrijving | **🟢 groen** | | #322 · **#372** |
| **A4 — de skill-wrapper `/specialists-init`** | — | ⚪ niet gemeten | ⚪ niet gemeten | **🟢 groen** | | nieuw in v13 |
| A4 — de verwachte `Done:`-regel | 🟢 groen | 🟡 wrijving | 🟢 groen | 🟢 groen | | #358 |
| A4 — de hook-stub in het settings-voorstel | — | — | 🟢 groen | 🟢 groen | | #363 |
| A4 — de roster-hook ná een schone bootstrap | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #333 |
| A4 — de output van de bootstrap zelf | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #337.4 |
| A4 — regeleindes + slotregel | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #337.2 |
| A5 — de afzendercheck | 🟢 groen | 🟡 wrijving | 🟢 groen | 🟢 groen | | #361 |
| A′ — het `@`-importpad | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #330 |
| A6 — de `RELEASE.md`-kaart leesbaar | — | — | 🟢 groen | 🟢 groen | | #368 |

### Test B — UNINSTALL tegen `UNINSTALL.md`

| stap | v10 | v11 | v12 | v13 | klasse | issue |
|---|---|---|---|---|---|---|
| B0 — het document vinden | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #338 |
| B1a — pre-flight 1 met zijn filter | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| B1b — succesgeval geeft exit 1 | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #332 |
| B1c — pre-flight 2 meet commits | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #332 |
| B1d — safety-net commit te smal | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen (tekst) | | #332 |
| B1h — de `<plugin>`-placeholder | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #330 |
| B2a/b — preview, en preview = apply | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| B2c — lege `scripts\lib\` blijft staan | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #331 |
| B2d — het `kept`-getal naast de markers | — | 🟡 wrijving | 🟢 groen | 🟢 groen | | #356 |
| B2e — wat het overgebleven proza dóét | — | — | 🟢 groen | 🟢 groen | | #362 |
| B2f — de fixture-README niet als werk-item | — | — | 🟢 groen | 🟢 groen | | stap 0 |
| B3a — `uninstall --scope project` | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| B3c — de voorspelde CLI-melding | — | — | 🟢 groen | 🟢 groen | | #359 |
| B3b — de ongedocumenteerde `.orphaned_at` | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #337.5 |
| B4a — de sleutels uit `settings.json` | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| **B4b — de lege `extraKnownMarketplaces`** | 🟢 groen | 🟡 wrijving | 🟡 wrijving | **🟢 groen** | | #357 · **#374** |
| B4c — stap 3 verwijdert het document | 🔴 geblokkeerd | 🟢 groen | 🟢 groen | 🟢 groen | | #328 |
| B4d — permission-key overleeft | 🟡 wrijving | ⚪ niet gemeten | ⚪ niet gemeten | ⚪ niet gemeten | | #337.6 |
| B5 — de cache met de hand opruimen | 🟢 groen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| **B5b — wanneer stap 4's audit verdwijnt** | 🔴 geblokkeerd | 🟢 groen | 🟡 wrijving | **🟢 groen** | | #328 · **#373** |
| B6 — record geschreven, sessie inert | 🟡 wrijving | 🟢 groen | 🟢 groen | 🟢 groen | | #327 |
| B6 — de padloze `user`-vorm | niet opgetreden | ⚪ niet gemeten | ⚪ niet gemeten | ⚪ niet gemeten | | #323 |
| B6 — remedie leesbaar uit de sessie | niet gemeten | ⚪ niet gemeten | 🟢 groen | 🟢 groen | | #324 |
| — de stap-0-tabel na test B | — | 🟡 wrijving | 🟢 groen | 🟢 groen | | N3(v11) |

**Totalen v10: 5 geblokkeerd, 18 wrijving, 14 groen.**
**Totalen v11: 0 geblokkeerd, 7 wrijving, 26 groen, 5 niet gemeten.**
**Totalen v12: 0 geblokkeerd, 3 wrijving, 38 groen, 2 niet gemeten.**
**Totalen v13: 0 geblokkeerd, 1 wrijving, 43 groen, 2 niet gemeten.** *(46 rijen — zie **N7**: de
v12-totalenregel hierboven telt er 43 en klopt niet met zijn eigen kolom, die 0/4/39/3 geeft. De v13-totalen
zijn geteld over de tabel zoals hij hier staat.)*

**De drie wrijvingsrijen waar de ronde om draaide zijn alle drie groen geworden** — A3's tagvergelijking,
B4b en B5b. De enige overgebleven wrijving is de A2-rij die v12 al vond en die niet in de vier reparaties zat.

## De vier reparaties van `v3.1.2`

| # | Wat er gerepareerd is | Waar | Uitkomst | Toelichting |
|---|---|---|---|---|
| #371 | de ijkpunt-tabel wordt gegenereerd, met beide regelconventies als eigen rij | stap 0 | **geverifieerd** | **Alle 8 rijen kloppen** tegen mijn eigen meting: 1105 / 1127 / 22 delta / 22 terminated / 23 posities / 15 via `Measure-Object` / 4 commits / `50ec727`. Plus 0 tags, niet shallow, `README.md` enige tracked file. De *how measured*-kolom deed precies zijn werk: 22 en 23 waren allebei goed en ik hoefde niet te raden |
| #372 | de clone draagt wél tags, en een geannoteerde tag moet met `^{}` gepeeld worden | A3 | **geverifieerd** | Valse *"no tags"*-clausule **weg**; de derde uitkomst (tag-object) staat er mét uitgewerkt `v3.1.1`-voorbeeld; `^{}`-peeling expliciet; `gh api`-route als immuun benoemd. **Mijn clone was shallow, main-only, 0 tags** → `fatal: ambiguous argument`, wat het document *"evidence of nothing"* noemt en dat klopt. Het stuurde me naar de immune route, die de **juiste conclusie om de juiste reden** gaf: `101d597` ≠ `ad84ef9`, ik draai `main` |
| #373 | `teardown.ps1` overleeft stap 2; stap 4 biedt de her-run uit de cache | B1 / B5b | **geverifieerd** | **Daadwerkelijk gemeten na stap 2: beide bestanden bestaan nog** — `teardown.ps1` 57.244 bytes, `UNINSTALL.md` 30.492 bytes, cache-map aanwezig. De juiste reden staat er nu (cache volgt de marketplace, niet de installatie), en de her-run uit de cache is **uitgevoerd** en werkte: `0 item(s) to remove, 2 kept`, `[FREE]`. Zie **N1** voor wat die her-run zichtbaar maakte |
| #374 | de lege-sleutel-waarschuwing is voorwaardelijk; de byte-claim is een reeks | B4b | **geverifieerd** | Beide passages kloppen voor mijn pad. `~/.claude/settings.json` bestond **niet** vóór en niet ná; `known_marketplaces.json` 275 → **2 bytes `{}`**; `marketplace list` → `No marketplaces configured`. Het spiegelbeeld is nu een tweekolomstabel die een reeks afbakent, mét *"read your own numbers against your own starting point"*. **Mijn eindmeting valt exact op de rechterkolom** (35 / 2 / afwezig). **Ja** — een volgende ronde mag een volledig schone stap-0-tabel als normaal rapporteren; het document zegt dat nu met zoveel woorden |

**Vier van vier gehouden.** Niet gemeten ≠ niet gerepareerd: buiten scope, aan de bronkant, blijven de
craft-regel van #378 en poort 16 van #379.

## De metingen die blijven liggen

| # | Wat | Hoe lang al | Uitkomst v13 |
|---|---|---|---|
| #323 | de padloze `user`-vorm als `[RECORD-SHAPE]` | v10, v11, v12 niet opgetreden | **niet gemeten — de voorwaarde deed zich niet voor.** Vierde ronde. Elk record dat deze ronde geschreven is (door de CLI én door een sessiestart) was `project` mét `projectPath`. Niet op gejaagd |
| #337.6 | een `permissions`-entry die naar de plugin-map wijst | v11, v12 voorwaarde deed zich niet voor | **niet gemeten — de voorwaarde deed zich niet voor.** Derde ronde. Geen `permissions`-blok in `.claude/settings.json`, niet in `.claude/settings.local.json`, en `~/.claude/settings.json` bestaat niet. De reden is structureel: het gedocumenteerde pad laat `settings.suggested.jsonc` ter review staan in plaats van hem te kopiëren, en de teardown verwijdert dat voorstel zelf |
| — | de **skill-wrapper** `/specialists-init` interactief | nog nooit gemeten (v11 + v12 draaiden `bootstrap.ps1` direct) | **🟢 GEMETEN EN GROEN — de opbrengst van deze ronde.** Zie hieronder |

**De wrapper, uitgesplitst, want dit is waar de ronde voor was.** De skill laadde, drukte zijn volledige
pagina af, en het gedrukte `powershell -NoProfile -File …bootstrap.ps1` **liep door** — exact dezelfde
aanroepvorm die headless werd geweigerd met *"nested process, not validatable"*.

**Harnasgrens of bevinding over de plugin? Ondubbelzinnig een harnasgrens**, en preciezer dan v11/v12
konden zeggen. De headless poging van deze ronde toonde aan dat de skill zelf prima laadt en start
(`specialists:specialists-init` stond in de slash-lijst, alle drie de session-hooks meldden zich, de skill
deed zijn eigen 0a/0c-controles en kwam correct uit op *"fresh-consumer path"*), en dat hij **weigerde de
bootstrap met de hand na te bouwen** met een expliciete motivering. Het breekpunt was uitsluitend het
uitvoeren van een script in een non-interactieve sessie. **Er is niets aan de plugin te repareren.**

`Done: 4 persona-lens(es) created, 0 already present; 15 lens-scaffold(s) created, 0 already present;
2 script-scaffold(s) created, 0 already present.` — de voorspelde `2 … created, 0 already present` exact
gehaald (#358), en het getal 19 komt in drie onafhankelijke plekken terug (lenzen, `[ROSTER-PENDING]`,
connector-manifest).

## Nieuwe bevindingen

| # | Klasse | Stap | Wat | Issue |
|---|---|---|---|---|
| **N1** | **1** | B5b / stap 4 | **De her-run die #373 nieuw voorschrijft drukt een onware regel af.** `teardown.ps1` voegt de note *"The plugin install itself is untouched: run `claude plugin uninstall … --scope project`"* **onvoorwaardelijk** toe (`$notes += …`, geen test), terwijl de note erboven over `settings.json` wél gegate is op bestandsinhoud. Op de stap-4-her-run — ná een geslaagde uninstall — leest een consument dus dat de installatie onaangeroerd is, met het advies een commando te draaien dat hij net heeft gedraaid. Nieuw bereikbaar **doordat** #373 die her-run tot voorgeschreven stap maakte. Fix ligt voor de hand: gate op dezelfde `projectPath`-query die het document zelf twee keer afdrukt | nieuw |
| **N2** | **1** | B6 | **"Disarmed, stray key or not" houdt niet.** Het document zegt: *"Walk it through to the end of Step 5 and the mechanism is disarmed, stray key or not."* Gemeten ná een volledige teardown t/m stap 5, met **beide** sleutels achtergelaten: sessie 1 herregistreerde de marketplace en herbouwde de clone (geen record); **sessie 2 schreef een volledig `project`-record** (`installedAt 19:13:20.051Z`) terwijl ze zelf niets laadde. Twee sessiestarts en de machine staat weer op een install. De ontmanteling hangt dus aan **stap 3 die béíde sleutels weghaalt**, niet aan stap 5. Met alleen `enabledPlugins` blijft het wél ontmanteld — dat deel is bevestigd. **Tweede helft:** dat record werd geschreven terwijl de **cache afwezig** was, wat de door v12 vastgepinde voorwaarde (*"marketplace registered **and** cache present"*) tegenspreekt, en zijn `installPath` wees naar een map die niet bestond — een dangling record | nieuw |
| **N3** | **1** (klein) | A4 | **De `[UNREGISTERED]`-belofte is onvoorwaardelijk maar geldt voorwaardelijk.** `specialists-init` stap 6 zegt: *"Until it is registered, this repo's own session start says so — `connector-sessioncheck` surfaces an `[UNREGISTERED]` line."* Deze repo was niet geregistreerd en kreeg **geen** `[UNREGISTERED]`; de hook zei *"no verified workshop checkout found on this machine -- check skipped"*. Hij slaat zichzelf over vóórdat hij aan registratie toekomt. De normale consument — zonder workshop-checkout — krijgt de herinnering dus nooit, en dat is precies de lezer voor wie stap 6 het makkelijkst blijft liggen | nieuw |
| **N4** | **1** (klein) | A6 | **`RELEASE.md` regel 8 zegt onvoorwaardelijk "You are on this release."** Voor mij aantoonbaar onwaar: mijn payload komt van `ad84ef9` (`main`), drie commits ná `v3.1.2`. Inhoudelijk maakt het niets uit (die commits raken de payload niet — gemeten: alle vier persona-bodies byte-identiek tussen clone en cache), maar het is een kale bewering op precies het punt waar de QUICKSTART zelf zegt dat *"the documented update path cannot deliver a tagged release"*. De kaart kan dit niet weten en zou het dus niet moeten stellen | nieuw |
| **N5** | **1** (klein) | A2 | **De #336-waarschuwing is te sterk.** Het document zegt over het hash-paar: *"those two hashes are that profile's and are not something to match — yours will differ from each other too, at different values."* **Beide van mijn hashes zijn identiek aan de gedrukte** (224 → 246 bytes, +22). Dat is geen toeval: wie het gedrukte blok letterlijk in een leeg bestand plakt krijgt dezelfde bytes en de serialiser is deterministisch. Op het voorgeschreven pad is het paar juist **reproduceerbaar**, en dat is bruikbaarder dan *"niet iets om te matchen"* | nieuw |
| **N6** | **2** (klein) | B4a | **Een `.claude/settings.json` die tot `{}` is teruggebracht staat niet in *"What is left behind, honestly"*.** Voor een verse consument bestond dat bestand vóór de adoptie niet — QUICKSTART stap 1 laat hem het aanmaken. Stap 3 zegt de sleutels te verwijderen, wat `{}` overlaat, en die lijst van vijf leftovers noemt hem niet. Klein, maar het is de enige repo-leftover naast `CLAUDE.md` en die is er wél gedekt | nieuw |
| **N7** | **1** (klein) | de papieren zelf | **De v12-totalenregel klopt niet met zijn eigen kolom.** Geteld uit de tabel: **0 / 4 / 39 / 3 = 46 rijen**. De regel eronder zegt **0 / 3 / 38 / 2 = 43** — één te weinig in *elke* categorie. De opdracht erft dezelfde fout: *"Alle drie de wrijvingsrijen (A3's tagvergelijking, B4b, B5b)"* laat de vierde 🟡-rij weg (*A2 — de lege `Available marketplaces`-lijst*). Dit is exact de klasse die #371 afsloot voor de ijkpunt-tabel, in de tabel ernaast die **niet** gegenereerd wordt — en de opdracht waarschuwt er zelf voor: *"Blijf de rest van dezelfde klasse wél met de hand bewaken."* Kandidaat voor dezelfde behandeling: laat `round-baseline.measure.ps1` (of een broertje) ook de totalenregels rekenen | nieuw |
| **N8** | **3** (klein) | stap 0 / B2f | **"Wordt alleen geteld als prose" overdrijft.** De stap-0-tekst zegt dat de fixture-`README.md` buiten de audit valt *"en alleen geteld wordt als prose"*. Gemeten: de audit scant `CLAUDE.md`, `.claude/**` en `scripts/**`, en `README.md` valt buiten die wortels — hij wordt **helemaal niet geteld** (`scanned 3 file(s)`, en die drie zijn `CLAUDE.md` + de twee settings-bestanden). De check zelf slaagt ruimschoots; alleen de formulering belooft een telling die niet plaatsvindt | nieuw |
| **N9** | observatie | B6 | **Een door een sessiestart geschreven record heeft een andere sleutelvolgorde dan een door de CLI geschreven record.** CLI-install: `scope, projectPath, installPath, version, …`. Zelfgeschreven: `scope, installPath, version, installedAt, lastUpdated, gitCommitSha, projectPath` — `projectPath` **achteraan**. Twee keer gereproduceerd. Dat is een gratis vingerafdruk naast `installedAt`, en het is er precies één die géén hand-edit van `installed_plugins.json` vereist om af te lezen | nieuw |
| **N10** | observatie, **niet geïsoleerd** | opruiming | **De uitgepakte cache kwam terug tijdens het opruimen.** Na B6c was `cache/davekjohns-workshop/` afwezig; na de drie herstelcommando's (`uninstall --scope project`, sleutels wissen, `marketplace remove`) stond de **volledige payload** er weer, 1.086.067 bytes, met een verse `.orphaned_at`. Welk van de drie het deed heb ik **niet** geïsoleerd, dus dit is een observatie en geen bevinding. Vermoeden: de uninstall tegen een dangling record pakt eerst opnieuw uit. In de gedrukte volgorde doet dit zich niet voor (uninstall is stap 2, cache-delete stap 5) | nieuw |
| **N11** | **4** | fixture | **De fixture-README wordt tijdens een ronde onwaar over zichzelf.** `README.md:3` zegt *"Vers, leeg, geen `.claude/`"* — vanaf A1 klopt dat niet meer. Een verse sessie ná de teardown merkte het **uit zichzelf** op, naast de `CLAUDE.md`-tegenspraak. Geen bevinding tegen de plugin; wel iets voor wie v14 schrijft, want het is een tweede stem die tegen de meter in praat op het moment dat hij `[FREE]` probeert vast te stellen | nieuw |
| **N12** | **1** (klein) | B2e | **Het document citeert een betrouwbaarder resultaat dan twee rondes daarna meten.** *"Measured in round v11: two separate fresh sessions flagged the contradiction unprompted."* Correct voor v11, maar **v12 en v13 meten allebei 1 op 2**. Een lezer die op die zin kalibreert overschat hoe zeker de sessie het zelf opmerkt. Het document behandelt de rij gelukkig al als **to-do** en niet als notitie, dus het advies klopt; alleen de bewijsvoering is inmiddels optimistischer dan de reeks | nieuw |

## Scope — lees dit vóór je iets hierboven als groen leest

- **Was dit A′?** **Ja.** De uitvoering is aan een sessie overgedragen, zoals v10 t/m v12. Dit meet
  **AI-geassisteerde adoptie**, niet hand-gelopen adoptie. Dave heeft de rolverdeling aan het begin van de
  ronde expliciet zo gekozen.
- **Sessiestarts headless of interactief?** **Beide, en dat is deze ronde relevant.** De bootstrap (A4) is
  **interactief** gedraaid — dat was de hele opzet. De verificatiesessies van A5, B2e, stap 4 en B6 zijn
  **headless** (`claude -p`) gedraaid. Headless komt de hookuitvoer in de modelcontext en niet op stdout,
  dus de `[ERROR]`/`[ROSTER-PENDING]`-telling is gedaan door de sub-sessie te vragen die meldingen
  **letterlijk te citeren in een codeblok**; de pre-bootstrap `[BOOTSTRAP]`-varianten heb ik daarnaast
  **direct** kunnen lezen, want die kwamen in de sessiestart van deze interactieve sessie zelf.
- **Skill-wrapper of `bootstrap.ps1` direct?** **De wrapper**, interactief, en hij werkte. Headless liep hij
  vast op de permission-classifier bij het uitvoeren van het script — niet bij het aanroepen van de skill.
- **Welk deel van het pre-stap-1-blok is opnieuw gemeten?** Alleen de `ExecutionPolicy`, en dan indirect
  (elk `.ps1` liep zonder `PSSecurityException`). Claude Code stond al, PATH en editor-herstart zijn niet
  opnieuw gemeten. Geen verse installatie.
- **Wat is er met het profiel gedaan?** **Volledig opgeruimd, en nagemeten.** Alle zes rijen van de
  stap-0-tabel staan weer op afwezig/leeg: `installed_plugins.json` 35 bytes `{"version": 2, "plugins": {}}`,
  `known_marketplaces.json` 2 bytes `{}`, `~/.claude/settings.json` afwezig, clone/cache/data weg,
  `marketplace list` → `No marketplaces configured`. Onder `marketplaces/` staat alleen nog
  `claude-plugins-official`, dat er bij stap 0 ook al stond en niet van deze familie is. **De fixture-repo is
  ook teruggezet**: `CLAUDE.md` en `.claude/` verwijderd, waarna de ijkpunt-tabel weer exact klopt
  (1105 / 1127 / 22 / 4 commits / `50ec727` / 0 tags / `git status` leeg). **v14 heeft een geldige nulstand.**

## Wat deze ronde bewust niet gemeten heeft

Overgenomen uit de opdracht, zodat de bron het niet als groen leest: de teardown-regressie in twee cycli
(dat is de bezette-consumertest; `life-hub` is niet aangeraakt), een vers **apparaat** (zelfde machine, CLI,
git en OS — dit was een verse *gebruiker*), de poorten en checks bij de bron (#378, #379 — van buiten
onzichtbaar), het script achter de ijkpunt-tabel zelf (`round-baseline.measure.ps1` staat in de workshop; de
tabel is geverifieerd met de commando's uit zijn eigen *how measured*-kolom), de aantallen-sweep door de
documentatie, en het #283-CRLF-artefact (deze repo heeft geen `.gitignore` en er is er geen gebouwd).

## Nog één ding, voor wie v14 schrijft

**De ronde die de reparaties allemaal ziet houden, is de ronde waarin je de nieuwe fouten in de reparaties
zelf vindt — en die zaten dit keer niet in de tekst maar in wat de tekst nu voorschrijft.** Vier van vier
hield, de drie wrijvingsrijen werden groen, en de wrapper die twee rondes ongemeten bleef bleek een
harnasgrens en geen plugin-probleem. De twee scherpste bevindingen komen uit handelingen die dit document
**pas sinds `v3.1.2` voorschrijft**: N1 bestaat alleen omdat #373 de her-run uit de cache tot stap
verhief, en N2 werd zichtbaar omdat de vraag *"is het echt ontmanteld?"* pas gesteld kon worden nu het
document beweert van wel. Een reparatie voegt gedrag toe, en dat gedrag is ongemeten tot iemand het loopt.
Plan in v14 dus expliciet een pas langs de **stappen die in de vorige release nieuw zijn**, in plaats van
alleen langs de bevindingen die ze moesten oplossen. En N7 is de stille herinnering ernaast: #371 heeft de
ijkpunt-tabel bij de bron ondergebracht, maar de tabel eronder — de stappentabel met zijn totalenregels —
wordt nog steeds met de hand geteld, en klopt niet.
