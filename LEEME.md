# La Catedral — sitio web

Cinco archivos. Se suben tal cual a cualquier hosting.

```
index.html    la web pública
editor.html   el editor de la carta (no enlazado desde la web)
carta.json    los datos: es la única fuente de verdad
logo.png
taza.png
```

---

## Cómo se actualiza la carta

El PDF nunca entra, sólo sale. Lo que viaja de un sitio a otro es `carta.json`.

**Cambio de precios del día a día**
1. Abrir `editor.html`
2. Cambiar lo que haga falta
3. «Descargar carta.json»
4. Subir ese archivo al hosting, encima del anterior

La web queda actualizada. No hace falta tocar nada más.

**Platos nuevos, rediseño, análisis de costo, PDF para las fundas**
1. Pasar `carta.json` a Claude
2. Hacer los cambios en conversación
3. Claude devuelve el `carta.json` nuevo y el PDF de 6 páginas
4. Subir el `carta.json` al hosting

---

## Subir precios en bloque

En el editor, arriba de las secciones: eliges la sección (o toda la carta),
el porcentaje y a cuánto redondear. Un número negativo baja los precios.

---

## Imprimir desde el editor

El botón «Imprimir / PDF» saca una versión aproximada para consulta rápida.
**Para las fundas del restaurante usa siempre el PDF que genera Claude**, que es
el que respeta el formato exacto de 6 caras.

---

## Fotos

En `index.html`, en la sección «El lugar», hay tres huecos:

```html
<div class="foto vacia" data-lbl="Salón"></div>
```

Se sustituyen por:

```html
<div class="foto"><img src="salon.jpg" alt="Salón de La Catedral" loading="lazy"></div>
```

Guarda las fotos a **1200 px de ancho como máximo** y por debajo de 200 KB
cada una, o la web dejará de cargar rápido donde la conexión es mala.

---

## Datos del local

Teléfono, WhatsApp, Instagram y horario se editan desde `editor.html`,
en el panel «Identidad del local». Los campos vacíos no se muestran en la web.

---

## Notas técnicas

- Sin frameworks, sin fuentes descargadas, sin peticiones a terceros
- Primera carga ≈ 26 KB con gzip
- La carta queda guardada en el navegador: quien ya la vio una vez la
  vuelve a ver aunque se quede sin conexión
- Funciona con el móvil en vertical y respeta `prefers-reduced-motion`

Para el hosting: activa gzip o brotli y cachea `logo.png` y `taza.png` con
`Cache-Control: max-age=31536000`, pero `carta.json` con `no-cache` para que
los cambios de precio se vean al momento.
