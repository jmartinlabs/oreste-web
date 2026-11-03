# Plan de reconstrucción — Fase 1

Estado: **PROPUESTA para revisión**. No autoriza desarrollo, despliegue ni cambios en WordPress.
Fecha: 2026-11-03. Base: auditoría de la web actual y research técnico delegado.

## 0. Resumen

La web nueva será una **web corporativa estática** construida con Astro, Tailwind CSS y
TypeScript, publicada en el **Hostinger compartido existente**, sin Node.js en producción y sin
CMS. El contenido vivirá en archivos Markdown validados por esquema, de modo que un agente pueda
proponer cambios mediante una rama y una Pull Request, y sólo un humano pueda aprobarlos y
publicarlos.

La estrategia es deliberadamente pequeña: **una portada, seis páginas de servicio, un hub de
trabajos, cinco casos de estudio, contacto y tres páginas legales**. Se conservan exactamente la
portada y los cinco slugs de proyecto; `/blog/` y los archivos de categoría se redirigen con 301
salvo que se apruebe mantener un blog (D3). No se toca WordPress hasta la fase de publicación y
sólo con autorización expresa.

## 1. Objetivo y criterios de aceptación

Objetivo: sustituir la web actual por una web más rápida, clara y mantenible, que genere
contactos locales y que pueda actualizarse sin depender de un CMS.

Criterios medibles (se detallarán en la especificación de cada fase):

| Área | Criterio |
| --- | --- |
| Rendimiento | Lighthouse móvil ≥ 95; LCP < 1,8 s; peso de portada < 500 KB; JS ≤ 5 KB por página |
| Accesibilidad | Lighthouse ≥ 95; contraste AA; un solo H1; foco visible; navegación por teclado |
| SEO | URLs conservadas o 301; canonical; sitemap; `HousePainter` válido; NAP coherente |
| Contenido | Cero «Lorem ipsum»; español de España; datos verificados por el propietario |
| Legal | Aviso legal, privacidad y cookies; analítica sólo con consentimiento |
| Operación | Publicación reproducible; copia previa; rollback probado; agente sin permiso de publicar |
| Hosting | `dist/` funciona en Hostinger compartido sin Node; HTTPS y 301 verificados |

## 2. Principios

1. **Ligero por defecto.** Sin framework de UI, sin estado global, sin base de datos, sin SSR.
2. **Cero JavaScript innecesario.** Navegación y contenido funcionan sin JS; las islas se
   justifican una por una.
3. **Contenido como código.** Markdown + esquema Zod + validación en CI; revisable en Git.
4. **Conservar antes que reescribir.** Las URLs y el contenido real mandan; el diseño se adapta.
5. **Aprobación humana explícita.** Ningún agente publica, borra, renombra ni despliega.
6. **Una sola fuente de verdad.** NAP en `src/data/site.ts`; URLs en el frontmatter; decisiones
   en `specs/`.
7. **Compatible con Hostinger.** Nada que requiera Node, procesos persistentes o base de datos.

## 3. Stack propuesto

Versiones verificadas en el registro de npm y en la documentación oficial el 2026-11-03. Se
fijarán en el momento de crear el proyecto y se revisarán antes de la fase 3.

| Pieza | Versión propuesta | Notas |
| --- | --- | --- |
| Astro | 7.4.x | `output: 'static'`; sin adapter; Node ≥ 22.23.4 |
| Tailwind CSS | 4.3.x | Configuración CSS-first con `@theme`; sin `tailwind.config.js` |
| `@tailwindcss/vite` | 4.3.x | Integración vigente; `@astrojs/tailwind` está obsoleto |
| TypeScript | 6.0.x (fijar) | `@astrojs/check` aún no admite TypeScript 7; no usar `latest` |
| `@astrojs/check` | 0.9.x | Puerta de tipos en CI |
| `@astrojs/sitemap` | 3.7.x | Sitemap automático |
| `sharp` | 0.35.x | Optimización de imágenes en build |
| Node local/CI | 22 LTS o superior | Producción no ejecuta Node |

Dependencias que **no** se instalan: React/Vue/Svelte, adapters de servidor, CMS, ORM, librería
de componentes, jQuery, Font Awesome, Google Fonts remotas.

## 4. Arquitectura propuesta

### 4.1 Modelo de renderizado

- Generación estática completa en build (`astro build` → `dist/`).
- HTML por página en `dist/<ruta>/index.html`, gracias a `build.format: 'directory'` y
  `trailingSlash: 'always'`.
- Apache/LiteSpeed sirve `index.html` mediante `DirectoryIndex`; las URLs antiguas con barra
  final se conservan sin reescrituras.
- Sin funciones serverless, sin API, sin base de datos. El único código de servidor opcional
  sería un pequeño endpoint PHP para el formulario, y sólo si se aprueba (ver 4.7).

### 4.2 Estructura de carpetas

