# Arquitectura de hosting, preproducción y despliegue

Estado: **propuesta para revisión**. No autoriza cambios en Hostinger, DNS ni WordPress.
Fecha: 2026-11-03. Destino confirmado: la cuenta actual de Hostinger y el dominio
`oresteespinosacampos.es`.

## 1. Requisitos que impone Astro estático

Astro con `output: 'static'` genera un árbol de archivos:

- HTML por ruta (`dist/<ruta>/index.html` con `build.format: 'directory'`).
- CSS, JavaScript mínimo, imágenes AVIF/WebP/JPEG y fuentes WOFF2.
- `sitemap-index.xml`, `robots.txt`, `404.html` y `.htaccess`.

Para servirlo sólo se necesita:

1. Un servidor web que entregue archivos con el MIME correcto.
2. `index.html` como índice de directorio para conservar las URLs con barra final.
3. HTTPS válido en el dominio y en el subdominio de preproducción.
4. Capacidad opcional de reescritura para 301 y cabeceras (`.htaccess`).
5. Espacio en disco suficiente (la web completa se estima por debajo de 100 MB).

**No se necesita**: Node.js, base de datos, PHP (salvo formulario), procesos persistentes,
cron ni compilación en el servidor. Astro compila en local o en GitHub Actions.

## 2. Matriz de compatibilidad con el plan actual

Vocabulario:

- **Observado**: comprobado en la web pública el 2026-11-03.
- **Documentado**: aparece en la documentación oficial de Hostinger, pero no se ha comprobado
  en esta cuenta.
- **Pendiente**: requiere acceso a hPanel o una prueba en preproducción.
- **No requerido**: fuera del alcance de esta arquitectura.

| Capacidad | Necesidad | Estado | Cómo se comprueba |
| --- | --- | --- | --- |
| Servir HTML/CSS/JS/imágenes | Imprescindible | **Observado** | Cabeceras y assets de la web actual |
| Directorio `public_html` | Imprescindible | **Pendiente**: ruta exacta | hPanel → Administrador de archivos |
| Índice de directorio y barra final | Imprescindible | **Pendiente** | Prueba de `dist/` en staging |
| `.htaccess` y `mod_rewrite` | Imprescindible para 301 | **Documentado**, pendiente de probar | Prueba de redirección en staging |
| 404 personalizado | Recomendado | **Documentado**, pendiente | `ErrorDocument` en staging |
| HTTPS del dominio | Imprescindible | **Observado** (certificado válido) | hPanel → SSL |
| SSL del subdominio | Imprescindible para staging | **Pendiente** | hPanel → SSL del subdominio |
| Subdominios | Imprescindible para staging | **Pendiente**: límite del plan | hPanel → Subdominios |
| SSH/SFTP | Útil para desplegar | **Pendiente**: según plan | hPanel → SSH/SFTP |
| `rsync` por SSH | Útil para automatizar | **Pendiente** | Prueba en staging |
| Despliegue Git de hPanel | Alternativa | **Pendiente**: no ejecuta build | hPanel → Git |
| Copias automáticas | Imprescindible | **Pendiente**: frecuencia y retención | hPanel → Backups |
| Copia manual | Imprescindible antes del corte | **Pendiente**: según plan | hPanel → Backups |
| PHP 8.x | Sólo si hay formulario | **Observado** en la web actual | hPanel → PHP |
| SMTP autenticado | Sólo si hay formulario | **Pendiente**: límites | hPanel → Correo |
| Node.js | **No requerido** | — | — |
| Cron | **No requerido** | — | — |
| CDN y caché LiteSpeed | Recomendado | **Observado** (`hcdn`, `x-litespeed-cache`) | Purga tras desplegar |
| MIME de AVIF/WebP/WOFF2 | Recomendado | **Pendiente** | Prueba de tipos MIME |
| HTTP/3 | Recomendado | **Observado** (`alt-svc: h3`) | — |
| DNS y MX | No se tocan | **Pendiente** confirmar autoridad | hPanel → DNS |
| Espacio y ancho de banda | Necesario | **Pendiente**: consumo actual | hPanel → Recursos |
| Comprobar que se puede crear/renombrar el directorio raíz | Imprescindible para la variante A | **Pendiente** | Prueba controlada en staging |
| Autenticación básica (`.htpasswd`) | Necesaria para staging | **Pendiente** | hPanel o `.htaccess` en staging |

**Conclusión honesta:** la arquitectura es compatible con hosting compartido y la web actual ya
sirve assets estáticos desde Hostinger, pero **no se puede afirmar que el plan contratado cumpla
todos los requisitos hasta completar D1**. Ninguna capacidad se da por supuesta.

## 3. Verificación pendiente en hPanel (sólo lectura)

Esta lista no cambia nada. Se ejecuta con autorización y se registra el resultado.

