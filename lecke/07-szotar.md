# 7. lépés – Saját szótár

## Mit csinálunk?

Megmutatjuk, hogyan lehet a parancsok betűit tetszőlegesen megváltoztatni a `VOCABULARY` szótár szerkesztésével.

## Lépések

A `VOCABULARY` objektum az `index.html`-ben jelenleg így néz ki:

```js
    const VOCABULARY = {
      FORWARD:  'f',
      BACKWARD: 'b',
      RIGHT:    'r',
      LET:      'l',
      PEN_UP:   'u',
      PEN_DOWN: 'd',
      REPEAT:   'x',
    };
```

Minden jobb oldali érték (az idézőjelek közötti betű) szabadon átírható. Például ha magyar betűket szeretnénk:

```js
    const VOCABULARY = {
      FORWARD:  'e',  // előre
      BACKWARD: 'h',  // hátra
      RIGHT:    'j',  // jobbra
      LET:      'b',  // balra
      PEN_UP:   'f',  // fel
      PEN_DOWN: 'l',  // le
      REPEAT:   'i',  // ismétlés
    };
```

A kód többi részét nem kell módosítani — a `VOCABULARY` konstansok mindenhol ugyanazok maradnak, csak az értékük változik.

## Magyarázat

**Miért nem kell mást módosítani?**

A `parse()` és az `exec()` soha nem hivatkozik közvetlenül a betűkre (pl. `'f'`), hanem mindig a `VOCABULARY.FORWARD` konstanson keresztül éri el az értéket. Ez azt jelenti, hogy ha a `VOCABULARY`-ban átírjuk az értéket, az automatikusan minden helyen érvényes lesz.

```js
// Rossz — kötött a betűhöz:
if (tok === 'f') move(+c.val);

// Helyes — a szótáron keresztül:
case VOCABULARY.FORWARD: move(+c.val); break;
```

Ez a tervezési minta neve: **egyetlen igazságforrás** (single source of truth).

**Mire figyeljünk?**

- Minden parancsnak különböző betűt kell adni — ha két parancs ugyanazt a betűt kapja, az egyik sosem fog lefutni.
- A `[` és `]` karakterek foglaltak (a blokkok jelölésére), ezeket ne adjuk ki parancsként.

## Próbáljuk ki!

Frissítsük a böngészőt az új szótárral, és próbáljuk ki a saját betűkkel:

```
e 100 j 90 e 100 j 90 e 100 j 90 e 100
```

Alkossuk meg a saját "nyelvünket", és rajzoljuk meg a kedvenc alakzatunkat!