```text
oreste-web/
├─ astro.config.mjs
├─ tsconfig.json
├─ package.json
├─ public/
│  ├─ .htaccess              # 301, 404, caché, seguridad
│  ├─ favicon.svg
│  ├─ og-default.jpg
│  └─ robots.txt             # o src/pages/robots.txt.ts
├─ src/
│  ├─ assets/                # imágenes y fuentes optimizables
│  ├─ components/
│  │  ├─ Header.astro
│  │  ├─ Footer.astro
│  │  ├─ Seo.astro
│  │  ├─ Hero.astro
│  │  ├─ ServiceCard.astro
│  │  ├─ ProjectCard.astro
│  │  ├─ Gallery.astro
│  │  ├─ ContactCta.astro
│  │  └─ Breadcrumbs.astro
│  ├─ content/
│  │  ├─ servicios/*.md
│  │  └─ trabajos/*.md
│  ├─ content.config.ts
│  ├─ data/
│  │  └─ site.ts             # NAP, horario, redes, áreas servidas
│  ├─ layouts/
│  │  ├─ Base.astro
│  │  └─ Page.astro
│  ├─ lib/
│  │  ├─ schema.ts           # JSON-LD
│  │  └─ urls.ts             # helpers de canonical y slugs
│  ├─ pages/
│  │  ├─ index.astro
│  │  ├─ servicios/index.astro
│  │  ├─ servicios/[...slug].astro
│  │  ├─ trabajos/index.astro
│  │  ├─ [slug].astro        # conserva las 5 URLs de proyecto en la raíz
│  │  ├─ contacto.astro
│  │  ├─ aviso-legal.astro
│  │  ├─ privacidad.astro
│  │  ├─ cookies.astro
│  │  ├─ 404.astro
│  │  └─ robots.txt.ts
│  └─ styles/
│     └─ global.css
├─ scripts/
│  ├─ check-content.mjs
│  ├─ check-images.mjs
│  ├─ check-links.mjs
│  └─ check-redirects.mjs
└─ specs/                     # documentación del proyecto
```

La ruta dinámica `src/pages/[slug].astro` existe **sólo** para conservar los cinco slugs de
proyecto en la raíz. Se filtra contra una lista de rutas reservadas (`servicios`, `trabajos`,
`contacto`, `aviso-legal`, `privacidad`, `cookies`, `blog`) y la validación falla si un proyecto
intenta ocupar una ruta reservada.

### 4.3 Modelo de contenido

Dos colecciones, deliberadamente pequeñas:

```ts
// src/content.config.ts (esquema propuesto, no implementado)
import { defineCollection, reference } from 'astro:content';
import { glob } from 'astro/loaders';
import { z } from 'astro/zod';

const servicios = defineCollection({
  loader: glob({ base: './src/content/servicios', pattern: '*.md' }),
  schema: ({ image }) => z.object({
    title: z.string().min(20).max(65),
    slug: z.string().regex(/^[a-z0-9-]+$/),
    description: z.string().min(120).max(165),
    order: z.number().int().default(99),
    summary: z.string().min(80).max(220),
    cover: image(),
    coverAlt: z.string().min(20),
    benefits: z.array(z.string().min(10)).min(3).max(6),
    faqs: z.array(z.object({ q: z.string(), a: z.string() })).default([]),
    draft: z.boolean().default(true),
  }),
});

const trabajos = defineCollection({
  loader: glob({ base: './src/content/trabajos', pattern: '*.md' }),
  schema: ({ image }) => z.object({
    title: z.string().min(15).max(70),
    slug: z.string().regex(/^[a-z0-9-]+$/), // slug legado exacto
    description: z.string().min(120).max(165),
    date: z.coerce.date(),
    updated: z.coerce.date().optional(),
    location: z.string().min(3),
    propertyType: z.string().optional(),
    services: z.array(reference('servicios')).min(1),
    cover: image(),
    coverAlt: z.string().min(20),
    gallery: z.array(z.object({ src: image(), alt: z.string().min(15) })).max(12).default([]),
    duration: z.string().optional(),
    featured: z.boolean().default(false),
    draft: z.boolean().default(true),
  }),
});

export const collections = { servicios, trabajos };
```

Reglas:

- `slug` debe coincidir con el nombre del archivo y con la URL legada; `check-content.mjs` lo
  verifica.
- `draft: true` excluye la entrada de páginas, listados, sitemap y búsqueda interna.
- Las imágenes viven junto al contenido o en `src/assets/`; nunca en `public/` si deben
  optimizarse.
- El NAP vive sólo en `src/data/site.ts`; ningún texto duplica teléfono, correo o dirección a
  mano. Son datos públicos tomados de la web actual y verificados con el propietario.

### 4.3.1 Correspondencia entre servicios declarados y páginas

La portada declara diez servicios. Se agrupan en seis páginas para no diluir la intención de
búsqueda ni crear páginas de una sola frase:

| Página propuesta | Servicios declarados que cubre |
| --- | --- |
| `/servicios/pintura-interior-exterior/` | Pintura interior y exterior; reparación de paredes |
| `/servicios/pintura-de-fachadas/` | Restauración de fachadas |
| `/servicios/alicatado-y-soleria/` | Alicatado; solería |
| `/servicios/reforma-de-banos/` | Reforma integral de baños |
| `/servicios/reforma-de-cocinas/` | Renovación de cocinas |
| `/servicios/mantenimiento-general/` | Mantenimiento general; pequeñas reformas; fontanería y electricidad básicas |

