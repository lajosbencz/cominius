# 5. lépés - Parancsok

## Mit csinálunk?

Összekapcsoljuk a felhasználói felületet a teknőssel: a beviteli mezőbe írt szöveget parancsokká alakítjuk, és végrehajtjuk őket.

## Lépések

A `move` függvény után adjuk hozzá az alábbiakat:

```js
    const VOCABULARY = {
      FORWARD:  'f',
      BACKWARD: 'b',
      RIGHT:    'r',
      LET:      'l',
      PEN_UP:   'u',
      PEN_DOWN: 'd',
    };

    const ZERO_ARG = new Set([VOCABULARY.PEN_UP, VOCABULARY.PEN_DOWN]);

    function tokenize(src) {
      return src.trim().split(/\s+/).filter(Boolean);
    }

    function parse(tokens) {
      const cmds = [];
      let i = 0;
      while (i < tokens.length) {
        const tok = tokens[i].toLowerCase();
        if (ZERO_ARG.has(tok)) {
          cmds.push({ cmd: tok });
          i++;
        } else {
          cmds.push({ cmd: tok, val: tokens[i + 1] });
          i += 2;
        }
      }
      return cmds;
    }

    function exec(cmds) {
      for (const c of cmds) {
        switch (c.cmd) {
          case VOCABULARY.FORWARD:  move(+c.val); break;
          case VOCABULARY.BACKWARD: move(-c.val); break;
          case VOCABULARY.RIGHT:    turtleState.angle += +c.val; break;
          case VOCABULARY.LET:      turtleState.angle -= +c.val; break;
          case VOCABULARY.PEN_UP:   turtleState.penDown = false; break;
          case VOCABULARY.PEN_DOWN: turtleState.penDown = true; break;
        }
      }
    }

    function run() {
      const raw = document.getElementById('commandInput').value.trim();
      if (!raw) return;
      exec(parse(tokenize(raw)));
      render();
    }

    function clear() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      resetTurtle();
      render();
    }

    const inp = document.getElementById('commandInput');
    inp.addEventListener('keydown', e => { if (e.key === 'Enter') run(); });
    document.getElementById('runBtn').addEventListener('click', run);
    document.getElementById('clearBtn').addEventListener('click', clear);
```

Egészítsük ki a `load` eseményt is, hogy a beviteli mező azonnal fókuszban legyen:

```js
    window.addEventListener('load', () => {
      canvas.width = canvas.offsetWidth;
      canvas.height = canvas.offsetHeight;
      resetTurtle();
      render();
      inp.focus(); // <- új sor
    });
```

## Magyarázat

**`VOCABULARY`**

Szótár az egykarakteres parancsnevekhez. Ha egy parancs betűjét meg akarjuk változtatni, elég itt módosítani.

**`tokenize(src)`**

Szóközök mentén szavakra bontja a szöveget:
```
"f 100 r 90" → ["f", "100", "r", "90"]
```

**`parse(tokens)`**

A token-listát parancs-objektumok listájává alakítja. Két eset van:
- Argumentum nélküli parancs (`u`, `d`): csak `{ cmd }` objektum
- Egyargumentumos parancs (`f`, `b`, `r`, `l`): `{ cmd, val }` objektum

**`exec(cmds)`**

Végigmegy a parancsokon, és mindegyiket végrehajtja a `switch` segítségével. A `+c.val` a szöveget számmá alakítja.

## Próbáljuk ki!

Frissítsük a böngészőt, majd írjuk be a beviteli mezőbe:

```
f 100
```
```
f 100 r 90 f 100 r 90 f 100 r 90 f 100
```
```
u f 50 d f 100
```

Figyeljük meg: ha a Run gombot többször megnyomjuk, az előző rajz eltűnik. Ezt fogjuk megoldani a következő lépésben!
