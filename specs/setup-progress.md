# Setup progress

Current stage: 1

Fase 1 (auditoría y plan) está **completada como evidencia**, no como aprobación. Los
documentos de identidad, tecnología y arquitectura son propuestas que esperan revisión humana.

| Stage | Status | Evidence / canonical decision links |
| --- | --- | --- |
| 1 Identity | awaiting-approval | [`project.md`](project.md) · [`audit/current-site-audit.md`](audit/current-site-audit.md) |
| 2 Technology | awaiting-approval | [`phase-1-plan.md`](phase-1-plan.md#4-arquitectura-propuesta) (propuesta) |
| 3 Architecture | awaiting-approval | [`phase-1-plan.md`](phase-1-plan.md#4-arquitectura-propuesta) · [`architecture/hosting-and-deployment.md`](architecture/hosting-and-deployment.md) (propuesta) |
| 4 Bootstrap | pending | Requiere aprobar las etapas 2 (Technology) y 3 (Architecture) del setup |
| 5 Guidelines | pending | `guidelines/*.md` siguen siendo el texto genérico de la plantilla |
| 6 Roadmap | pending | Propuesta de 4 épicas en [`phase-1-plan.md`](phase-1-plan.md#9-fases-y-roadmap-propuesto) |
| 7 Handoff | pending | |

## Confirmed decisions

Fuente: instrucciones del usuario (2026-11-03). El detalle está en
[`project.md`](project.md#3-restricciones-y-decisiones-confirmadas).

- Astro + Tailwind CSS + TypeScript, `output: 'static'`.
- Hosting definitivo: **la misma cuenta** de Hostinger y el **mismo dominio**
  `oresteespinosacampos.es`.
- Sin Node.js en producción; publicación de `dist/` en `public_html`.
- Sin cambios de DNS, SSL ni configuración de Hostinger durante la fase 1.
- Sin contratar servicios adicionales.
- Compatibilidad del plan no asumida: lo no comprobado queda pendiente de D1.
- Sin CMS; contenido en archivos, actualizado por agentes con aprobación humana.
- WordPress actual intacto hasta autorización expresa de migración.
- Fase 1 limitada a auditoría y planificación; sin desarrollo ni despliegue.
- Repositorio `jmartinlabs/oreste-web`; sin merges ni cambios remotos sin aprobación.

## Pending decisions

Numeración estable para referencias. Detalle, opciones y recomendación en
[`phase-1-plan.md`](phase-1-plan.md#11-decisiones-pendientes).

| ID | Decisión | Bloquea | Recomendación |
| --- | --- | --- | --- |
| D1 | Plan exacto, ruta pública, subdominios, SSL, SSH y backups | Staging y despliegue | Verificar en hPanel; no asumir capacidades |
| D2 | Visibilidad del repositorio (hoy público) | Contenido y secretos | Privado, o público sólo con datos no sensibles |
| D3 | Arquitectura de URLs: `/blog/` y categorías → 301 a `/trabajos/` | Fase 2 (entrada), URLs y SEO | 301, conservando portada y 5 proyectos |
| D4 | Páginas de servicio (6) y datos verificados de cada una | Contenido (fase 4 y textos de fase 2) | 6 páginas; entrevista previa al propietario |
| D5 | Formulario de contacto: ninguno o PHP+SMTP | Alcance de fase 6 | Lanzar sin formulario; decidir después |
| D6 | Analítica: GA4 con consentimiento o sin analítica | Legal y rendimiento | GA4 con Consent Mode v2 o ninguna; sin servicios nuevos |
| D7 | Dirección visual (fase 2) | Fase 2 (salida) | Dos propuestas, elección del propietario |
| D8 | Contenido legal y responsable del tratamiento | Lanzamiento | Redactar y revisar antes de publicar |
| D9 | Uso de fotos, vídeo y logo originales | Migración | Pedir originales y autorización de uso |
| D10 | Reseñas/testimonios y enlace a Google Business Profile | Contenido local | Sólo reseñas reales y verificables |
| D11 | Automatización de despliegue con GitHub Actions | Fase 6 | Después del lanzamiento y con aprobación; verificar revisor obligatorio según plan/visibilidad |
| D12 | Estrategia de copias y rollback en Hostinger | Publicación | Backup manual + archivo local + artefacto de CI |
| D13 | Nombre del subdominio y autorización DNS | Preproducción | `staging.oresteespinosacampos.es`; no se crea en fase 1 |
| D14 | Ventana de corte y aprobador | Publicación | Fuera de horario comercial; propietario + técnico |
| D15 | Método final de despliegue | Fase 6 | SSH/rsync, SFTP o manual según D1 |

## Next action

Revisar el informe de fase 1 y responder a D3 y D4 antes de autorizar la fase 2. D1 y D13 son
necesarias antes de la fase 3 (staging). La dirección visual (D7) se decide al final de la fase 2.
