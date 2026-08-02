# specialists-adoptietest

Wegwerp-consument voor de testrondes van de specialisten-plugin. Vers, leeg, geen `.claude/`.

## Draai je hier een testronde?

**De opdracht staat niet in deze branch, en dat is bewust.** Deze branch is de *fixture*: hij moet leeg zijn,
want stap 0 van elke ronde bewijst dat er niets is om correct over te zijn.

De opdracht, de richtlijnen en het resultaatformulier van de huidige ronde staan op branch **`tests/v13`**, in
de map `tests/v13/`. Begin bij `tests/v13/README.md`.

**Lees ze in de browser, of clone die branch in een EIGEN map.** Doe géén `git checkout tests/v13` hier: dan
staan de papieren in de repo die je aan het meten bent.

```
git clone -b tests/v13 https://github.com/DaveKJohn/specialists-adoptietest.git v13-opdracht
```

> Deze sectie is de enige inhoudelijke toevoeging aan de fixture, en ze is opzettelijk kort en naamloos: geen
> specialist-id, geen specialistennaam, geen contract-functie. De vrij-staande audit van de teardown scant
> `CLAUDE.md`, `.claude/**` en `scripts/**` — deze README valt daarbuiten en wordt alleen geteld als prose.
