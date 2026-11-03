# Fase 2 — Siete propuestas visuales en dos rondas

Estado: **prototipos listos para decisión**. Autorización directa del propietario: 2026-11-03.
Salida: el propietario elige una dirección o una combinación (D7).

## 1. Objetivo

Comparar siete direcciones de identidad visual con contenido real, antes de implementar el diseño
definitivo. No se busca un ganador técnico: se busca una dirección que el propietario reconozca
como propia y que funcione para captar contactos locales.

## 2. Direcciones

| ID | Nombre | Idea | Por qué se propone |
| --- | --- | --- | --- |
| A | Artesano contemporáneo | Neutros cálidos, verde profundo, terracota, tipografía editorial, fotografía grande | Cercanía y oficio; transmite confianza sin parecer una gran empresa |
| B | Precisión luminosa | Piedra caliza, azul profundo, retícula clara, datos visibles | Orden, rapidez y claridad; encaja con un servicio técnico y mediterráneo |
| C | Cuaderno de obra | Papel, grafito, naranja de seguridad, azul de plano, fichas y anotaciones | Convierte el portfolio en evidencia del proceso; se diferencia claramente de A y B |
| D | Brutalista elegante | Negro, papel, rojo señal, tipografía enorme, tablas y bloques asimétricos | Personalidad fuerte y memorable sin perder legibilidad |
| E | Editorial premium | Blanco cálido, serif de revista, fotografía a sangre y silencio visual | Percepción premium y fotografía protagonista |
| F | Mediterráneo contemporáneo | Caliza, azul marino, arcos, luz y acentos serif | Identidad local contemporánea, sin tópicos turísticos |
| G | Before/After driven | Comparador antes/después, etiquetas, proceso y evidencia real | Convierte la transformación en el argumento principal |

No son variantes de color: cambian paleta, tipografía, retícula, tono y jerarquía.

## 3. Alcance de cada prototipo

Tres páginas navegables, en español, para cada dirección:

1. Portada `/`.
2. Servicio `/servicios/reforma-de-banos/` en A–F; `/servicios/pintura-de-fachadas/` en G.
3. Caso de estudio `/trabajos/remodelacion-integral-de-bano/` en A–F;
   `/trabajos/revitalizacion-exterior/` en G.

Cada una con versión móvil y escritorio, capturas y una ficha de ventajas/inconvenientes.

## 4. Contenido

- Fuente única: `prototypes/shared/content.json` en A–F y `content-g.json` en G.
- Fotos reales: `prototypes/shared/assets/` (furgoneta, baño, chalet, fachada, cocina).
- Mismo texto, mismas fotos y mismo orden de proyectos en A–F. G usa la fachada porque es el
  único proyecto con fotos verificadas de antes y después.
- **Provisional:** el alcance, proceso, materiales, garantía y preguntas del servicio; la
  ubicación del proyecto y la foto del antes. Se marcan con banner, etiqueta o nota visible.
- No se inventan reseñas, precios, garantías, certificaciones, ubicaciones ni fotos.

## 5. Organización

| Directorio | Propuesta | Puerto | Propiedad |
| --- | --- | --- | --- |
| `prototypes/a` | A | 4321 | Agente A |
| `prototypes/b` | B | 4322 | Agente B |
| `prototypes/c` | C | 4323 | Agente C |
| `prototypes/d` | D | 4324 | Agente D |
| `prototypes/e` | E | 4325 | Agente E |
| `prototypes/f` | F | 4326 | Agente F |
| `prototypes/g` | G | 4327 | Agente G |
| `prototypes/shared` | Contenido, fotos y captura | — | Coordinador, sólo lectura |

Cada agente trabaja sólo en su directorio. No hay edición simultánea de archivos. No se usan
worktrees separados porque los directorios son disjuntos y el contenido es compartido de sólo
lectura.

## 6. Requisitos técnicos

- Astro 7, Tailwind 4 y TypeScript estricto.
- Sin frameworks de UI, sin librerías de estado, sin JavaScript de cliente salvo el menú móvil
  con `<details>`/CSS. G añade un único script inline para el comparador antes/después.
- Sin fuentes, imágenes, CDNs ni servicios externos.
- Imágenes con `<Image>` de Astro y alt real.
- Un H1 por página, skip link, foco visible, contraste y `prefers-reduced-motion`.
- Responsive real a 390 px y 1440 px.
- Build estático correcto; capturas con Chrome headless.

## 7. Entregables

- Siete prototipos locales navegables.
- Seis capturas por propuesta (portada, servicio y trabajo; móvil y escritorio).
- `README.md` por propuesta con rationale, fortalezas, riesgos y rutas.
- Comparación A–G en `specs/phase-2/comparison.md`.
- Decisión D7 registrada por el propietario.

## 8. Criterios de comparación

1. Claridad de la propuesta en 5 segundos.
2. Confianza y cercanía.
3. Calidad percibida del trabajo real.
4. Facilidad para contactar desde el móvil.
5. Diferenciación frente a competidores locales.
6. Accesibilidad y legibilidad.
7. Coherencia con la Costa del Sol y con el oficio.
8. Coste de mantenimiento y facilidad para el flujo de agentes.

## 9. Límites

- Sin merge de la PR #1.
- Sin staging, DNS, Hostinger, WordPress, redirecciones ni despliegues.
- Sin compras ni servicios adicionales.
- Sin publicar los prototipos en producción.
- Los prototipos no son la implementación definitiva.

## 10. Después de la elección

1. Registrar la dirección elegida y la fecha.
2. Extraer tokens de diseño (color, tipografía, espaciado, componentes).
3. Conservar el prototipo elegido como referencia y archivar/limpiar los descartados.
4. Pasar a la fase 3 (fundaciones y esqueleto) con la dirección aprobada.
