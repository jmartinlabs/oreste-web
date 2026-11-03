# Auditoría de la web actual — oresteespinosacampos.es

Estado: **evidencia para planificación**. No es una aprobación ni autoriza cambios.
Fecha de medición: 2026-11-03. Alcance: datos públicos (HTTP, sitemaps, API REST de WordPress,
Lighthouse, capturas y revisión manual). Sin acceso a hPanel, DNS, analítica ni base de datos.

## 1. Resumen ejecutivo

La web actual es un WordPress 6.8.12 con OceanWP + Elementor Pro, alojado en Hostinger
(servidor LiteSpeed + CDN de Hostinger), con **dos páginas, cinco proyectos y 34 archivos de
medios**. La mayor parte del valor está concentrado en la portada, que es una *one-page* con
anclas; no existen páginas de servicio ni páginas legales.

El problema no es el volumen de contenido (es pequeño y migrable en días), sino la **calidad de
la ejecución**: secciones visualmente rotas o vacías, textos de relleno, iconos equivocados,
contraste insuficiente y un rendimiento móvil de **57/100 con LCP de 14,8 s**. La auditoría
automática de SEO da 100/100, pero eso mide sólo lo que Lighthouse sabe comprobar: faltan
páginas de servicio, datos estructurados de negocio local, contenido legal y una arquitectura
de enlazado interno.

Conclusión: **la migración es viable y de bajo riesgo técnico**. El esfuerzo real está en
reescribir contenido y diseño, no en trasladar datos. La web nueva puede alcanzar 95–100 en
rendimiento móvil con Astro estático y mantener exactamente las URLs que hoy posicionan.

## 2. Inventario

### 2.1 Páginas y entradas publicadas

| Tipo | URL | Título | H1 | Observación |
| --- | --- | --- | --- | --- |
| Página | `/` | Oreste Pintura y mantenimiento, Mijas, Fuengirola, Marbella y Malaga. | 4 | One-page con anclas `#servicios`, `#trabajos`, `#about`, `#contacto` |
| Página | `/blog/` | Blog - Oreste Pintura y mantenimiento… | 0 | Índice de entradas; sin H1 |
| Entrada | `/remodelacion-integral-de-bano/` | Remodelación integral de baño | 0 | Baño; 4 fotos «después» + vídeo |
| Entrada | `/renovacion-total-de-chalet-pintura-y-limpieza-para-un-acabado-impecable/` | Renovación total de chalet… | 0 | Antes/después + vídeos |
| Entrada | `/renovacion-de-suelo-en-cocina-industrial-durabilidad-y-funcionalidad/` | Renovación de Suelo en Cocina Industrial… | 0 | 2 antes + 1 después + vídeos |
| Entrada | `/revitalizacion-exterior-transformacion-completa-de-la-fachada-limpieza-reparacion-y-pintura-en-blanco/` | Revitalización Exterior: Transformación Completa de la Fachada… | 1 | 7 fotos + vídeo |
| Entrada | `/rapida-reforma-azulejos-cocina/` | ¡Azulejos de Cocina de Nuevo en su Sitio!… | 0 | 2 fotos |

Además existen taxonomías públicas:

| URL | Tipo | Contenido |
| --- | --- | --- |
| `/category/trabajos/` | Archivo | 5 entradas |
| `/category/blog/` | Archivo | 2 entradas (las de baño y chalet) |

Y recursos técnicos: `/feed/`, `/comments/feed/`, `/wp-json/`, `/xmlrpc.php`, sitemaps de
AIOSEO. No hay páginas de aviso legal, privacidad o cookies.

### 2.2 Servicios declarados en la portada

| Bloque | Servicios | Estado del texto |
| --- | --- | --- |
| Pintura | Pintura interior y exterior; restauración de fachadas; reparación de paredes | Correcto |
| Revestimientos y superficies | Alicatado; solería | **«Lorem ipsum»** en el segundo ítem |
| Mantenimiento y reformas | Mantenimiento general; pequeñas reformas; fontanería y electricidad | Correcto |
| Renovación de baños y cocinas | Reforma integral de baños; renovación de cocinas | **«Lorem ipsum»** en el segundo ítem |

No hay páginas propias por servicio, ni descripción de proceso, materiales, garantías, plazos o
zona de cobertura. La única prueba de trabajo son los cinco proyectos.

### 2.3 Proyectos (casos de estudio)

