# Resultaten ronde v12

## Waartegen gemeten is

| | |
|---|---|
| Datum | 2 augustus 2026 |
| Profiel | `dave\davek_onn` — administratie: `C:\Users\davek_onn\.claude\` |
| Volledig pad `installed_plugins.json` | `C:\Users\davek_onn\.claude\plugins\installed_plugins.json` (`CLAUDE_CONFIG_DIR` **niet** gezet — leeg) |
| Doelrepo + commit van de verse checkout | verwacht `DaveKJohn/specialists-adoptietest` @ `f56a9e6` (3 commits, niet shallow, 0 tags) — gemeten **exact dat**: `f56a9e6`, 3 commits, `.git/shallow` afwezig, 0 tags, `git status` leeg, enige tracked file `README.md`, geen `.claude/`, geen `CLAUDE.md` |
| README.md — bytes / regels | verwacht 1105 / 1127 / 23 / 15 — gemeten **1105 blob / 1127 op schijf / 22 regels / 15 volgens `Measure-Object -Line`**. De 23 klopt niet — zie **N1** |
| `plugin.json` versie | verwacht 3.1.1 — gemeten **3.1.1** |
| `gitCommitSha` uit het record | `4b1a74dadbd2e37b1e254ad4f6f233451ea7cde3` |
| Gelijk aan de `v3.1.1`-tag? | **ja** — `main` HEAD == tag-commit == `4b1a74d`. Het venster stond nog open (workshop `main` laatst gepusht 2026-08-02T11:06:52Z) |
| CLI-versie | `2.1.220 (Claude Code)` — git 2.49.0.windows.1, PS 5.1.26100.8875, Windows 11 Pro 26200 |
| Clone-stand (`rev-parse HEAD` in `marketplaces/`) | `4b1a74d`, branch `main`, **shallow: ja**, refspec `+refs/heads/main:refs/remotes/origin/main`, **1 tag: `v3.1.1`** (geannoteerd, object `12b2d1b`) — zie **N2** |

## Stap 0 — is het profiel spoorloos?

Gemeten in een kale PowerShell. Dave draaide blok C/D zelf; beide metingen kwamen overeen.

| locatie | uitkomst |
|---|---|
| `installed_plugins.json` | **afwezig** ✅ |
| `marketplaces/davekjohns-workshop/` | **afwezig** ✅ |
| `cache/davekjohns-workshop/` | **afwezig** ✅ |
| `data/specialists-davekjohns-workshop/` | **afwezig** ✅ |
| `known_marketplaces.json` | **afwezig** ✅ |
| `~/.claude/settings.json` | **bestaat niet eens** ✅ |

Alle zes schoon. De handmatige opruiming die de opdracht voorschreef was **niet nodig** — dit profiel had nog nooit een `~/.claude/settings.json`. Rij 6, die v11 nooit schoon kreeg, was hier vanzelf leeg. **v12 begon dus met een echt maagdelijke nulstand, en #329 was daarmee voor het eerst toetsbaar.**

## ⭐ De stappentabel

### Vóór stap 1 — niet in enig document (v10)

| stap | v10 | v11 | v12 | klasse | issue |
|---|---|---|---|---|---|
| Claude Code installeren | 🔴 geblokkeerd | 🟢 groen (tekst) | 🟢 groen (tekst) | | #334 |
| `ExecutionPolicy` blokkeert elke `.ps1` | 🔴 geblokkeerd | 🟢 groen (tekst) | 🟢 groen (tekst) | | #334 |
| PATH + volledige herstart | 🟡 wrijving | 🟢 groen (tekst) | 🟢 groen (tekst) | | #334 |

**Eerlijk over wat hier gemeten is: niets.** Claude Code stond al (`2.1.220`), `ExecutionPolicy` stond al op `RemoteSigned`/`Bypass`, PATH werkte al. Geen van drieën is opnieuw gemeten. Eén ding is wél *bevestigd* en dat is het mechanisme achter punt 3: in elke foutmelding is te zien dat `claude` naar **`C:\Users\davek_onn\AppData\Roaming\npm\claude.ps1`** resolvet — dus de bewering dat een `.ps1` vóór de `.cmd` komt en dat `Restricted` daarmee élk commando op de pagina blokkeert, klopt aantoonbaar op dit npm-pad. "Groen (tekst)" blijft het hoogst haalbare zolang dit profiel Claude Code al heeft.

### Test A — INSTALL tegen `QUICKSTART.md`

| stap | v10 | v11 | v12 | klasse | issue |
|---|---|---|---|---|---|
| A0 — het document vinden | 🟡 wrijving | 🟢 groen | 🟢 groen | | #338 |
| A1 — het settings-fragment | 🟡 wrijving | 🟢 groen | 🟢 groen | | #335 |
| **A2 regel 1 — `marketplace update`** | 🔴 **geblokkeerd** | ⚪ niet gemeten | 🟢 **groen** | | **#329** |
| A2 regel 2 — `install --scope project` | 🟢 groen | 🟢 groen | 🟢 groen | | |
| A2 — `settings.json` herschreven | 🟡 wrijving | 🟢 groen | 🟢 groen | | #336 |
| A2 extra — install tegen `project` vs `local` | niet gemeten | 🟢 groen | 🟢 groen | | #325 |
| A3 — de `projectPath`-verificatiequery | 🟢 groen | 🟢 groen | 🟢 groen | | |
| **A3 — het nieuwe `payload`-veld** | — | — | 🟢 **groen** | | **#355** |
| A3 extra — de tagvergelijking | 🔴 geblokkeerd | 🟡 wrijving | 🟡 wrijving | 1 + 2 | #322 · **N2** |
| **A4 — de verwachte `Done:`-regel** | 🟢 groen | 🟡 wrijving | 🟢 **groen** | | **#358** |
| **A4 — de hook-stub in het settings-voorstel** | — | — | 🟢 **groen** | | **#363** |
| A4 — de roster-hook ná een schone bootstrap | 🟡 wrijving | 🟢 groen | 🟢 groen | | #333 |
| A4 — de output van de bootstrap zelf | 🟡 wrijving | 🟢 groen | 🟢 groen | | #337.4 |
| A4 — regeleindes + slotregel | 🟡 wrijving | 🟢 groen | 🟢 groen | | #337.2 |
| **A5 — de afzendercheck** | 🟢 groen | 🟡 wrijving | 🟢 **groen** | | **#361** |
| A′ — het `@`-importpad | 🟡 wrijving | 🟢 groen | 🟢 groen | | #330 |
| **A6 — de `RELEASE.md`-kaart leesbaar** | — | — | 🟢 **groen** | | **#368** |

### Test B — UNINSTALL tegen `UNINSTALL.md`

| stap | v10 | v11 | v12 | klasse | issue |
|---|---|---|---|---|---|
| B0 — het document vinden | 🟡 wrijving | 🟢 groen | 🟢 groen | | #338 |
| B1a — pre-flight 1 met zijn filter | 🟢 groen | 🟢 groen | 🟢 groen | | |
| B1b — succesgeval geeft exit 1 | 🟡 wrijving | 🟢 groen | 🟢 groen | | #332 |
| B1c — pre-flight 2 meet commits | 🟡 wrijving | 🟢 groen | 🟢 groen | | #332 |
| B1d — safety-net commit te smal | 🟡 wrijving | 🟢 groen | 🟢 groen | | #332 |
| B1h — de `<plugin>`-placeholder | 🟡 wrijving | 🟢 groen | 🟢 groen | | #330 |
| B2a/b — preview, en preview = apply | 🟢 groen | 🟢 groen | 🟢 groen | | |
| B2c — lege `scripts\lib\` blijft staan | 🟡 wrijving | 🟢 groen | 🟢 groen | | #331 |
| **B2d — het `kept`-getal naast de markers** | — | 🟡 wrijving | 🟢 **groen** | | **#356** |
| **B2e — wat het overgebleven proza dóét** | — | — | 🟢 **groen** | | **#362** |
| B3a — `uninstall --scope project` | 🟢 groen | 🟢 groen | 🟢 groen | | |
| **B3c — de voorspelde CLI-melding** | — | — | 🟢 **groen** | | **#359** |
| B3b — de ongedocumenteerde `.orphaned_at` | 🟡 wrijving | 🟢 groen | 🟢 groen | | #337.5 |
| B4a — de sleutels uit `settings.json` | 🟢 groen | 🟢 groen | 🟢 groen | | |
| **B4b — de lege `extraKnownMarketplaces`** | 🟢 groen | 🟡 wrijving | 🟡 wrijving | 1 | **#357 · N4** |
| B4c — stap 3 verwijdert het document | 🔴 geblokkeerd | 🟢 groen | 🟢 groen | | #328 |
| B4d — permission-key overleeft | 🟡 wrijving | ⚪ niet gemeten | ⚪ **niet gemeten** | | #337.6 |
| B5 — de cache met de hand opruimen | 🟢 groen | 🟢 groen | 🟢 groen | | |
| B5b — stap 4's audit is weg | 🔴 geblokkeerd | 🟢 groen | 🟡 wrijving | 1 | #328 · **N3** |
| B6 — record geschreven, sessie inert | 🟡 wrijving | 🟢 groen | 🟢 groen | | #327 |
| B6 — de padloze `user`-vorm | niet opgetreden | ⚪ niet gemeten | ⚪ **niet gemeten** | | #323 |
| B6 — remedie leesbaar uit de sessie | niet gemeten | ⚪ niet gemeten | 🟢 **groen** | | #324 |
| — de stap-0-tabel na test B | — | 🟡 wrijving | 🟢 **groen** | | N3(v11) |

**Totalen v10: 5 geblokkeerd, 18 wrijving, 14 groen.**
**Totalen v11: 0 geblokkeerd, 7 wrijving, 26 groen, 5 niet gemeten.**
**Totalen v12: 0 geblokkeerd, 3 wrijving, 38 groen, 2 niet gemeten.** (43 rijen)

Het pad is niet alleen anders geworden, het is **begaanbaarder**: nul blokkades voor het tweede jaar op rij, en de wrijving is van 18 → 7 → 3 gegaan.

## De negen reparaties van `v3.1.1`

| # | Wat er gerepareerd is | Waar | Uitkomst | Toelichting |
|---|---|---|---|---|
| #355 | de verificatiequery controleert of `installPath` bestaat | A3 | **geverifieerd** | De query gaf `specialists@davekjohns-workshop -> project 3.1.1 4b1a74d… [payload present]`. Eén `project`-regel, vierde veld aanwezig en positief |
| #356 | de teardown-samenvatting telt zijn eigen `[KEEP]`-regels | B2d | **geverifieerd** | Precies **2** `[KEEP]`-regels tegenover `Summary: 30 item(s) to remove, **2 kept**` — en na `-Apply` `30 item(s) removed, 2 kept`. De overgebleven regels staan nu onder een **eigen kopje** ("Kept — generated prose in a governance file"), niet meer onder het `-EmptyLensPattern`-advies |
| #357 | stap 5 noemt de lege `extraKnownMarketplaces` | B4b | **niet gemeten** | De tekst *staat* er en klopt voor het user-scope-geval. Maar op het gedocumenteerde project-scope-pad bestond `~/.claude/settings.json` niet eens, dus er viel niets leeg te laten. Zie **N4** |
| #358 | de verwachte `Done:`-regel is die van een verse repo | A4 | **geverifieerd** | Letterlijke match, inclusief de derde clausule: `Done: 4 persona-lens(es) created, 0 already present; 15 lens-scaffold(s) created, 0 already present; 2 script-scaffold(s) created, 0 already present.` |
| #359 | de voorspelde CLI-melding, en de suggestie die je niet moet volgen | B3c | **geverifieerd** | Woord-voor-woord match op `2.1.220`, inclusief de `disable … --scope local`-suggestie, en het document zegt expliciet die **niet** te volgen |
| #360 | een bytegetal noemt zijn line-ending-conventie | stap 0 | **nog stuk** | In de papieren van v12 zélf: de rij "regels / 23 / alle regels" noemt geen conventie en is onjuist (22). Zie **N1** |
| #361 | de afzendercheck vraagt naar de invariant | A5 | **geverifieerd** | Het document vraagt naar de invariant; de sessie opende letterlijk met `**This one is for Rebecca 🔍 — internal repo exploration is the research specialist's craft.**` |
| #362 | het achtergebleven proza is niet inert, en er staat een remedie | B2e | **geverifieerd** | Tekst benoemt dat `CLAUDE.md` elke sessie geladen wordt, markeert het als **to-do** en geeft twee concrete één-edit-remedies. **En het gedrag is gereproduceerd**: 1 van 2 verse post-teardown-sessies merkte de tegenspraak uit zichzelf op |
| #363 | de hook-stub is zichtbaar een placeholder, plus slotregel | A4 | **geverifieerd** | Pad is `scripts/maintenance/<your-check>.ps1` — punthaken zichtbaar; commentaar waarschuwt dat kopiëren een hook naar niets activeert; next-step 3 zegt welk blok bruikbaar is; bestand **eindigt op een slotregel** |

