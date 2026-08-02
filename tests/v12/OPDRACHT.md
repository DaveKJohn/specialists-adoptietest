# Opdracht ronde v12 — houden de negen reparaties, en is de eerste blokkade eindelijk weg?

Lees eerst [`README.md`](README.md) hiernaast: de rollen, de regel dat je nooit stil repareert, en de vier
klassen blokkade. Deze opdracht is de stappenlijst.

**Twee vragen, en ze zijn even belangrijk.**

1. **Houden de reparaties?** Er zijn er **negen** — de bevindingen van v11 — uitgebracht als **`v3.1.1`**.
2. **Is `#329` weg?** v10's zwaarste blokkade, en de enige die v11 **niet kon meten** omdat zijn eigen stap 0a
   de voorwaarde vernietigde. Die stap is nu weg (zie hieronder), dus dit is de eerste ronde waarin het
   antwoord te halen valt.

> **Meet nu, vóór er iets nieuws naar `main` merget.** Een consument installeert **`main`**, niet de tag.
> Op het moment dat deze opdracht is geschreven zijn `main` van de workshop en de tag `v3.1.1` **dezelfde
> commit** (`4b1a74d`). Dat venster sluit bij de eerstvolgende merge. Leg de `gitCommitSha` uit je
> install-record vast en zeg of die gelijk is aan de tag.

> **Waarom stap 0a van v11 hier niet meer staat.** v11 opende met een meting voor
> [#350](https://github.com/DaveKJohn/davekjohns-workshop/issues/350) — zijn de twee `claude plugin`-commando's
> overbodig? — en dat experiment herregistreerde de marketplace en herkloonde hem, waarmee het de voorwaarde
> van #329 om zeep hielp. **#350 is beantwoord** (nee, ze zijn niet overbodig: een sessiestart schrijft een
> compleet record maar vult de cache niet, dus er laadt niets). Dat maakt de weg vrij voor een schone purge
> als allereerste handeling — en daarmee voor #329.

---

## Stap 0 — maak profiel en checkout maagdelijk, en bewijs het

### Eerst met de hand, buiten Claude Code

**Dit moet vóór de eerste sessie, en het kan niet ván binnenuit.** v11 kreeg rij 6 van de tabel hieronder
nooit schoon: Claude Code's permission-classifier weigert wijzigingen aan `~/.claude/settings.json`, vanuit
zowel PowerShell als de bestandstool. Dat is een harnasgrens, geen bevinding over de plugin — maar het
resultaat was dat v11 zijn eigen nulstand niet rond kreeg.

Open dat bestand dus **zelf**, in een editor of een kale PowerShell zonder Claude Code, en haal er
`enabledPlugins` en `extraKnownMarketplaces` uit. Daarna pas de rest.

### De lokale checkout weggooien en opnieuw clonen

```powershell
# buiten de repo-map
Remove-Item -Recurse -Force <pad>\specialists-adoptietest
git clone https://github.com/DaveKJohn/specialists-adoptietest.git
```

**Let op: je kloont `main`, en je haalt deze map er niet bij.** `tests/v12/` staat op een aparte branch en
komt dus **niet** mee in de werkkopie — dat is de bedoeling. Doe géén `git checkout tests/v12` in de repo die
je test: dan staan de papieren in je werkkopie. Lees de opdracht in de browser of in een **tweede clone** —
zie [`README.md`](README.md) onder *"Waar je de ronde draait, en waar je dit leest"*.