Ningún servicio desaparece. «Fontanería y electricidad» y «pequeñas reformas» se presentan como
parte del mantenimiento, no como especialidades independientes, salvo que el propietario confirme
que quiere posicionarlas por separado. Los diez servicios deben validarse en la entrevista (D4).

### 4.4 Layouts y componentes

- `Base.astro`: `<html lang="es">`, skip link, header, `<main>`, footer, estilos globales, SEO y
  JSON-LD.
- `Page.astro`: migas de pan, H1, entradilla, slot y CTA final.
- `Seo.astro`: title, description, canonical, Open Graph, Twitter Card, `noindex` opcional y
  JSON-LD inline con `set:html`.
- `Header.astro`: logo, seis enlaces como máximo, teléfono y CTA «Pedir presupuesto». En móvil,
  menú con `<details>` o un script inline de ~20 líneas; sin isla de framework.
- `Gallery.astro`: grid responsive con `<Picture>`; lightbox opcional con isla de 2–3 KB.
- Sin barra lateral, sin buscador, sin comentarios, sin widgets.

### 4.5 SEO local

- **Un H1 por página**, jerarquía H2/H3 coherente.
- Títulos con patrón: `Servicio en Mijas y Costa del Sol | Oreste Espinosa Campos`.
- Metadescripciones únicas de 120–165 caracteres, escritas a mano.
- Canonical absoluto con `www` y HTTPS; `trailingSlash: 'always'`.
- JSON-LD en un único `@graph`:
  - `HousePainter` (subtipo de `LocalBusiness`) con `name`, `url`, `telephone`, `email`,
    `address` (Pje. Juan Gris, 29651 Las Lagunas de Mijas, Málaga; dato público de la web
    actual), `areaServed`, `openingHours`,
    `image`, `sameAs` (Facebook, Instagram, Google Business Profile).
  - `WebSite` y `BreadcrumbList`.
  - `Service` en cada página de servicio; `Article`/`CreativeWork` en cada proyecto.
- `sitemap-index.xml` con sólo URLs canónicas, sin drafts ni redirecciones.
- `robots.txt` con `Sitemap:` absoluto.
- Open Graph con imagen real de 1200×630 por página.
- Enlazado interno: servicio → proyectos relacionados y proyecto → servicios usados.
- Google Business Profile y Search Console como parte de la fase de publicación, no del código.

### 4.6 Imágenes y rendimiento

- `src/assets/` + `<Picture formats={['avif','webp']}>` con `widths` y `sizes` explícitos.
- Formatos de origen: JPEG de calidad alta; salida AVIF/WebP; máximo 2000 px de lado mayor.
- `loading="lazy"` y `decoding="async"` en todo salvo la imagen LCP del hero, que usa
  `fetchpriority="high"` y dimensiones explícitas.
- Fuentes autoalojadas en woff2 (máximo dos familias), `font-display: swap` y precarga de la
  variante usada en el hero.
- CSS: un único `global.css` con Tailwind 4 y tokens en `@theme`; sin CSS de terceros.
- JS: cero por defecto. Presupuesto: ≤ 5 KB gzip por página.
- Vídeo: sustituir el MP4 de 12 MB por un clip comprimido (< 5 MB) o un reproductor ligero con
  póster; nunca autoplay con sonido.
- Caché: `/_astro/*` inmutable un año; HTML sin caché larga; purga de LiteSpeed tras publicar.

### 4.7 Formularios y funcionalidades adicionales

Requisito del usuario: verificar compatibilidad con Hostinger compartido.

| Opción | Compatibilidad | Coste | Riesgo | Valoración |
| --- | --- | --- | --- | --- |
| Sin formulario: teléfono, WhatsApp y correo clicables | Total (HTML) | 0 | Ninguno | **Recomendada para el lanzamiento** |
| Endpoint PHP propio + SMTP de Hostinger | Sí (PHP disponible; no requiere Node) | 0 | Spam, mantenimiento, mala configuración SMTP | Aceptable si el propietario exige formulario |
| Servicio externo (Web3Forms, Formspree) | Sí (POST desde HTML) | 0–gratis con límite | Transferencia de datos a terceros, dependencia | Alternativa si se quiere evitar PHP |
| Formulario sólo JavaScript | No fiable | — | Usuarios sin JS, spam | Descartada |

Decisión propuesta: **lanzar sin formulario** y medir llamadas/WhatsApp. Si se aprueba un
formulario, hacerlo en fase 6 con endpoint PHP + SMTP, honeypot, límite por IP, validación en
servidor, consentimiento RGPD y correo `Reply-To` del visitante. Nunca subida de archivos.

Otras funcionalidades: galería con lightbox (isla opcional), mapa de Google embebido sólo tras
consentimiento o como enlace estático, y ningún chat en vivo.

### 4.8 Analítica y consentimiento

