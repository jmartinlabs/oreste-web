# Migración segura de WordPress a Astro

Estado: **propuesta para revisión**. No autoriza tocar WordPress, DNS ni Hostinger.
Fecha: 2026-11-03. Dominio y cuenta de hosting se conservan.

## 1. Principios

1. **WordPress es de sólo lectura.** No se actualizan plugins, temas ni el núcleo; no se escribe
   en la base de datos; no se cambia la configuración.
2. **Nada se borra.** El WordPress actual se conserva completo durante al menos 30 días tras el
   corte.
3. **Todo es reversible.** Cada paso tiene una copia y un procedimiento de vuelta atrás.
4. **Las URLs con valor se conservan.** La portada y los cinco proyectos mantienen su slug
   exacto; el resto se redirige con 301.
5. **El corte se aprueba por escrito.** Sin aprobación no se publica.
6. **DNS y correo no se tocan.** Los registros MX y la autoridad DNS quedan como están.

## 2. Línea base

- 2 páginas (`/`, `/blog/`), 5 entradas, 2 categorías y 34 medios.
- 18 imágenes incrustadas en las entradas; 33 imágenes y 1 vídeo en la biblioteca.
- 7 URLs públicas principales: portada, blog y cinco proyectos.
- Inventario: [`audit/url-inventory.csv`](../audit/url-inventory.csv) y
  [`audit/asset-inventory.csv`](../audit/asset-inventory.csv).
- Auditoría: [`audit/current-site-audit.md`](../audit/current-site-audit.md).

## 3. Copia de seguridad previa

Antes de cualquier cambio en producción:

1. **Backup de Hostinger** desde hPanel (si el plan lo permite).
2. **Archivos**: `tar.gz` completo de `public_html`, guardado fuera del directorio público
   (`/home/<usuario>/backups/` o descarga local).
3. **Base de datos**: exportación SQL desde phpMyAdmin o `mysqldump` por SSH. Sólo lectura.
4. **Contenido**: exportación REST de `pages`, `posts`, `media` y `categories`.
5. **Verificación**: tamaño, número de archivos, hash SHA-256 y una restauración de prueba en
   local o en el subdominio de preproducción.
6. **Correo**: confirmar que los registros MX no se modifican y que los buzones siguen activos.

La copia se conserva 30 días como mínimo y se documenta su ubicación y fecha. **D2: el
repositorio es público.** Las copias, exportaciones SQL y archivos de WordPress se guardan fuera
de Git; nunca en GitHub ni en `specs/`.

## 4. Exportación de contenido (sólo lectura)

- API REST: `/wp-json/wp/v2/pages`, `/posts`, `/media`, `/categories`.
- Medios originales descargados desde `source_url`.
- HTML renderizado como referencia para el texto visible.
- Guardar en `migration-export/`, **fuera del control de versiones** mientras el repositorio
  sea público. Nunca en `specs/migration/`, que es documentación rastreada.
- **Paso 0, antes de la primera exportación:** añadir `migration-export/` a `.gitignore` cuando
  se autorice la implementación. No se exporta nada hasta que esa protección exista.
- Manifiesto con fecha, URLs, IDs y hashes.
- Repetir la exportación justo antes del corte para capturar cambios de última hora.

No se copian usuarios, comentarios, formularios ni datos personales innecesarios.

## 5. Reescritura y medios

- Convertir HTML de Elementor a Markdown limpio, sin shortcodes ni clases.
- Español de España, frases cortas, un H1 por página y jerarquía correcta.
- Crear las seis páginas de servicio con datos verificados en entrevista: alcance, materiales,
  plazos, zona, garantía y proceso. **No inventar.**
- Convertir las cinco entradas en casos de estudio con ficha (municipio, tipo, servicios, fecha).
- Corregir erratas y eliminar «Lorem ipsum».
- Enlazar servicios con proyectos y proyectos con servicios.
- Medios: seleccionar, renombrar, eliminar EXIF/GPS, redimensionar a 2000 px y generar AVIF/WebP.
- Comprimir o sustituir el vídeo de 12 MB.
- Pedir originales y autorización escrita de uso; obtener logo vectorial.

## 6. Conservación de URLs y redirecciones 301

### 6.1 Se conservan sin cambios

| URL | Tipo |
| --- | --- |
| `/` | Portada |
| `/remodelacion-integral-de-bano/` | Proyecto |
| `/renovacion-total-de-chalet-pintura-y-limpieza-para-un-acabado-impecable/` | Proyecto |
| `/renovacion-de-suelo-en-cocina-industrial-durabilidad-y-funcionalidad/` | Proyecto |
| `/revitalizacion-exterior-transformacion-completa-de-la-fachada-limpieza-reparacion-y-pintura-en-blanco/` | Proyecto |
| `/rapida-reforma-azulejos-cocina/` | Proyecto |

Se implementan con `build.format: 'directory'` y `trailingSlash: 'always'`. Las rutas de proyecto
en la raíz se generan desde la colección `trabajos` con una lista de rutas reservadas.

### 6.2 Redirecciones 301

| Origen | Destino | Motivo |
| --- | --- | --- |
| `/blog/` | `/trabajos/` | El índice lista los mismos cinco proyectos |
| `/category/trabajos/` | `/trabajos/` | Archivo duplicado |
| `/category/blog/` | `/trabajos/` | Archivo duplicado y metadescripción en inglés |
| `/<proyecto>/<adjunto>/` | `/<proyecto>/` | 34 páginas de adjunto; hoy 301 al archivo |
| `/sitemap.xml`, `/sitemap_index.xml` | `/sitemap-index.xml` | Compatibilidad con AIOSEO |
| `/feed/` | `410 Gone` o `/feed.xml` si se aprueba RSS | Feed nuevo |
| `/comments/feed/`, `/wp-json/`, `/xmlrpc.php` | `410 Gone` | Ya no existen |