> **De nulstand van deze repo.** Sinds v11 staat er een **derde commit**: de fixture-README wijst nu naar
> `tests/v12` in plaats van naar `tests/v11`. De maten hieronder zijn opnieuw gemeten op een **verse clone**,
> niet overgenomen — en ze zijn toevallig gelijk aan die van v11, omdat `v11` en `v12` even lang zijn.
>
> | maat | waarde | hoe gemeten |
> |---|---|---|
> | grootte, repo-zijde | **1105 bytes** | de git-blob, dus met LF |
> | grootte, op schijf (Windows) | **1127 bytes** | `core.autocrlf=true` maakt van 22 LF's een CRLF |
> | regels | **23** | alle regels |
> | regels volgens `Measure-Object -Line` | **15** | die cmdlet slaat lege regels over |
> | commits op `main` | **3** | `git log --oneline` op een niet-shallow clone |
> | `HEAD` | **`f56a9e6`** | de fixture-commit die naar v12 wijst |
>
> **Noteer die getallen als jouw ijkpunt en lees ze niet als restant van een vorige ronde** — dat is precies
> de fout die v9 zijn nulstandtabel kostte. Wijkt jouw meting af, kijk dan eerst naar de kolom *hoe gemeten*:
> 1105 en 1127 zijn allebei goed, ze meten iets anders.
>
> Die verwijzing in de README is opzettelijk kort en naamloos: **geen specialist-id, geen specialistennaam,
> geen contract-functie** (nageteld: 0 van elk). De vrij-staande audit van stap B2 scant `CLAUDE.md`,
> `.claude/**` en `scripts/**`, en het script zegt zelf dat andere prose zoals `README.md` daarbuiten valt en
> alleen **geteld** wordt in plaats van als werk-item gelijst. **Controleer dat bij B2**: als die README wél
> als werk-item opduikt, is dát een bevinding.

### Bewijs de nulstand

Alle zes moeten **afwezig** zijn; is er één aanwezig, dan meet je geen verse consument en is **dát je eerste
bevinding**. Meet in een kale PowerShell, **vóórdat** de eerste Claude Code-sessie start — die volgorde ís de
meting, want een sessiestart schrijft een ontbrekend record zelf.

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
nulstandtabel omdat niemand dat opschreef.

---

## Test A — INSTALL, alleen met de QUICKSTART

Je krijgt één ding: het document. Per stap: wat deed je, wat kwam eruit, wat voorspelde het document, en kon
je verder. Bij een reparatie: **werkt hij?**

### A0 — hoe kom je er überhaupt?

Via welke route kom je binnen, en hoeveel beslissingen zitten er vóór stap 1? In v11 was dit groen; de vraag
is of dat zo blijft nu de pagina langer is geworden.

### Vóór A1 — de drie obstakels *(#334, in v11 alleen als tekst beoordeeld)*

Claude Code aanwezig, de PowerShell `ExecutionPolicy` die **élke** `.ps1` blokkeert (ook `claude.ps1` zelf),
en daarna PATH plus een volledige editor-herstart. v11 kon dit niet echt meten omdat dat profiel alles al
had.

> **Zeg eerlijk welk deel je opnieuw hebt gemeten en welk deel al stond** — anders leest een groen hier als
> meer dan het is. Dit blijft de eerlijkste manier om het te rapporteren zolang er geen kaal profiel is.

### A1 — het settings-fragment

Plak het blok letterlijk in een leeg bestand en parse het. Weet je waar `.claude/settings.json` staat, en dat
je die map zelf moet aanmaken?

### A2 — de twee commando's *(#329 — DE vraag van deze ronde)*

Op een profiel waar deze marketplace **nooit** heeft bestaan faalde `claude plugin marketplace update` in v10:
het **eerste uitvoerbare commando van het hele pad**. Het document zet de registratie er nu vóór — je
herstart eerst één keer, want de marketplace wordt door een **sessiestart** geregistreerd, niet door het
schrijven van de sleutel.

**Dankzij de geschrapte stap 0a is jouw profiel nu werkelijk maagdelijk, dus dit is de eerste keer dat het
antwoord telt.** Loopt het weer vast, dan is dat de zwaarste bevinding van de ronde. Loopt het door, dan is
v10's laatste blokkade aantoonbaar weg.

Twee extra metingen op deze stap:

- **#336** — neem **SHA256 van `settings.json` vóór en ná** `claude plugin install --scope project`, mét de
  key al aanwezig. Het document belooft een *equivalente* inhoud maar **geen** byte-identiek bestand.
- **#325** — twee installs met de verificatiequery erna: één tegen een **`project`**-record, één tegen een
  **`local`**-record. De eerste moet **één** regel geven, de tweede **twee**.

### A3 — herstart en de verificatiequery *(#355 — nieuw in `v3.1.1`)*

De query is uitgebreid met een **vierde veld**. Hij drukt nu per regel `payload present` of `PAYLOAD MISSING`
af, omdat v11 een profiel mat waar het record `project 3.1.0` met de juiste sha claimde terwijl `installPath`
naar een map wees die **niet bestond** — drie sessies lang laadde er niets, en de oude query gaf een schone
regel.

