# Testronde v11 — hoe deze ronde gedraaid wordt

Dit is de **werkmap** van ronde v11. Hier staat de opdracht, hier komen de resultaten, en dit bestand legt
uit hoe er gemeten wordt.

| bestand | wat het is |
|---|---|
| [`OPDRACHT.md`](OPDRACHT.md) | de uitvoerbare opdracht — stap voor stap, in de volgorde waarin je hem loopt |
| [`RESULTATEN.md`](RESULTATEN.md) | de uitkomst: de stappentabel, de zeventien reparaties, en de gaps |

> **Waarom dit op een aparte branch staat en niet op `main`.** `main` van deze repo is de **fixture**: één
> commit, alleen een `README.md`, geen `.claude/`, geen `CLAUDE.md`. Stap 0 van de ronde bewijst dat er
> *niets* is om correct over te zijn, en test B meet daarna of de repo weer helemaal vrij is van de plugin.
> Die tweede meting kijkt naar **elke verwijzing naar de plugin in de repo** — en een opdrachttekst vol
> `specialists-init` en `enabledPlugins` zou daar tientallen valse treffers opleveren, precies in de stap die
> een van de zeventien reparaties moet verifiëren. Dus: `main` blijft maagdelijk, deze branch draagt de
> papieren.
>
## Waar je de ronde draait, en waar je dit leest

**Je draait de ronde op `main`.** Dat is de repo onder test: je kloont hem, adopteert hem, en breekt hem
daarna af. Deze map is alleen papier.

**Doe dit NIET.** Het is de meest natuurlijke handeling en hij vervuilt precies wat de aparte branch moet
beschermen — `tests/v11/` staat dan ineens in de werkkopie die je aan het meten bent, en de teardown-audit van
stap B2 vindt daar tientallen verwijzingen naar de plugin die niets met de adoptie te maken hebben:

```powershell
cd specialists-adoptietest
git checkout tests/v11    # FOUT -- dit haalt de papieren in de geteste repo
```

**Doe dit wel** — twee losse werkkopieën, of lees mee in de browser:

```powershell
# 1. de repo die je test
git clone https://github.com/DaveKJohn/specialists-adoptietest.git

# 2. de papieren, in een EIGEN map ernaast
git clone -b tests/v11 https://github.com/DaveKJohn/specialists-adoptietest.git v11-opdracht
```

Of gewoon op GitHub: `tests/v11` op de branch `tests/v11`.

> **Deze alinea is zelf een klasse-3-bevinding geweest**, en hij staat er daarom uitgeschreven. De eerste
> versie zei *"lees deze map op GitHub of via `git checkout tests/v11` in een tweede map"* — dubbelzinnig, want
> een `checkout` in een tweede map bestaat niet als handeling, en de meest natuurlijke lezing is precies de
> foute. Dave stelde de vraag *"ik start de opdracht wel op `main` toch?"* voordat er iets misging. Dat is
> exact de klasse die deze rondes meten: de informatie stond er, en toch kon je hem verkeerd lezen.

> **De `README.md` op `main` zegt nog "voor testronde v10".** Dat is bewust niet aangepast: v10 heeft de
> bytegrootte van dat bestand als ijkpunt vastgelegd, en die veranderen kost de vergelijking met v10.

## De rollen, en de ene regel die de meting draagt

| rol | wie | wat hij doet |
|---|---|---|
| **De consument** | Dave | Leest **uitsluitend** het document dat bij de test hoort. Volgt de stappen zoals ze er staan. Vraagt de meter niets |
| **De meter** | Claude | Kijkt en noteert, grijpt niet in. Legt per stap vast: het commando, de werkelijke output, wat het document voorspelde, en of de consument verder kon |

**De regel die alles draagt: nooit stil repareren.** Loopt de consument vast en weet de meter het antwoord,
dan wordt **eerst de blokkade vastgelegd** — letterlijk, met wat de consument op dat moment voor zich had en
wat hij zocht — en pas daarna geholpen. Een blokkade die is opgelost vóórdat hij is opgeschreven, heeft voor
het rapport niet bestaan.

Dit is de omkering van rondes v3 t/m v9, en het is waarom v10 dingen vond die die negen niet konden: daar was
de meter ook de uitvoerder, en die wist te veel. **Een pad dat werkt omdat de uitvoerder de omweg al kende,
is niet gemeten.**

## Vier soorten blokkade

De klasse hoort bij elke bevinding, want de bron heeft er een ander antwoord op nodig.

| klasse | wat het is |
|---|---|
| **1** | het document zegt iets dat **niet klopt** |
| **2** | het document **zegt het niet** — een stap die je moet weten en die er niet staat |
| **3** | het document zegt het **te laat of op de verkeerde plek** — de informatie is er, maar nadat je hem nodig had |
| **4** | het document **klopt en de consument miste het toch** — óók een bevinding, over vorm en vindbaarheid. **Niet** wegstrepen als gebruikersfout: een document dat correct is en niet werkt, werkt niet |

## Eén regel erbij, want dit is een verificatieronde

**Een reparatie die je niet hebt zien werken, is niet geverifieerd.** *"Het staat er nu"* is geen meting — de
vraag is of de stap die vroeger vastliep nu doorloopt. Waar een reparatie alleen tekst is, citeer de nieuwe
tekst en zeg of hij het probleem dat hij beschrijft oplost **voor iemand die het niet al wist**.

In `RESULTATEN.md` krijgt elke reparatie daarom één van drie woorden: **geverifieerd**, **niet gemeten**, of
**nog stuk**. Een lijst waarin dat onderscheid ontbreekt is voor de bron onbruikbaar — dan weet hij niet
welke hij zelf nog moet natrekken.

## Wat er aan het eind naar de bron gaat

1. **Per bevinding één issue** op `DaveKJohn/davekjohns-workshop` met label `inbound` (er staat een template
   klaar).
2. **Één overzichtsissue** dat de ronde als geheel klaarzet. Vorige: v5 #287, v6 #298, v7 #306, v8 #316,
   v9 #326, v10 #340.
3. **De stappentabel**, naast die van v10 — dat is het enige dat laat zien of het pad *begaanbaarder* is
   geworden in plaats van alleen anders.

En daarna: de uitkomst vastleggen in het dossier in `life-hub`
(`Brains/plutchik-brain/RAW/feiten/dossiers/testronde-specialisten/README.md`), via branch + PR. Dat dossier
is de canonieke plek voor de historie van de reeks; deze map is de werkmap van deze ene ronde.

## Repareer niets tijdens de ronde

Niet in deze repo en niet in de twee documenten van de plugin. De ronde is een **meting**; repareren gebeurt
bij de bron, daarna, op grond van de issues. Dat is regel 4 van het protocol en hij is er omdat een repo die
tijdens de meting is bijgewerkt geen meting meer is.
