# 1. lépés - HTML alap

## Mit csinálunk?

Létrehozzuk az oldal vázát: egy rajzfelületet (`canvas`) és egy egyszerű vezérlősort (szövegbevitel + gombok).

## Lépések

Hozzuk létre az `index.html` fájlt, és írjuk bele az alábbiakat:

```html
<!doctype html>
<html>

<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Cominius</title>
</head>

<body>

  <canvas id="canvas"></canvas>

  <div id="ui">
    <input id="commandInput" type="text" autocomplete="off" spellcheck="false">
    <button id="runBtn">Run</button>
    <button id="clearBtn">Clear</button>
  </div>

</body>

</html>
```

## Magyarázat

| Elem | Szerepe |
|---|---|
| `<!doctype html>` | Nem egy igazi tag, hanem egy deklaráció: megmondja a böngészőnek, hogy HTML5 dokumentumot olvas |
| `<html>` | Az egész oldal gyökéreleme - minden más tag ezen belül van |
| `<head>` | Az oldal „fejléce": nem jelenik meg a képernyőn, de fontos beállításokat tartalmaz (cím, stílusok, karakterkészlet stb.) |
| `<meta name="viewport" ...>` | Mobilos beállítás: megmondja a telefonnak, hogy ne kicsinyítse be az oldalt, hanem pontosan akkora legyen, mint a képernyő |
| `<title>` | Az oldal neve - ez jelenik meg a böngésző fülén |
| `<body>` | Az oldal „törzse": amit ide írunk, az látható a képernyőn |
| `<canvas>` | Egy üres rajzterület - JavaScript segítségével lehet rá rajzolni |
| `<div>` | Egy általános doboz, amely más elemeket fog össze; önmagában láthatatlan, de segít az elrendezésben |
| `<input>` | Szövegbeviteli mező - ide írjuk majd a parancsokat |
| `<button>` | Kattintható gomb - a `Run` futtatja, a `Clear` törli a rajzot |

## Próbáljuk ki!

Nyissuk meg a fájlt a böngészőben. Egyelőre csak egy üres oldalt látunk néhány elemmel — ez teljesen normális.