- Situación actual: dos etiquetas GA4 cargando a la vez, sin banner ni consentimiento.
- Propuesta: **una sola** propiedad GA4 con Consent Mode v2 y banner ligero que bloquee
  analítica hasta la aceptación; o, si el propietario prefiere cero cookies, Cloudflare Web
  Analytics / Plausible.
- Search Console y Google Business Profile son obligatorios para SEO local, con o sin GA4.
- El banner debe permitir «Rechazar» con la misma visibilidad que «Aceptar» y enlazar a
  `/cookies/`.

### 4.9 Seguridad

- Sitio estático: sin login, sin base de datos, sin plugins.
- Cabeceras en `.htaccess`: `Strict-Transport-Security` (tras verificar HTTPS),
  `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`,
  `Permissions-Policy` restrictiva y `X-Frame-Options: SAMEORIGIN`.
- Sin `X-Powered-By` si el hosting lo permite; sin listado de directorios.
- `.htaccess` con `Options -Indexes` y bloqueo de archivos sensibles (`.env`, `.git`, `*.md`).
- Repositorio: decidir visibilidad (hoy público). Si sigue público, no commitear datos
  personales, exportaciones de WordPress ni secretos.
- Credenciales de despliegue sólo en GitHub Environments/Secrets, nunca en el repo.
- Dependencias: `npm ci`, lockfile, Dependabot opcional y revisión mensual.

## 5. URLs y redirecciones

### 5.1 URLs que se conservan exactamente

| URL | Tipo |
| --- | --- |
| `/` | Portada |
| `/remodelacion-integral-de-bano/` | Proyecto |
| `/renovacion-total-de-chalet-pintura-y-limpieza-para-un-acabado-impecable/` | Proyecto |
| `/renovacion-de-suelo-en-cocina-industrial-durabilidad-y-funcionalidad/` | Proyecto |
| `/revitalizacion-exterior-transformacion-completa-de-la-fachada-limpieza-reparacion-y-pintura-en-blanco/` | Proyecto |
| `/rapida-reforma-azulejos-cocina/` | Proyecto |

### 5.2 URLs nuevas

`/servicios/`, seis páginas de servicio, `/trabajos/`, `/contacto/`, `/aviso-legal/`,
`/privacidad/`, `/cookies/`.

### 5.3 Redirecciones 301 propuestas

| Origen | Destino | Motivo |
| --- | --- | --- |
| `/blog/` | `/trabajos/` | El índice actual lista los mismos cinco proyectos |
| `/category/trabajos/` | `/trabajos/` | Archivo duplicado |
| `/category/blog/` | `/trabajos/` | Archivo duplicado y metadescripción en inglés |
| `/sitemap.xml`, `/sitemap_index.xml` | `/sitemap-index.xml` | Compatibilidad con AIOSEO |
| `/feed/` | `/feed.xml` (si se publica RSS) | Feed nuevo |
| `/comments/feed/`, `/wp-json/`, `/xmlrpc.php` | `410 Gone` | Ya no existen |

Si el propietario quiere mantener un blog, la alternativa es conservar `/blog/` como sección de
artículos y no redirigir; se decide en D3. Las redirecciones se implementan en `.htaccess` y se
prueban con `scripts/check-redirects.mjs` contra una lista cerrada.

Inventario completo: [`audit/url-inventory.csv`](audit/url-inventory.csv).

## 6. Hosting y despliegue en Hostinger

### 6.1 Hechos observados y límites a verificar

Lo observado directamente en la web (cabeceras del 2026-11-03):

- `platform: hostinger`, `panel: hpanel`, `server: hcdn`, `x-litespeed-cache: hit`,
  `x-powered-by: PHP/8.2.35`, HTTP/2 y HTTP/3. Es hosting compartido con LiteSpeed y CDN.

Lo que afirma la documentación oficial de Hostinger y **debe confirmarse en hPanel**:

- Los sitios estáticos funcionan en todos los planes web; `public_html` es el directorio público
  y se puede subir un ZIP desde el Administrador de archivos.
- **Node.js no está en el plan Premium**; sólo en Unlimited/Cloud. No lo necesitamos: Astro
  compila en local o en GitHub Actions y produce archivos estáticos.
- SSH/SFTP está en Premium y superiores; el puerto aparece como 22 en una guía y 65002 en otra.
  `rsync` no está documentado. **Probar ambos.**
- El despliegue Git de hPanel no ejecuta `npm run build`; sirve el contenido de la rama tal cual.
- `.htaccess` es compatible (LiteSpeed emula Apache): `Redirect 301`, `ErrorDocument`,
  `DirectoryIndex`, cabeceras y compresión.
- SSL gratuito con renovación automática por dominio; HTTPS se fuerza desde hPanel.
- Copias automáticas semanales en Premium y diarias en Unlimited, según la documentación; la
  copia manual puede requerir un plan superior.
- PHP está disponible para un formulario, pero `mail()` tiene mala entregabilidad; usar SMTP.