- [ ] Nombre exacto del plan, fecha de renovación y límites.
- [ ] Número de sitios y subdominios permitidos.
- [ ] Ruta real del directorio público (`public_html` o `domains/<dominio>/public_html`).
- [ ] ¿Se puede renombrar/crear el directorio raíz? ¿Y proteger un directorio con contraseña?
- [ ] Estado de SSL del dominio y del subdominio; renovación automática.
- [ ] SSH/SFTP: activo, usuario, puerto y disponibilidad de `rsync`.
- [ ] Backups: frecuencia, retención y botón de copia manual.
- [ ] PHP: versión y extensiones (sólo si hay formulario).
- [ ] Correo: buzones, límites SMTP y registros MX que no se deben modificar.
- [ ] DNS: nameservers y autoridad actual.
- [ ] Caché LiteSpeed y CDN: cómo se purga.
- [ ] Espacio usado y ancho de banda del mes.
- [ ] Existencia de `public_html` con WordPress y de la base de datos asociada.

Resultado esperado: un registro fechado en este documento o en el checkpoint de setup. Si algo
no está disponible, se documenta como limitación y se elige la alternativa correspondiente.

## 4. Prueba técnica de compatibilidad (fase 3, con autorización)

No se ejecuta en fase 1. Consiste en un *spike* de dos páginas desplegado **sólo en el
subdominio de preproducción**:

1. Página `/` y página anidada `/spike/`.
2. Un enlace a `/spike` sin barra final para comprobar la redirección.
3. Un 404 personalizado.
4. Una regla 301 de prueba (`/antiguo/` → `/spike/`).
5. Una imagen AVIF/WebP, una fuente WOFF2 y CSS.
6. `.htaccess` con caché y cabeceras de seguridad.

Comprobaciones:

| Prueba | Resultado esperado |
| --- | --- |
| `GET /` | 200, `text/html` |
| `GET /spike/` | 200, `text/html` |
| `GET /spike` | 301 a `/spike/` |
| `GET /no-existe/` | 404 con página propia |
| `GET /antiguo/` | 301 a `/spike/` |
| MIME de CSS, JS, AVIF, WebP, WOFF2 | Correctos |
| Compresión | Brotli o gzip |
| Caché de `/_astro/*` | Larga e inmutable |
| HTTPS y HSTS | Sin errores ni contenido mixto |
| Lighthouse móvil | ≥ 95 en rendimiento y accesibilidad |

Si AVIF no se sirve con el MIME correcto, se elimina AVIF y se mantiene WebP/JPEG. Si
`mod_rewrite` no funciona, se revisa el soporte de `.htaccess` antes de continuar. El spike se
elimina al terminar y se registran los resultados.

## 5. Entorno de preproducción por subdominio

**Diseño propuesto:** `staging.oresteespinosacampos.es` (alternativa: `nuevo.`).

| Aspecto | Decisión propuesta |
| --- | --- |
| Directorio | Raíz propia del subdominio, separada de `public_html` |
| SSL | SSL del subdominio: **pendiente**; verificar disponibilidad y coste en D1 |
| Indexación | `X-Robots-Tag: noindex, nofollow` + `robots.txt` con `Disallow: /` |
| Acceso | Autenticación básica; usuario y contraseña fuera del repositorio |
| Analítica | Desactivada |
| Contenido | Real o provisional, nunca datos personales de clientes |
| Aislamiento | Sin conexión ni credenciales de la base de datos; docroot, `.htaccess` y credenciales separados; sin PHP |
| Aprobación | El propietario revisa la URL real en móvil y escritorio |
| Retirada | Se elimina tras el lanzamiento, con autorización |

**No se crea en fase 1.** Crear el subdominio implica un registro DNS y un cambio en Hostinger,
acciones prohibidas por las restricciones actuales. Se hará en fase 3 con autorización expresa.
**D1 no bloquea el diseño ni la fase 2**: sólo debe resolverse antes de configurar staging.

**Si el plan no permite subdominios:** la alternativa es revisar el build en local
(`astro preview`), generar capturas y un ZIP del artefacto. No se propone ningún servicio externo
porque las restricciones impiden contratar servicios adicionales. Si el propietario necesita una
URL, se decide entonces entre autorizar un subdominio o publicar en un directorio temporal
protegido, siempre con `noindex`.

## 6. Procedimiento de despliegue de `dist/` en `public_html`

### 6.1 Empaquetado

```sh
npm ci
npm run validate        # astro check, contenido, imágenes, enlaces
npm run build
# revisar dist/: sin fuentes, sin node_modules, sin specs, sin .git
( cd dist && zip -r ../site-<sha>.zip . )
unzip -l site-<sha>.zip | head   # index.html y .htaccess deben estar en la raíz del ZIP
shasum -a 256 site-<sha>.zip    # macOS; en Linux/CI usar sha256sum
```

