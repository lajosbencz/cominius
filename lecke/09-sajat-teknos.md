# Extra - Saját teknős

## Mit csinálunk?

Megváltoztatjuk a teknős kinézetét: vagy átrajzoljuk kódból, vagy egy saját képfájlt töltünk be.

## Lépések

A teknős megjelenítése a `render()` függvényben, a `ctx.save()` és `ctx.restore()` közötti részen történik.
Ez a blokk most egy háromszöget rajzol:

```js
      ctx.beginPath();
      ctx.moveTo(0, -size);
      ctx.lineTo(size * 0.55, size * 0.7);
      ctx.lineTo(0, size * 0.3);
      ctx.lineTo(-size * 0.55, size * 0.7);
      ctx.closePath();
      ctx.fill();
```

### A. lehetőség - Más alakzat kódból

Cseréljük ki a `beginPath()`-tól a `fill()`-ig terjedő részt bármilyen más rajzra. Például kör:

```js
      ctx.beginPath();
      ctx.arc(0, 0, size / 2, 0, Math.PI * 2);
      ctx.fill();
```

A `translate` és `rotate` már el van végezve,
ezért a `(0, 0)` pont mindig a teknős aktuális pozíciója,
a rajz pedig az aktuális irányba néz.

### B. lehetőség - Képfájl betöltése

Helyezzünk egy képfájlt (pl. [turtle.png](turtle.png)) ugyanabba a mappába, ahol az `index.html` van.

A képet a `<script>` elején töltsük be:

```js
    const turtleImage = new Image();
    turtleImage.src = 'turtle.png';
```

A `render()` `save()`/`restore()` blokkjában rajzoljuk ki a kép helyett:

```js
      ctx.drawImage(turtleImage, -size / 2, -size / 2, size, size);
```

> **Fontos:** Az `index.html` fájlt közvetlenül a böngészőben nyitjuk meg (`file://` protokollon).
A képfájlnak pontosan ugyanabban a mappában kell lennie, mint az `index.html`-nek.

## Magyarázat

**`ctx.save()` / `ctx.restore()`**

A `translate` és `rotate` módosítja a vászon koordináta-rendszerét.
A `save()` elmenti az eredeti állapotot, a `restore()` visszaállítja — így a többi rajzelem nem csúszik el.

**`ctx.drawImage(img, x, y, w, h)`**

Kirajzolja a képet a megadott pozícióba és méretbe. Mivel a koordináta-rendszer már a teknős köré van igazítva, `(-size/2, -size/2)` a kép közepét a teknős pozíciójára helyezi.

## Próbáljuk ki!

Frissítsük a böngészőt — a teknős most az új alakzattal vagy képpel jelenik meg.

Próbáljunk különböző méreteket a `size` változóban, és figyeljük meg a változást!