| Proyecto | Fecha | Fotos en la entrada | Vídeo | Categorías |
| --- | --- | --- | --- | --- |
| Remodelación integral de baño | 2025-10-21 | 4 | 1 MP4 (12 MB) | blog, trabajos |
| Renovación total de chalet | 2025-10-21 | 2 | Sí (YouTube) | blog, trabajos |
| Renovación de suelo en cocina industrial | 2024-03-29 | 3 | Sí | trabajos |
| Revitalización exterior de fachada | 2023-12-12 | 7 | Sí | trabajos |
| Rápida reforma de azulejos de cocina | 2023-12-12 | 2 | No | trabajos |

En total, **18 imágenes incrustadas en las cinco entradas**. La biblioteca de medios contiene 33
imágenes; el resto son fotos subidas pero no usadas, variantes de logo e iconos. El inventario
detallado marca en qué entrada se usa cada archivo.

### 2.4 Fotografías y medios

- **33 imágenes + 1 vídeo**. Peso total: **19,0 MiB** (7,1 MiB de imágenes y 11,9 MiB del MP4).
- Resolución máxima: 2048 px de ancho; formatos JPEG/JPG.
- **19 de 33 imágenes sin texto alternativo** (57 %). De ellas, 13 son fotos de contenido y 6 son
  iconos o recortes de logo donde el alt vacío puede ser intencionado.
- Nombres de archivo inconsistentes: `reforma1-azulejos-concina-antes` (error «concina»),
  `despues4`, `antes2`, `cropped-cropped-cropped-temp-logo`.
- Logotipo: recortes de 512×512 y 332×117, con nombre «temp-logo»; no hay SVG ni versión
  vectorial.
- El vídeo de 12 MB se sirve sin poster ni compresión adaptativa.

Inventario detallado: [`asset-inventory.csv`](asset-inventory.csv).

## 3. Problemas detectados

### 3.1 Diseño y experiencia de usuario

1. **Logotipo invisible.** El CSS de OceanWP fija `#site-logo a.site-logo-text { font-size: 1px }`;
   la cabecera queda sin marca visible.
2. **Hero roto.** El H1 se superpone a la fotografía de la furgoneta; el texto pierde
   legibilidad y el conjunto no tiene jerarquía.
3. **Sección de servicios vacía.** El bloque «¿Que hacemos?» muestra un marco de imagen en
   blanco, iconos equivocados (logo de WordPress para «Trabajos», icono de código `</>` para
   «Contacto») y grandes espacios en blanco.
4. **Textos de relleno** («Lorem ipsum») visibles en producción.
5. **Ritmo visual incoherente**: bandas grises, oro y negro sin relación con la marca; tipografías
   Montserrat, Roboto y Roboto Slab mezcladas.
6. **Proyectos con contraste insuficiente** (texto claro sobre fondo oscuro) y miniaturas negras
   en «También podría gustarte».
7. **Páginas de proyecto rotas**: áreas de cientos de píxeles vacías donde deberían aparecer
   galerías y vídeos; barra lateral con buscador y entradas recientes; formulario de comentarios
   sin uso.
8. **Contacto pobre**: teléfono y correo son texto plano (no `tel:` ni `mailto:`), no hay
   dirección escrita, el mapa depende de un iframe con `data-src`, y no existe formulario.
9. **Botones «volver arriba» duplicados**: aparecen varios flotantes apilados en la misma vista.
10. **Navegación mínima**: cinco enlaces de 12–13 px, sin logo, sin menú de servicios, sin
    migas de pan y sin estado activo claro.

### 3.2 Contenido y mensaje

1. **Mezcla de idiomas**: «Home», «About me», «Read more» junto a contenido en español.
2. **Faltas y erratas**: «Malaga», «¿Que hacemos?», «Sobre mi», «Ultimos trabajos»,
   «ContacTO», «concina».
3. **Propuesta de valor difusa**: no se explica en 5 segundos qué se hace, para quién y dónde.
4. **Sin prueba social**: no hay reseñas, testimonios, años de experiencia, garantías ni
   certificaciones.
5. **Sin contenido local**: no se nombran barrios, municipios ni tipos de inmueble; la dirección
   sólo aparece dentro del mapa.
6. **Sin llamada a la acción fuerte**: dos botones compiten en el hero («Contacto» y «About me»)
   y no hay WhatsApp, que es el canal habitual del sector.
7. **Textos largos sin estructura** en «Sobre Oreste»; párrafos de 60–80 palabras sin
   subtítulos ni datos concretos.

### 3.3 SEO

Fortalezas actuales: canonical correcto, `lang="es"`, títulos únicos, sitemap XML, `robots.txt`
válido, HTTPS, 404 real y metadescripciones en las entradas.

Problemas:

