# TRESNAK privado: despliegue seguro

Esta copia no modifica la carpeta original ni GitHub Pages. El Worker comprueba una sesión de Supabase **antes** de devolver cualquier archivo estático; por ello también bloquea accesos directos a HTML, JSON, PDFs, imágenes, CSS y JavaScript.

## Preparar Supabase

1. Crea un proyecto Supabase exclusivo para TRESNAK.
2. En **Authentication > Providers > Email**, deja activado el proveedor de correo y desactiva el registro público si aparece habilitado.
3. En **Authentication > URL Configuration**, añade la URL final del Worker como `Site URL` y como `Redirect URL`.
4. Crea las personas usuarias únicamente mediante **Authentication > Users > Invite user**. Las bajas se realizan desde esa misma lista (eliminar o bloquear el usuario).
5. Copia el `Project URL` y la clave `anon`/`publishable`; no uses ni publiques la `service_role`.

## Preparar Cloudflare

1. Instala Wrangler en un equipo de administración y autentícate en Cloudflare.
2. Desde esta carpeta, define estos secretos (no se guardan en el repositorio):

   ```powershell
   wrangler secret put SUPABASE_URL
   wrangler secret put SUPABASE_ANON_KEY
   ```

3. Despliega con `wrangler deploy`.
4. Prueba la URL `workers.dev` con una cuenta invitada y en una ventana privada sin sesión. Confirma que una URL directa como `/erne.html` o `/tips/segurgunea.pdf` lleva al acceso.
5. Solo después, asocia el dominio definitivo y retira GitHub Pages. Conserva esta copia y el despliegue de GitHub hasta completar la prueba.

## Notas operativas

- La sesión se guarda en una cookie `HttpOnly`, por lo que el JavaScript de TRESNAK no puede leerla.
- El Worker valida el token contra Supabase en cada solicitud; prima protección sobre rendimiento. Si la app crece, se puede añadir validación JWT local con JWKS.
- No hay cuentas, claves ni correos incluidos en estos archivos.


