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

1. Abrir `/editor`
2. Cambiar lo que haga falta — en cuanto tocas algo aparece abajo una barra
   roja que avisa de los cambios pendientes
3. Pulsar **Guardar cambios**. Se descarga `carta.json` y salen en pantalla
   los tres pasos siguientes
4. En GitHub: abrir `carta.json` → lápiz → borrar todo → pegar el contenido
   nuevo → **Commit changes**

Vercel republica solo en unos segundos.

> Si cierras el navegador sin guardar, el editor conserva el borrador y te
> pregunta si quieres recuperarlo la próxima vez.

> **Descartar** vuelve a la última carta publicada, sin los cambios.

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

Las fotos se manejan en dos pasos: **subir el archivo** a GitHub y **decir su
nombre** en el editor.

### 1. Subir la foto

En GitHub, en la carpeta raíz del repositorio (donde está `logo.png`):
**Add file → Upload files** y arrastrar las fotos.

**Antes de subirlas**, redúcelas: máximo **1200 px de ancho** y por debajo de
**200 KB** cada una. Se puede hacer gratis en squoosh.app. Una foto de móvil
sin comprimir pesa 4 MB y tumba la carga en conexiones lentas.

### 2. Decir su nombre en el editor

Abrir `/editor` → panel **Imágenes**:

| Campo | Para qué sirve |
|---|---|
| Foto de portada | Fondo del encabezado, detrás del logo |
| Galería · Archivo | Las tres fotos de «El lugar» |
| Galería · Texto alternativo | Lo que se lee si la foto no carga |

Se escribe sólo el nombre del archivo: `salon.jpg`, no la ruta completa.
Descargar `carta.json` y subirlo, como cualquier otro cambio.

Un campo en blanco deja el hueco vacío sin romper nada.

### Sobre la foto de portada

La web la pasa a monocromo sobre el color piedra y la funde por arriba y por
abajo. Así el logo siempre se lee y cualquier foto entra en la paleta de la
marca, aunque tenga colores que choquen.

Funciona mejor una foto **horizontal y con profundidad**: el salón entero, la
terraza a lo largo, la barra en perspectiva. Un primer plano de un plato queda
raro de fondo.

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

---

## Sobre el cache de imágenes

Si cambias una foto **y mantienes el mismo nombre de archivo**, puede tardar
hasta un día en verse: los navegadores guardan las imágenes para no
descargarlas cada vez.

Para que el cambio se vea al momento, **súbela con otro nombre**
(`terraza-2.webp`) y actualiza el nombre en el editor. Es la forma segura.