Fuentes consultadas el 2026-11-03: <https://docs.hostinger.com/websites/file-manager.md>,
<https://docs.hostinger.com/websites/ssl.md>, <https://docs.hostinger.com/websites/backups.md>,
<https://docs.hostinger.com/websites/git.md>, <https://docs.hostinger.com/websites/redirects.md>,
<https://www.hostinger.com/support/1583645-how-to-enable-ssh-access-in-hostinger/> y
<https://www.hostinger.com/support/11393648-php-mail-limitation-explained-how-to-improve-email-delivery-with-smtp/>.

El plan exacto es **desconocido** y es el único bloqueo real para fijar el método definitivo.

### 6.2 Método manual (fase 5, antes de automatizar)

1. Confirmar plan, SSH, backups y directorio raíz en hPanel.
2. Congelar cambios de WordPress y generar copia: backup de Hostinger + `tar.gz` local de
   `public_html` + exportación SQL, guardados **fuera** de `public_html`.
3. Construir en local: `npm ci && npm run build`.
4. Validar `dist/`: `npm run preview`, enlaces, 301, 404, Lighthouse y revisión visual.
5. Publicar primero en un subdominio de preproducción (`staging.oresteespinosacampos.es` o
   `nuevo.oresteespinosacampos.es`), con `noindex` y autenticación básica. Esto **no toca** la
   web actual y permite aprobar sobre el hosting real.
6. Tras la aprobación escrita, mover el WordPress actual fuera de `public_html` (nunca borrarlo)
   y subir el contenido de `dist/` a `public_html`, incluido `.htaccess`.
7. Purgar la caché de LiteSpeed y la CDN; verificar HTTPS, `www`, 301, 404, sitemap y formulario
   (si existe).
8. Enviar el sitemap a Search Console y comprobar el estado durante 48 horas.

### 6.3 Checklist previo en hPanel (bloqueante para D1)

- [ ] Nombre exacto del plan y fecha de renovación.
- [ ] SSH activo, puerto real y disponibilidad de `rsync`.
- [ ] Directorio raíz real (`public_html` vs `domains/<dominio>/public_html`).
- [ ] Frecuencia y retención de backups; botón de copia manual disponible.
- [ ] SSL activo, renovación automática y opción «forzar HTTPS».
- [ ] Estado de la caché LiteSpeed y de la CDN.
- [ ] DNS: nameservers de Hostinger o externos; registros MX que no se deben tocar.
- [ ] Correo: buzones existentes y límites de envío SMTP.
- [ ] Confirmar que el WordPress actual no se modifica hasta la autorización de migración.

### 6.4 HTTPS, redirecciones y cabeceras

- Un solo salto: `http://oresteespinosacampos.es` → `https://www.oresteespinosacampos.es/`.
  Evitar la cadena actual de dos saltos.
- Forzar `www` de forma canónica; el ápice redirige.
- HSTS sólo después de comprobar todos los subdominios; empezar con `max-age` corto y subir.
- 404 personalizado a `/404.html`, sin redirección blanda.
- `DirectoryIndex index.html` y `Options -MultiViews` para que las rutas limpias funcionen.
- Cabeceras de seguridad y caché descritas en 4.9 y 4.6.

### 6.5 Copias y rollback

- Antes del corte: backup de Hostinger + archivo local + exportación de la base de datos.
- Después del corte: conservar la copia 30 días y una copia semanal externa.
- Rollback manual: restaurar el `tar.gz` en `public_html` y purgar caché (≈ 5 minutos).
- Rollback automatizado (fase 6): conservar los 5 últimos artefactos de GitHub Actions y
  redesplegar el artefacto anterior con `workflow_dispatch`.
- Probar el rollback una vez en preproducción antes de confiar en él.

### 6.6 Publicación automatizada futura (fase 6, sólo tras aprobación)

Diseño propuesto, a validar contra el plan real:

```text
Pull Request → CI (check + build + enlaces + Lighthouse)
             → preview en staging
             → aprobación humana del PR
             → merge a main
             → job build (artefacto site-<sha>)
             → environment "production" con revisor obligatorio
             → deploy por rsync/SSH o SFTP
             → purga de caché + smoke test
```

- `concurrency: deploy-production` y `cancel-in-progress: false`.
- Secretos en el environment `production`, nunca en el repositorio.
- Host key de SSH fijada; no aceptar claves desconocidas.
- Si no hay SSH/rsync: acción SFTP oficial mantenida, con FTPS si el plan lo permite (el FTP
  plano no es aceptable para credenciales).
- El job de despliegue **exige aprobación manual**; un merge no publica por sí solo.
- Alternativa oficial: publicar `dist/` en una rama `deploy` y dejar que hPanel la sirva. Evita
  credenciales FTP, pero ensucia el historial y no permite borrar archivos obsoletos con
  facilidad. Se evalúa en fase 6.
- El repositorio es público hoy: los secretos de entorno no se exponen a PRs de forks, pero
  conviene revisar la visibilidad antes de activar despliegues.

### 6.7 Formularios y correo

- Si se aprueba formulario: endpoint PHP en `public/contacto.php`, SMTP autenticado del dominio,
  `From` fijo, `Reply-To` del visitante, honeypot, límite por IP y validación estricta.
