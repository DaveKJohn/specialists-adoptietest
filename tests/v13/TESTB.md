# Test B — UNINSTALL, alleen met `UNINSTALL.md`

Uitgevoerd 2026-08-02, CLI `2.1.220`. Document gelezen van `main` (`30492` bytes, 460 regels) en
byte-identiek aan de kopie in de clone (sha256 `60262ca2…`).

## B0 — het document vinden — GROEN

Zelfde ingang als A0. De workshop-README noemt `UNINSTALL.md` op **regel 14**, in dezelfde blockquote
direct onder de openingsalinea die ook de QUICKSTART draagt, expliciet als *"its mirror"*. Eén hop, geen
beslissingen vooraf.

## B0 extra — de bewaaropdracht, en waarom hij terecht is

*"Before you start"* zegt: `UNINSTALL.md` zit **niet** in de plugin-payload, alleen in de gecachte clone,
en stap 5 verwijdert die clone volledig. **Gemeten en bevestigd:**

| bestand | in de cache (`…/specialists/3.1.2/`) | in de clone |
|---|---|---|
| `teardown.ps1` | **ja** | ja |
| `UNINSTALL.md` | **nee** | ja |

Het document heeft dus gelijk, en het onderscheid is precies waarom de twee instrumenten op een ander
moment verdwijnen. Kopie vooraf opgeslagen zoals voorgeschreven — zie B4c, waar dat de enige reden bleek
dat het document er aan het eind nog was.

## B1a/B1b — pre-flight 1 en zijn filter — GROEN