1. **Sin páginas de servicio.** La única página que compite es la portada; no hay URLs para
   «pintor en Mijas», «reforma de baño en Fuengirola» o «pintura de fachadas en Marbella».
2. **Sin datos estructurados de negocio local.** AIOSEO emite `Person`, `WebPage` y `WebSite`,
   pero no `LocalBusiness`/`HousePainter` con NAP, horario, área servida y coordenadas.
3. **Jerarquía de encabezados rota.** Cuatro H1 en la portada; cero H1 en `/blog/` y en cuatro
   de las cinco entradas; H6 usado como título de sección y H3 para el correo.
4. **Duplicidad `/blog/` vs `/category/blog/`**: mismo título («Blog»), metadescripción en inglés
   («Your blog category») y contenido solapado.
5. **Sin imagen Open Graph** en portada y blog; al compartir no hay tarjeta visual.
6. **Enlace interno roto**: el menú apunta a `/blog/%20` (espacio codificado), que responde 301
   sobre sí mismo.
7. **Cadena de redirecciones**: `http://oresteespinosacampos.es` → `https://…` → `https://www.…`
   (dos saltos). Debería resolverse en uno.
8. **Sitemap incompleto**: `/blog/` aparece en el sitemap de entradas, no en el de páginas;
   `/category/blog/` está indexable sin valor.
9. **Contenido duplicado potencial** entre la portada y las entradas, que repiten la misma
   introducción.
10. **Sin enlazado interno contextual**: las entradas no enlazan a servicios ni a otros
    proyectos; sólo existe el bloque automático «También podría gustarte».

### 3.4 Rendimiento (Lighthouse 13.6.0, móvil, 2026-11-03)

| Métrica | Valor |
| --- | --- |
| Rendimiento | **57 / 100** |
| First Contentful Paint | **8,0 s** |
| Largest Contentful Paint | **14,8 s** |
| Speed Index | 8,0 s |
| Total Blocking Time | 0 ms |
| Cumulative Layout Shift | 0 |
| Peso total | **3.182 KB** |
| Accesibilidad | 91 / 100 |
| Buenas prácticas | 73 / 100 |
| SEO (Lighthouse) | 100 / 100 |

Causas concretas:

- **30 hojas de estilo y 26 scripts**. Entre ellos, jQuery, jQuery Migrate, tres familias de
  Google Fonts, Font Awesome completo (100 KB) y Simple Line Icons.
- Un único CSS de Elementor Pro de **440 KB**; `style.min.css` de OceanWP, 176 KB;
  `thepostgrid.min.css`, 154 KB.
- **Dos instalaciones de Google Analytics cargando a la vez**: MonsterInsights
  (`G-3ZJY6BD4JP`) y Site Kit (`GT-NSSXTXN`). Cada una inyecta su propio `gtag.js` y su
  configuración; la portada carga ambas.
- 1.660 ms de recursos bloqueantes de render; 229 KB de JavaScript sin usar y 101 KB de CSS sin
  usar.
- Imágenes sin `loading="lazy"` en 6 de 10 etiquetas de la portada y vídeo de 12 MB.
- Caché de navegador insuficiente (102 KB con vida corta).

### 3.5 Accesibilidad

- **Contraste insuficiente** en varias secciones (fallo Lighthouse `color-contrast`).
- **Orden de encabezados** no descendente (`heading-order`).
- **Enlaces sin nombre accesible** (`link-name`): iconos sin texto alternativo ni `aria-label`
  útil.
- Navegación de 12–13 px y objetivos táctiles pequeños.
- Animaciones de entrada de Elementor sin respeto a `prefers-reduced-motion`.
- Existe enlace «Saltar al contenido» (positivo), pero el foco no se refuerza de forma visible.
- Teléfono y correo no son enlaces, lo que penaliza a usuarios de lector de pantalla y a móvil.

### 3.6 Cumplimiento y seguridad

1. **No existen aviso legal, política de privacidad ni política de cookies.**
2. **Google Analytics se carga antes de cualquier consentimiento**, con cookies de terceros
   (2 detectadas por Lighthouse). Riesgo claro frente a la Guía de cookies de la AEPD y el
   RGPD/LOPDGDD para una web comercial española.
3. **Superficie de WordPress expuesta**: versión en el HTML, `/wp-json/`, `/xmlrpc.php`,
   comentarios abiertos, Elementor Pro, AIOSEO, MonsterInsights, Site Kit, EWWW y Post Grid.
   No es un fallo explotable por sí mismo, pero es deuda de mantenimiento.
4. **Cabeceras de seguridad ausentes**: no hay `Strict-Transport-Security`,
   `X-Content-Type-Options`, `X-Frame-Options` ni `Referrer-Policy`.