- Si se usa servicio externo: revisar RGPD, encargado de tratamiento y transferencia internacional.
- Sin formulario: `tel:`, `mailto:` y `https://wa.me/34672659923` como enlaces reales.

## 7. Flujo de actualización de contenido mediante agentes

### 7.1 Actores

- **Coordinador:** interpreta la petición, prepara el paquete, revisa el resultado y decide.
- **Agente de contenido:** edita sólo contenido y medios en una rama; no publica.
- **CI:** valida tipos, esquema, imágenes, enlaces y build.
- **Revisor humano:** propietario del negocio para contenido; responsable técnico para el diff.
- **Job de despliegue:** sólo tras merge y aprobación del environment.

### 7.2 Flujo

1. El propietario solicita un cambio (nuevo proyecto, texto, fotos) por el canal acordado.
2. El coordinador confirma que existen **todos** los datos verificables y los entrega en un
   paquete: slug, textos, fotos, municipio, servicios, fecha.
3. El agente crea una rama `content/<slug>` desde `main` y edita el Markdown y las imágenes.
4. Ejecuta `npm run validate` (tipos, esquema, imágenes, enlaces, build).
5. Abre una Pull Request con resumen, fuente de cada dato, número de fotos y enlace de preview.
   **Se detiene aquí.**
6. CI construye y publica una preview en `staging`.
7. El propietario revisa en móvil y escritorio; el responsable técnico revisa el diff.
8. Un humano aprueba y hace merge. El agente no aprueba ni fusiona.
9. El job de producción espera aprobación del environment y despliega.
10. Se registra el cambio en el historial (tag `content-AAAA.MM.DD` y notas de release).

### 7.3 Validaciones automáticas

| Comprobación | Herramienta | Si falla |
| --- | --- | --- |
| Tipos y sintaxis Astro | `astro check` | Bloquea |
| Esquema de contenido | Zod + `astro sync` | Bloquea |
| Slug = archivo = URL; slugs únicos; rutas reservadas | `check-content.mjs` | Bloquea |
| Longitud de title/description; fechas; servicios válidos | `check-content.mjs` | Bloquea |
| Imágenes: formato, tamaño, dimensiones, EXIF GPS | `check-images.mjs` | Bloquea |
| Alt text obligatorio y no genérico | `check-images.mjs` | Bloquea |
| Enlaces internos y anclas | `check-links.mjs` sobre `dist/` | Bloquea |
| NAP coherente con `site.ts` | `check-content.mjs` | Bloquea |
| 301 y 404 | `check-redirects.mjs` | Bloquea |
| Ortografía española | `cspell` + diccionario es | Avisa (subir a bloqueo tras ajustar) |
| Build de producción | `astro build` | Bloquea |
| Fuga de drafts | grep sobre `dist/` | Bloquea |

### 7.4 El agente nunca

- Hace push a `main`, force-push, merge, aprueba PRs ni activa auto-merge.
- Despliega, purga caché, toca DNS, hosting, `.htaccess` o workflows.
- Edita el NAP, precios, garantías, textos legales o datos fiscales.
- Borra, renombra o cambia el slug de contenido existente.
- Inventa datos, ubicaciones, fechas, materiales, reseñas o fotos.
- Usa imágenes de stock o generadas como si fueran trabajos reales.
- Publica (`draft: false`) por su cuenta.
- Añade dependencias, scripts de analítica o formularios.
- Sube archivos directamente a Hostinger.

### 7.5 Previsualización

- **Recomendada:** subdominio `staging.oresteespinosacampos.es` en el mismo Hostinger, `noindex`,
  autenticación básica y despliegue manual o por `workflow_dispatch`. No afecta a producción.
- **Alternativa sin tocar Hostinger:** Cloudflare Pages gratuito conectado al repo, sólo para
  previews de PR. Añade una cuenta y una dependencia externa.
- **Mínimo viable:** build local + capturas; no sirve para que el propietario revise en su móvil.

## 8. Plan de migración de contenido y URLs

### 8.1 Exportación de sólo lectura

- Fuente: API REST de WordPress (`pages`, `posts`, `media`, `categories`) y HTML renderizado.
- No se instala nada en WordPress, no se modifican plugins ni temas, no se accede a la base de
  datos.
- Guardar la exportación en `migration/` **fuera del control de versiones** mientras el
  repositorio sea público. Al autorizar la implementación se añadirá `migration/` a `.gitignore`;
  esta fase no modifica `.gitignore`. Incluir un manifiesto de fecha y URLs.
- Repetir la exportación justo antes del corte para capturar cambios.

### 8.2 Reescritura

- Convertir HTML a Markdown limpiando shortcodes y clases de Elementor.
- Reescribir en español de España, con acentos, frases cortas y estructura escaneable.
- Crear las seis páginas de servicio a partir de una entrevista con el propietario: qué incluye,
  materiales, plazos, zona, garantía y proceso. **No inventar.**
- Convertir las cinco entradas en casos de estudio con ficha (municipio, tipo, servicios, fecha).
- Corregir «Malaga», «Sobre mi», «¿Que hacemos?», «concina» y demás erratas.
- Sustituir «Lorem ipsum» por contenido real o eliminar el bloque.
- Enlazar cada proyecto con los servicios usados y cada servicio con proyectos reales.

