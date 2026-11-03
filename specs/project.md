# Proyecto: Oreste Web

Estado: **borrador de trabajo**. La identidad y el alcance requieren aprobación explícita.
Fecha: 2026-11-03. Origen de los datos: instrucciones del usuario, auditoría de la web pública
y research técnico delegado.

## 1. Identidad

**Nombre del proyecto:** Oreste Web (reconstrucción de `oresteespinosacampos.es`).

**Negocio:** Oreste Espinosa Campos — pintura, reformas y mantenimiento general en Mijas,
Fuengirola, Marbella y resto de la provincia de Málaga.

**Objetivo:** sustituir la web actual de WordPress por una web corporativa estática, rápida,
responsive y fácil de actualizar mediante agentes de IA, conservando el dominio, las URLs con
valor y la visibilidad local.

**Usuarios:**

1. **Cliente particular** de la Costa del Sol que busca pintor, albañil o reformista y decide
   desde el móvil, muchas veces por llamada o WhatsApp.
2. **Propietario de vivienda o negocio** (chalet, cocina industrial, local) que necesita una
   reforma concreta y compara presupuestos.
3. **Oreste** (propietario del negocio), que quiere enseñar trabajos reales y recibir contactos
   sin depender de una agencia ni de un CMS.
4. **Agente de contenido** (futuro), que actualizará textos y proyectos mediante archivos y
   revisión humana.

**Problema:** la web actual funciona como escaparate, pero está visualmente rota en secciones
clave, tiene contenido de relleno, registra un LCP de 14,8 s en una medición Lighthouse móvil
(2026-11-03), no tiene páginas de servicio ni contenido legal, y no transmite la calidad del
trabajo real. El negocio depende de un WordPress con plugins que nadie mantiene.

**Propuesta de valor de la web nueva:** «Trabajos reales, bien explicados y fáciles de
contratar, en Mijas y la Costa del Sol».

## 2. Alcance

### Dentro del alcance

- Renovación visual completa con dos propuestas en la fase 2.
- Migración y reescritura del contenido real (servicios, proyectos, fotos).
- Arquitectura Astro estática, Tailwind CSS y TypeScript.
- SEO local, datos estructurados, sitemap, canonical y conservación de URLs.
- Páginas legales y consentimiento de cookies.
- Publicación en la misma cuenta y dominio de Hostinger, con preproducción por subdominio,
  copia de seguridad, 301 y rollback probado.
- Procedimiento manual de publicación y estudio de automatización posterior con aprobación.
- Flujo de actualización de contenido mediante agentes, sin CMS.

### Fuera del alcance (por ahora)

- Desarrollo de la web nueva durante la fase 1.
- Cualquier cambio en la instalación de WordPress actual.
- Configuración de hosting, DNS o dominio durante la fase 1.
- Contratación de servicios adicionales.
- Creación de repositorios remotos, cambios de visibilidad o merges sin autorización.
- Modificación de Agentic Template (skills, metodología, scripts, workflows).
- CMS, base de datos, SSR, Node.js en producción, área privada o e-commerce.
- Gestión de campañas, redes sociales o email marketing.

## 3. Restricciones y decisiones confirmadas

Fuente: instrucciones del usuario, 2026-11-03. Se registran aquí; tras la aprobación, el detalle
técnico se trasladará a `guidelines/architecture.md`.

