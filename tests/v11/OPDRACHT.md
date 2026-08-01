# Opdracht ronde v11 — houden de zeventien reparaties, en is het pad nu begaanbaar?

Lees eerst [`README.md`](README.md) hiernaast: de rollen, de regel dat je nooit stil repareert, en de vier
klassen blokkade. Deze opdracht is de stappenlijst.

**Twee vragen, en ze zijn even belangrijk.**

1. **Houden de reparaties?** Er zijn er **zeventien** — de dertien bevindingen van v10 en de vier van v9 —
   uitgebracht als **`v3.1.0`**. Voor het eerst sinds v9 is die voorwaarde vervuld.
2. **Is het pad nu van begin tot eind te lopen?** v10 vond **5 geblokkeerd, 18 wrijving, 14 groen**. Alleen
   naast die tabel is te zien of het beter is geworden.

> **Meet nu, vóór er iets nieuws naar `main` merget.** Een consument installeert **`main`**, niet de tag —
> daarom mat v10 tegen `6a9ef82`, drie commits voorbij `v3.0.9`, en moest elk issue uitleggen dat het niet
> tegen een release mat. Op het moment dat deze opdracht is geschreven zijn `main` en de tag `v3.1.0`
> **dezelfde commit** (`6feac75`). Dat venster sluit bij de eerstvolgende merge. Leg de `gitCommitSha` uit je
> install-record vast en zeg of die gelijk is aan de tag.

---

## Stap 0a — meet issue #350 EERST, vóór je iets opruimt

