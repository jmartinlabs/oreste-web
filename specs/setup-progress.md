# Setup progress

Current stage: 1

Fase 1 (auditoría y plan) está **completada como evidencia**, no como aprobación. Los
documentos de identidad, tecnología y arquitectura son propuestas que esperan revisión humana.
La resolución parcial de decisiones del 2026-11-03 reduce bloqueos, pero no aprueba la fase 1 ni
autoriza la fase 2.

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

Fuente: instrucciones del usuario (2026-11-03), incluida la resolución parcial de decisiones.
El detalle está en [`project.md`](project.md#3-restricciones-y-decisiones-confirmadas).

- Astro + Tailwind CSS + TypeScript, `output: 'static'`.
- Hosting definitivo: **la misma cuenta** de Hostinger y el **mismo dominio**
  `oresteespinosacampos.es`.
- Sin Node.js en producción; publicación de `dist/` en `public_html`.
- Sin cambios de DNS, SSL ni configuración de Hostinger durante la fase 1.
- Sin contratar servicios adicionales.
- Sin CMS; contenido en archivos, actualizado por agentes con aprobación humana.
- WordPress actual intacto hasta autorización expresa de migración.
- Fase 1 limitada a auditoría y planificación; sin desarrollo ni despliegue.
- Repositorio `jmartinlabs/oreste-web`; sin merges ni cambios remotos sin aprobación.
- **D1 (parcial):** Hostinger se mantiene; verificar plan, SSH y backups antes de configurar
  staging. No bloquear el diseño por esta información.
- **D2:** mantener el repositorio **público**; no incluir secretos, credenciales, backups de
  WordPress ni contenido privado.
- **D3:** la sección nueva será `/trabajos/`; 301 desde `/blog/`; conservar preferentemente las
  URLs individuales de los proyectos. No aplicar redirecciones todavía.
- **D4 (parcial):** estructura de las páginas de servicio preparada en
  [`content/service-pages-structure.md`](content/service-pages-structure.md); no inventar
  información y señalar los datos que necesitan confirmación.
- **D7 (parcial):** presentar dos propuestas visuales diferenciadas y elegir antes de implementar
  el diseño definitivo.
- **D11:** priorizar un despliegue manual verificable; GitHub Actions queda para una fase
  posterior.

## Decision status

| ID | Estado | Decisión | Bloquea |
| --- | --- | --- | --- |
| D1 | Parcial | Hostinger definitivo; plan, SSH y backups por verificar antes de staging | Staging y despliegue; **no** el diseño |
| D2 | Resuelta | Repositorio público sin secretos, credenciales, backups ni contenido privado | Nada |
| D3 | Resuelta en planificación | `/trabajos/` + 301 desde `/blog/`; conservar proyectos; no aplicar aún | Nada; implementación en fase 5 |
| D4 | Parcial | Estructura de servicios preparada; datos por confirmar sin inventar | Contenido final (fase 4); fase 2 con marcadores provisionales |
| D5 | Pendiente | Formulario: ninguno o PHP+SMTP | Fase 6 |
| D6 | Pendiente | Analítica: GA4 con consentimiento o sin analítica | Lanzamiento |
| D7 | Parcial | Dos propuestas visuales; elección antes de implementar | Fase 2 (salida) |
| D8 | Pendiente | Contenido legal y responsable del tratamiento | Lanzamiento |
| D9 | Pendiente | Fotos, vídeo y logo originales | Migración |
| D10 | Pendiente | Reseñas y Google Business Profile | Contenido local |
| D11 | Resuelta | Despliegue manual primero; automatización después | Fase 6 |
| D12 | Pendiente | Copias y rollback en Hostinger | Publicación |
| D13 | Pendiente | Subdominio y autorización DNS | Staging |
| D14 | Pendiente | Ventana de corte y aprobador | Publicación |
| D15 | Pendiente | Método final de despliegue | Fase 6 |

## Next action

La fase 2 **no ha comenzado** y requiere autorización explícita. Cuando se autorice, empezar por
las dos propuestas visuales A/B con contenido provisional marcado, sin crear staging ni aplicar
redirecciones. Ninguna decisión pendiente bloquea el inicio de la fase 2: D1 y D13 sólo son
necesarias antes de la fase 3 (staging); D4 se completará con la entrevista de contenido; D7 se
decide al final de la fase 2.
