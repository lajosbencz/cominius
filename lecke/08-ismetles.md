# Extra - Ismétlés

## Mit csinálunk?

Bevezetjük az `x N [ ... ]` ismétlő parancsot, amely egy blokkot N-szer hajt végre.
Ehhez a jelenlegi `parse()` függvényt rekurzívra cseréljük.

## Lépések

**1.** Adjuk hozzá az `x` kulcsszót a `VOCABULARY`-hoz:

```js
    const VOCABULARY = {
      FORWARD:  'f',
      BACKWARD: 'b',
      RIGHT:    'r',
      LET:      'l',
      PEN_UP:   'u',
      PEN_DOWN: 'd',
      REPEAT:   'x', // <- új
    };
```

**2.** Cseréljük le a `parse()` függvényt a rekurzív változatra:

```js
    function parse(tokens, i = 0) {
      const cmds = [];
      while (i < tokens.length) {
        const tok = tokens[i].toLowerCase();
        if (tok === ']') break;
        if (tok === VOCABULARY.REPEAT) {
          const n = parseInt(tokens[i + 1]);
          i += 2;
          if (tokens[i] === '[') i++;
          const [body, j] = parse(tokens, i);
          i = j;
          if (tokens[i] === ']') i++;
          cmds.push({ cmd: VOCABULARY.REPEAT, n, body });
        } else if (ZERO_ARG.has(tok)) {
          cmds.push({ cmd: tok });
          i++;
        } else {
          cmds.push({ cmd: tok, val: tokens[i + 1] });
          i += 2;
        }
      }
      return [cmds, i];
    }
```

Módosítsuk a `run()` hívását is, mivel `parse()` most egy tömbpárt ad vissza:

```js
    function run() {
      const raw = document.getElementById('commandInput').value.trim();
      if (!raw) return;
      const [cmds] = parse(tokenize(raw)); // <- destrukturálás
      exec(cmds);
      render();
    }
```

**3.** Egészítsük ki az `exec()` `switch`-et az ismétlés esetével:

```js
          case VOCABULARY.REPEAT:
            for (let i = 0; i < c.n; i++) exec(c.body);
            break;
```

## Magyarázat

**Rekurzió**

Az egyszerű parancsok (pl. `f 100`) lineárisan olvashatók.
Az ismétlés azonban egy blokkot tartalmaz (`[ ... ]`), amelyen belül újabb ismétlés is lehet.
Ezt rekurzióval kezeljük: a `parse()` önmagát hívja meg a `[` után, és megáll, ha `]`-t talál.

```
x 3 [ f 50 x 2 [ r 90 ] ]
         ↑                ↑
    belső parse()   itt áll meg
```

**Miért ad vissza indexpárt a `parse()`?**

A belső `parse()` hívásnak tudnia kell, hol állt meg (melyik tokennél), hogy a külső hívás onnan folytassa az olvasást.
Ezért `[cmds, i]` párt ad vissza.

## Próbáljuk ki!

Frissítsük a böngészőt, majd próbáljuk ki:

```
x 4 [ f 100 r 90 ]
```

```
x 6 [ x 4 [ f 60 r 90 ] r 60 ]
```

```
u f 50 r 90 f 50 r 90 d x 4 [ f 100 r 90 ]
```
