# Estructura de las páginas de servicio (D4)

Estado: **preparación de estructura**, no implementación. Fecha: 2026-11-03.
Objetivo: fijar el esqueleto de las seis páginas de servicio y separar lo que ya sabemos de lo
que debe confirmar el propietario. **No se inventa información.**

## 1. Reglas de contenido

1. Sólo se publica lo que esté confirmado por el propietario o sea visible en la web actual.
2. Lo desconocido se marca como `[confirmar]` y no se publica.
3. No se inventan precios, plazos, garantías, materiales, certificaciones, reseñas ni zonas.
4. Las fotos deben ser reales, del propietario y con autorización de uso.
5. Una página no se considera lista hasta que todos los campos obligatorios estén confirmados.
6. En la fase 2 se puede usar contenido provisional para las propuestas visuales, siempre marcado
   como provisional y sin publicar.

## 2. Plantilla común

| Bloque | Contenido | Obligatorio |
| --- | --- | --- |
| Hero | H1, subtítulo de una frase, CTA «Pedir presupuesto» / WhatsApp | Sí |
| Introducción | Problema del cliente y resultado esperado | Sí |
| Qué incluye | Lista de trabajos concretos del servicio | Sí |
| Proceso | 3–5 pasos, con lo que aporta el cliente | Sí |
| Materiales y calidad | Marcas, tipos de pintura o material, garantía | Confirmar |
| Zona de servicio | Municipios y límites reales | Sí |
| Trabajos relacionados | 1–3 proyectos reales enlazados | Sí |
| Preguntas frecuentes | 3–5 dudas reales del propietario | Recomendado |
| CTA final | Teléfono, WhatsApp, correo y enlace a contacto | Sí |
| Servicios relacionados | 2–3 enlaces internos | Recomendado |

## 3. Seis páginas y correspondencia

| Página | Slug | Servicios declarados que cubre |
| --- | --- | --- |
| Pintura interior y exterior | `/servicios/pintura-interior-exterior/` | Pintura interior/exterior; reparación de paredes |
| Pintura y restauración de fachadas | `/servicios/pintura-de-fachadas/` | Restauración de fachadas |
| Alicatado y solería | `/servicios/alicatado-y-soleria/` | Alicatado; solería |
| Reforma de baños | `/servicios/reforma-de-banos/` | Reforma integral de baños |
| Reforma de cocinas | `/servicios/reforma-de-cocinas/` | Renovación de cocinas |
| Mantenimiento y pequeñas reformas | `/servicios/mantenimiento-general/` | Mantenimiento general; pequeñas reformas; fontanería y electricidad básicas |

Los diez servicios declarados quedan cubiertos. «Fontanería y electricidad» y «pequeñas
reformas» se presentan dentro de mantenimiento salvo que el propietario confirme que quiere
páginas propias.

## 4. Ficha por servicio

Cada ficha indica lo confirmado por la web actual y lo que falta. No hay copy definitivo.

### 4.1 Pintura interior y exterior

- **Confirmado:** pintura interior y exterior; reparación de paredes.
- **Por confirmar:** tipos de pintura y marcas; preparación de superficies; tratamiento de humedad;
  acabados (mate, satinado, plástica, temple); altura y acceso; protección de muebles; limpieza
  final; plazo orientativo; garantía; zona; precio o rango.
- **Fotos necesarias:** antes/después de interior y exterior.
- **Proyectos relacionados:** chalet y baño si se confirma que incluyeron pintura.

### 4.2 Pintura y restauración de fachadas

- **Confirmado:** restauración de fachadas; la entrada de fachada documenta limpieza, reparación y
  pintura en blanco.
- **Por confirmar:** diagnosis de grietas y humedad; impermeabilización; andamios y permisos;
  altura máxima; tipos de revestimiento; garantía; plazo; zona; precio.
- **Fotos necesarias:** fachada antes/después y detalle de reparación.
- **Proyectos relacionados:** revitalización exterior de fachada.