El artefacto se guarda con el SHA del commit y se conserva para rollback. Incluye la capa de
compatibilidad `wp-content/uploads/` con los originales heredados; no incluye `src/`,
`node_modules/`, `specs/` ni `.git/`.

### 6.2 Despliegue en preproducción

1. Copiar el artefacto al directorio del subdominio (Administrador de archivos o SFTP).
2. Extraer el ZIP y comprobar que `index.html` y `.htaccess` están en la raíz.
3. Permisos: archivos 644, directorios 755.
4. Purgar la caché de LiteSpeed/CDN.
5. Ejecutar el smoke test y Lighthouse.
6. Revisión del propietario y aprobación registrada.

### 6.3 Corte en producción

Requisitos previos: D1 resuelto, copia verificada, aprobación escrita y ventana acordada (D14).

1. Generar copia completa: backup de Hostinger + `tar.gz` de `public_html` + exportación SQL,
   guardados **fuera** de `public_html`.
2. Verificar la copia (tamaño, hash y restauración de prueba en local o staging).
3. Exportar el contenido de WordPress por REST API para capturar cambios de última hora.
4. En ventana de baja actividad, ejecutar una de estas dos variantes:

   **A. Raíz renombrable (preferida, sólo si el spike lo confirma):**
   - Renombrar `public_html` a `public_html_legacy_YYYYMMDD`.
   - Crear un `public_html` nuevo y subir el contenido de `dist/`.
   - La ruta antigua queda fuera del directorio público si el hosting lo permite.

   **B. Raíz fija:**
   - Mover el contenido de WordPress a una carpeta de backup fuera de `public_html`.
   - Subir el contenido de `dist/` a `public_html`.

5. Ajustar permisos y comprobar `.htaccess`.
6. Purgar caché de LiteSpeed y CDN.
7. Ejecutar el smoke test completo.
8. Enviar el sitemap a Search Console y vigilar 48 h.

**Nunca** se suben `src/`, `node_modules/`, `specs/`, `migration-export/`, `.git/` ni archivos de
entorno. **Sí** forma parte de `dist/` la capa estática `wp-content/uploads/` con los medios
originales heredados. **Nunca** se tocan DNS, MX ni la base de datos de WordPress durante el corte.

### 6.4 Smoke test posterior

- [ ] Portada 200 y sin errores de consola.
- [ ] Servicios y proyectos 200.
- [ ] Contacto: teléfono, WhatsApp y correo funcionan.
- [ ] 301 antiguos sin cadenas.
- [ ] 404 propio.
- [ ] Al menos 5 URLs heredadas de `/wp-content/uploads/...` responden 200.
- [ ] Una página de adjunto responde 301 a su archivo original.
- [ ] Sitemap y `robots.txt` correctos.
- [ ] HTTPS y `www` canónicos.
- [ ] Imágenes y fuentes cargan.
- [ ] Formulario, sólo si existe.
- [ ] Lighthouse móvil dentro del presupuesto.

## 7. HTTPS, redirecciones y cabeceras

- Un solo salto: `http://oresteespinosacampos.es` → `https://www.oresteespinosacampos.es/`.
- Si hPanel ya fuerza HTTPS y `www`, no se duplican reglas en `.htaccess`: se comprueba primero.
- 301 desde la matriz de [`audit/url-inventory.csv`](../audit/url-inventory.csv), con
  `[R=301,L]`, sin bucles y sin afectar a assets.
- HSTS sólo cuando todos los subdominios estén en HTTPS; empezar con `max-age` corto.
- Cabeceras: `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`,
  `X-Frame-Options` y `Strict-Transport-Security` cuando proceda.
- `DirectoryIndex index.html` y `Options -MultiViews` para rutas limpias.
- Caché larga para `/_astro/*`; HTML sin caché prolongada.

## 8. Copias de seguridad

Antes del corte:

1. Backup automático de Hostinger (si el plan lo permite).
2. `tar.gz` de archivos fuera de `public_html`.
3. Exportación SQL de la base de datos.
4. Copia local del artefacto `site-<sha>.zip`.
5. Prueba de restauración en local o staging.

Después del corte:

- Backup automático del plan + copia semanal externa.
- Conservar la copia de WordPress 30 días.
- Conservar los 5 últimos artefactos de despliegue.
- Documentar fecha, responsable y ubicación de cada copia.

## 9. Rollback

**Cuándo activarlo:** error 5xx, páginas clave caídas, redirecciones rotas, fallo de SSL,
contenido crítico incorrecto o formulario inoperativo.

**Procedimiento (según la variante confirmada en D1 y en el spike):**

