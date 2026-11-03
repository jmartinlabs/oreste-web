# Plan de reconstrucción — Fase 1

Estado: **PROPUESTA para revisión**. No autoriza desarrollo, despliegue ni cambios en WordPress.
Fecha: 2026-11-03; actualizado tras confirmar Hostinger como destino definitivo.

Documentos de detalle:

- Arquitectura de hosting y despliegue: [`architecture/hosting-and-deployment.md`](architecture/hosting-and-deployment.md)
- Migración desde WordPress: [`migration/wordpress-to-astro.md`](migration/wordpress-to-astro.md)
- Auditoría y evidencia: [`audit/current-site-audit.md`](audit/current-site-audit.md)

## 0. Resumen

La web nueva será una **web corporativa estática** construida con Astro, Tailwind CSS y
TypeScript, publicada en el **Hostinger compartido existente**, sin Node.js en producción y sin
CMS. El contenido vivirá en archivos Markdown validados por esquema, de modo que un agente pueda
proponer cambios mediante una rama y una Pull Request, y sólo un humano pueda aprobarlos y
publicarlos.

La estrategia es deliberadamente pequeña: **una portada, seis páginas de servicio, un hub de
trabajos, cinco casos de estudio, contacto y tres páginas legales**. Se conservan exactamente la
portada y los cinco slugs de proyecto; `/blog/` y los archivos de categoría se redirigen con 301
a `/trabajos/` (D3 resuelta). La publicación se probará primero en un subdominio de
preproducción y después en `public_html`, con copia previa y rollback probado. No se toca
WordPress hasta la fase de publicación y sólo con autorización expresa.

### Requisitos incorporados en la actualización del 2026-11-03

| Requisito | Dónde se resuelve |
| --- | --- |
| 1. Compatibilidad de Astro estático con el plan actual | [`architecture/hosting-and-deployment.md`](architecture/hosting-and-deployment.md) §1–§4; D1 |
| 2. Entorno de preproducción por subdominio | Arquitectura de hosting §5; D13 |
| 3. Procedimiento de `dist/` en `public_html` | Arquitectura de hosting §6–§7 |
| 4. Migración segura con copia, URLs, 301 y rollback | [`migration/wordpress-to-astro.md`](migration/wordpress-to-astro.md) §3–§10 |
| 5. Publicación futura con GitHub Actions y aprobación | Arquitectura de hosting §10; fase 6 |

Todo lo que depende del plan real queda marcado como **pendiente**; no se asume ninguna
capacidad. No se ha modificado WordPress, DNS ni Hostinger, y no se ha desplegado nada.

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
| Preproducción | Subdominio aislado, `noindex`, autenticación básica y SSL verificado |
| Migración | Copia verificada, 301 sin cadenas, restauración probada y WordPress conservado |
| Hosting | `dist/` se sirve desde Hostinger compartido sin Node; HTTPS y 301 verificados en staging y producción |

## 2. Principios

1. **Ligero por defecto.** Sin framework de UI, sin estado global, sin base de datos, sin SSR.
2. **Cero JavaScript innecesario.** Navegación y contenido funcionan sin JS; las islas se
   justifican una por una.
3. **Contenido como código.** Markdown + esquema Zod + validación en CI; revisable en Git.
4. **Conservar antes que reescribir.** Las URLs y el contenido real mandan; el diseño se adapta.
5. **Aprobación humana explícita.** Ningún agente publica, borra, renombra ni despliega.
6. **Una sola fuente de verdad.** NAP en `src/data/site.ts`; URLs en el frontmatter; decisiones
   en `specs/`.
7. **Nada se asume sobre el hosting.** Cada capacidad se marca como observada, documentada o
   pendiente; si falta información, se documenta y se espera a verificarla.
8. **Compatible con Hostinger.** Nada que requiera Node, procesos persistentes o base de datos.

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
- El servidor debe servir `index.html` como índice de directorio; se verifica en el spike de
  compatibilidad. Las URLs antiguas con barra final se conservan sin reescrituras.
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

