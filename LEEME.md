# La Catedral — sitio web

Sitio estático. Sin dependencias, sin build. Se despliega tal cual.

```
index.html    la web pública
editor.html   el editor de la carta
carta.json    los datos: única fuente de verdad
logo.png
taza.png
vercel.json   cache y cabeceras
robots.txt
sitemap.xml
```

---

## Publicar por primera vez

**1. Crear el repositorio en GitHub**

Nuevo repositorio, nombre `la-catedral`, público o privado (da igual, Vercel
lee ambos). Subir los archivos arrastrándolos a la interfaz de GitHub, o por
terminal:

```bash
git init
git add .
git commit -m "Sitio de La Catedral"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/la-catedral.git
git push -u origin main
```

**2. Conectar Vercel**

1. Entrar en vercel.com con la cuenta de GitHub
2. **Add New → Project**
3. Elegir el repositorio `la-catedral`
4. Framework Preset: **Other**
5. Build Command: dejar **vacío**
6. Output Directory: dejar **vacío**
7. **Deploy**

En un minuto queda publicado. Vercel da una dirección tipo
`la-catedral.vercel.app`.

**3. Cambiar el nombre de la dirección**

En el panel del proyecto → **Settings → Domains**. Ahí se edita el subdominio
o se conecta un dominio propio si lo hay.

Después de fijar la dirección definitiva, hay que actualizarla en dos sitios:
`robots.txt` y `sitemap.xml`.

---

## Actualizar la carta

**El PDF nunca entra, sólo sale.** Lo que viaja es `carta.json`.

### Cambio de precios del día a día

1. Abrir la dirección `/editor` en el navegador
2. Cambiar lo que haga falta
3. **Descargar carta.json**
4. En GitHub: entrar en el archivo `carta.json` → icono del lápiz →
   borrar todo → pegar el contenido nuevo → **Commit changes**

Vercel republica solo en unos segundos. La web queda actualizada.

> Si se trabaja por terminal:
> ```bash
> git add carta.json && git commit -m "Precios actualizados" && git push
> ```

### Platos nuevos, rediseño, PDF para las fundas

1. Pasar `carta.json` a Claude
2. Hacer los cambios en conversación
3. Claude devuelve el `carta.json` nuevo y el PDF de 6 páginas
4. Subir el `carta.json` a GitHub igual que arriba

---

## Subir precios en bloque

En el editor, encima de las secciones. Se elige la sección (o toda la carta),
el porcentaje y a cuánto redondear. Un número negativo baja los precios.

---

## Imprimir

El botón «Imprimir / PDF» del editor saca una versión aproximada para
consulta rápida.

**Para las fundas del restaurante usa siempre el PDF que genera Claude**, que
respeta el formato exacto de 6 caras.

---

## Fotos

En `index.html`, sección «El lugar», hay tres huecos:

```html
<div class="foto vacia" data-lbl="Salón"></div>
```

Se sustituyen por:

```html
<div class="foto"><img src="salon.jpg" alt="Salón de La Catedral" loading="lazy"></div>
```

Las fotos van en la raíz del repositorio, junto a `logo.png`.

**Importante para conexiones lentas:** máximo 1200 px de ancho y por debajo de
200 KB cada una. Se pueden comprimir en squoosh.app antes de subirlas.

---

## Datos del local

Teléfono, WhatsApp, Instagram y horario se editan desde el editor, panel
«Identidad del local». Los campos vacíos no aparecen en la web.

Al cambiar el horario, hay que actualizarlo también en el bloque
`application/ld+json` de `index.html` para que Google lo muestre bien.

---

## El editor

Está en `/editor` y no aparece en Google ni enlazado desde la web. Cualquiera
con la dirección puede abrirlo, pero **no puede cambiar la web**: sólo descarga
un archivo a su ordenador. El cambio real sólo ocurre al subirlo a GitHub.

---

## Notas técnicas

- Sin frameworks, sin fuentes descargadas, sin peticiones a terceros
- Primera carga ≈ 26 KB con la compresión de Vercel
- La carta se guarda en el navegador: quien la vio una vez la vuelve a ver sin conexión
- `carta.json` se sirve sin cache, las imágenes con cache de un año
- Datos estructurados Schema.org para búsquedas locales de Google
- Responsive, foco visible por teclado, respeta `prefers-reduced-motion`
