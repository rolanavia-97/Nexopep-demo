# Nexopep — Guía de puesta en marcha

Esta guía te explica, paso a paso, cómo publicar tu página y cómo subir tú mismo los Certificados de Análisis (COA) sin tocar código.

---

## 1. Qué archivos tienes

- `index.html` → la página principal (catálogo, información, botón de WhatsApp).
- `coa.html` → la página donde el cliente ve el certificado al ingresar su número de lote.
- `config.js` → el único archivo que necesitas editar (número de WhatsApp y link de tus certificados).
- `images/` → carpeta con las fotos de los viales.

Estos archivos deben subirse juntos, en la misma carpeta, a tu hosting (o a un servicio gratuito como GitHub Pages, Netlify o Vercel, arrastrando la carpeta completa).

---

## 2. Crear tu "base de datos" de certificados (Google Sheets)

No necesitas programar ni una base de datos real. Vas a usar una Hoja de Google como si fuera tu archivo de certificados.

1. Entra a [sheets.google.com](https://sheets.google.com) y crea una hoja nueva.
2. En la primera fila, escribe exactamente estos encabezados (uno por columna):

   | lote | peptido | mg | fecha | imagen_url |
   |------|---------|----|-------|------------|

3. Por cada vial que quieras registrar, agrega una fila. Ejemplo:

   | lote | peptido | mg | fecha | imagen_url |
   |------|---------|----|-------|------------|
   | NXP-0824-A | Retatrutida | 10 MG | 2026-08-20 | https://... |

4. **Sube la imagen del COA a Google Drive** (o Imgur), haz clic derecho → "Compartir" → "Cualquier persona con el enlace". Copia el enlace y conviértelo a un enlace de imagen directa:
   - Si es un link de Drive como `https://drive.google.com/file/d/ABC123/view`, cámbialo a:
     `https://drive.google.com/uc?export=view&id=ABC123`
   - Si usas Imgur, sube la imagen y copia el link que termina en `.jpg` o `.png` directamente.
5. Pega ese link en la columna `imagen_url` de la fila correspondiente.

### Publicar la hoja como CSV (una sola vez)
1. En Google Sheets: **Archivo → Compartir → Publicar en la Web**.
2. Elige la hoja y selecciona el formato **CSV**.
3. Haz clic en "Publicar" y copia el link que te da.
4. Pega ese link en `config.js`, en `sheetCsvUrl`.

Desde este momento, **cada vez que agregues una fila nueva en la Hoja, aparecerá automáticamente disponible para consulta por número de lote** — no necesitas volver a tocar el código.

---

## 3. Configurar `config.js`

Abre `config.js` con cualquier editor de texto (o directamente en GitHub) y completa:

```js
const NEXOPEP_CONFIG = {
  whatsapp: "59168237696",
  sheetCsvUrl: "PEGA_AQUI_TU_LINK_CSV_PUBLICADO",
  siteUrl: "https://tu-dominio.com"
};
```

Guarda y vuelve a subir el archivo a tu hosting.

---

## 4. Escribir el número de lote en cada vial

Como el sistema funciona únicamente por número de lote (sin código QR), asegúrate de que cada vial que despaches tenga su código de lote visible en la etiqueta o en una pegatina adicional — el mismo código que registraste en la columna `lote` de tu Hoja de Google.

El cliente entra a la sección "Verificar COA" de tu página, escribe ese código y ve directamente el péptido, la concentración, la fecha del análisis y la imagen del certificado.

---

## 5. Flujo resumido para cada lote nuevo

1. Recibes el COA del laboratorio (imagen o PDF convertido a imagen).
2. Lo subes a Google Drive y obtienes el link directo.
3. Agregas una fila en la Hoja de Google con el lote, péptido, mg, fecha y el link de la imagen.
4. Escribes ese mismo número de lote en la etiqueta del vial correspondiente.

Listo — no necesitas volver a editar ningún archivo del sitio para futuros lotes, solo la Hoja de Google.

---

## 6. Fotos de los viales en el catálogo

Ya están incluidas las fotos reales de **GHK-Cu**, **BPC-157** y **Melanotan II** en la carpeta `images/`. Para **Retatrutida (10mg y 20mg)** y **MOTS-C**, el catálogo usa por ahora una ilustración con el mismo formato de etiqueta, mientras generas o fotografías esos viales.

Cuando tengas esas fotos:
1. Guárdalas en la carpeta `images/` con estos nombres exactos: `retatrutida-10mg.png`, `retatrutida-20mg.png`, `mots-c.png`.
2. Abre `index.html`, busca el bloque `const products = [...]` y agrega la línea `image: "images/retatrutida-10mg.png",` (por ejemplo) dentro del producto correspondiente, igual a como está hecho con GHK-Cu, BPC-157 y Melanotan II.

---

## Notas importantes

- La página **no vende ni cobra en línea**: todo botón de contacto redirige a WhatsApp.
- Si un cliente ingresa un número de lote que aún no registraste en la Hoja, verá un mensaje de "no encontrado" con la opción de escribirte por WhatsApp — no un error confuso.
- Puedes editar los textos, precios y descripciones del catálogo directamente en `index.html`, dentro del bloque `const products = [...]`.