// Alineado con specs/content/service-pages-structure.md
const servicios = defineCollection({
  loader: glob({ base: './src/content/servicios', pattern: '*.md' }),
  schema: ({ image }) => z.object({
    title: z.string().min(20).max(65),
    slug: z.string().regex(/^[a-z0-9-]+$/),
    description: z.string().min(120).max(165),
    order: z.number().int().default(99),
    summary: z.string().min(80).max(220),
    intro: z.string().min(80).max(400),
    includes: z.array(z.string().min(10)).min(3).max(8),
    process: z.array(z.object({
      step: z.string().min(3),
      detail: z.string().min(10),
    })).min(3).max(5),
    materials: z.string().max(400).optional(),
    serviceArea: z.array(z.string().min(3)).min(1),
    warranty: z.string().max(200).optional(),
    cover: image(),
    coverAlt: z.string().min(20),
    benefits: z.array(z.string().min(10)).min(3).max(6),
    gallery: z.array(z.object({ src: image(), alt: z.string().min(15) })).max(8).default([]),
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

La estructura concreta de cada página, los bloques obligatorios y los datos que necesitan
confirmación están en [`content/service-pages-structure.md`](content/service-pages-structure.md).
No se inventa ningún dato: lo desconocido se marca como `[confirmar]`.

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
| Servicio externo de formularios | Requiere cuenta y servicio nuevo | — | Dependencia y transferencia de datos | **No permitido en esta fase** por la restricción de no contratar servicios adicionales |
| Formulario sólo JavaScript | No fiable | — | Usuarios sin JS, spam | Descartada |

Decisión propuesta: **lanzar sin formulario** y medir llamadas/WhatsApp. Si se aprueba un
formulario, hacerlo en fase 6 con endpoint PHP + SMTP, honeypot, límite por IP, validación en
servidor, consentimiento RGPD y correo `Reply-To` del visitante. Nunca subida de archivos y
ningún servicio externo.

Otras funcionalidades: galería con lightbox (isla opcional), mapa de Google embebido sólo tras
consentimiento o como enlace estático, y ningún chat en vivo.

### 4.8 Analítica y consentimiento

- Situación actual: dos etiquetas GA4 cargando a la vez, sin banner ni consentimiento.
- Propuesta: **una sola** propiedad GA4 con Consent Mode v2 y banner ligero que bloquee
  analítica hasta la aceptación; o, si el propietario prefiere cero cookies, renunciar a la
  analítica y trabajar sólo con Search Console. **No se añaden servicios nuevos.**
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
- Repositorio público (D2): no commitear secretos, credenciales, exportaciones ni datos
  personales. Revisar cada cambio antes de publicarlo.
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
| `/feed/` | `410 Gone` (o `/feed.xml` si se aprueba RSS) | Feed nuevo |
| `/comments/feed/`, `/wp-json/`, `/xmlrpc.php` | `410 Gone` | Ya no existen |

D3 resuelta: `/trabajos/` será la sección; `/blog/` y sus categorías se redirigen con 301 y las
URLs individuales de los proyectos se conservan. **No se aplica ninguna redirección en fase 1 ni
en fase 2**: se implementarán y probarán en la fase 5. Las redirecciones se implementan en
`.htaccess` y se prueban con `scripts/check-redirects.mjs` contra una lista cerrada.

Inventario completo: [`audit/url-inventory.csv`](audit/url-inventory.csv). Los medios heredados
se rigen por [`audit/media-url-policy.csv`](audit/media-url-policy.csv) y las páginas de adjunto
por [`audit/attachment-url-matrix.csv`](audit/attachment-url-matrix.csv).

## 6. Hosting, preproducción y despliegue

El detalle operativo vive en [`architecture/hosting-and-deployment.md`](architecture/hosting-and-deployment.md).
Resumen:

- **Compatibilidad:** Astro estático sólo necesita servir archivos; no requiere Node, base de
  datos ni procesos persistentes. La web actual ya sirve CSS, JS e imágenes desde Hostinger,
  pero **el plan exacto no está verificado**. No se asume ninguna capacidad: la matriz marca
  cada requisito como observado, documentado o pendiente de comprobar en hPanel (D1).
- **Preproducción:** subdominio `staging.oresteespinosacampos.es`, directorio aislado, SSL,
  `noindex` y autenticación básica (SSL pendiente de D1; ver
  [`architecture/hosting-and-deployment.md`](architecture/hosting-and-deployment.md) §5). No se
  crea en la fase 1; implica DNS y requiere autorización.
- **Despliegue:** artefacto `dist/` validado, publicación primero en staging y después en
  `public_html`, con copia previa y ventana de baja actividad. Sólo se sube el contenido de
  `dist/`, nunca fuentes, `node_modules`, `specs/` ni `.git`.
- **HTTPS y 301:** un solo salto a `https://www.oresteespinosacampos.es/`; redirecciones en
  `.htaccess`; HSTS sólo después de verificar todos los subdominios.
- **Copias y rollback:** backup de Hostinger + archivo fuera de `public_html` + exportación de la
  base de datos; restauración probada; WordPress conservado 30 días.
- **Automatización (fase 6):** GitHub Actions con environment `production`, revisor obligatorio
  y artefacto reversible; sólo si SSH/SFTP está disponible y el repositorio lo permite.
- **Formularios:** sin formulario o PHP+SMTP; no se contrata ningún servicio adicional.

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
6. CI construye el artefacto. La preview en staging se despliega bajo demanda con un workflow
   autorizado desde la rama, nunca automáticamente desde PRs de forks.
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
| Matriz de 301 y reglas de `.htaccess` | `check-redirects.mjs` (estático, sin red) | Bloquea |
| Redirecciones y 404 reales | `curl` o script HTTP en staging, tras desplegar | Bloquea antes de producción |
| Ortografía española | `cspell` + diccionario es | Avisa (subir a bloqueo tras ajustar) |
| Build de producción | `astro build` | Bloquea |
| Fuga de drafts | grep sobre `dist/` | Bloquea |

`.htaccess` no se ejecuta en `dist/`: la matriz y la configuración se validan estáticamente en CI,
pero las pruebas HTTP reales de 301/404 se hacen en staging Hostinger y, de nuevo, tras el corte.

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
- **Alternativa si no hay subdominio:** build local con `astro preview` y capturas, o un
  directorio temporal protegido con `noindex`. No se añaden servicios externos de hosting.
- **Mínimo viable:** build local + capturas de móvil y escritorio; suficiente para elegir la
  dirección visual en fase 2, no para aprobar el contenido definitivo del propietario.

## 8. Migración desde WordPress

El detalle vive en [`migration/wordpress-to-astro.md`](migration/wordpress-to-astro.md). Resumen:

- Exportación de sólo lectura por la API REST; WordPress no se modifica ni se actualiza.
- Copia completa (archivos + base de datos) fuera de `public_html`, verificada y con prueba de
  restauración antes del corte.
- Conservación de la portada, los cinco slugs de proyecto y las 34 URLs de medios heredados;
  301 para `/blog/`, categorías, adjuntos, feeds y sitemaps.
- Reescritura y medios en la fase 4; verificación con matriz de URLs, enlaces, schema y
  Lighthouse.
- Corte en ventana de baja actividad, smoke test, Search Console y vigilancia 48 h.
- Rollback documentado y probado; WordPress se conserva 30 días y no se borra sin autorización.

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

No serán variantes de color: difieren en paleta, tipografía, retícula, tono y jerarquía visual. El
propietario elige entre ambas antes de que se implemente el diseño definitivo.

Ambas se prototipan de verdad en Astro + Tailwind (no sólo imágenes): portada, una página de
servicio y un caso de estudio, en móvil y escritorio, con **el mismo contenido real** para que la
comparación sea justa. Entregables: dos prototipos ejecutables y comparables, ficha comparativa,
tokens de diseño y decisión registrada. La previsualización será local (build + capturas) salvo
que D1/D13 ya estén autorizados y exista staging; no se crea infraestructura nueva para la fase 2.
**Sin migración completa ni publicación.** Salida: dirección elegida por el propietario.

Criterios de comparación, en este orden: claridad de la propuesta en 5 segundos, confianza y
cercanía, calidad percibida del trabajo, facilidad para contactar desde el móvil, diferenciación
frente a competidores locales, accesibilidad y coste de mantenimiento. Cada propuesta incluye
portada, página de servicio, caso de estudio, menú móvil, pie, botones y estados de foco.

La fase 2 **no ha comenzado** y requiere autorización explícita del propietario. D3 está
resuelta y D4 tiene la estructura preparada en
[`content/service-pages-structure.md`](content/service-pages-structure.md): no hay que esperar a
D1/D13, que sólo son necesarias antes de la fase 3 (staging). El material que falte se marca como
provisional. D7 no es un requisito de entrada: **es la salida de la fase 2**.

### Fase 3 — Fundaciones y esqueleto

- Verificar D1 en hPanel (sólo lectura) y registrar la matriz de compatibilidad real.
- Con autorización, crear el subdominio de preproducción (D13), su SSL y su protección.
- Crear el proyecto Astro, Tailwind 4, TypeScript estricto, estructura y `site.ts`.
- Implementar layout, header, footer, SEO, JSON-LD, 404, sitemap y `.htaccess` base.
- Implementar el sistema visual de la dirección elegida y tres páginas reales.
- Ejecutar el spike de compatibilidad: rutas con barra final, 301, 404, MIME, HTTPS y caché.
- CI: `astro check`, build, enlaces y Lighthouse.
- Despliegue a staging y línea base de rendimiento.
- Salida: staging navegable, checks verdes, matriz de compatibilidad cumplida y aprobación del
  esqueleto.

### Fase 4 — Contenido, SEO local y migración

- Seis páginas de servicio y cinco casos de estudio con contenido aprobado.
- Páginas legales, cookies y consentimiento.
- Reglas 301 preparadas y validadas en staging, sin activar en producción; sitemap, schema,
  Open Graph y enlazado interno.
- Scripts de validación de contenido e imágenes.
- Flujo de agentes probado con un cambio real.
- Salida: staging completo, matriz de URLs verde, contenido aprobado.

### Fase 5 — Publicación controlada

- **Despliegue manual verificable** con el runbook de hosting; la automatización no es requisito.
- Checklist de hPanel, copia completa, restauración de prueba y exportación final.
- Corte con el runbook, HTTPS/HSTS, activación de los 301 en producción, caché, Search Console y
  Google Business Profile.
- Prueba de rollback y vigilancia 48 h.
- Salida: producción aprobada, verificada y reversible.

### Fase 6 — Automatización y traspaso

- GitHub Actions con environment protegido y despliegue aprobado, sólo después de que el
  despliegue manual esté probado y si D1 confirma SSH/SFTP.
- Documentación para el propietario y para agentes.
- Formulario, si se aprueba; analítica definitiva; mantenimiento.
- Salida: un cambio de contenido completo realizado por un agente y aprobado por un humano.

### Correspondencia con el roadmap de 4 épicas (para aprobar en la etapa 6 de setup)

| Épica | Fases incluidas | Resultado observable |
| --- | --- | --- |
| 01 Fundaciones y sistema visual | 2–3 | Sitio base en staging con dirección visual aprobada |
| 02 Contenido, SEO local y migración | 4 | Contenido real migrado, URLs y schema verificados |
| 03 Publicación controlada en Hostinger | 5 | Web en producción con copia y rollback probados |
| 04 Operación con agentes y automatización | 6 | Agente propone, humano aprueba, CI publica |

## 10. Riesgos y mitigaciones

| Riesgo | Probabilidad | Impacto | Mitigación |
| --- | --- | --- | --- |
| Plan de Hostinger sin SSH/rsync | Media | Medio | Verificar D1; usar SFTP/FTPS o despliegue manual |
| Plan sin subdominios o sin SSL de subdominio | Baja | Medio | Verificar D1/D13; previsualización local y ZIP como alternativa |
| Cambios accidentales de DNS o MX | Baja | Alto | Prohibidos en fase 1; checklist y autorización en fase 5 |
| Pérdida de URLs o SEO al migrar | Media | Alto | Inventario completo y probado, 301 verificados, staging, Search Console |
| Contenido insuficiente o no verificado | Alta | Alto | Entrevista estructurada; no inventar; publicar sólo lo confirmado |
| Fotos de baja calidad o sin derechos | Alta | Medio | Pedir originales y autorización escrita |
| Repositorio público con datos del cliente | Media | Medio | D2 resuelta: no commitear exportaciones, backups ni datos personales |
| Dos propuestas visuales eternas | Media | Medio | Límite de tiempo de una iteración por propuesta y decisión registrada |
| Regresiones de rendimiento | Baja | Medio | Presupuesto y Lighthouse en CI |
| Caché LiteSpeed sirviendo versiones antiguas | Media | Bajo | Purga tras cada despliegue y verificación con `curl` |
| Formulario con spam o mala entregabilidad | Media | Medio | Sin formulario al lanzar; SMTP + honeypot + límite si se aprueba |
| Legal incompleto | Media | Alto | Páginas y consentimiento antes del lanzamiento; revisión humana |
| Dependencia de Astro 7 reciente | Baja | Medio | Fijar versiones, spike de 2 páginas, lockfile |
| WordPress roto durante el corte | Baja | Alto | Copia, staging, ventana de baja actividad, rollback probado |

## 11. Decisiones: estado

Resolución parcial del 2026-11-03. Ninguna decisión pendiente bloquea la fase 2; D1/D13 sólo son
necesarias antes del staging y las demás se resolverán cuando toque.

| ID | Estado | Decisión | Bloquea |
| --- | --- | --- | --- |
| D1 | Parcial | Hostinger definitivo; verificar plan, SSH y backups antes de staging; no bloquear diseño | Staging y despliegue; no el diseño |
| D2 | Resuelta | Repositorio público, sin secretos, credenciales, backups ni contenido privado | Nada |
| D3 | Resuelta en planificación | `/trabajos/` + 301 desde `/blog/`; conservar URLs individuales; no aplicar aún | Nada; implementación en fase 5 |
| D4 | Parcial | Estructura de seis páginas preparada; datos por confirmar sin inventar | Contenido final (fase 4); fase 2 con marcadores provisionales |
| D5 | Pendiente | Formulario: ninguno o PHP+SMTP | Fase 6 |
| D6 | Pendiente | Analítica: GA4 con consentimiento o sin analítica | Lanzamiento |
| D7 | Parcial | Dos propuestas visuales diferenciadas; elección antes del diseño definitivo | Fase 2 (salida) |
| D8 | Pendiente | Contenido legal y responsable del tratamiento | Lanzamiento |
| D9 | Pendiente | Fotos, vídeo y logo originales | Migración |
| D10 | Pendiente | Reseñas y Google Business Profile | Contenido local |
| D11 | Resuelta | Despliegue manual verificable primero; automatización después | Fase 6 |
| D12 | Pendiente | Copias y retención | Publicación |
| D13 | Pendiente | Subdominio y autorización DNS | Staging |
| D14 | Pendiente | Ventana de corte y aprobador | Publicación |
| D15 | Pendiente | Método final de despliegue | Fase 6 |

## 12. Anexo: checklist de aceptación de la fase 1

- [x] Web pública analizada (HTTP, sitemaps, API REST, Lighthouse, capturas).
- [x] Páginas, servicios, proyectos y medios inventariados.
- [x] Problemas de diseño, navegación, contenido y SEO identificados con evidencia.
- [x] Arquitectura Astro propuesta y justificada.
- [x] Flujo de contenido con agentes definido, con límites explícitos.
- [x] Plan de migración sin pérdida de URLs con valor; el resto se redirige con 301 según D3,
      planificado pero no aplicado.
- [x] Fases definidas, incluidas dos propuestas visuales para la fase 2.
- [x] Requisitos de Hostinger incorporados: estático, `public_html`, HTTPS, 301, copias y
      compatibilidad de formularios.
- [x] Compatibilidad de Astro estático analizada; capacidades del plan marcadas como pendientes
      de D1 en lugar de asumidas.
- [x] Preproducción por subdominio diseñada, sin crearla.
- [x] Runbook de `dist/` → `public_html` y rollback documentados; se probará en fase 5.
- [x] Publicación automatizada diseñada con aprobación humana, sin implementarla.
- [x] Decisiones D1–D4, D7 y D11 registradas; D2, D3 y D11 resueltas.
- [x] Estructura de servicios preparada sin inventar datos (D4).
- [x] Hallazgos P1/P2 de la revisión 0004 corregidos en documentación.
- [ ] Nueva revisión de la PR #1 tras las correcciones.
- [x] Sin desarrollo, sin cambios en WordPress/DNS/Hostinger, sin servicios nuevos, sin
      repositorios nuevos, sin despliegues ni merges.
- [ ] Autorización explícita para iniciar la fase 2 (pendiente).