### 8.3 Medios

- Descargar originales; seleccionar los mejores; descartar duplicados y miniaturas.
- Renombrar de forma semántica (`reforma-bano-mijas-despues-01.jpg`).
- Eliminar metadatos EXIF/GPS por privacidad.
- Redimensionar a 2000 px máximo y generar AVIF/WebP en build.
- Comprimir o sustituir el vídeo de 12 MB.
- Pedir al propietario originales de mayor calidad y autorización escrita de uso.
- Obtener un logo vectorial (SVG) o reconstruirlo; el actual es un JPEG de 332×117.

### 8.4 Verificación

- Matriz de URLs: cada URL antigua responde 200 (conservada) o 301 (redirigida), sin cadenas.
- Rastreo de enlaces internos sin 404.
- Validador de datos estructurados (Schema Markup Validator / Rich Results Test).
- Lighthouse móvil ×3 y comparación con la línea base.
- Revisión ortográfica y de tono.
- Revisión visual del propietario en móvil y escritorio.
- Comprobación de accesibilidad con teclado y contraste.

### 8.5 Corte y rollback

1. Copia completa y verificada.
2. Publicación en staging y aprobación escrita.
3. Corte en producción en ventana de baja actividad.
4. Smoke test: portada, servicios, proyectos, contacto, 301, 404, sitemap, HTTPS, formulario.
5. Search Console: sitemap y comprobación de indexación.
6. Vigilancia 48 h; rollback si hay errores críticos.
7. WordPress se conserva 30 días fuera de `public_html` y luego se decide su retirada.

## 9. Fases y roadmap propuesto

### Fase 1 — Auditoría y planificación (actual)

Entregables: auditoría, inventario de URLs y medios, arquitectura propuesta, plan de hosting,
flujo de agentes, plan de migración, decisiones pendientes. **Completada como evidencia.**

### Fase 2 — Dos propuestas visuales (siguiente)

Objetivo: elegir una dirección antes de construir.

- **Propuesta A — «Artesano contemporáneo»**: neutros cálidos, verde profundo y terracota,
  tipografía editorial, fotografía grande, textura de oficio; transmite confianza y cercanía.
- **Propuesta B — «Precisión luminosa»**: blancos y piedra caliza, azul profundo, retícula clara,
  datos visibles (años, plazos, m²), estética mediterránea y técnica; transmite orden y rapidez.

Ambas se prototipan de verdad en Astro + Tailwind (no sólo imágenes): portada, una página de
servicio y un caso de estudio, en móvil y escritorio, con **el mismo contenido real** para que la
comparación sea justa. Entregables: dos URLs de preview, ficha comparativa, tokens de diseño y
decisión registrada. **Sin migración completa ni publicación.** Salida: dirección elegida por el
propietario.

Criterios de comparación, en este orden: claridad de la propuesta en 5 segundos, confianza y
cercanía, calidad percibida del trabajo, facilidad para contactar desde el móvil, diferenciación
frente a competidores locales, accesibilidad y coste de mantenimiento. Cada propuesta incluye
portada, página de servicio, caso de estudio, menú móvil, pie, botones y estados de foco.

La fase 2 no empieza hasta que el propietario confirme D3 y D4 y aporte el material mínimo
(textos verificados, fotos y logo). D1 sólo es necesario antes de la fase 3 (staging) y de la
fase 5 (publicación). Si falta material, se prototipa con contenido marcado como provisional y
se sustituye antes de la fase 4. D7 no es un requisito de entrada: **es la salida de la fase 2**.

### Fase 3 — Fundaciones y esqueleto

- Crear el proyecto Astro, Tailwind 4, TypeScript estricto, estructura y `site.ts`.
- Implementar layout, header, footer, SEO, JSON-LD, 404, sitemap y `.htaccess` base.
- Implementar el sistema visual de la dirección elegida y tres páginas reales.
- CI: `astro check`, build, enlaces y Lighthouse.
- Despliegue a staging y línea base de rendimiento.
- Salida: staging navegable, checks verdes, aprobación del esqueleto.

### Fase 4 — Contenido, SEO local y migración

- Seis páginas de servicio y cinco casos de estudio con contenido aprobado.
- Páginas legales, cookies y consentimiento.
- 301, sitemap, schema, Open Graph, enlazado interno.
- Scripts de validación de contenido e imágenes.
- Flujo de agentes probado con un cambio real.
- Salida: staging completo, matriz de URLs verde, contenido aprobado.

### Fase 5 — Publicación controlada

- Checklist de hPanel, copia, corte, HTTPS/HSTS, 301, caché, Search Console, GBP.
- Prueba de rollback y vigilancia 48 h.
- Salida: producción aprobada y verificada.

### Fase 6 — Automatización y traspaso

- GitHub Actions con environment protegido y despliegue aprobado.
- Documentación para el propietario y para agentes.
- Formulario, si se aprueba; analítica definitiva; mantenimiento.
- Salida: un cambio de contenido completo realizado por un agente y aprobado por un humano.

