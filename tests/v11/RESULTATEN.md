# Resultaten ronde v11

> **Nog niet gedraaid.** Dit bestand is de vorm waarin de uitkomst wordt vastgelegd, met de v10-kolommen al
> ingevuld zodat er straks iets is om tegen af te zetten. Vul in tijdens en na de ronde; laat niets leeg staan
> zonder te zeggen *waarom* — "niet gemeten" is een geldige uitkomst, weglaten is dat niet.

## Waartegen gemeten is

Vul dit **eerst** in, vóór test A. v9 verloor zijn hele nulstandtabel omdat dit ontbrak.

| | |
|---|---|
| Datum | |
| Profiel | `davek_onn` — administratie: `C:\Users\davek_onn\.claude\` |
| Volledig pad `installed_plugins.json` | |
| Doelrepo + commit van de verse checkout | `DaveKJohn/specialists-adoptietest` @ |
| `plugin.json` versie | verwacht **3.1.0** — gemeten: |
| `gitCommitSha` uit het record | |
| Gelijk aan de `v3.1.0`-tag? | verwacht **ja** (`main` en de tag waren `6feac75` toen deze opdracht werd geschreven) — gemeten: |
| CLI-versie | |
| Clone-stand (`rev-parse HEAD` in `marketplaces/`) | |

## Stap 0a — issue #350

De vraag: zijn de twee `claude plugin`-commando's van stap 1 overbodig?

| locatie / meting | uitkomst |
|---|---|
| `installed_plugins.json` na twee herstarts | |
| de andere vijf locaties | |
| **skill-lijst** — zit `specialists-init` erin? | |
| **Conclusie** | |

## Stap 0 — is het profiel spoorloos?

Alle zes moeten **afwezig** zijn. Is er één aanwezig, dan is dát je eerste bevinding.

| locatie | uitkomst |
|---|---|
| `installed_plugins.json` | |
| `marketplaces/davekjohns-workshop/` | |
| `cache/davekjohns-workshop/` | |
| `data/specialists-davekjohns-workshop/` | |
| `known_marketplaces.json` | |
| `~/.claude/settings.json` | |

## ⭐ De stappentabel

Het hoofdproduct. **groen** / **wrijving** / **geblokkeerd**, met de klasse erbij. De v10-kolom staat er al in,
want alleen naast die kolom is te zien of het pad begaanbaarder is geworden in plaats van alleen anders.

### Vóór stap 1 — niet in enig document (v10)

| stap | v10 | v11 | klasse | issue |
|---|---|---|---|---|
| Claude Code installeren | 🔴 geblokkeerd | | | #334 |
| `ExecutionPolicy` blokkeert elke `.ps1` | 🔴 geblokkeerd | | | #334 |
| PATH + volledige herstart | 🟡 wrijving | | | #334 |

### Test A — INSTALL tegen `QUICKSTART.md`

| stap | v10 | v11 | klasse | issue |
|---|---|---|---|---|
| A0 — het document vinden | 🟡 wrijving | | | #338 |
| A1 — het settings-fragment | 🟡 wrijving | | | #335 |
| **A2 regel 1 — `marketplace update`** | 🔴 **geblokkeerd** | | | #329 |
| A2 regel 2 — `install --scope project` | 🟢 groen | | | |
| A2 — `settings.json` herschreven | 🟡 wrijving | | | #336 |
| A2 extra — install tegen `project` vs `local` | niet gemeten | | | #325 |
| A3 — de `projectPath`-verificatiequery | 🟢 groen | | | |
| **A3 extra — de tagvergelijking** | 🔴 **geblokkeerd** | | | #322 |
| A4 — `specialists-init`, de aantallen | 🟢 groen | | | |
| A4 — de roster-hook ná een schone bootstrap | 🟡 wrijving (19 errors) | | | #333 |
| A4 — de output van de bootstrap zelf | 🟡 wrijving | | | #337 |
| A4 — regeleindes + slotregel | 🟡 wrijving | | | #337 |
| A5 — Chris neemt het woord | 🟢 groen | | | |
| A′ — het `@`-importpad | 🟡 wrijving | | | #330 |

### Test B — UNINSTALL tegen `UNINSTALL.md`

| stap | v10 | v11 | klasse | issue |
|---|---|---|---|---|
| B0 — het document vinden | 🟡 wrijving | | | #338 |
| B1a — pre-flight 1 met zijn filter | 🟢 groen | | | |
| B1b — succesgeval geeft exit 1 | 🟡 wrijving | | | #332 |
| B1c — pre-flight 2 meet de index | 🟡 wrijving | | | #332 |
| B1d — safety-net commit te smal | 🟡 wrijving | | | #332 |
| B1h — de `<plugin>`-placeholder | 🟡 wrijving | | | #330 |
| B2a/b — preview, en preview = apply | 🟢 groen | | | |
| B2c — lege `scripts\lib\` blijft staan | 🟡 wrijving | | | #331 |
| B2d — `[FREE]` terwijl prose overleeft | 🟡 wrijving | | | #331 |
| B3a — `uninstall --scope project` | 🟢 groen | | | |
| B3b — de ongedocumenteerde `.orphaned_at` | 🟡 wrijving | | | #337 |
| B4a — de sleutels uit `settings.json` | 🟢 groen | | | |
| B4b — `marketplace remove` | 🟢 groen | | | #339 |
| **B4c — stap 3 verwijdert het document** | 🔴 **geblokkeerd** | | | #328 |
| B4d — permission-key overleeft | 🟡 wrijving | | | #337 |
| B5 — de cache met de hand opruimen | 🟢 groen | | | |
| **B5b — stap 4's audit is weg** | 🔴 **geblokkeerd** | | | #328 |
| B6 — record geschreven, sessie inert | 🟡 wrijving | | | #327 |
| B6 — de padloze `user`-vorm | niet opgetreden | | | #323 |
| B6 — remedie leesbaar uit de sessie | niet gemeten | | | #324 |

**Totalen v10: 5 geblokkeerd, 18 wrijving, 14 groen.**
**Totalen v11:** _geblokkeerd:_ · _wrijving:_ · _groen:_

## De zeventien reparaties

Per stuk één woord: **geverifieerd**, **niet gemeten**, of **nog stuk**. Een lijst zonder dat onderscheid is
voor de bron onbruikbaar.

| # | Wat er gerepareerd is | Waar je het ziet | Uitkomst | Toelichting |
|---|---|---|---|---|
| #322 | tagvergelijking vervangen door iets uitvoerbaars | A3 extra | | |
| #323 | de padloze degradatie wordt gemeld | B6 | | |
| #324 | het remedie is uit de sessie-output leesbaar | B6 | | |
| #325 | trigger versmald naar een scope-mismatch | A2 extra | | |
| #327 | "record aanwezig, sessie inert" heeft een naam | B6 | | |
| #328 | stapvolgorde: document en audit overleven | B4c, B5b | | |
| #329 | het eerste commando is geen doodlopend spoor | A2 | | |
| #330 | importpad + `<plugin>`-placeholder | A′, B1h | | |
| #331 | `[KEEP]` bij overlevende prose, lege map weg | B2c, B2d | | |
| #332 | pre-flight meet commits, niet de index | B1b, B1c, B1d | | |
| #333 | roster-hook leest de seam, één `[ROSTER-PENDING]` | A4 | | |
| #334 | prerequisites vóór stap 1 | vóór A1 | | |
| #335 | settings-fragment is geldige JSON | A1 | | |
| #336 | byte-identiek-claim versmald | A2 | | |
| #337 | zes papercuts (aantallen, LF, gitignore, `.orphaned_at`, permission-key, één import) | A4, B3b, B4d | | |
| #338 | de documenten zijn vindbaar | A0, B0 | | |
| #339 | `marketplace remove` — vraag beantwoord en tot regel gemaakt | B4b, B5 | | |

## Nieuwe bevindingen

Per bevinding één issue bij de bron, label `inbound`. Hier de lijst met nummer, klasse en één regel.

| issue | klasse | stap | één regel |
|---|---|---|---|
| | | | |

## Wat deze ronde niet bewees

Overschrijven uit de opdracht, plus wat er onderweg bij kwam. Dit als groen lezen is de dure fout.

-

## Het profiel daarna

Opgeruimd of laten staan? **Zeg welke van de twee** — een half opgeruimd profiel is voor de volgende ronde
geen nulstand meer.

-