| Decisión | Estado | Fuente |
| --- | --- | --- |
| Astro + Tailwind CSS + TypeScript | Confirmada | Usuario |
| `output: 'static'`; sin Node.js en producción | Confirmada | Usuario |
| Hosting definitivo: **la misma cuenta** de Hostinger y el **mismo dominio** | Confirmada | Usuario, 2026-11-03 |
| Dominio: `oresteespinosacampos.es` (con `www`) | Confirmada | Usuario / web actual |
| Sin cambios de DNS, SSL ni configuración de Hostinger en fase 1 | Confirmada | Usuario, 2026-11-03 |
| Sin contratar servicios adicionales | Confirmada | Usuario, 2026-11-03 |
| Compatibilidad del plan no asumida: lo no comprobado queda pendiente | Confirmada | Usuario, 2026-11-03 |
| Sin CMS; contenido en archivos gestionado por agentes | Confirmada | Usuario |
| Publicación sólo tras aprobación humana | Confirmada | Usuario |
| No tocar WordPress hasta autorizar la migración | Confirmada | Usuario |
| Implementación ligera, adecuada a una web corporativa pequeña | Confirmada | Usuario |
| Repositorio: `jmartinlabs/oreste-web` | Confirmada | Usuario; `origin` verificado 2026-11-03 |
| Repositorio **público**; sin secretos, credenciales, backups ni contenido privado (D2) | Confirmada | Usuario, 2026-11-03 |
| Hostinger definitivo; plan, SSH y backups se verifican antes de staging y no bloquean diseño (D1, parcial) | Confirmada | Usuario, 2026-11-03 |
| Sección `/trabajos/` con 301 desde `/blog/`; conservar URLs de proyectos; no aplicar aún (D3) | Confirmada | Usuario, 2026-11-03 |
| Estructura de seis páginas de servicio, sin inventar datos (D4, parcial) | Confirmada | Usuario, 2026-11-03 |
| Dieciocho propuestas visuales A–R en cuatro tandas antes del diseño definitivo (D7, en curso) | Confirmada | Usuario, 2026-11-03 |
| Despliegue manual verificable primero; automatización después (D11) | Confirmada | Usuario, 2026-11-03 |

Estado completo de D1–D15 en [`setup-progress.md`](setup-progress.md#decision-status).

## 4. Referencias

- Web actual: <https://www.oresteespinosacampos.es/>
- Repositorio: <https://github.com/jmartinlabs/oreste-web>. Comprobado con `gh repo view` el
  2026-11-03: existe, no está vacío y es **público**. D2 confirma mantenerlo público, sin
  secretos, credenciales, backups de WordPress ni contenido privado.
- Auditoría: [`audit/current-site-audit.md`](audit/current-site-audit.md)
- Inventario de URLs: [`audit/url-inventory.csv`](audit/url-inventory.csv)
- Inventario de medios: [`audit/asset-inventory.csv`](audit/asset-inventory.csv)
- Política de URLs de medios heredados: [`audit/media-url-policy.csv`](audit/media-url-policy.csv)
- Matriz de páginas de adjunto: [`audit/attachment-url-matrix.csv`](audit/attachment-url-matrix.csv)
- Arquitectura de hosting y despliegue:
  [`architecture/hosting-and-deployment.md`](architecture/hosting-and-deployment.md)
- Migración desde WordPress: [`migration/wordpress-to-astro.md`](migration/wordpress-to-astro.md)
- Estructura de páginas de servicio:
  [`content/service-pages-structure.md`](content/service-pages-structure.md)
- Fase 2: [`phase-2/plan.md`](phase-2/plan.md)
- Plan de fase 1: [`phase-1-plan.md`](phase-1-plan.md)
- Progreso: [`setup-progress.md`](setup-progress.md)

## 5. Criterios de éxito

1. Las siete URLs públicas actuales (portada, blog y cinco proyectos) responden 200 o 301
   correcto, sin cadenas ni 404; la portada y los cinco proyectos conservan su slug exacto.
2. Lighthouse móvil ≥ 95 en rendimiento, ≥ 95 en accesibilidad, 100 en SEO técnico y ≥ 95 en
   buenas prácticas, medido en tres ejecuciones.
3. LCP móvil < 1,8 s y peso de página < 500 KB en la portada.
4. Cero JavaScript imprescindible para navegar y ver contenido; islas sólo si aportan.
5. Datos estructurados `HousePainter`/`LocalBusiness` válidos y NAP idéntico en toda la web.
6. Aviso legal, privacidad y cookies publicados; analítica sólo tras consentimiento.
7. Publicación en la misma cuenta de Hostinger, con preproducción en subdominio, copia de
   seguridad verificada, 301 sin cadenas y rollback probado antes del corte.
8. Un agente puede añadir un proyecto con fotos, validar el cambio y abrir una PR sin poder
   publicar ni modificar contenido existente por su cuenta.
9. Documentación suficiente para que otra sesión retome el proyecto sin este chat.
10. La matriz de compatibilidad del plan está verificada en hPanel y con un spike desplegado en
    staging; lo no disponible se documenta como limitación, no se asume.

## 6. Preguntas abiertas

Ver la lista priorizada en [`phase-1-plan.md`](phase-1-plan.md#11-decisiones-estado) y el
resumen operativo en [`setup-progress.md`](setup-progress.md#decision-status).
