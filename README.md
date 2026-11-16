# Gastos B&A

Web para registrar los gastos de Bastien y Antonia y saber, al final de cada mes, quién le debe a quién.

- **Página web** (este repositorio, publicado con GitHub Pages): lo que se usa desde el iPhone.
- **Google Apps Script** (`apps-script/Code.gs`): recibe los gastos, revisa el PIN y calcula el reporte.
- **Google Sheet**: donde quedan guardados todos los datos (privado).

> En este repositorio no hay datos ni el PIN. Solo código.

---

## Instalación (una sola vez, 20 a 30 minutos)

Hazlo desde un computador. Son 4 partes.

### Parte 1: El Google Sheet y el script

1. Entra a [sheets.google.com](https://sheets.google.com) y crea una hoja en blanco. Ponle de nombre **Gastos B&A**.
2. En el menú, ve a **Extensiones > Apps Script**. Se abre una pestaña nueva con un editor de código.
3. Borra todo lo que aparece en el editor (`function myFunction() {...}`) y pega el contenido completo del archivo `apps-script/Code.gs`.
4. Haz clic en el ícono de **Guardar** (disquete). Arriba a la izquierda, cambia el nombre del proyecto a **Gastos B&A** si quieres.
5. Vuelve a la pestaña del Sheet y **recarga la página**. Después de unos segundos aparece un menú nuevo llamado **Gastos**.
6. Haz clic en **Gastos > 1) Preparar hojas**.
   - Google te pedirá autorización. Elige tu cuenta.
   - Saldrá un aviso de "Google no ha verificado esta app". Es normal, porque la app la creaste tú. Haz clic en **Configuración avanzada** y luego en **Ir a Gastos B&A (no seguro)**, y después en **Permitir**.
   - Vuelve a hacer clic en **Gastos > 1) Preparar hojas** si no pasó nada. Aparecerán las pestañas **Gastos** y **Config**.
7. Haz clic en **Gastos > 2) Configurar o cambiar PIN** y escribe el PIN de 6 dígitos que van a usar.

### Parte 2: Publicar el script (para que la web pueda hablarle)

1. En la pestaña de Apps Script, arriba a la derecha: **Implementar > Nueva implementación**.
2. Al lado de "Seleccionar tipo", haz clic en el engranaje y elige **Aplicación web**.
3. Completa:
   - Descripción: `v1`
   - Ejecutar como: **Yo** (tu correo)
   - Quién tiene acceso: **Cualquier usuario**
4. Haz clic en **Implementar** (si pide autorizar de nuevo, repite lo del paso 6 anterior).
5. Copia la **URL de la aplicación web** (termina en `/exec`). La necesitas en la Parte 3.

> "Cualquier usuario" significa que cualquiera puede *llegar* al script, pero sin el PIN no puede leer ni escribir nada. El script responde "PIN incorrecto" y bloquea por 15 minutos tras 5 intentos fallidos.

### Parte 3: Publicar la página en GitHub

1. Entra a [github.com](https://github.com) con tu cuenta. Arriba a la derecha: **+ > New repository**.
2. Repository name: `gastos`. Marca **Public**. No marques nada más. Haz clic en **Create repository**.
3. En la página que aparece, haz clic en el link **uploading an existing file**.
4. Descomprime el zip. Abre la carpeta `gastos` y arrastra **su contenido** (no la carpeta misma): todos los archivos más la carpeta `apps-script`. Abajo, haz clic en **Commit changes**.
5. En la lista de archivos del repositorio, abre `config.js` y haz clic en el **lápiz** (Edit). Pega la URL de la Parte 2 entre las comillas:
   ```js
   window.API_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
   ```
   Haz clic en **Commit changes**.
6. Ve a **Settings** (arriba en el repositorio) > **Pages** (menú izquierdo).
   - Source: **Deploy from a branch**
   - Branch: **main** y carpeta **/ (root)**. Haz clic en **Save**.
7. Espera 1 o 2 minutos y recarga. Arriba aparecerá tu link, del tipo `https://TU-USUARIO.github.io/gastos/`.

### Parte 4: En el iPhone (cada uno en el suyo)

1. Abre el link en **Safari**.
2. Toca el botón **Compartir** (cuadrado con flecha) > **Agregar a inicio** > **Agregar**.
3. Abre la app desde el ícono nuevo e ingresa el PIN. Queda recordado en ese teléfono.

Opcional: comparte el Google Sheet con Antonia (botón **Compartir** del Sheet, como **Editor**) para que también pueda ver o corregir datos.

---

## Uso del día a día

| Quiero... | Cómo |
|---|---|
| Registrar un gasto | App > **Nuevo gasto**. Antes de guardar, la app te dice qué deuda genera. |
| Ver quién debe a quién | App > **Reporte**, elige el mes con las flechas. **Copiar resumen** lo deja listo para WhatsApp. |
| Corregir o borrar un gasto | En el Sheet, pestaña **Gastos**: edita o elimina la fila. |
| Cambiar el reparto (ej. a 60/40) | Sheet > pestaña **Config** > celda **B2** (% Bastien). Afecta solo a gastos nuevos; los anteriores guardan su %. |
| Agregar una categoría | Sheet > **Config** > escríbela al final de la columna D. Aparece al volver a abrir la app. |
| Cambiar el PIN | Sheet > menú **Gastos > 2) Configurar o cambiar PIN**. |
| Quitar un bloqueo | Sheet > menú **Gastos > Desbloquear acceso** (o esperar 15 minutos). |

**Reglas de cálculo**

- Cada gasto cuenta en el mes calendario de su fecha.
- Las compras en cuotas se reparten en meses consecutivos desde el mes de compra (si la división no es exacta, la diferencia va en la primera cuota).
- Gasto propio pagado por uno mismo: no genera deuda.
- Gasto de uno pagado por el otro: se debe el 100%.
- Gasto común: quien no pagó debe su % (según el % guardado en ese gasto).

**No cambies los títulos de las columnas** de la pestaña Gastos (la fila 1): el script los usa para encontrar cada dato.

---

## Si algo cambia en el código

- **Cambios en la página** (`index.html`, etc.): sube el archivo nuevo a GitHub (Add file > Upload files). En 1 o 2 minutos se actualiza. En el iPhone, cierra y vuelve a abrir la app.
- **Cambios en `Code.gs`**: pega el código nuevo en Apps Script, guarda, y luego **Implementar > Gestionar implementaciones > lápiz > Versión: Nueva versión > Implementar**. Así la URL se mantiene y no hay que tocar `config.js`.

---

## Problemas frecuentes

- **"No se pudo conectar"**: revisa que la URL en `config.js` sea la correcta y termine en `/exec`, y que en la implementación diga "Quién tiene acceso: Cualquier usuario".
- **"Aún no se configura el PIN en el Sheet"**: falta el paso 7 de la Parte 1.
- **"No existe la pestaña..."**: falta el paso 6 de la Parte 1, o se cambió el nombre de una pestaña.
- **"Categoría no válida"**: alguien borró o renombró esa categoría en Config.

---

## Qué hace cada archivo

| Archivo | Para qué sirve |
|---|---|
| `index.html` | Toda la página: diseño (CSS) y funcionamiento (JavaScript). |
| `config.js` | Solo la URL del Apps Script. |
| `manifest.json`, `icon-*.png` | Nombre e ícono cuando se agrega a la pantalla de inicio. |
| `apps-script/Code.gs` | Copia de respaldo del código que vive en Google. Está comentado por secciones para que se entienda. |
