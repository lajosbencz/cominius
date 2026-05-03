# 2. lépés - CSS stílusok

## Mit csinálunk?

Az oldalt úgy formázzuk, hogy a rajzterület töltse ki az egész ablakot, és a vezérlősor alul legyen.

## Lépések

A `<style>` blokkot adjuk hozzá a `<head>` elem alá:

```html
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { display: flex; flex-direction: column; height: 100dvh; overflow: hidden; }
    canvas { flex: 1; width: 100%; }
    button { padding: 6px 12px; }
    #ui { display: flex; gap: 8px; padding: 10px; }
    #commandInput { flex: 1; padding: 6px; }
  </style>
```

## Magyarázat

| Szabály | Mit csinál |
|---|---|
| `* { margin: 0; padding: 0; }` | Minden elem belső és külső margóját nullára állítja - tiszta alapállapot |
| `body { display: flex; flex-direction: column; }` | Az oldal elemei egymás alá rendeződnek |
| `height: 100dvh` | Az oldal pontosan az ablak magasságát tölti ki (mobilon is) |
| `canvas { flex: 1; }` | A rajzterület elfoglalja az összes maradék helyet |
| `#ui { display: flex; gap: 8px; }` | A vezérlősor elemei egymás mellett állnak, kis réssel |

## Próbáljuk ki!

Frissítsük a böngészőt. Most a `canvas` kitölti az ablak nagy részét, és alul megjelenik a beviteli sor.
