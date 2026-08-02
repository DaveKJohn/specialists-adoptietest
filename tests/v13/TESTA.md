# Test A — INSTALL, alleen met de QUICKSTART (vervolg van MEETLOG.md)

## A0 — de route naar binnen — GROEN

Ingang: `github.com/DaveKJohn/davekjohns-workshop`. De Quickstart-link staat in een blockquote **direct onder
de openingsalinea**, tweede blok van de pagina, met de `Before you start`-verwijzing erbij en UNINSTALL.md als
spiegel. Nul beslissingen vooraf: een consument klikt één keer. #338 houdt (groen in v11, v12, v13).

## Vóór A1 — de drie obstakels (#334) — GROEN (tekst), niets opnieuw gemeten

Eerlijk gescoord: **alle drie stonden al**. Claude Code draait (`2.1.220`), ingelogd, en de
`ExecutionPolicy` staat al ruim genoeg — elk `.ps1` in deze ronde liep zonder `PSSecurityException`,
inclusief `claude.ps1` zelf (`C:\Users\davek_onn\AppData\Roaming\npm\claude.ps1`, zichtbaar in de
foutuitvoer van A2). PATH + editor-herstart: niet opnieuw gemeten, geen verse installatie. Hoogst haalbare
score is dus "groen (tekst)", net als v10 t/m v12.

## A1 — het settings-fragment (#335) — GROEN

