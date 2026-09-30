# El formato de la casa

Todas las aplicaciones siguen el formato de **Las piezas de Miut**: la misma
disposición, las mismas hojas y los mismos botones. Cada una pone encima **su
propio fondo** (una textura bonita de su mundo) y **su propia letra de
rótulos**. Miut es la referencia viva: ante la duda, se mira
[su `index.html`](https://github.com/diazepamina-creator/miut-piezas/blob/main/index.html).
La palestra es el ejemplo de cómo se traslada a otra app sin copiar la madera.

> Mejor que peque de sencilla que de complicada. Miut es un éxito porque
> hace lo que tiene que hacer, y nada más.

## 1. El fondo: una textura propia, nunca la madera de Miut

- La textura se dibuja en CSS con SVG en línea: nada de imágenes externas.
  Se superponen cuatro capas sobre el `body`, con `background-attachment:fixed`:
  1. **La luz**: `radial-gradient(ellipse 90% 55% at 50% 0%, var(--luz), transparent 70%)`, que cae desde arriba.
  2. **El grano**: `feTurbulence` en un SVG en mosaico (220–600 px), muy suave.
  3. **El detalle del mundo**: la veta de la madera, los surcos del rastrillo, las fibras del papel, las juntas de la piedra…
  4. **El degradado de base**: `linear-gradient(--fondo1, --fondo2)`.
- `html{background:var(--fondo2)}`, para que no asome blanco al hacer scroll.
- Tiene que ser **del mundo de la app** y reconocible:
  - Miut: la mesa de madera, vista desde arriba.
  - La palestra: arena rastrillada.
  - Una pizzería: el mármol de la encimera o la harina.
  - Una oficina: el papel o el corcho.
  - Una corte medieval: la piedra.
- Se ve siempre **sutil**: el texto va sobre las hojas, nunca sobre la textura.
- **Temas**:
  - uno **claro**, por defecto, que es el que mejor se ve en el proyector;
  - uno **oscuro**, para aulas con poca luz (en Miut, haya y nogal);
  - si hace falta, **Proyector**: papel blanco, tinta negra y sin textura.

## 2. Los colores: los nombres de Miut

Todas usan estas variables en `:root`, y cada tema las redefine:

| Variable | Para qué | Miut, claro |
|---|---|---|
| `--noche` | fondo de las cajas dentro de una hoja | `#F4EBD9` |
| `--panel`, `--panel2` | las hojas (degradado de `panel2` a `panel`) | `#FFFBF2`, `#FBF3E3` |
| `--linea` | bordes y separadores | `#D8C3A0` |
| `--tinta`, `--tenue` | texto y texto secundario | `#2E2218`, `#7D6650` |
| `--oro` | el color de acento: títulos, pestaña activa, botón principal | `#A8661B` |
| `--morado`, `--rojo`, `--verde` | pasar el ratón, error, acierto | `#8A6A9E`, `#B0442C`, `#4F7A2E` |
| `--sombra`, `--brillo` | sombra de las hojas y fondo de lo activo | `rgba(60,35,10,.28)`, `rgba(214,150,50,.18)` |

Si una app ya tenía sus nombres, se dejan como **alias** de estos
(`--ink:var(--tinta)`), para no reescribir los módulos.

## 3. La letra

- **Rótulos**: títulos, pestañas, botones y etiquetas, en mayúsculas y con
  espaciado (`letter-spacing` de 1.2 a 2px). La letra es **temática y de cada
  app**:
  - Miut: Rokkitt;
  - La palestra: Cinzel;
  - otras: la que pegue con su mundo.
- **Texto para leer**: una sans clara, de 15px (Miut usa Atkinson
  Hyperlegible; La palestra, Alegreya Sans).
- **Números y cuentas**: Space Mono.
- Todo desde Google Fonts, y la app tiene que seguir funcionando sin conexión.

## 4. La disposición

```
.lienzo  (max-width:760px; margin:0 auto; padding:12px 12px 36px)
 ├─ header.cab       la cabecera: UNA hoja
 │   ├─ .rotulo      sello (26px, máscara SVG en --oro) · h1 · .sub · nav.acc
 │   └─ .juegos      los juegos o modos como pestañas de la misma hoja
 ├─ .panel.ajustes   los ajustes: una hoja que se abre bajo la cabecera
 ├─ .panel …         el encargo, la mesa, lo que diga el personaje…
 └─ footer
```

- **La cabecera** (`.cab`):
  - Un sello pequeño, el nombre en `--oro` (19px, mayúsculas) y un subtítulo diminuto en `--tenue`.
  - A la derecha, las acciones en **letra pequeña y sin cajas** (Guía, Practicar, Ajustes, Aula, Acta…). Se subrayan en `--oro` al pasar el ratón o cuando están activas.
  - En el móvil (≤620px) las acciones bajan a su propia regleta.
- **Las pestañas** (`.juegos`): una rejilla de 2 a 4 columnas separadas por `--linea`.
  - Cada pestaña lleva el nombre en `--oro` y una línea de explicación en `--tenue`, que desaparece a ≤480px.
  - La activa lleva el borde inferior de 3px en `--oro` y el fondo `--brillo`.
- **Las hojas** (`.panel`): `linear-gradient(180deg,var(--panel2),var(--panel))`, borde `--linea`, `border-radius:13px`, `padding:12px 13px`, `margin-top:9px` y `box-shadow:0 6px 18px var(--sombra)`.
- **Las cajas dentro de una hoja** (`.caja`): fondo `--noche`, borde `--linea` y `border-radius:12px`. El rótulo de cada caja, pequeño y en mayúsculas.

## 5. Los botones

```css
button.b{font-family:<rótulo>;font-weight:700;font-size:14px;letter-spacing:1.2px;text-transform:uppercase;
  padding:8px 14px;min-height:42px;border-radius:9px;border:1px solid var(--linea);cursor:pointer;
  background:linear-gradient(180deg,var(--panel2),var(--panel));color:var(--tinta);box-shadow:0 2px 0 rgba(0,0,0,.28)}
button.b:hover{border-color:var(--morado)}
button.b:active{transform:translateY(2px);box-shadow:none}
button.b.ok{background:linear-gradient(180deg,#F4CB72,#DDA23A);color:#2A1D06;border-color:#A8661B;box-shadow:0 2px 0 #7E4C12}
button.b:disabled{opacity:.4;cursor:default}
```

- **Un solo botón dorado** (`.ok`) por pantalla: el de seguir o comprobar.
- **Las opciones de los ajustes** (`.ops`): botones en fila dentro de una
  píldora (`border-radius:20px`). La elegida va rellena de `--oro`.
- **Los enlaces discretos** (`button.enlace`): sin caja, subrayados, en `--tenue`.
- **Tamaño**: todo lo que se toca mide al menos 42px de alto; se usa en pizarra
  y en móvil.

## 6. El personaje que habla

Es una caja (`.miut` en Miut) en rejilla `44px 1fr`:
- la cara a la izquierda;
- encima, el nombre en `--oro` y en mayúsculas;
- debajo, lo que dice, a 15.5px.

Poco texto, y nunca parpadea.

## 7. Las ventanas y la escena de inicio

- **Modal** (ayuda, acta, entrevistas):
  - El fondo, `rgba(12,8,20,.82)`.
  - La hoja, `.caja2`: `min(600px,100%)`, `border-radius:16px`, el mismo degradado que las hojas.
  - Los `h2` y `h3` van en `--oro` con la letra de rótulos.
- **Escena de inicio**:
  - Una animación corta con el objeto de la app y, al final, el nombre en grande en `--oro`.
  - Abajo, «toca para empezar».
  - Se salta tocando, se puede quitar en Ajustes y respeta `prefers-reduced-motion`.
  - Mientras dura, se ve el fondo solo.

## 8. El pie

Siempre el mismo, y en este orden:

```html
<footer>
  <a href="https://creativecommons.org/licenses/by-nc-nd/4.0/deed.es" rel="license">© 2026 Andrés Asensio · CC BY-NC-ND 4.0</a>
  · <a href="…">La app anterior (anterior)</a>   <!-- solo si la hay -->
  · <a href="https://diazepamina-creator.github.io/">Materiales</a>
  · versión X.Y.Z
</footer>
```

```css
footer{margin-top:26px;padding-top:12px;border-top:1px solid var(--linea);text-align:center;
  font-family:'Space Mono',monospace;font-size:10.5px;letter-spacing:1.2px;text-transform:uppercase;color:var(--tenue)}
footer a{color:var(--tenue)}
```

**Sin** enlace a GitHub.

## 9. La cabecera del HTML y el repositorio

- `<meta name="robots" content="noindex, nofollow, noarchive, noimageindex">`
  y `<meta name="author" content="Andrés Asensio">`.
- Un `LICENSE.md` con CC BY-NC-ND 4.0: el de Miut, con el título, la cita y las
  tipografías de la app.
- Un solo `index.html`, publicado con GitHub Pages.
- Una línea de `CLAUDE.md` que remita a esta guía.

## 10. Para pasar una app al formato

1. Se leen su `index.html` y su README, y se apunta **qué tiene que no se puede perder**: su mundo, sus personajes y lo que la hace buena en clase.
2. Se elige con la autora o el autor **la textura del fondo** y **la letra de rótulos**, y se enseñan capturas antes de decidir.
3. Se cambia **la piel**: variables, fondo, cabecera, hojas, botones, modal y pie. El funcionamiento no se toca.
4. Se comprueba:
   - en móvil (390px, sin scroll horizontal), en escritorio y en el modo aula o proyector;
   - en claro y en oscuro;
   - las pruebas, si las hay.
5. Se hace un PR por app, con capturas antes y después. Se fusiona cuando la autora o el autor dice «fusiona en main».