1. Detener el despliegue y anunciar el rollback.
2. Si la variante A está confirmada y se puede renombrar el docroot: renombrar el `public_html`
   nuevo a `public_html_failed_YYYYMMDD`. Si es variante B: vaciar `public_html` conservando una
   copia del intento fallido y extraer el `tar.gz` verificado.
3. Si existe `public_html_legacy_YYYYMMDD`, restaurarlo; si no existe (variante B), extraer el
   `tar.gz` verificado en `public_html`.
4. Purgar caché de LiteSpeed y CDN.
5. Comprobar portada, servicios, proyectos, contacto, 301, 404 y HTTPS.
6. Registrar la causa y no reintentar sin corregirla.

La base de datos de WordPress no se toca durante el rollback. Objetivo orientativo: menos de 15
minutos, **no garantizado**; se medirá en el ensayo de preproducción.

## 10. Publicación automatizada futura (fase 6)

No se implementa en fase 1. **D11 resuelta:** el despliegue manual verificable es la prioridad;
esta automatización es fase 6. Requisitos de entrada: D1 resuelto, SSH/SFTP confirmado,
repositorio público sin secretos (D2), branch protection y checks verdes.

```text
Pull Request → CI de sólo lectura (check + build + enlaces + Lighthouse)
             → artefacto del PR, sin despliegue
             → aprobación humana del PR
             → merge a main
             → build y artefacto site-<sha>
             → (opcional) workflow_dispatch a staging, environment "staging"
             → environment "production" con revisor obligatorio
             → deploy por rsync/SSH o SFTP
             → purga de caché + smoke test
```

Reglas:

- **CI en PR:** sólo lectura y build; sin secretos, sin despliegue y sin acceso a producción.
- **Preview de staging:** sólo por `workflow_dispatch` desde `main` o una ref protegida
  `preview`, con environment `staging`, revisor obligatorio y secretos propios. Nunca desde PRs
  de forks.
- **Producción:** sólo desde `main` y con el environment `production` y revisor obligatorio.
- `concurrency: deploy-production`, `cancel-in-progress: false`.
- Secretos en el environment correspondiente, nunca en el repositorio ni compartidos con staging.
- Acciones fijadas por SHA; permisos mínimos (`contents: read`).
- Host key de SSH fijada; no aceptar claves desconocidas.
- Si no hay SSH/rsync: SFTP con FTPS si el plan lo permite. **FTP plano no es aceptable.**
- Si no hay SFTP seguro, no se automatiza: se mantiene el despliegue manual.
- Rollback: `workflow_dispatch` que descarga y redespliega un artefacto anterior.
- Purga de caché: automatizable sólo si el plan expone una API; si no, paso manual documentado.
- Aprobación humana: el environment exige un revisor; un merge no publica por sí solo.
- La protección con revisor obligatorio depende del plan de GitHub. Se verifica en D11. Si no
  está disponible, el despliegue sigue siendo manual y la aprobación se registra en el PR.

No se contrata ningún servicio adicional: GitHub Actions y los environments forman parte del
repositorio.

## 11. Formularios y correo

Opciones permitidas sin servicios nuevos:

1. **Sin formulario:** `tel:`, `mailto:` y WhatsApp. Recomendada para el lanzamiento.
2. **PHP + SMTP del dominio:** endpoint en `public/contacto.php`, autenticación SMTP, `From`
   fijo, `Reply-To` del visitante, honeypot, límite por IP y validación estricta.

No se proponen servicios externos de formularios mientras esté vigente la restricción de no
contratar servicios adicionales. Si se aprueba el formulario, se prueba el envío real y la
entregabilidad antes del lanzamiento.

## 12. Criterios de salida de la verificación

- D1 documentado con plan, ruta, SSL, subdominios, SSH y backups.
- Spike de compatibilidad superado en staging.
- Subdominio de preproducción operativo, protegido y no indexable.
- Runbook de despliegue y rollback probados una vez.
- Artefacto y hashes conservados.
- Aprobación escrita para el corte.

## 13. Decisiones específicas: estado

| ID | Estado | Decisión | Efecto |
| --- | --- | --- | --- |
| D1 | Parcial | Plan, ruta, SSH, subdominios, SSL y backups | Verificar antes de staging; no bloquea diseño |
| D2 | Resuelta | Repositorio público sin secretos ni backups | Nunca subir credenciales ni exportaciones |
| D3 | Resuelta en planificación | `/trabajos/` + 301 desde `/blog/`; conservar proyectos | No aplicar redirecciones todavía |
| D11 | Resuelta | Despliegue manual primero; automatización después | Fase 6 |
| D13 | Pendiente | Subdominio y DNS | `staging.oresteespinosacampos.es` |
| D14 | Pendiente | Ventana de corte y aprobador | Fuera de horario comercial |
| D15 | Pendiente | Método final: SSH/rsync, SFTP o manual | Elegir tras D1 |
