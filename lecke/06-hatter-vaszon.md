# 6. lépés - Háttér vászon

## Mit csinálunk?

A `render()` minden híváskor törli a vásznat, így az előző rajzok eltűnnek.
Ezt egy rejtett háttér-vászonnal fogjuk megoldani, amelyre csak a vonalakat rajzoljuk, és a `render()` nem törli.

## Lépések

**1.** A `canvas` és `ctx` mellé adjunk hozzá egy második, rejtett vásznat:

```js
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    const canvasOffscreen = document.createElement('canvas'); // <- új
    const ctxOffscreen = canvasOffscreen.getContext('2d');    // <- új
```

**2.** A `move()` függvényben cseréljük le `ctx`-et `ctxOffscreen`-re, hogy a vonalak a háttérre kerüljenek:

```js
    function move(dist) {
      const rad = turtleState.angle * Math.PI / 180;
      const nx = turtleState.x + dist * Math.cos(rad);
      const ny = turtleState.y + dist * Math.sin(rad);
      if (turtleState.penDown) {
        ctxOffscreen.beginPath();           // <- ctxOffscreen
        ctxOffscreen.moveTo(turtleState.x, turtleState.y);
        ctxOffscreen.lineTo(nx, ny);
        ctxOffscreen.stroke();
      }
      turtleState.x = nx;
      turtleState.y = ny;
    }
```

**3.** A `render()` függvényben rajzoljuk rá a hátteret a látható vászonra:

```js
    function render() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.drawImage(canvasOffscreen, 0, 0); // <- új sor
      const size = 24;
      // ... (többi kód változatlan)
```

**4.** A méretváltozás kezeléséhez adjunk hozzá egy `setSize()` függvényt, amely megőrzi a háttér tartalmát:

```js
    function setSize() {
      const img = ctxOffscreen.getImageData(0, 0, canvasOffscreen.width, canvasOffscreen.height);
      canvas.width = canvasOffscreen.width = canvas.offsetWidth;
      canvas.height = canvasOffscreen.height = canvas.offsetHeight;
      ctxOffscreen.putImageData(img, 0, 0);
    }
```

**5.** A `clear()` függvényben töröljük a hátteret is:

```js
    function clear() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctxOffscreen.clearRect(0, 0, canvasOffscreen.width, canvasOffscreen.height); // <- új
      resetTurtle();
      render();
    }
```

**6.** A `load` és `resize` eseményekben használjuk a `setSize()`-t, és állítsuk be a háttér méretét is:

```js
    window.addEventListener('load', () => {
      canvas.width = canvasOffscreen.width = canvas.offsetWidth;   // <- canvasOffscreen
      canvas.height = canvasOffscreen.height = canvas.offsetHeight; // <- canvasOffscreen
      resetTurtle();
      render();
      inp.focus();
    });

    window.addEventListener('resize', () => { setSize(); render(); });
```

## Magyarázat

**Miért két vászon?**

A látható `canvas` minden `render()` hívásnál törlődik, hogy a teknős újrarajzolható legyen a megfelelő helyen.
Ha a vonalakat ugyanide rajzolnánk, azok is törlődnének.

A megoldás: a vonalak egy rejtett (`canvasOffscreen`) vászonra kerülnek, amelynek tartalma megmarad.
A `render()` először rámásolja ezt a hátteret a látható vászonra (`drawImage`), majd rárajzolja a teknőst.

**`getImageData` / `putImageData`**

Ablakátméretezéskor a `canvas` automatikusan törlődik.
A `setSize()` ezért az átméretezés előtt lementi a háttér tartalmát, és visszaállítja utána.

## Próbáljuk ki!

Frissítsük a böngészőt, és futtassuk ugyanazt a parancsot többször egymás után — a korábbi rajzok mostantól megmaradnak!

```
f 100 r 90
```

A Clear gomb továbbra is mindent töröl.
