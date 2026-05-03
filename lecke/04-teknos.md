# 4. lépés - Teknős

## Mit csinálunk?

Lecseréljük a próbarajzokat egy teknősre: bevezetjük az állapotát (pozíció, irány, toll),
megírjuk a megjelenítőt és a mozgatót, majd feliratkozunk az ablak eseményeire.

## Lépések

Cseréljük le a `<script>` tartalmát az alábbiakra:

```js
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');

    let turtleState = {};

    function resetTurtle() {
      turtleState = {
        x: canvas.width / 2,
        y: canvas.height / 2,
        angle: -90,
        penDown: true,
      };
    }

    function render() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      const size = 24;
      ctx.save();
      ctx.translate(turtleState.x, turtleState.y);
      ctx.rotate((turtleState.angle + 90) * Math.PI / 180);
      ctx.beginPath();
      ctx.moveTo(0, -size);
      ctx.lineTo(size * 0.55, size * 0.7);
      ctx.lineTo(0, size * 0.3);
      ctx.lineTo(-size * 0.55, size * 0.7);
      ctx.closePath();
      ctx.fill();
      ctx.restore();
    }

    function move(dist) {
      const rad = turtleState.angle * Math.PI / 180;
      const nx = turtleState.x + dist * Math.cos(rad);
      const ny = turtleState.y + dist * Math.sin(rad);
      if (turtleState.penDown) {
        ctx.beginPath();
        ctx.moveTo(turtleState.x, turtleState.y);
        ctx.lineTo(nx, ny);
        ctx.stroke();
      }
      turtleState.x = nx;
      turtleState.y = ny;
    }

    window.addEventListener('load', () => {
      canvas.width = canvas.offsetWidth;
      canvas.height = canvas.offsetHeight;
      resetTurtle();
      render();
    });

    window.addEventListener('resize', () => {
      canvas.width = canvas.offsetWidth;
      canvas.height = canvas.offsetHeight;
      resetTurtle();
      render();
    });
```

## Magyarázat

**`turtleState`**

| Mező | Jelentése |
|---|---|
| `x`, `y` | A teknős pozíciója pixelben |
| `angle` | Merre néz, fokban (`-90` = felfelé, mert a canvas `0` foka jobbra mutat) |
| `penDown` | Ha `true`, rajzol menet közben |

**`render()`**

1. `clearRect` — törli a teljes vásznat
2. `ctx.save() / ctx.restore()` — elmenti és visszaállítja a rajzolási beállításokat, hogy a forgatás ne hasson más elemekre
3. `translate + rotate` — a vászon koordináta-rendszerét a teknős pozíciójába és irányába fordítja,így a háromszöget mindig `(0,0)` körül rajzoljuk

**`move(dist)`**

Trigonometriával számítja ki az új pozíciót:
- `cos(szög) × távolság` → vízszintes elmozdulás
- `sin(szög) × távolság` → függőleges elmozdulás

Ha `penDown` igaz, vonalat húz a régi és az új pont között.

## Próbáljuk ki!

Frissítsük a böngészőt — a teknősnek meg kell jelennie a képernyő közepén.

A böngésző konzolján (F12) próbáljuk meg manuálisan meghívni:
```js
move(100); render();
```
