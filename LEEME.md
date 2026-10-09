# Acta de Inspección de Ómnibus — instalación

App para tablet de la División Transporte (UPTU). Guarda las inspecciones en Supabase y las comparte en vivo entre todas las tablets del equipo.

**Datos personales:** el nombre del notificado/chofer, las observaciones y las fotos se cifran en la tablet con la *clave del equipo* antes de enviarse. Supabase guarda solo texto ilegible y nunca recibe la clave.

## Archivos

| Archivo | Para qué es |
|---|---|
| `index.html` | La app |
| `sw.js` | Permite abrir la app sin conexión |
| `manifest.json`, `icon.svg` | Ícono y nombre al instalarla en la tablet |
| `supabase.sql` | Crea la base de datos (se pega una sola vez) |

## 1. Crear el proyecto en Supabase (10 min)

1. En supabase.com → **New project**. Nombre: `inspeccion-stm`. Región: **South America (São Paulo)**. Usá un proyecto **aparte** del de desvíos.
2. **SQL Editor → New query**: pegá todo `supabase.sql` y tocá **Run**. Antes, revisá al final del archivo el correo del primer administrador.
3. **Authentication → Sign In / Providers → Email**: desactivá **Allow new users to sign up**. Así nadie puede crearse un usuario por su cuenta.
4. **Authentication → Users → Add user → Create new user**: creá tu usuario con el mismo correo del paso 2 y una contraseña. Marcá **Auto Confirm User**.
5. **Project Settings → API**: copiá la **Project URL** y la clave **anon public**.

## 2. Configurar y publicar la app (10 min)

1. Abrí `index.html` con un editor de texto. Cerca del principio del `<script>` reemplazá:
   - `https://TU-PROYECTO.supabase.co` por la Project URL
   - `TU-CLAVE-ANON-PUBLICA` por la clave anon public

   La clave anon es pública por diseño: lo que protege los datos son los permisos de la base y el cifrado.
2. En GitHub, creá un repositorio nuevo (por ejemplo `inspeccion-stm`) y subí los 5 archivos de la app (`index.html`, `sw.js`, `manifest.json`, `icon.svg`; `LEEME.md` es opcional). **No subas `supabase.sql` si el repositorio es público**: no tiene secretos, pero no hace falta exponerlo.
3. **Settings → Pages**: Source **Deploy from a branch**, rama `main`, carpeta `/ (root)`. En un minuto queda en `https://<tu-usuario>.github.io/inspeccion-stm/`.

## 3. Primer ingreso (administrador)

1. Abrí el link, ingresá con tu usuario.
2. La app te pide **crear la clave del equipo** (mínimo 10 caracteres). **Guardala en un lugar seguro de UPTU**: si se pierde, los nombres, observaciones y fotos guardados no se pueden recuperar.
3. En la pestaña **Equipo**, habilitá a cada inspector (nombre y correo). Después creale el usuario en Supabase → Authentication → Add user, con ese mismo correo, una contraseña inicial y **Auto Confirm User**.

## 4. Instalar en cada tablet

1. Abrir el link en **Chrome**.
2. Ingresar con el usuario del inspector.
3. Ingresar la clave del equipo (se pide una sola vez por tablet).
4. Menú de Chrome (⋮) → **Agregar a la pantalla principal** / **Instalar app**.

## Uso diario

- **Sin señal:** la inspección se guarda en la tablet y se envía sola cuando vuelve la conexión. Arriba aparece un aviso con las pendientes. La primera vez que se usa una tablet sí hace falta internet.
- **Historial:** muestra las inspecciones de todas las tablets, se actualiza en vivo. Las fotos se descargan y descifran al tocar "Ver fotos".
- **Numeración:** el número de inspección lo asigna el servidor al recibirla. Si se guardó sin señal, se ve "(prov.)" hasta que se envía.
- **Excel:** "Exportar a Excel" descarga lo filtrado, con los nombres descifrados. Ese archivo sí contiene datos personales: guardarlo en un lugar de la IM.
- **Borrar:** solo los administradores pueden eliminar inspecciones ya enviadas.

## Bajas y cambios

- **Un inspector deja el equipo:** en la pestaña Equipo → **Deshabilitar**. Pierde el acceso a la base al instante, aunque conozca la clave. Si querés, borrá también su usuario en Supabase → Authentication.
- **Tablet perdida o robada:** deshabilitá el usuario que estaba usando. Sin usuario habilitado, la clave guardada en la tablet no sirve para leer la base.
- **Actualizar la app:** se reemplaza `index.html` en GitHub. Las tablets toman la versión nueva al abrirla con conexión.

## Límites del plan gratuito de Supabase

500 MB de base de datos y 1 GB de archivos. Cada foto pesa unos 200–300 KB, así que alcanza para varios miles de inspecciones con fotos. Un proyecto gratuito se **pausa tras 7 días sin uso**; con uso diario no pasa, y si pasa se reactiva desde el panel.