- **Krijg je `payload present`?** Zo ja, dan doet de nieuwe query wat hij belooft.
- **Levert de query één `project`-regel per plugin?** En kun je uit `version` en `gitCommitSha` aflezen welke
  code je draait?
- **#322** — de tagvergelijking gebruikt `rev-parse HEAD` plus een `gh api`-route. Noteer wat je clone draagt
  (shallow? welke tags?) en of het document doet wat het belooft — inclusief of de `fatal: ambiguous
  argument`-uitkomst als **verwachte derde mogelijkheid** benoemd staat.

> v11 vond hier één claim die niet klopte: *"`main` only, **geen tags**"* was niet waar van zijn verse clone,
> die `v3.1.0` wél droeg. **De verse clone van deze ronde draagt 0 tags** (gemeten op 2 augustus 2026, vóór de
> ronde). Kijk of het document daar inmiddels iets anders over zegt, en of jouw clone het bevestigt.

### A4 — stap 2, de bootstrap-skill

Bestaat `specialists-init` in de nieuwe sessie? Dan drie metingen die alle drie op `v3.1.1`-reparaties slaan:

- **#358** — de QUICKSTART drukt een verwachte `Done:`-regel af. In v11 was de **derde** clausule omgekeerd:
  de pagina beloofde `0 script-scaffold(s) created, 2 already present` terwijl een verse repo het omgekeerde
  print. De pagina zou nu het **verse**-geval moeten tonen én moeten uitleggen dat je elk paar leest als
  `created + already present`. **Klopt de afgedrukte regel nu met wat jij ziet?** Jouw repo is vers, dus je
  hoort `2 script-scaffold(s) created, 0 already present` te krijgen.
- **#363** — kijk in `.claude/settings.suggested.jsonc`. Het voorgestelde `Stop`-hook-pad hoort nu zichtbaar
  een **placeholder** te zijn (iets met punthaken) in plaats van een geloofwaardig echt pad, en stap 3 van de
  next-steps hoort te zeggen **welk blok bruikbaar is en welk niet**. Controleer ook of het bestand op een
  **slotregel** eindigt — dat deed het niet.
- **#333 / #337.4 / #337.2** — tel de `[ERROR]`-regels van de sessie ná de bootstrap (verwacht: nul, plus
  precies één niet-tellende `[ROSTER-PENDING]` die de seam noemt); kijk of de bootstrap *"the orchestrator
  import"* in **enkelvoud** zegt en of de `settings.suggested.jsonc`-regel zegt welke kant deze repo op staat
  (deze repo heeft geen `.gitignore`, dus: **niet** gitignored); meet de regeleindes en de slotregel van
  `CLAUDE.md`.

### A5 — stap 3, herstart en verificatie *(#361 — de check is veranderd)*

v11 kreeg hier de scherpste soort bevinding: de QUICKSTART zei *"controleer of elke beurt opent met een
afzenderregel zoals `🧭 Chris — intake & routing`"*, en **geen enkele** gebootstrapte repo produceert die
regel. De portable persona schrijft hem nergens voor; hij is een huisregel van de workshop zelf. De check was
dus onhaalbaar, op de stap die moet bewijzen dát de installatie werkt.

De pagina hoort nu te vragen naar de **invariant**: een genoemde eigenaar mét reden, in de vorm
*"This one is for \<naam\> — \<reden\>."*

- **Vraagt het document nu naar de invariant in plaats van naar een vaste string?**
- **En zie je dat gedrag ook echt?** Doe een gewone werkvraag en noteer letterlijk hoe de beurt opent.
- **#330** — `Select-String '@' .claude/specialists/SPECIALISTS.md`: wijst het importpad naar `marketplaces/`
  of naar `cache/`? Het document zegt dat het de **clone** is en dat die `main` volgt.

### A6 — de release-kaart *(#368 — dit was consumentzichtbaar kapot)*

Open `RELEASE.md` in je plugin-cache (de QUICKSTART wijst je erheen na een update).