**Deze stap gaat vóór stap 0 en de volgorde is onomkeerbaar.** Het profiel `davek_onn` draagt op dit moment
precies de fixture die
[#350](https://github.com/DaveKJohn/davekjohns-workshop/issues/350) nodig heeft: keys gezet, marketplace
geregistreerd, clone **en** cache aanwezig, en één correct project-scoped install-record dat een
**sessiestart zelf** heeft geschreven. **Purgen vernietigt die fixture**, en dan heeft #350 een compleet
nieuwe omgeving nodig.

**De vraag:** zijn de twee `claude plugin`-commando's van QUICKSTART stap 1 overbodig? Twee helften zijn apart
gemeten — een sessiestart registreert de marketplace (#329), en een sessiestart schrijft het record (#327) —
maar nooit als één keten, en de tweede meting bereikte zijn voorwaarde via een handmatige
`claude plugin marketplace add`.

1. Start een sessie in de repo. Start er nóg een.
2. Lees **zonder één `claude plugin`-commando** de zes locaties uit de tabel bij stap 0b, **én de
   skill-lijst**.
3. Noteer per locatie wat er staat, en of `specialists-init` in je slash-lijst zit.

**Let op de les van #327:** verifieer via de **skill-lijst**, niet via het record. Een sessie kan een volledig
gezond record hebben en zelf compleet inert zijn — geen skills, geen subagents, geen hook-output. Dat is de
enige staat die van élke kant gezond leest.

Wat elke uitkomst betekent staat in het issue zelf.

## Stap 0b — maak profiel en checkout maagdelijk, en bewijs het

**De lokale checkout weggooien en opnieuw clonen.** Doe dit op het `davek_onn`-profiel; vanaf het andere
account is dat profiel niet zichtbaar, dus dit is de eerste echte handeling van de ronde.

```powershell
# buiten de repo-map
Remove-Item -Recurse -Force <pad>\specialists-adoptietest
git clone https://github.com/DaveKJohn/specialists-adoptietest.git
```

Wat je weggooit is v10's ongecommitte teardown-staat en de twee `CLAUDE.md`-regels die als bewijs voor #331
bewaard waren. **Dat bewijs is opgebruikt:** #331 is gerepareerd en deze ronde hertest hem bij B2.

**Let op: je kloont `main`, en je haalt deze map er niet bij.** `tests/v11/` staat op een aparte branch en
komt dus **niet** mee in de werkkopie — dat is de bedoeling. Doe géén `git checkout tests/v11` in de repo die
je test: dan staan de papieren in je werkkopie en vindt de teardown-audit van stap B2 daar tientallen
verwijzingen naar de plugin die niets met de adoptie te maken hebben. Lees de opdracht in de browser of in een
**tweede clone** — zie [`README.md`](README.md) onder *"Waar je de ronde draait, en waar je dit leest"*.

**Daarna de administratie leegmaken en stap 0 bewijzen.** Alle zes moeten **afwezig** zijn; is er één
aanwezig, dan meet je geen verse consument en is **dát je eerste bevinding**. Meet in een kale PowerShell,
**vóórdat** de eerste Claude Code-sessie start — die volgorde ís de meting, want een sessiestart schrijft een
ontbrekend record zelf.

| locatie | verwacht |
|---|---|
| `~/.claude/plugins/installed_plugins.json` | afwezig, of zonder enig record |
| `~/.claude/plugins/marketplaces/davekjohns-workshop/` | afwezig |
| `~/.claude/plugins/cache/davekjohns-workshop/` | afwezig |
| `~/.claude/plugins/data/specialists-davekjohns-workshop/` | afwezig |
| `~/.claude/plugins/known_marketplaces.json` | afwezig, of zonder `davekjohns-workshop` |
| `~/.claude/settings.json` | geen `enabledPlugins`, geen `extraKnownMarketplaces` |

**Noteer welke administratie je meet** — het volledige pad van `installed_plugins.json` én het
gebruikersprofiel. Twee profielen op één machine zijn twee administraties, en v9 verloor zijn hele
nulstandtabel omdat niemand dat opschreef. Leg ook bytegrootte en regeltelling van de verse checkout vast.

---

## Test A — INSTALL, alleen met de QUICKSTART

Je krijgt één ding: het document. Per stap: wat deed je, wat kwam eruit, wat voorspelde het document, en kon
je verder. Bij een reparatie: **werkt hij?**

### A0 — hoe kom je er überhaupt? *(#338)*

v10 mat hier wrijving: de documenten waren moeilijk vindbaar en `WebFetch` gaf gehallucineerde inhoud voor de
QUICKSTART terug. Er staat nu een verwijzing bovenaan de pagina. **Via welke route kom je binnen, en hoeveel
beslissingen zitten er vóór stap 1?**

### Vóór A1 — de drie obstakels die in geen document stonden *(#334)*

v10's grootste verrassing zat vóór regel één: Claude Code was er niet, de PowerShell `ExecutionPolicy`
blokkeerde **élke** `.ps1` (ook `claude.ps1` zelf), en daarna PATH plus een volledige editor-herstart. Er is
nu een `Before you start`-sectie. **Kun je de commandoblokken draaien zoals ze staan afgedrukt?**

> Dit profiel heeft al eens Claude Code gehad, dus dit is gedeeltelijk voorgekookt. **Zeg eerlijk welk deel je
> opnieuw hebt gemeten en welk deel al stond** — anders leest een groen hier als meer dan het is.

### A1 — het settings-fragment *(#335)*

Het blok was `jsonc` zonder omsluitende accolades en parseerde niet als je het plakte. Nu geldige JSON.
**Plak het letterlijk in een leeg bestand en parse het.** En: weet je waar `.claude/settings.json` staat, en
dat je die map zelf moet aanmaken? Het document onderscheidt nu expliciet `.claude/` in je repo van
`~/.claude/` op de machine.

### A2 — de twee commando's *(#329 — dit was een blokkade)*

`claude plugin marketplace update` faalde op een profiel waar die marketplace nooit had bestaan: het
**eerste uitvoerbare commando van het hele pad**. Het document zet de registratie er nu vóór. **Dit moet nu
doorlopen.** Loopt het weer vast, dan is dat de zwaarste bevinding van de ronde.

Twee extra metingen op deze stap:

- **#336** — neem **SHA256 van `settings.json` vóór en ná** `claude plugin install --scope project`, mét de
  key al aanwezig. Het document belooft nu een *equivalente* inhoud maar **geen** byte-identiek bestand.
  Klopt dat, en klopt de voorspelling van wat er verschuift (sleutelvolgorde, uitgeklapt `source`-object)?
- **#325** — twee installs met de verificatiequery erna: één tegen een **`project`**-record, één tegen een
  **`local`**-record. De eerste moet **één** regel geven, de tweede **twee**. Dat is de versmalde trigger.

### A3 — herstart en de verificatiequery

Levert de afgedrukte query **één `project`-regel per plugin**? Kun je uit `version` en `gitCommitSha` aflezen
welke code je draait?

- **#322** — de tagvergelijking is **vervangen**: geen `rev-list` tegen een tag meer, maar `rev-parse HEAD`
  plus een `gh api`-route. Op een **verse clone** is dit de enige plek waar dag één te zien is. Noteer wat die
  clone draagt (shallow? welke tags?) en of het document doet wat het belooft — inclusief of de
  `fatal: ambiguous argument`-uitkomst als **verwachte derde mogelijkheid** benoemd staat.

### A4 — stap 2, de bootstrap-skill

Bestaat `specialists-init` in de nieuwe sessie? **Kloppen de aantallen met wat de QUICKSTART nu zelf noemt?**
*(#337.3)* — 4 persona-lenzen + 15 lens-scaffolds = **19** lensbestanden, plus 2 script-scaffolds en 1
`@`-import. Die aantallen staan er nu vóór het moment dat je ze nodig hebt.

Direct daarna, en dit is de scherpste verificatie van de ronde:

- **#333** — v10 kreeg **negentien `[ERROR]`-regels** in de sessie ná een volledig geslaagde bootstrap, die
  naar `CLAUDE.md` wezen terwijl de roster in `SPECIALISTS.md` hoort. **Verwacht nu één niet-tellende
  `[ROSTER-PENDING]`-regel.** Tel de regels, en kijk welk bestand ze noemen. Controleer ook dat
  `Get-RosterPath` in de gegenereerde `scripts/repo-config.ps1` naar de seam wijst en niet naar `CLAUDE.md`.
- **#337.4** — kijk naar de output van de bootstrap zelf. Zegt hij *"the orchestrator import"* (**enkelvoud**)
  en zegt de `settings.suggested.jsonc`-regel nu **welke kant deze repo op staat**, in plaats van *"gitignored
  in many repos"*? Deze repo heeft geen `.gitignore`, dus het antwoord moet "niet gitignored" zijn.
- **#337.2** — meet de regeleindes van wat de bootstrap neerzet (LF of CRLF) en of `CLAUDE.md` een slotregel
  heeft. Het document waarschuwt hier nu voor; klopt de waarschuwing?

### A5 — stap 3, herstart en verificatie

Neemt Chris het woord, met een afzenderregel?

- **#330** — `Select-String '@' .claude/specialists/SPECIALISTS.md`: wijst het importpad naar
  `marketplaces/` of naar `cache/`? Het document zegt nu dat het de **clone** is, dat die `main` volgt, en
  waarom dat bewust zo is. Klopt dat met wat je ziet?

**Slotvraag van test A, en die is binair:** heb je het **zonder de meter** gered? Zo nee: op welke stap en in
welke klasse. En naast v10: **welke van de vijf blokkades zijn weg?**

---

## Test B — UNINSTALL, alleen met UNINSTALL.md

Begint waar A eindigt: een werkende, geadopteerde repo. Je krijgt nu **alleen** `UNINSTALL.md`. Leun niet op
wat je in test A hebt geleerd.

### B0 — vind je het document? *(#338)*

Zelfde vraag als A0, andere route.

### B1 — de pre-flight

- **#332** — doe `git add -A` **zonder** te committen, en draai dan pre-flight 2. Die mat vroeger de
  **index** en zei "committed" over een repo met nul commits. Hij moet nu **commits** meten
  (`git ls-tree ... HEAD`) en de `fatal: ... HEAD`-uitkomst benoemen als "helemaal geen commits".
- **#332, tweede helft** — klopt het dat het succesgeval van commando 1 **exit 1** geeft, en dat het
  safety-net commit ook `CLAUDE.md` en `scripts/` omvat?
- **#330, tweede helft** — wordt de `<plugin>`-placeholder nu uitgelegd, en kun je dat pad vinden **zonder te
  gokken** tussen de clone en de cache?

### B2 — de teardown: preview, dan `-Apply`

Kloppen de aantallen, en is preview = apply?

- **#331** — dit is de **verse**-consumerrij: een repo die vóór de adoptie géén `CLAUDE.md` had. Blijft er
  bootstrap-prose staan, en wordt die dan als **`[KEEP]`** gerapporteerd in plaats van dat de audit `[FREE]`
  zegt? En is `scripts\lib\` **weg** in plaats van als lege map achtergebleven?

### B3 — `claude plugin uninstall … --scope project`, per plugin

Werkt de vlag zoals beschreven? Komt de `local`-weigering voor, en noemt de CLI zelf de juiste vlag?

- **#337.5** — verschijnt er een `.orphaned_at`? Het document voorspelt hem nu. Stond hij er?

### B4 — de sleutels terug uit `settings.json`, plus `marketplace remove`

- **#328 — dit was een blokkade.** Stap 3 verwijderde **het document dat je aan het lezen bent**, en stap 4's
  audit-gereedschap ging met stap 2 mee. De stapvolgorde is herbouwd: **document en audit moeten aan het eind
  nog bestaan.** Loop de stappen in de gedrukte volgorde en kijk of dat nu zo is.
- **#337.6** — staat er nog een `permissions`-entry die naar de plugin-map wijst? Stap 3 noemt die nu.
- **#339** — het document had een open vraag over wat `marketplace remove` van schijf haalt. Die is beantwoord
  en tot regel gemaakt. **Loop de zes locaties opnieuw af** en kijk of het antwoord klopt.

### B6 — de zelfherstellende val: drie reparaties op één plek

Laat bewust **één `enabledPlugins`-sleutel** staan en start een sessie.

- **#327** — verschijnt er een vers install-record terwijl die sessie **zelf niets laadt**? Verifieer via de
  **skill-lijst**, niet via het record.
- **#323** — ontstaat de **padloze `user`**-vorm, en wordt die nu **gemeld** als `[RECORD-SHAPE]`?
- **#324** — is het **remedie** uit de sessie-output te lezen, **zonder** de check zelf te draaien? Dat was de
  hele klacht: de roll-up beloofde details die de hook wegfilterde.

**Slotvraag van test B:** staat het profiel daarna aantoonbaar weer op de stap-0-tabel? Zo nee: wat is er
over, en **zegt het document dat vooraf**?

---

## Wat deze ronde bewust niet doet

Schrijf dit over in het rapport, zodat de bron weet wat er **niet** gemeten is en het niet als groen leest.

| Niet in scope | Waarom |
|---|---|
| De teardown-regressie in twee cycli | Dat is de **bezette**-consumertest. v11 is de verse; `life-hub` wordt niet aangeraakt |
| Een vers **apparaat** | Zelfde machine, CLI, git en OS. Dit is een verse **gebruiker** |
| De poorten en checks bij de bron | Van buiten onzichtbaar, zoals in v8 t/m v10 |
| Een handmatige test A van begin tot eind | Draag je de uitvoering over aan een sessie, dan meet je **A′** — AI-geassisteerde adoptie. Zeg dat dan, zoals v10 deed |
| De aantallen-sweep door de documentatie | v11 meet het pad en de zeventien reparaties, niet of elk getal in elk document klopt |
| Het #283-CRLF-artefact | Deze repo heeft geen `.gitignore`. **Bouw er geen** — een oneerlijke fixture is geen meting |

## Aan het eind

Vul [`RESULTATEN.md`](RESULTATEN.md) in: de stappentabel naast die van v10, de zeventien reparaties met per
stuk **geverifieerd / niet gemeten / nog stuk**, en de gaps. Daarna de issues bij de bron en de uitkomst in
het dossier in `life-hub`.

**En zeg wat je met het profiel doet** — opruimen of laten staan is beide goed, maar een half opgeruimd
profiel is voor de vólgende ronde geen nulstand meer. Dat is dezelfde les die v9 zijn nulstandtabel kostte.

Reken op **minder** bevindingen dan v10 en op een andere verdeling: waar v10 vooral klasse 2 en 3 vond
(ontbrekende en te laat gegeven informatie), zou dat nu grotendeels gedicht moeten zijn. Wat overblijft is de
interessante categorie: **klasse 4** — het staat er, en de lezer miste het toch.