```
git check-ignore -v .claude/specialists/lenses/ | Where-Object { ($_ -split "`t")[0] -notmatch ':$' }
→ lege uitvoer, exit code 1
```

Precies het geval dat het document *"the answer you WANT here"* noemt, mét de uitleg waarom exit 1 goed
nieuws is. Zonder die alinea zou een harnas dit als een gefaalde stap lezen — de reden dat #332's derde
generatie hier hoort. **Groen.**

## B1c — pre-flight 2 meet commits, niet de index (#332) — GROEN, exact gereproduceerd

De hele val nagespeeld:

| toestand | nieuwe vorm (`git ls-tree … HEAD`) | oude vorm (`git ls-files`) |
|---|---|---|
| schoon, niets gestaged | 0 regels | 0 regels |
| **na `git add -A`, géén commit** | **0 regels** | **20 regels** |

`git rev-list --count HEAD` bleef op **4** — er is geen commit bijgekomen. De 20 regels van de oude vorm
zijn exact het getal dat v10 mat (19 lenzen + `SPECIALISTS.md`). **De reparatie doet precies wat ze moet
doen**, en de fout die ze verving is in dezelfde meting zichtbaar gemaakt. Daarna `git reset` — de repo
bleef op 4 commits.

## B1d — het safety-net-commit — GROEN (tekst), bewust niet uitgevoerd

Het document zegt nu `git add CLAUDE.md .claude scripts`, dus breder dan alleen de lens-tree, met de
motivering dat de teardown óók `CLAUDE.md` bewerkt en met `-VendorScripts` onder `scripts/` schrijft. Dat
is de correcte verzameling voor deze repo — alle drie stonden hier untracked.

**Bewust niet gecommit, en dat is een meterskeuze, geen bevinding.** Een commit hier zou de fixture op 5
commits zetten en de nulstand van v14 breken. De undo was niet nodig: alles wat de teardown aanraakt is
regenereerbaar uit de bootstrap.

## B1h — de `<plugin>`-placeholder (#330, tweede helft) — GROEN

De query uit stap 1 gaf één regel:

```
specialists@davekjohns-workshop -> project 3.1.2 ad84ef97…f70 C:\Users\davek_onn\.claude\plugins\cache\davekjohns-workshop\specialists\3.1.2
```

**Zonder gokken gevonden**, en het document waarschuwt expliciet tegen de val waar ik in test A in had
kunnen lopen: *"Do not use the path from your `SPECIALISTS.md` imports — those point into
`~\.claude\plugins\marketplaces\`, the git clone, which is a different directory."* Dat is exact het pad
dat A4 me liet zien. De waarschuwing staat op de plek waar je hem nodig hebt.

## B2a/B2b — preview en `-Apply` (#356) — GROEN

| | `[remove]`-regels | `[KEEP]`-regels | `Summary:` |
|---|---|---|---|
| preview | 30 | 2 | `30 item(s) to remove, 2 kept` |
| `-Apply` | 30 | 2 | `30 item(s) removed, 2 kept` |

**De markers en het getal komen in beide modi overeen**, en de overgebleven regels staan onder hun eigen
kopje (*"Kept — generated prose in a governance file"*) met de twee regelnummers eronder. #356 / #362 /
#331 alle drie groen.

## B2c — de verse-consumerrij — GROEN

Na `-Apply` staat er van de 23 gemaakte bestanden nog precies over wat het document voorspelt:

| | |
|---|---|
| `scripts\` | **weg**, inclusief de lege `scripts\lib\` |
| `.claude\specialists\` | **weg**, inclusief `lenses\` |
| `.claude\settings.suggested.jsonc` | weg |
| `CLAUDE.md` | **229 bytes, 4 regels** — kop + 2 regels bootstrap-proza |

De twee proza-regels zijn als `[KEEP]` gerapporteerd, niet stil laten staan. Geen lege mappen achtergebleven.

## B2e — wat het overgebleven proza dóét (#331/#362) — GROEN, met één nuance

*"What is left behind, honestly"* zegt het nu expliciet: **`CLAUDE.md` wordt elke sessie als
projectinstructie geladen**, *"so this is the one leftover that is not merely inert"*, gemarkeerd als
**to-do** en niet als notitie, met twee concrete remedies (`Remove-Item CLAUDE.md`, of de twee regels
vervangen). Dat is precies wat #331 vroeg.

**De praktijkmeting, twee verse sessies na de teardown:**

| sessie | vraag | merkte de tegenspraak op? |
|---|---|---|
| 1 | *"kleine verduidelijking in de README, hoe pak ik dat aan?"* | **nee** — zag `CLAUDE.md` wél als untracked restant dat *"niet in de fixture hoort"*, maar niet de tegenspraak zelf |
| 2 | *"korte oriëntatie: wat is dit en welke conventies gelden hier?"* | **ja**, letterlijk: *"`CLAUDE.md` zegt dat de repo 'governed by Claude Specialists' is met een Chief of Staff — maar er is geen enkele specialist-definitie aanwezig, en `.claude/settings.json` is letterlijk `{}`. Het is een scaffold-stub van `specialists-init`, geen werkende governance."* |

**1 van de 2 — exact de v12-uitkomst.** De nuance voor de bron: het document citeert v11's *"two separate
fresh sessions flagged the contradiction unprompted"*. Dat is een correcte weergave van v11, maar v12 én
v13 meten allebei 1 op 2. Een lezer die op die zin kalibreert overschat hoe betrouwbaar de sessie het zelf
opmerkt — en dat is juist het argument om de rij als to-do te behandelen, wat het document gelukkig al doet.

## B2f — de fixture-README niet als werk-item (stap 0) — GROEN, scherper dan verwacht

```
scanned 3 file(s) under CLAUDE.md, .claude/ and scripts/ against 19 known specialist name(s)
```

De drie zijn `CLAUDE.md`, `settings.json` en `settings.local.json` (26 bestanden onder die wortels, min de
23 die deze run verwijdert). **`README.md` valt buiten de gescande wortels en komt in geen enkele telling
voor** — niet als werk-item en ook niet als prose. De stap-0-tekst zegt *"wordt alleen geteld als prose"*;
gemeten wordt hij **helemaal niet geteld**. Voor de check maakt dat niets uit (hij is geen werk-item, wat
de eis was), maar de formulering is iets sterker dan de werkelijkheid.

Verder telt de audit netjes uit wat hij uitsluit en waarom, tot op regelniveau: *"1 reference(s) on
`CLAUDE.md` line(s) this run would remove … were excluded for the same reason, at line granularity."*

## B3c — de voorspelde CLI-melding (#359) — GROEN, woordelijk

Bewust zonder vlag, CLI `2.1.220`:

```
✘ Failed to uninstall plugin "specialists@davekjohns-workshop": Plugin "specialists@davekjohns-workshop"
is enabled at project scope (.claude/settings.json, shared with your team). To disable just for you:
claude plugin disable specialists@davekjohns-workshop --scope local
```

**Woordelijk identiek aan wat het document afdrukt.** En het document zegt nog steeds, direct eronder, de
gesuggereerde remedie **niet** te volgen, met de reden (het schrijft een sleutel bij in plaats van weg, en
stap 4 komt dan niet leeg terug). Groen.

## B3a — met de vlag — GROEN

`✔ Successfully uninstalled plugin: specialists (scope: project)`, exit 0. En de drie voorspelde
neveneffecten, alle drie gemeten:

| voorspeld | gemeten |
|---|---|
| `settings.json` houdt `"enabledPlugins": {}` over | ja — 246 → **199 bytes** |
| data-directory verdwijnt (geen `--keep-data`) | ja — `data/specialists-davekjohns-workshop/` weg, `data/` leeg |
| `.orphaned_at` blijft achter (#337.5) | ja — `…/specialists/3.1.2/.orphaned_at`, inhoud `1785697493963` |

Recordquery voor dit pad: **leeg**. `installed_plugins.json` = `{"version": 2, "plugins": {}}`.

## B5b — wanneer de instrumenten sterven (#373) — GROEN, de reparatie is geverifieerd

**Direct ná stap 2 gemeten**, wat de kern van #373 is:

| | na `uninstall --scope project` |
|---|---|
| `teardown.ps1` | **AANWEZIG** (57.244 bytes) |
| `UNINSTALL.md` | **AANWEZIG** (30.492 bytes) |
| cache-map | **AANWEZIG** |

Dus de oude claim (*"the last point at which you can produce it"*, omdat stap 2 de payload zou weghalen)
is aantoonbaar onjuist, en het document zegt nu de **juiste reden**: de cache volgt de *marketplace*, niet
de installatie, en wat de instrumenten uiteindelijk weghaalt is de **handmatige cache-delete van stap 5**.

**En stap 4 biedt de her-run aan, in plaats van re-install → re-audit → opnieuw uninstallen.** Ook echt
gedaan, ná de uninstall:

```
Summary: 0 item(s) to remove, 2 kept.
[FREE] no live reference to a specialist, persona, roster or lens is left in the scanned set.
```

Draait, verwijdert niets, heeft geen `-Apply` nodig. **#373 → geverifieerd, in tekst én in uitvoering.**

## B4a — de sleutels terug — GROEN

`enabledPlugins` (stond op `{}`) en `extraKnownMarketplaces` uit `.claude/settings.json`; het bestand houdt
`{}` over. `settings.local.json` (restant van de #325-proef in A2) verwijderd.

## B4d — de permissions-entry (#337.6) — NIET GEMETEN, voorwaarde deed zich niet voor

Geen `permissions`-blok in `.claude/settings.json`, niet in `.claude/settings.local.json`, en
`~/.claude/settings.json` bestaat niet. **Derde ronde op rij dat de voorwaarde uitblijft**, om de reden die
de opdracht al noemt: het gedocumenteerde pad laat `settings.suggested.jsonc` ter review staan in plaats van
hem te kopiëren — en de teardown verwijdert dat voorstel zelf. Niet naar gejaagd.

## B4b — de lege sleutel (#374) — GROEN, en beide passages kloppen

**Stap 5, gemeten op mijn pad:**

| | |
|---|---|
| `~/.claude/settings.json` vóór | **bestaat niet** |
| `~/.claude/settings.json` ná | **bestaat niet** |
| `known_marketplaces.json` | 275 bytes → **2 bytes, `{}`** |
| `marketplace list` | `No marketplaces configured` |

Het document zegt nu voorwaardelijk: *"On the path the QUICKSTART prescribes, none of that happens — and
that is the expected result, not an anomaly"*, met twee onafhankelijke redenen (de sleutels staan in de
**repo**, en stap 3 haalde ze al weg). Gevolgd door: *"a clean-machine check after a by-the-book
project-scoped teardown **is** literally clean."* **Dat klopt exact voor mijn pad.**

**Het spiegelbeeld is nu een tweekolomstabel die een reeks afbakent** — *"a profile that had run other
plugins"* naast *"a profile that had only ever run this one"* — met de instructie *"Read your own numbers
against your own starting point, not against either column."* Mijn eindmeting valt op alle drie de rijen
**exact op de rechterkolom**: `installed_plugins.json` 35 bytes, `known_marketplaces.json` 2 bytes,
`~/.claude/settings.json` afwezig.

**Mag een volgende ronde een volledig schone stap-0-tabel als normaal rapporteren?** **Ja** — het document
zegt dat nu met zoveel woorden, en v13 is er het tweede bewijs van.

## B4c — stap 5 verwijdert het document (#328) — GROEN

Ná `marketplace remove`:

| | |
|---|---|
| `UNINSTALL.md` in de clone | **WEG** |
| `UNINSTALL.md` ergens onder `~/.claude` | **nergens meer** |
| `teardown.ps1` in de cache | nog aanwezig, tot de handmatige delete |

De waarschuwing uit *"Before you start"* is dus letterlijk waar, en ik had het document aan het eind alleen
nog omdat ik hem vooraf had opgeslagen — zoals diezelfde alinea voorschrijft. De stappen zijn in de gedrukte
volgorde gelopen en er is niets misgegaan. **Groen.**

## B5 — de handmatige cache-delete (#339) — GROEN, tabel gereproduceerd

| locatie | vóór | ná `marketplace remove` |
|---|---|---|
| `marketplaces/davekjohns-workshop/` — de clone | **4.665.243 bytes** | **weg** |
| `cache/davekjohns-workshop/` — de payload | **1.086.158 bytes** | **nog steeds aanwezig** |

De regel die het document vraagt mee te nemen in plaats van de getallen — *"the unpacked cache belongs to
the marketplace, not to the install"* — houdt. Daarna met de hand verwijderd volgens het gedrukte commando;
`teardown.ps1` ging in dezelfde beweging mee.

## B6 — de zelfherstellende val (#327) — GROEN, beide richtingen gemeten

**Middenrij (marketplace geregistreerd + cache aanwezig), vóór stap 5:**

Alleen een `enabledPlugins`-sleutel teruggezet, daarna één sessie. Die sessie laadde **niets** — op de vraag
naar `specialists*`-skills antwoordde ze `geen` — en tóch verscheen:

```json
"scope": "project", "installedAt": "2026-08-02T19:11:01.097Z", "gitCommitSha": "ad84ef97…"
```

Een vers, volledig `project`-record. **Val exact gereproduceerd, en geverifieerd via de skill-lijst en niet
via het record**, zoals de opdracht vraagt.

**Onderste rij (na de volledige teardown t/m stap 5):** sleutel teruggezet, sessie gestart → record blijft
`{}`, `known_marketplaces.json` blijft `{}`, clone en cache blijven weg. **Het mechanisme is ontmanteld.**

Het document beschrijft alle drie de toestanden nu in een tabel, en zegt: *"Walk it through to the end of
Step 5 and the mechanism is disarmed, stray key or not."* Zie **N2** — die laatste vier woorden houden niet.

## B6 — de padloze `user`-vorm (#323) — NIET GEMETEN, voorwaarde deed zich niet voor

**Vierde ronde op rij.** Geen enkele sessie schreef een `user`-record zonder `projectPath`; alle geschreven
records waren `project` mét pad. Niet gejaagd, conform de opdracht. Niet gemeten is niet gerepareerd en ook
niet stuk.

## B6 — remedie leesbaar uit de sessie (#324) — GROEN

Twee gemeten gevallen waarin de sessie zelf de remedie noemt zonder dat je een record hoeft te lezen: de
`[BOOTSTRAP]`-hooks vóór de adoptie wezen naar de `specialists-init`-skill met de reden erbij, en de
`[ROSTER-PENDING]`-regel erna zegt wat er te doen staat én dat het niet urgent is. In beide gevallen staat
de handeling in de melding zelf.

## Slotvraag test B — staat het profiel weer op de stap-0-tabel?

**Ja, alle zes.**

| locatie | eindmeting |
|---|---|
| `installed_plugins.json` | 35 bytes, `{"version": 2, "plugins": {}}` — geen enkel record |
| `marketplaces/davekjohns-workshop/` | afwezig |
| `cache/davekjohns-workshop/` | afwezig |
| `data/specialists-davekjohns-workshop/` | afwezig |
| `known_marketplaces.json` | 2 bytes, `{}` |
| `~/.claude/settings.json` | afwezig |

`claude plugin marketplace list` → `No marketplaces configured`. Onder `marketplaces/` staat alleen nog
`claude-plugins-official`, dat er bij stap 0 ook al stond en niet van deze familie is.

**En zegt het document dat vooraf?** Ja — dat is precies de #374-reparatie, en die is hiermee voor de
tweede ronde bevestigd.

**De repo-kant, eerlijk:** een consument die het document tot de letter volgt houdt twee dingen over die
`"What is left behind, honestly"` niet noemt — `CLAUDE.md` met 229 bytes bootstrap-proza (wél gedekt, als
to-do) en `.claude/settings.json` teruggebracht tot **`{}`** (niet gedekt; zie **N6**). Voor deze fixture
zijn beide na de meting verwijderd, waarna de ijkpunt-tabel van stap 0 weer exact klopt: 1105 / 1127 / 22 /
4 commits / `50ec727` / 0 tags / `README.md` als enige tracked file / `git status` leeg.