`v3.1.0` verscheepte die kaart met **145 dubbel-geëncodeerde reeksen** erin — streepjes, pijlen en ellipsen,
ontstaan doordat een reparatiegereedschap ze niet herkende en de poort ernaast daarom "clean" meldde. (Geteld
op de tag `v3.1.0` met een detector die op reeksen telt; wie op losse tekenparen telt komt op 435. Twee
manieren om hetzelfde te meten.) **Lees de kaart en zeg of de tekst nu leesbaar is** — em-streepjes als `—`
en niet als een reeks vreemde tekens. Dit is de enige reparatie van deze ronde die je met het blote oog
beoordeelt.

**Slotvraag van test A, en die is binair:** heb je het **zonder de meter** gered? Zo nee: op welke stap en in
welke klasse. En naast v10 en v11: **is #329 weg?**

---

## Test B — UNINSTALL, alleen met UNINSTALL.md

Begint waar A eindigt: een werkende, geadopteerde repo. Je krijgt nu **alleen** `UNINSTALL.md`. Leun niet op
wat je in test A hebt geleerd.

### B0 — vind je het document?

Zelfde vraag als A0, andere route.

### B1 — de pre-flight

- **#332** — doe `git add -A` **zonder** te committen, en draai dan pre-flight 2. Die moet **commits** meten
  (`git ls-tree ... HEAD`), niet de index, en de `fatal: ... HEAD`-uitkomst benoemen als "helemaal geen
  commits". Let op: jouw clone hééft commits, dus dit is de andere kant van die meting dan v11 mat.
- **#330, tweede helft** — wordt de `<plugin>`-placeholder uitgelegd, en kun je dat pad vinden **zonder te
  gokken** tussen de clone en de cache?

### B2 — de teardown: preview, dan `-Apply` *(#356 en #362)*

- **#356** — dit is de scherpste telling van de ronde. v11 zag de teardown **twee `[KEEP]`-regels** printen,
  een `[note]` die *"2 line(s)"* zei, en daaronder een samenvatting die **`0 kept`** meldde. **Tel de
  `[KEEP]`-regels en vergelijk ze met het getal in de `Summary:`-regel.** Die twee horen nu gelijk te zijn,
  in preview én na `-Apply`. Kijk ook of de overgebleven regels onder een **eigen kopje** staan in plaats van
  onder het `-EmptyLensPattern`-advies, dat voor proza nergens op slaat.