Blok letterlijk geplakt in een nieuw `.claude/settings.json` (map bestond niet, moest zelf aangemaakt —
het document zegt dat expliciet: *"`.claude/` is a directory in the root of your repository, beside your
`README.md` — create it if it is not there"*). `ConvertFrom-Json` parst zonder fout; twee sleutels
gevonden. Geen BOM, LF-regeleindes, sluit af met newline. 224 bytes.

**SHA256 vóór de install: `F694FB443E52E86B7861E5DDBA9D6BBC79CF0B1D5C147887672F4CDBBF15EFA8`.**

## A2 regel 1 — `marketplace update` (#329) — GROEN, val opnieuw gereproduceerd

Volgorde bewust zo gelopen: settings geschreven, **geen** sessiestart, meteen regel 1.

```
✘ Failed to update marketplace(s): Marketplace 'davekjohns-workshop' not found. Available marketplaces:
```

exit 1. `claude plugin marketplace list` → `No marketplaces configured`.

Daarna één sessiestart in de repo (`claude -p` headless, exit 0). Direct erna:
`claude plugin marketplace list` → `davekjohns-workshop / Source: GitHub (DaveKJohn/davekjohns-workshop)`.
En regel 1 opnieuw → `✔ Successfully updated marketplace: davekjohns-workshop`, exit 0.

**#329's drie toestanden exact gereproduceerd, en de reparatie houdt.** Een sessiestart registreert de
marketplace; het schrijven van de sleutel doet dat niet.

## A2 — de lege `Available marketplaces`-lijst — WRIJVING, niet gerepareerd

De v12-observatie reproduceert, en scherper. Het document drukt af:

```
✘ Failed to update marketplace(s): Marketplace 'davekjohns-workshop' not found.
  Available marketplaces: claude-plugins-official
```

Mijn maagdelijke profiel drukt af: `Available marketplaces:` **en dan niets**. Het document dekt dit nog
steeds alleen af met *"The wording is version-bound and may differ on your CLI"* — maar het verschil zit
niet in de bewoording, het zit in de **toestand** van het profiel. Precies wat v12 opschreef; het document
zegt het nog niet.

Extra detail dat de diagnose aanscherpt: `~/.claude/plugins/marketplaces/claude-plugins-official/` bestaat
wél op schijf, terwijl `marketplace list` `No marketplaces configured` zegt. Een map onder `marketplaces/`
is dus géén registratie, en de lijst in de foutmelding leest registraties, niet mappen.

## A2 regel 2 — `install --scope project` — GROEN

`✔ Successfully installed plugin: specialists@davekjohns-workshop (scope: project)`, exit 0. Geen
versienummer in de uitvoer, zoals het document zegt.

## A2 — `settings.json` herschreven (#336) — GROEN, exact voorspeld

| | |
|---|---|
| vóór | `F694FB44…BF15EFA8`, **224 bytes** |
| ná | `EB8834F7…AB275E4A`, **246 bytes** |
| delta | **+22 bytes** |

De twee wijzigingen zijn precies de gedocumenteerde: `enabledPlugins` schuift vóór
`extraKnownMarketplaces`, en het geneste `source`-object wordt over eigen regels uitgeklapt. Geen
gedragsverandering.

**Nieuwe observatie (klein, klasse 1).** Het document zegt over dit hash-paar: *"Those two hashes are that
profile's and are not something to match — they are here to show a pair that differs; yours will differ
from each other too, at different values."* Mijn beide hashes zijn **identiek aan de gedrukte**. Dat is
geen toeval: wie het gedrukte blok letterlijk in een leeg bestand plakt krijgt dezelfde 224 bytes, en de
serialiser is deterministisch. De waarschuwing is dus te sterk — op het voorgeschreven pad is het paar
juist **reproduceerbaar**, en dat is bruikbaarder dan "niet iets om te matchen".

## A2 extra — install tegen `project` vs `local` (#325) — GROEN

Query met alleen het project-record: **1 regel**. Na `install --scope local`: **2 regels**
(`project 3.1.2 …` en `local 3.1.2 …`). Exact de gedocumenteerde aantallen. Opgeruimd met
`uninstall --scope local` → terug naar 1 regel; `.claude/settings.local.json` blijft achter met
`{"enabledPlugins": {}}`, wat het document ook voorspelt.

## A3 — de verificatiequery + `payload` (#355) — GROEN

```
specialists@davekjohns-workshop -> project 3.1.2 ad84ef97906d6e68023a3834cd8fcf56aedbaf70 [payload present]
```

Eén `project`-regel, `payload present`. Uit `version` (3.1.2) en `gitCommitSha` (`ad84ef9`) is af te lezen
welke code draait — en die twee spreken elkaar hier tegen, wat precies het interessante geval is.
`installPath` = `…\cache\davekjohns-workshop\specialists\3.1.2`, `installedAt` 2026-08-02T18:25:58Z.

## A3 extra — de tagvergelijking (#372) — GROEN, en de reparatie is geverifieerd

**Mijn clone, gemeten in de gedrukte volgorde:**

| commando | uitkomst |
|---|---|
| `git rev-parse HEAD` | `ad84ef97906d6e68023a3834cd8fcf56aedbaf70` |
| `git tag` | **0 tags** |
| `git cat-file -t v3.1.2` | `fatal: Not a valid object name v3.1.2` |
| `git rev-parse v3.1.2` | `fatal: ambiguous argument 'v3.1.2'` |
| `git rev-parse "v3.1.2^{}"` | `fatal: ambiguous argument 'v3.1.2^{}'` |

Clone-stand: **shallow** (`.git/shallow` bevat `ad84ef9`), **depth 1** (`rev-list --count HEAD` = 1),
refspec `+refs/heads/main:refs/remotes/origin/main`, branch `main` met upstream `origin/main`, geen enkele
tag-ref. De release-commit `101d597` zit **niet eens in de object-store**.

**Toetsing van de vier eisen aan het gerepareerde `#322`-blok:**

1. *"main only, no tags"* / *"a shallow, tag-less clone"* — **weg.** Er staat nu: shallow + main-only
   refspec *"does not mean the clone is tag-less"*, met de nuance dat de tagset bevroren is op wat er bij
   het klonen meekwam. ✓
2. **derde uitkomst genoemd** — ja, met een uitgewerkt voorbeeld op `v3.1.1` uit v12: `rev-parse v3.1.1`
   geeft het tag-object `12b2d1b…`, `rev-parse "v3.1.1^{}"` de commit `4b1a74d…`. ✓
3. **peel met `^{}`** — expliciet: *"If you resolve a tag locally at all, peel it with `^{}`"*. ✓
4. **`gh api`-route immuun** — expliciet: *"the API's `.commit.sha` is the commit already, annotated tag or
   not."* ✓

**Kwam ik op de juiste reden uit?** Ja. Mijn uitkomst is `fatal: ambiguous argument`, en het document zegt
letterlijk: *"If the `fatal: ambiguous argument` line is what you get, that is expected and it is evidence
of nothing — the tag is simply not in your clone."* Het stuurt me door naar de immune route:

```
gh api …/tags → v3.1.2 .commit.sha = 101d597db3754f973bd67c92930963be43108bdd
record gitCommitSha                = ad84ef97906d6e68023a3834cd8fcf56aedbaf70
→ VERSCHILLEND → ik draai main
```

Dat is de **juiste conclusie om de juiste reden**: ik draai `main`, ná de release. Het document heeft de
val die v10 blokkeerde en v11/v12 wrijving gaf, deze ronde volledig weggenomen. **#372 → geverifieerd.**

**Wel een nieuw datapunt voor de bron.** Het gerepareerde bullet zegt *"both measured clones had them — a
fresh one carrying `v3.1.1`, an older one carrying 66 tags"* en noemt de oude *"no tags"*-tekst *"simply
wrong"*. Mijn verse clone — aangemaakt door **sessiestart-registratie** op CLI `2.1.220` — is een
**depth-1 shallow clone met nul tags**. Dus de oude tekst was niet universeel fout, hij was
niet-universeel juist. Het document overleeft dit ongeschonden (de twee bullets erna zeggen exact dat het
per machine verschilt, en de `fatal:`-regel vangt mijn geval op), maar de bewijsvoering van dat ene bullet
mist de tag-loze verse clone. Geen blokkade, wel iets dat de bron zou willen weten.

## A6 — de release-kaart (#368) — GROEN

`RELEASE.md` van `v3.1.2` in de cache: em-streepjes renderen als `—`, geen mojibake, geen
vervangingstekens. Met het blote oog leesbaar.

**Nieuwe observatie (klasse 1, klein).** Regel 8 van de kaart zegt onvoorwaardelijk: **"You are on this
release."** Gemeten: dat is voor mij niet waar — mijn payload komt van `ad84ef9` (`main`), drie commits ná
`v3.1.2`. Inhoudelijk maakt het hier niets uit (die drie commits raken de payload niet), maar de zin is een
kale bewering op precies het punt waar de QUICKSTART zelf zegt dat *"the documented update path cannot
deliver a tagged release"*. De kaart kan dit niet weten en zou het dus niet moeten stellen.

## A4 — de skill-wrapper `/specialists-init` — poging 1 (headless) mislukt

Zoals v11/v12: `claude -p "/specialists-init"` in de repo-root. **Exit 0, maar de bootstrap is niet
gedraaid.** De sub-sessie rapporteerde letterlijk:

> *"I could not complete the bootstrap — the script execution is blocked by permissions in this
> non-interactive session."*

met een tabel van vier geweigerde aanroeppogingen (`powershell -NoProfile -File …bootstrap.ps1` via de
PowerShell-tool → *"nested process, not validatable"*; via Bash → *"requires approval"*; in-process `&` →
*"requires approval"*; kaal scriptpad → *"requires approval"*). Ook lezen buiten de projectmap werd
geweigerd, dus zelfs `bootstrap.ps1` inzien lukte niet.

**Dit is een harnasgrens, geen bevinding over de plugin** — en de meting is deze keer preciezer dan
v11/v12's samenvatting. Wat wél is aangetoond door deze poging:

- de skill **laadt en start gewoon**: `specialists:specialists-init` stond in de slash-lijst;
- **alle drie de session-start hooks meldden zich** (`connector-sessioncheck`,
  `script-contract-sessioncheck`, `roster-sessioncheck`);
- de skill deed zijn eigen stap 0a/0c-controles en kwam correct uit op *"fresh-consumer path"*;
- en hij **weigerde de bootstrap met de hand na te bouwen**, met een expliciete motivering (*"A hand-built
  approximation that looks correct is worse than nothing here — it would pass a glance and fail the drift
  lint"*) plus het exacte terugvalcommando. Dat is precies het gedrag dat je wilt zien.

Het breekpunt is dus **niet** de skill-aanroep — dat was de aanname na v11/v12 — maar uitsluitend het
uitvoeren van `bootstrap.ps1` vanuit een non-interactieve sessie. **Poging 2 (interactief) staat open.**

## A4 — poging 2, INTERACTIEF — GROEN. Eerste keer in de reeks dat dit pad gelopen is

Uitgevoerd 2026-08-02, in een interactieve sessie (deze), na een echte herstart van Claude Code. Dave
hervatte met `--continue` in de repo; `/specialists-init` is daarna in die sessie aangeroepen.

**De skill laadde en drukte zijn volledige pagina af.** Daarna het gedrukte commando, letterlijk, vanuit
de repo-root:

```powershell
powershell -NoProfile -File "C:/Users/davek_onn/.claude/plugins/cache/davekjohns-workshop/specialists/3.1.2/skills/specialists-init/bootstrap.ps1"
```

**Dit liep door.** Precies dezelfde aanroepvorm die headless werd geweigerd met *"nested process, not
validatable"*. De harnasgrens van v11/v12 is dus **uitsluitend** een eigenschap van de non-interactieve
sessie — de skill zelf en zijn script zijn in orde.

### De `Done:`-regel — #358 geverifieerd

```
Done: 4 persona-lens(es) created, 0 already present; 15 lens-scaffold(s) created, 0 already present; 2 script-scaffold(s) created, 0 already present.
```

De opdracht voorspelde voor een verse repo `2 script-scaffold(s) created, 0 already present` — **exact
gehaald**. De andere twee tellers kloppen met wat de pagina belooft: vier persona-lenzen (Chris `01-01`,
Bianca `03-02`, Derek `05-05`, Rendall `05-06` — de pagina noemt die vier bij naam) en 15 lege
lens-scaffolds, samen 19 lenzen. Dat getal 19 komt exact terug in de `[ROSTER-PENDING]`-regel hieronder
en in het connector-manifest (19 extensions). **Drie onafhankelijke plekken, hetzelfde getal.**

### Wat er op schijf kwam

23 nieuwe bestanden, geen enkele overschrijving (de repo had er nul van). `CLAUDE.md` is aangemaakt als
scaffold, 463 bytes, 9 regels, met **precies één** `@`-importregel — zoals de seam-belofte zegt.
`.claude/settings.json` is **niet** aangeraakt (nog steeds 246 bytes, de A2-waarde).

### #330 — waar wijst het `@`-importpad heen?

Naar de **clone**, niet naar de cache:

```
@~/.claude/plugins/marketplaces/davekjohns-workshop/claude-code-plugins/claude-specialists/specialists/personas/01-01-persona.md
```

Dat pad bestaat en resolvet (7943 bytes, 122 regels). **En deze ronde is dat voor het eerst een
inhoudelijke vraag**, want de clone staat op `main` (`ad84ef9`) en het install-record wijst naar de
version-pinned cache `…/specialists/3.1.2`. Gemeten: **alle vier de persona-bodies zijn byte-identiek**
tussen clone en cache (`01-01` sha256 `73b67c1b872b0fa6…`, idem voor `03-02`, `05-05`, `05-06`). De drie
commits die `main` vóór ligt raken de payload inderdaad niet, zoals de opdracht voorspelde.

**Wel het structurele punt vasthouden:** de persona-body wordt geladen uit de clone, die `main` volgt en
door elke `marketplace update` verschuift — terwijl het install-record een *gepinde* 3.1.2-cache noemt.
Deze ronde vallen die samen; dat is niet gegarandeerd. Geen blokkade, wel het antwoord op #330: **clone**.

### #363 / #337.4 — de hook-stub

`.claude/settings.suggested.jsonc` (1090 bytes) is zichtbaar een voorstel: kopregel *"PROPOSAL — created
by specialists-init. This is NOT active configuration."* De hooks-stub is onmiskenbaar een placeholder —
het scriptpad is `scripts/maintenance/<your-check>.ps1`, mét punthaken — en heeft de gevraagde slotregel:

> *"Replace the angle-bracketed name with a real script in this repo, or drop the `hooks` key."*

Daarboven staat ook nog waaróm: *"Copying this block as-is activates a hook pointing at nothing."*
**Groen.** Het `permissions.deny`-blok is wél direct bruikbaar en het bestand zegt dat onderscheid zelf.

## A4 — de sessie ná de bootstrap (#333 / #337.2) — GROEN

Verse sessie in de repo, hookuitvoer letterlijk:

```
connector-sessioncheck: no verified workshop checkout found on this machine -- check skipped.
script-contract-sessioncheck: script contract in sync with the shared workflow scripts.
roster-sessioncheck: this repo is set up; the roster and lenses are still to be filled in:
  [ROSTER-PENDING] this repo was bootstrapped but the roster is still empty: 19 specialist(s) have a
  lens scaffold and no roster row in .claude/specialists/SPECIALISTS.md. Nothing is broken and nothing
  has drifted -- every lens is still an unfilled VUL-IN scaffold, so there is no work to have drifted
  from. Fill in the roster and the lenses at your own pace; this turns into real drift, reported per
  specialist, as soon as some of it is filled in and some is not.
```

**Nul `[ERROR]`-regels, precies één `[ROSTER-PENDING]`, en die telt zichzelf expliciet niet mee**
(*"Nothing is broken and nothing has drifted"*). Regressiecheck gehaald.

Ter vergelijking dezelfde twee hooks vóór de bootstrap, in de sessie waarin deze ronde hervat werd: beide
meldden `[BOOTSTRAP]` met *"this repo has not been set up yet"* en verwezen naar de `specialists-init`
skill. De omslag `[BOOTSTRAP]` → *"in sync"* / `[ROSTER-PENDING]` is dus zelf ook gemeten.

**Nieuwe observatie (klasse 1, klein) — de `[UNREGISTERED]`-belofte gaat hier niet op.** De skill zegt bij
zijn slotstap 6 onvoorwaardelijk: *"Until it is registered, this repo's own session start says so —
`connector-sessioncheck` surfaces an `[UNREGISTERED]` line next to its verdict."* Deze repo is **niet**
geregistreerd, en de hook drukt géén `[UNREGISTERED]` af — hij zegt *"no verified workshop checkout found
on this machine -- check skipped."* De check slaat zichzelf over vóórdat hij aan registratie toekomt. De
belofte is dus voorwaardelijk (ze geldt alleen op een machine mét geverifieerde workshop-checkout) maar
staat er onvoorwaardelijk. Een consument zonder workshop-checkout — de normale consument — krijgt de
herinnering nooit, en dat is precies de lezer voor wie stap 6 het makkelijkst blijft liggen.

## A5 — herstart en verificatie (#361) — GROEN

Gewone werkvraag in een verse sessie na de bootstrap: *"Ik wil de README van deze repo een stuk
duidelijker maken voor nieuwe lezers. Hoe pak ik dat hier aan?"*

**Hoe de beurt opent, letterlijk:** `Goed — ik zet de lijn uit. Eerst wat ik heb gezien, dan de route.`

Dus **geen vaste afzenderregel** — en dat is precies wat #361 wilde. De **invariant** staat er wél, onder
het kopje *De route*:

> **Dit is er een voor Tessa 📝 — de technical writer.**

Een **genoemde eigenaar mét reden**, in de vorm die het document beschrijft. Direct erna de keten (Tessa
schrijft → Edith doet de onafhankelijke taalcontrole op de diff → PR) en twee poortvoorwaarden vooraf:
branchdiscipline via de `new-branch`-skill, en de constatering dat de roster nog leeg is (*"ik route nu op
de plugin-beschrijvingen in plaats van op jouw ingevulde lens"*). Chris antwoordde in het Nederlands,
volgend op de taal van de vraag.

Twee dingen die dit sterker maken dan een formele "de invariant staat er"-vinkje: hij **weigerde** de
README uit te breiden op de manier die de fixture-blockquote verbiedt (hij las die blockquote en zag de
spanning met *"duidelijker voor nieuwe lezers"*), en hij liet de ongecommitte bootstrap-bestanden
expliciet met rust als *"van de bootstrap, niet van dit werk"*. De orchestrator werkt hier dus echt, niet
alleen zijn openingszin.

## Slotvraag test A — heb ik het zonder de meter gered?

**Ja.** Nul blokkades in test A. Eén stap (A4) had een tweede poging nodig via een ander sessietype, en
dat is een harnasgrens die het document niet kan wegnemen. Alle overige stappen liepen op het document
alleen.