### Correspondencia con el roadmap de 4 epics (para aprobar en la etapa 6 de setup)

| Epic | Fases incluidas | Resultado observable |
| --- | --- | --- |
| 01 Fundaciones y sistema visual | 2–3 | Sitio base en staging con dirección visual aprobada |
| 02 Contenido, SEO local y migración | 4 | Contenido real migrado, URLs y schema verificados |
| 03 Publicación controlada en Hostinger | 5 | Web en producción con copia y rollback probados |
| 04 Operación con agentes y automatización | 6 | Agente propone, humano aprueba, CI publica |

## 10. Riesgos y mitigaciones

| Riesgo | Probabilidad | Impacto | Mitigación |
| --- | --- | --- | --- |
| Plan de Hostinger sin SSH/rsync | Media | Medio | Verificar D1; usar FTP/FTPS o rama `deploy` |
| Pérdida de URLs o SEO al migrar | Media | Alto | Inventario completo y probado, 301 verificados, staging, Search Console |
| Contenido insuficiente o no verificado | Alta | Alto | Entrevista estructurada; no inventar; publicar sólo lo confirmado |
| Fotos de baja calidad o sin derechos | Alta | Medio | Pedir originales y autorización escrita |
| Repositorio público con datos del cliente | Media | Medio | Decidir visibilidad; no commitear exportaciones ni datos personales |
| Dos propuestas visuales eternas | Media | Medio | Timebox de una iteración por propuesta y decisión registrada |
| Regresiones de rendimiento | Baja | Medio | Presupuesto y Lighthouse en CI |
| Caché LiteSpeed sirviendo versiones antiguas | Media | Bajo | Purga tras cada despliegue y verificación con `curl` |
| Formulario con spam o mala entregabilidad | Media | Medio | Sin formulario al lanzar; SMTP + honeypot + límite si se aprueba |
| Legal incompleto | Media | Alto | Páginas y consentimiento antes del lanzamiento; revisión humana |
| Dependencia de Astro 7 reciente | Baja | Medio | Fijar versiones, spike de 2 páginas, lockfile |
| WordPress roto durante el corte | Baja | Alto | Copia, staging, ventana de baja actividad, rollback probado |

## 11. Decisiones pendientes

| ID | Pregunta | Opciones | Recomendación | Bloquea |
| --- | --- | --- | --- | --- |
| D1 | ¿Qué plan de Hostinger hay y permite SSH? | Premium / Unlimited / otro | Verificar en hPanel; no fijar método hasta entonces | Despliegue |
| D2 | ¿El repositorio debe seguir siendo público? | Público / privado | Privado; si sigue público, prohibir datos personales y exportaciones | Contenido y secretos |
| D3 | ¿Se conserva `/blog/` o se redirige? | Conservar / 301 a `/trabajos/` | 301 si no habrá blog; conservar si habrá artículos | URLs y SEO |
| D4 | ¿Se aprueban seis páginas de servicio? | 4 / 6 / otra | Seis, con datos verificados | Contenido (fase 4 y textos de fase 2) |
| D5 | ¿Formulario de contacto? | Ninguno / PHP+SMTP / externo | Ninguno al lanzar; revisar en fase 6 | Alcance |
| D6 | ¿Analítica? | GA4 con consentimiento / sin cookies | GA4 con Consent Mode v2 o Cloudflare Analytics | Legal y rendimiento |
| D7 | ¿Dirección visual? | A / B | Decidir tras ver ambas propuestas | Fase 2 (salida) |
| D8 | ¿Quién redacta y aprueba los textos legales? | Propietario / asesor / coordinador | Revisión humana antes de publicar | Lanzamiento |
| D9 | ¿Fotos y logo originales? | Reutilizar / pedir originales | Pedir originales y autorización | Migración |
| D10 | ¿Reseñas y Google Business Profile? | Sí / no | Sí, sólo reseñas reales | SEO local |
| D11 | ¿Automatización de despliegue? | Ahora / después | Después del lanzamiento, con aprobación | Fase 6 |
| D12 | ¿Copias y retención? | Hostinger / externa | Backup manual + archivo local + artefactos CI | Publicación |

## 12. Anexo: checklist de aceptación de la fase 1

- [x] Web pública analizada (HTTP, sitemaps, API REST, Lighthouse, capturas).
- [x] Páginas, servicios, proyectos y medios inventariados.
- [x] Problemas de diseño, navegación, contenido y SEO identificados con evidencia.
- [x] Arquitectura Astro propuesta y justificada.
- [x] Flujo de contenido con agentes definido, con límites explícitos.
- [x] Plan de migración sin pérdida de URLs ni contenido.
- [x] Fases definidas, incluidas dos propuestas visuales para la fase 2.
- [x] Requisitos de Hostinger incorporados: estático, `public_html`, HTTPS, 301, copias y
      compatibilidad de formularios.
- [x] Sin desarrollo, sin cambios en WordPress, sin hosting, sin repositorios nuevos, sin merges.
- [ ] Aprobación humana de la fase 1 y de las decisiones D1–D4; D7 se decide en la fase 2.
