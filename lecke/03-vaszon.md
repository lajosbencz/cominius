# 3. lépés - Vászon

## Mit csinálunk?

Megismerkedünk a `canvas` rajzolási lehetőségeivel: hogyan érjük el JavaScriptből, és hogyan rajzolunk rá vonalat és kört.

## Lépések

A `</body>` elem záró elé adjuk hozzá a `<script>` blokkot:

```html
  <script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');

    window.addEventListener('load', () => {
      canvas.width = canvas.offsetWidth;
      canvas.height = canvas.offsetHeight;

      // vonal
      ctx.beginPath();
      ctx.moveTo(100, 100);
      ctx.lineTo(300, 200);
      ctx.stroke();

      // kör
      ctx.beginPath();
      ctx.arc(200, 300, 50, 0, Math.PI * 2);
      ctx.stroke();
    });
  </script>
```

## Magyarázat

**Koordináta-rendszer**

A canvas bal felső sarka a `(0, 0)` pont. Az `x` jobbra, az `y` lefelé nő.

```
(0,0) ──────────► x
  │
  │
  ▼
  y
```

**`canvas.getContext('2d')`**

Visszaad egy rajzoló objektumot (`ctx`), amelynek metódusait meghívva a vászonra tudunk rajzolni.

**`canvas.width / height`**

A `load` eseményen belül állítjuk be, mert csak akkor ismert a canvas tényleges mérete a képernyőn.

| Metódus | Mit csinál |
|---|---|
| `ctx.beginPath()` | Új rajzot kezd |
| `ctx.moveTo(x, y)` | A toll felemeléssel a megadott pontra ugrik |
| `ctx.lineTo(x, y)` | Vonalat húz a jelenlegi pozícióból a megadott pontig |
| `ctx.arc(x, y, r, start, end)` | Kört (vagy ívét) rajzol `(x, y)` középponttal, `r` sugárral |
| `ctx.stroke()` | A megrajzolt útvonalat ténylegesen kirajzolja |

## Próbáljuk ki!

Nyissuk meg (vagy frissítsük) az `index.html` fájlt a böngészőben — a vásznon meg kell jelennie egy vonalnak és egy körnek.

Változtassuk meg a koordinátákat, és figyeljük meg, hogyan mozognak az alakzatok!
