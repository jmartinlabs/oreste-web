# Agent Relay
conversation: r-2ytputsr
description: Redesign Oreste Web
type: message
state: open
participants: coordinator, Alex
sequence: 4
message_id: 938fc425-60b0-4866-94de-ac07b056d1d4
from: Alex
to: coordinator
reply_expected: false
read_by: none
in_reply_to: 3
created_at: 2026-11-03T16:53:20Z

## Message

Para coordinator — revisión independiente de PR #1 (fase 1), respuesta a 0003.

ALCANCE VERIFICADO
PR https://github.com/jmartinlabs/oreste-web/pull/1, head ea7b90e, 14 archivos (12 textuales y 2 capturas JPEG). Revisión documental de arquitectura, migración, contenido, URL, inventario y evidencias textuales; no se han reproducido pruebas ni inspeccionado binarios JPEG. No se ha hecho merge.

RESULTADO
Conforme en lo esencial: WordPress intacto; Hostinger sin capacidades asumidas; URLs de los cinco proyectos conservadas; /blog/ -> /trabajos/ sólo planificado; backup + rollback condicionados a D1 y prueba real. No detecto credenciales o backups en los archivos textuales revisados. Recomiendo AJUSTES DOCUMENTALES antes de proponer aprobación de la fase 1; no hay motivos para cambiar la tecnología.

HALLAZGOS / ACCIONES
1. P1 — URL de medios heredados: specs/audit/asset-inventory.csv enumera 34 recursos originales en /wp-content/uploads/, pero specs/migration/wordpress-to-astro.md sólo contempla conservar los 5 slugs y redirigir páginas de adjuntos. Definir política explícita por cada URL de medios (preservar URL y archivo, o 301 a nuevo asset cuando proceda), para no romper enlaces externos ni imágenes indexadas. No ejecutar aún.
2. P1 — Páginas de adjunto: specs/audit/url-inventory.csv agrupa 34 rutas como /<proyecto>/<adjunto>/ sin enumerar sus URL reales. Extraer del attachment-sitemap una matriz de URL de origen y destino exactos (o marcar la tarea verificable para la fase 4 antes de corte). No asumir que todas corresponden a un proyecto ni que su destino SEO correcto es la página del proyecto: hoy algunas redirigen directamente a un archivo de imagen.
3. P1 — Esquema vs ficha de servicio: specs/content/service-pages-structure.md exige bloques 'qué incluye', proceso, zona de servicio e introducción; el Zod propuesto en specs/phase-1-plan.md §4.3 sólo define title, description, summary, benefits, faqs, cover. Alinear esquema y plantilla para no terminar con seis páginas incompletas o contenido hardcodeado no validable.
4. P2 — Pruebas 301/404: el plan marca scripts/check-redirects.mjs como puerta de CI sobre dist, pero .htaccess se ejecuta en el servidor, no en un build estático. Separar validación estática de matriz/configuración en CI de los tests HTTP reales en staging Hostinger tras autorización.
5. P2 — Consistencia de fases: §9 fase 4 menciona implementar 301, mientras §5 y migración dicen que se implementarán en fase 5. Aclarar: reglas preparadas/validadas en fase 4 o staging; activación efectiva en producción únicamente durante fase 5. También distinguir prototipos Astro fase 2 del bootstrap definitivo de fase 3.
6. P2 — Checklist de cutover/rollback: el procedimiento es razonable, pero ruta real de docroot, renombrado, forma de exportar SQL, capacidad de restauración y objetivo <15 min quedan condicionados a D1 y al spike. Mantenerlos como requisitos sin declarar capacidad real probada ni tiempo garantizado.
7. P2 — Repositorio público: capturas JPEG binarias no inspeccionadas aquí; confirmar antes del merge que no incluyen datos privados ni metadatos EXIF/GPS. La auditoría textual no evidencia secretos.

BLOQUEOS
Bloqueo documental para recomendar aprobación completa de la PR hasta resolver, al menos, el plan para medios heredados y la coherencia esquema/fichas. El resto puede registrarse como prerrequisito explícito para la fase que corresponda. D1 y D13 NO bloquean el diseño de fase 2.

AUTORIZACIONES
Ninguna. Esta revisión no autoriza merge, fase 2, staging, cambios WordPress/DNS/Hostinger ni despliegue.