### 4.3 Alicatado y solería

- **Confirmado:** alicatado y solería; la entrada de cocina industrial documenta renovación de
  suelo.
- **Por confirmar:** materiales (porcelánico, gres, cerámica); formatos; nivelación; retirada del
  material antiguo; rodapiés; sellado; plazo; garantía; zona; precio.
- **Fotos necesarias:** cocina industrial y baño.
- **Proyectos relacionados:** cocina industrial y reforma de baño.

### 4.4 Reforma de baños

- **Confirmado:** reforma integral de baños; la entrada documenta plato de ducha, doble lavamanos
  y espejos iluminados.
- **Por confirmar:** fontanería y desagües; electricidad; impermeabilización; sanitarios;
  mamparas; ventilación; plazos; garantía; zona; precio; necesidad de permisos.
- **Fotos necesarias:** baño antes/después y detalle de instalación.
- **Proyectos relacionados:** remodelación integral de baño.

### 4.5 Reforma de cocinas

- **Confirmado:** renovación de cocinas; la entrada de azulejos documenta una reforma rápida.
- **Por confirmar:** alcance (sólo alicatado/suelo o reforma completa); mobiliario; encimera;
  fontanería; electricidad; electrodomésticos; plazos; garantía; zona; precio.
- **Fotos necesarias:** cocina antes/después.
- **Proyectos relacionados:** reforma de azulejos de cocina.

### 4.6 Mantenimiento y pequeñas reformas

- **Confirmado:** mantenimiento general; pequeñas reformas; fontanería y electricidad.
- **Por confirmar:** qué se considera «pequeña reforma»; límites de fontanería y electricidad;
  urgencias; contratos de mantenimiento; frecuencia; zona; precio/hora o presupuesto.
- **Fotos necesarias:** trabajos variados reales.
- **Proyectos relacionados:** los que el propietario autorice.

## 5. Campos SEO a completar tras la confirmación

| Campo | Regla |
| --- | --- |
| `title` | 20–65 caracteres, con servicio + Mijas/Costa del Sol |
| `description` | 120–165 caracteres, con resultado y CTA |
| `summary` | 80–220 caracteres para tarjetas y hub |
| `intro` | 80–400 caracteres; problema y resultado |
| `includes` | 3–8 trabajos concretos incluidos |
| `process` | 3–5 pasos con título y detalle |
| `materials` | Marcas y materiales verificados; opcional |
| `serviceArea` | Municipios y límites confirmados |
| `warranty` | Garantía real; opcional |
| `benefits` | 3–6 ventajas verificables |
| `gallery` | Hasta 8 fotos reales con alt |
| `faqs` | 3–5 preguntas reales, sin inventar respuestas |
| `cover` y `coverAlt` | Foto real y alt descriptivo |
| `order` | Orden de aparición en el hub |

## 6. Entrevista de confirmación (D4)

Preguntas mínimas al propietario:

1. ¿Qué incluye exactamente cada servicio y qué no?
2. ¿Qué materiales y marcas usa habitualmente?
3. ¿Cuánto suele durar cada tipo de trabajo?
4. ¿Qué garantía ofrece y por escrito?
5. ¿En qué municipios trabaja y hasta dónde se desplaza?
6. ¿Cómo fija el precio: por m², por hora, por presupuesto cerrado?
7. ¿Qué permisos o requisitos necesita el cliente?
8. ¿Tiene fotos originales y autorización para usarlas?
9. ¿Qué preguntas le hacen más a menudo los clientes?
10. ¿Qué servicios no quiere ofrecer o quiere dejar de anunciar?

## 7. Criterio de salida

- Las seis fichas tienen todos los campos obligatorios confirmados.
- No queda ningún `[confirmar]` en contenido publicable.
- Cada página enlaza al menos un proyecto real.
- El propietario aprueba el texto final antes de publicar.
- Ninguna página se implementa en fase 1; la estructura sirve para la fase 2 y la fase 4.