5. HTTPS correcto, con redirección desde HTTP y certificado válido.

### 3.7 Alojamiento observado

- `platform: hostinger`, `panel: hpanel`, `server: hcdn`, `x-litespeed-cache: hit`,
  `x-powered-by: PHP/8.2.34 o 8.2.35 según la respuesta`, HTTP/2 y HTTP/3 (`alt-svc: h3`).
- **Plan exacto desconocido.** Es el dato que falta para fijar el método de despliegue
  (SSH, backups, Git, subdominios y límites varían entre planes).
- **Compatibilidad de Astro estático no verificada.** La web actual sirve assets estáticos desde
  Hostinger, pero no se puede confirmar que el plan contratado cumpla todos los requisitos hasta
  revisar hPanel. La matriz y el procedimiento están en
  [`../architecture/hosting-and-deployment.md`](../architecture/hosting-and-deployment.md).
- El dominio responde en `www`; el ápice redirige a `www`.

## 4. Oportunidades de la reconstrucción

1. **Seis páginas de servicio** con intención local real, enlazadas desde la portada y desde
   cada proyecto.
2. **Datos estructurados `LocalBusiness` + `Service` + `BreadcrumbList`** y NAP idéntico en web,
   Google Business Profile y directorios.
3. **Casos de estudio con narrativa antes/después**, galería optimizada y ficha técnica
   (municipio, tipo de inmueble, duración, servicios).
4. **Rendimiento 95–100 móvil** con HTML estático, CSS crítico mínimo, imágenes AVIF/WebP y
   cero JavaScript innecesario.
5. **Conservación de URLs** de portada y de los cinco proyectos; `/blog/` y las categorías se
   redirigen con 301 a `/trabajos/` (D3 resuelta; redirecciones planificadas, no aplicadas).
6. **Legalidad desde el día uno**: aviso legal, privacidad, cookies y consentimiento previo.
7. **Canal directo**: botón de llamada y WhatsApp visibles en móvil.

## 5. Método y reproducibilidad

Evidencia conservada en [`evidence/`](evidence/):

- [`lighthouse-mobile-summary.json`](evidence/lighthouse-mobile-summary.json): puntuaciones,
  métricas y auditorías fallidas de Lighthouse 13.6.0.
- [`http-headers.txt`](evidence/http-headers.txt): cabeceras y redirecciones observadas.
- [`url-matrix.txt`](evidence/url-matrix.txt): estado de las URLs principales y técnicas.
- [`home-2026-11-03.jpeg`](evidence/home-2026-11-03.jpeg) y
  [`proyecto-bano-2026-11-03.jpeg`](evidence/proyecto-bano-2026-11-03.jpeg): capturas de la
  portada y de una entrada.

Método:

- Cabeceras y redirecciones: `curl -I` sobre ápice, `www`, HTTP/HTTPS y 404.
- Inventario: `robots.txt`, `sitemap_index.xml`, `post-sitemap.xml`, `page-sitemap.xml`,
  `attachment-sitemap.xml`, `category-sitemap.xml` y API REST `/wp-json/wp/v2/{pages,posts,media,categories}`.
  Las páginas de adjunto responden 301 al archivo de imagen, no son páginas indexables.
- Rendimiento y accesibilidad: Lighthouse 13.6.0, emulación móvil por defecto, **una ejecución**
  sobre la portada. Los valores son indicativos; la comparación final se hará con tres
  ejecuciones sobre la web nueva.
- Capturas revisadas visualmente a ancho de escritorio.
- No se ha accedido a hPanel, DNS, Search Console, Analytics ni base de datos. Cualquier
  afirmación sobre el plan contratado, el tráfico o las conversiones queda fuera de alcance.

## 6. Consecuencias para el plan

1. El contenido es **suficiente para lanzar** si se reescribe; no hace falta inventar proyectos.
2. Hay que **pedir al cliente**: textos definitivos, fotos originales de mayor calidad, logo
   vectorial, datos de contacto verificados, horario, zona de servicio y reseñas reales.
3. La migración de WordPress es **sólo de lectura** y se puede hacer por API REST; no se toca
   la instalación actual.
4. El despliegue en Hostinger es compatible con Astro estático en principio, pero **requiere
   verificar el plan** (D1) y probar rutas, 301, MIME y SSL de subdominio en staging antes de
   fijar el método. Ver [`../architecture/hosting-and-deployment.md`](../architecture/hosting-and-deployment.md).
5. La fase 2 debe resolver **dos direcciones visuales** antes de construir componentes
   definitivos.