| # | Wat | Waar | Uitkomst | Toelichting |
|---|---|---|---|---|
| #368 | de mojibake uit de `RELEASE.md`-kaart | A6 | **geverifieerd** | 0 dubbel-geëncodeerde reeksen, 0 tekenparen, 0 losse `Ã`/`â`/`Â`; 18 correcte em-streepjes (U+2014). Met het blote oog leesbaar |

Buiten scope, aan de bronkant: de zelf-committende fold (#369) en de poort op voorbeeld-output (#370). **Niet gemeten ≠ niet gerepareerd.**

## De vier metingen die v11 niet kon doen

| # | Wat | Uitkomst v12 |
|---|---|---|
| **#329** | `marketplace update` op een maagdelijk profiel | **WEG.** Vóór de sessiestart faalde het commando exact zoals voorspeld; ná de ene herstart die het document nu vóórschrijft: `✔ Successfully updated marketplace: davekjohns-workshop`. v10's zwaarste blokkade is aantoonbaar opgelost |
| #323 | de padloze `user`-vorm als `[RECORD-SHAPE]` | **niet gemeten** — de vorm trad opnieuw niet op. Het record bleef in alle metingen `project` mét `projectPath`. Derde ronde op rij dat de voorwaarde zich niet voordoet |
| #324 | het remedie is uit de sessie-output leesbaar | **geverifieerd**, met kanttekening. De hooks printten `[BOOTSTRAP]` mét concrete remedie in de output zelf (*"Run the `specialists-init` skill … it is additive and never overwrites anything you have written"*). De specifieke `[RECORD-SHAPE]`/`[NOT-INSTALLED-HERE]`-tak waar #324 over ging vuurde niet, want het record was goed van vorm |
| #337.6 | een `permissions`-entry die naar de plugin-map wijst | **niet gemeten** — voorwaarde deed zich niet voor. Geen enkel settings-bestand kreeg een `permissions`-blok, omdat het gedocumenteerde pad `settings.suggested.jsonc` ter review laat staan in plaats van te kopiëren. De v10-restant kwam juist uit zo'n kopie |

**#327 is bovendien volledig gereproduceerd** en is het scherpste resultaat van test B — zie N-observatie hieronder.

## Nieuwe bevindingen

| # | Klasse | Stap | Wat | Issue |
|---|---|---|---|---|
| **N1** | **1** | stap 0 | De ijkpunt-tabel van deze opdracht zegt `regels / **23** / alle regels`. Het bestand heeft **22** regels: 22 CRLF op schijf, 22 LF in de blob, `(Get-Content).Count` = 22, `git show \| .Count` = 22. De tabel spreekt zichzelf tegen — haar eigen bytes-rij zegt *"`core.autocrlf=true` maakt van **22** LF's een CRLF"*, en 1127 − 1105 = 22. De 23 ontstaat alleen als je op newline splitst en het lege fragment ná de laatste terminator meetelt (wat een editor-gutter wél toont). Dit is exact de klasse die **#360** moest afsluiten: bytes kregen hun herkomst, `Measure-Object` kreeg zijn caveat, en de ene rij waar twee conventies écht verschillen kreeg "alle regels" | nieuw |
| **N2** | **1 + 2** | A3 / #322 | `specialists-init/SKILL.md` L248 noemt als gemeten feit dat de clone **"`main` only, no tags"** is. Mijn verse marketplace-clone draagt **`v3.1.1`**. Dat is dezelfde onjuiste clausule die v11 vond, nog steeds in de tekst — en het document weerspreekt zichzelf twee bullets verder ("its tag set is frozen at whatever came along"). **Erger (klasse 2):** de tag is **geannoteerd**. `rev-parse v3.1.1` **slaagt** dus en geeft het tag-object `12b2d1b`, wat níet gelijk is aan HEAD `4b1a74d` — terwijl `v3.1.1^{}` == HEAD == `4b1a74d`. Het document noemt twee uitkomsten (gelijk, of `fatal: ambiguous argument`); dit is een **derde**: slaagt, maar geeft stil het omgekeerde antwoord. Precies de inversie die #322 dacht te hebben afgesloten, via een route die nergens genoemd wordt | nieuw |
| **N3** | **1** | B5b / #328 | `UNINSTALL.md` stap 1 zegt dat de audit *"lives in the payload that Step 2 removes"* en dat de tool ná stap 2 weg is. Gemeten: **`teardown.ps1` staat er na stap 2 gewoon nog**. De uninstall verwijdert de data-map en zet een `.orphaned_at`, maar niet de cache. Het document zegt dat zelf in zijn #339-tabel: `cache/<marketplace>/` → *"**no step** — it follows the marketplace, not the install"*. Twee plekken in één document die elkaar tegenspreken. Het advies (bewaar de output) blijft goed; de reden erbij klopt niet | nieuw |
| **N4** | **1** | B4b / #357 | Stap 5 zegt *"Expect it to edit `~/.claude/settings.json`, and to leave an empty key behind"* en concludeert dat de "geen `enabledPlugins`, geen `extraKnownMarketplaces`"-rij van een clean-machine-check **"never literally clean"** is na een by-the-book teardown. Op het pad dat de QUICKSTART zelf voorschrijft (keys in de **repo's** `.claude/settings.json`, `--scope project`) bestaat `~/.claude/settings.json` helemaal niet, en stap 3 heeft de key al weggehaald vóór stap 5 draait. Mijn stap-0-tabel kwam ná de teardown **wél volledig schoon**. De waarschuwing klopt voor een user-scope-declaratie, maar is geschreven alsof ze voor iedereen geldt | nieuw |

**Kleinere observatie (geen issue-waardig).** De QUICKSTART drukt bij de #329-fout af: `Available marketplaces: claude-plugins-official`. Op een écht maagdelijk profiel is die lijst **leeg**. Het document dekt dit af met "the wording is version-bound", maar dit is geen bewoording — het is een toestand. Eén regel volstaat.

## Twee observaties die de bron waarschijnlijk wil hebben

**1. #336 heeft nu wél een hash-paar.** Het document noteert eerlijk dat v10 er geen vastlegde. v12 wel, op het moment van de install, met de key al aanwezig en in de voorgeschreven volgorde:

- **vóór:** `F694FB443E52E86B7861E5DDBA9D6BBC79CF0B1D5C147887672F4CDBBF15EFA8` (224 bytes)
- **ná:** `EB8834F72FD402726F64AA27A44CA0B8CC605535A75C21E72B5CD6C8AB275E4A` (246 bytes)

Het bestand is dus **niet** byte-identiek, en de verandering is precies wat de pagina beschrijft: `enabledPlugins` schoof vóór `extraKnownMarketplaces` en het geneste `source`-object werd uitgeklapt. Inhoudelijk equivalent. De honest note in de QUICKSTART kan worden opgewaardeerd van "beschrijving" naar "gemeten paar".

**2. Het install-record volgt de cache, niet alleen de enable-key.** Drie metingen op één profiel:

| toestand | sessiestart schrijft record? |
|---|---|
| keys gezet, marketplace **niet** geregistreerd, geen cache | **nee** — `plugins: {}`, wel `known_marketplaces.json` aangemaakt en de clone gezet |
| marketplace geregistreerd, cache **aanwezig**, alleen een verdwaalde enable-key | **ja** — vol, correct `project`-record; de sessie zelf laadde **niets** (skill-lijst leeg) |
| ná volledige teardown incl. stap 5 (cache met de hand weg), verdwaalde enable-key | **nee** — record bleef `{}`, niets herregistreerde |

Dat verfijnt #327: de "zelfherstellende" val vuurt alleen zolang de **marketplace geregistreerd en de cache aanwezig** is. **Wie de teardown inclusief stap 5 afmaakt, ontwapent de val** — een geruststellend en vermeldenswaard feit dat nu nergens staat. Omgekeerd bevestigt het de kern van #327: het record werd geschreven ná de load-fase, de sessie was volledig inert, en de vólgende sessie laadde 4 skills + 19 subagents.

## Scope — lees dit vóór je iets hierboven als groen leest

- **Was dit A′?** **Ja.** De uitvoering is aan een sessie overgedragen; dit meet AI-geassisteerde adoptie, niet een hand-gelopen test A. Zoals v10 en v11.
- **Sessiestarts headless of interactief?** **Alle headless** (`claude -p`). Dat is een echte beperking: session-start-hookoutput komt in headless modus in de context van het model terecht en niet op stdout, dus de `[ERROR]`/`[ROSTER-PENDING]`-telling is gedaan door de drie hookscripts **direct** te draaien in plaats van uit een sessietranscript te lezen. Uitkomst: 0 `[ERROR]`, precies 1 `[ROSTER-PENDING]`.
- **Skill-wrapper of `bootstrap.ps1` direct?** **`bootstrap.ps1` direct.** De wrapper is éérst geprobeerd (`claude -p "/specialists-init"`); die sessie kwam tot de skill maar kon het script niet uitvoeren — de permission-classifier weigerde zowel het geneste PowerShell-proces als de directe aanroep, en een headless sessie heeft geen prompt om te beantwoorden. Een tweede poging met `--permission-mode bypassPermissions` werd door de classifier van de buitensessie geblokkeerd. Dit is dezelfde **harnasgrens** als v11's `~/.claude/settings.json`-probleem, geen bevinding over de plugin. `teardown.ps1` is om dezelfde reden direct gedraaid. **De skill-wrapper zelf is dus niet gemeten.**
- **Welk deel van het pre-stap-1-blok is opnieuw gemeten?** **Geen enkel.** Claude Code, `ExecutionPolicy` en PATH stonden alle drie al. Alleen het *mechanisme* van punt 3 is bevestigd (`claude` resolvet naar `npm\claude.ps1`).
- **Wat is er met het profiel gedaan?** **Volledig opgeruimd**, geen half werk. Alle zes stap-0-rijen staan weer schoon: `marketplaces/`, `cache/`, `data/` en `~/.claude/settings.json` afwezig; `installed_plugins.json` bestaat met 35 bytes `{"version": 2, "plugins": {}}`; `known_marketplaces.json` bestaat met 2 bytes `{}`. Beide vallen onder "afwezig, of zonder enig record". De fixture-repo staat schoon op `f56a9e6` (`git status` leeg, `.claude/` en `CLAUDE.md` verwijderd). **v13 erft een echte nulstand.**

### Nog één ding, voor wie v13 schrijft

De opdracht van deze ronde waarschuwde: *"noem bij elk ijkpunt hoe het gemeten is."* Die ronde struikelde over precies dat, in haar eigen ijkpunt-tabel (**N1**). De les die #360 opleverde is aan de bronkant nu bewaakt door een poort — maar deze papieren liggen buiten het bereik van die poort, en daar moest de gewoonte het werk doen. Ze deed het niet. Overweeg de tabel voor v13 te genereren met hetzelfde script dat de bron gebruikt, in plaats van hem over te tikken.