- **#331** — deze repo had vóór de adoptie géén `CLAUDE.md`, dus dit is opnieuw de **verse**-consumerrij.
  Blijft er bootstrap-proza staan, wordt dat als `[KEEP]` gerapporteerd, en is `scripts\lib\` **weg** in
  plaats van als lege map achtergebleven?
- **#362** — lees in `UNINSTALL.md` de sectie *"What is left behind, honestly"*. Die zei dat het achtergebleven
  proza inert is. Dat is het **niet**: `CLAUDE.md` wordt elke sessie als project-instructie geladen, dus die
  twee regels blijven latere sessies vertellen dat de repo bestuurd wordt door een systeem dat weg is — twee
  verse sessies merkten die tegenspraak in v11 uit zichzelf op. **Staat er nu wat het proza dóét, en krijg je
  een concrete remedie?** En: start ná de teardown een verse sessie in die repo en kijk of hij de tegenspraak
  nog steeds opmerkt.

### B3 — `claude plugin uninstall … --scope project` *(#359)*

- Doe het commando **eerst bewust zonder** `--scope project`. v11 mat op CLI `2.1.220` dat de melding niet
  meer luidt zoals het document voorspelde, en dat de CLI zélf `claude plugin disable … --scope local`
  aanraadt — een **ander** commando dan de procedure, dat een sleutel toevoegt in plaats van de installatie
  weghaalt. **Noteer letterlijk wat jouw CLI zegt, en welke versie dat is.** Zegt het document nu dat de
  bewoording versiegebonden is, en dat je de suggestie van de CLI hier **niet** moet volgen?
- Daarna met de vlag. Werkt hij zoals beschreven?
- **#337.5** — verschijnt er een `.orphaned_at`? Het document voorspelt hem.

### B4 — de sleutels terug uit `settings.json`, plus `marketplace remove` *(#357)*

- **#328** — stap 3 verwijderde ooit **het document dat je aan het lezen bent**, en stap 4's audit-gereedschap
  ging met stap 2 mee. Loop de stappen in de gedrukte volgorde en kijk of document en audit aan het eind nog
  bestaan.
- **#337.6** — staat er nog een `permissions`-entry die naar de plugin-map wijst? v11 kwam hier niet aan toe
  omdat de voorwaarde zich niet voordeed; kijk of dat nu wel zo is.
- **#357** — draai `claude plugin marketplace remove davekjohns-workshop` en kijk daarna in
  `~/.claude/settings.json`. Er hoort een **lege** `extraKnownMarketplaces: {}` achter te blijven, en de
  sleutelvolgorde kan verschoven zijn. **Zegt stap 5 dat nu vooraf?** v11 vond dat het spiegelbeeld
  (`enabledPlugins: {}` bij stap 3) er wél stond en dit niet.
- **#339** — loop de zes locaties opnieuw af: clone weg, cache blijft, `marketplace list` noemt hem niet meer.

### B6 — de zelfherstellende val: drie reparaties op één plek

Laat bewust **één `enabledPlugins`-sleutel** staan en start een sessie.

- **#327** — verschijnt er een vers install-record terwijl die sessie **zelf niets laadt**? Verifieer via de
  **skill-lijst**, niet via het record.
- **#323** — ontstaat de **padloze `user`**-vorm, en wordt die gemeld als `[RECORD-SHAPE]`? *In v11 niet
  gemeten: die vorm deed zich niet voor.*
- **#324** — is het **remedie** uit de sessie-output te lezen, **zonder** de check zelf te draaien? *In v11
  niet gemeten: er laadde geen plugin, dus er draaide geen hook.*

**Slotvraag van test B:** staat het profiel daarna aantoonbaar weer op de stap-0-tabel? Zo nee: wat is er
over, en **zegt het document dat vooraf**?

---

## Wat deze ronde bewust niet doet

Schrijf dit over in het rapport, zodat de bron weet wat er **niet** gemeten is en het niet als groen leest.

| Niet in scope | Waarom |
|---|---|
| De teardown-regressie in twee cycli | Dat is de **bezette**-consumertest. v12 is de verse; `life-hub` wordt niet aangeraakt |
| Een vers **apparaat** | Zelfde machine, CLI, git en OS. Dit is een verse **gebruiker** |
| De poorten en checks bij de bron | Van buiten onzichtbaar. Twee van v3.1.1's reparaties (de zelf-committende fold, de poort op voorbeeld-output) zitten volledig aan de bronkant en zijn hier per definitie niet te zien |
| Een handmatige test A van begin tot eind | Draag je de uitvoering over aan een sessie, dan meet je **A′** — AI-geassisteerde adoptie. Zeg dat dan, zoals v10 en v11 deden |
| De aantallen-sweep door de documentatie | v12 meet het pad en de negen reparaties, niet of elk getal in elk document klopt |
| Het #283-CRLF-artefact | Deze repo heeft geen `.gitignore`. **Bouw er geen** — een oneerlijke fixture is geen meting |

## Aan het eind

Vul [`RESULTATEN.md`](RESULTATEN.md) in: de stappentabel naast die van v10 en v11, de negen reparaties met per
stuk **geverifieerd / niet gemeten / nog stuk**, en de gaps. Daarna de issues bij de bron en de uitkomst in
het dossier in `life-hub`.

**En zeg wat je met het profiel doet** — opruimen of laten staan is beide goed, maar een half opgeruimd
profiel is voor de vólgende ronde geen nulstand meer. Dat is dezelfde les die v9 zijn nulstandtabel kostte.

Reken op **weinig** bevindingen in klasse 1 en 2: v11 vond er negen en alle negen zijn gerepareerd. Wat
overblijft is de interessante categorie — **klasse 4**, het staat er en de lezer miste het toch — plus de
vier metingen die v11 niet kon doen (#323, #324, #329, #337.6). Die vier zijn de eigenlijke opbrengst van
deze ronde.

> **Voor wie v13 schrijft:** noem bij elk ijkpunt hoe het gemeten is, of geef de git-blob-grootte en zeg dat
> erbij. Hetzelfde geldt voor een geciteerde CLI-melding (versiegebonden) en voor voorbeeld-output van een
> script (gebonden aan de toestand van de repo waarin hij is opgenomen). Drie verschijningen van één klasse,
> alle drie gevonden in v11 — en aan de bronkant inmiddels bewaakt door een poort die déze papieren niet kan
> bereiken. Hier moet de gewoonte het werk doen.