Reglas:

- Un solo salto; sin cadenas.
- `[R=301,L]` y orden correcto.
- No redirigir assets ni el subdominio de staging.
- Conservar la cadena de consulta cuando exista.
- Canonical único: `https://www.oresteespinosacampos.es/...`.
- El sitemap sólo incluye URLs canónicas, sin drafts ni redirecciones.
- Probar cada fila con `curl` y con `scripts/check-redirects.mjs`.

Inventario completo y prioridades en [`audit/url-inventory.csv`](../audit/url-inventory.csv).

**D3 resuelta en planificación:** la sección será `/trabajos/`, `/blog/` se redirige con 301 y se
conservan las URLs individuales de los proyectos. **No se aplica ninguna redirección todavía**;
se implementarán y probarán en la fase 5, nunca en fase 1 o 2.

## 7. Preproducción y aprobación

1. Desplegar el build en `staging.oresteespinosacampos.es` (ver
   [`architecture/hosting-and-deployment.md`](../architecture/hosting-and-deployment.md)).
2. Subdominio aislado, `noindex`, autenticación básica y sin analítica.
3. Revisión del propietario en móvil y escritorio.
4. Revisión técnica: enlaces, 301, 404, schema, Lighthouse y accesibilidad.
5. Registro de la aprobación con fecha y responsable.

Sin esta aprobación no se programa el corte.

## 8. Corte

1. Congelar cambios de contenido en WordPress (sin modificarlo).
2. Generar copia final y exportación de última hora.
3. Confirmar ventana de baja actividad y responsables (D14).
4. Ejecutar el corte según el runbook de hosting, variante A o B.
5. Purgar caché y ejecutar el smoke test.
6. Enviar el sitemap a Search Console.
7. Vigilar 48 horas.

No se cambian DNS, MX, nameservers ni la base de datos.

## 9. Rollback

Se activa si hay 5xx, páginas clave caídas, 301 rotos, fallo de SSL o contenido crítico erróneo.
El procedimiento depende de la variante de corte confirmada en D1 y en el spike.

1. Detener el despliegue y anunciar el rollback.
2. Si la variante A está confirmada: renombrar el directorio nuevo a `public_html_failed_YYYYMMDD`.
   Si es variante B: vaciar `public_html` conservando una copia del intento fallido.
3. Restaurar `public_html_legacy_YYYYMMDD` si existe; en variante B, extraer el `tar.gz`
   verificado en `public_html`.
4. Purgar caché de LiteSpeed y CDN.
5. Verificar portada, servicios, proyectos, contacto, 301, 404 y HTTPS.
6. Registrar la causa y no reintentar sin corregirla.

La base de datos de WordPress permanece intacta. Objetivo: menos de 15 minutos. Se prueba una vez
en preproducción antes del corte.

## 10. Post-lanzamiento

- 48 horas de vigilancia: disponibilidad, 404, 301, Search Console y contactos.
- Sitemap enviado; comprobar indexación de las URLs conservadas.
- Mantener WordPress 30 días fuera de `public_html`.
- Retirar el subdominio de preproducción tras la aprobación.
- Conservar los 5 últimos artefactos y una copia semanal externa.
- Revisar Lighthouse y enlaces a los 7 y 30 días.
- No borrar la base de datos ni los archivos de WordPress sin autorización expresa.

## 11. Prohibiciones de la fase 1

- Modificar WordPress, plugins, temas o base de datos.
- Cambiar DNS, MX, SSL o configuración de Hostinger.
- Crear el subdominio o desplegar en staging.
- Aplicar redirecciones 301 antes de la autorización de migración.
- Contratar servicios adicionales.
- Publicar, fusionar o borrar contenido.
- Introducir Node.js en producción.

## 12. Verificación de aceptación de la migración

- [ ] Las 6 URLs conservadas responden 200 con el slug exacto.
- [ ] Las redirecciones 301 responden en un solo salto.
- [ ] No hay 404 en enlaces internos.
- [ ] Sitemap con sólo URLs canónicas.
- [ ] Schema `HousePainter` válido y NAP coherente.
- [ ] Imágenes optimizadas con alt.
- [ ] Lighthouse móvil dentro del presupuesto.
- [ ] Copia verificada y restauración probada.
- [ ] Rollback probado en preproducción.
- [ ] Aprobación escrita del propietario.
- [ ] Search Console y vigilancia 48 h.

## 13. Riesgos específicos

| Riesgo | Impacto | Mitigación |
| --- | --- | --- |
| Perder una URL con valor | Alto | Matriz cerrada, 301 probados, staging |
| Cadena o bucle de redirecciones | Medio | Pruebas automatizadas y revisión de `.htaccess` |
| Copia incompleta | Alto | Hash, restauración de prueba, 30 días de retención |
| WordPress roto tras el corte | Alto | Variante A, copia, rollback probado |
| Pérdida de correo | Alto | No tocar MX ni DNS; verificar antes y después |
| Caché sirviendo la versión antigua | Bajo | Purga y comprobación con `curl` |
| Contenido inventado o sin derechos | Medio | Entrevista, originales y autorización escrita |
| Indexación de staging | Medio | `noindex`, basic auth y robots |
| Formulario con spam | Medio | Sin formulario al lanzar; PHP+SMTP con límites |
