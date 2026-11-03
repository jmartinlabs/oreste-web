# Comparación A–G — Fase 2

Estado: **siete prototipos listos para decisión del propietario**. Fecha: 2026-11-03.
Autorización: directa del propietario para dos rondas (A/B/C y D/E/F/G).
Salida pendiente: elegir una dirección o combinar elementos (D7).

## 1. Prototipos

| ID | Dirección | Directorio | URL local | Contenido |
| --- | --- | --- | --- | --- |
| A | Artesano contemporáneo | `prototypes/a` | <http://localhost:4321/> | Baño |
| B | Precisión luminosa | `prototypes/b` | <http://localhost:4322/> | Baño |
| C | Cuaderno de obra | `prototypes/c` | <http://localhost:4323/> | Baño |
| D | Brutalista elegante | `prototypes/d` | <http://localhost:4324/> | Baño |
| E | Editorial premium | `prototypes/e` | <http://localhost:4325/> | Baño |
| F | Mediterráneo contemporáneo | `prototypes/f` | <http://localhost:4326/> | Baño |
| G | Before/After driven | `prototypes/g` | <http://localhost:4327/> | Fachada con antes/después real |

**Sala de comparación:** <http://localhost:4330/> (`prototypes/index.html`).
**Captura de conjunto:** [`screenshots/comparison.png`](screenshots/comparison.png).

Cada una incluye portada, una página de servicio y un caso de estudio, en móvil y escritorio.

## 2. Base común

- A–F usan el mismo `content.json` (hash idéntico) y las mismas fotos.
- G usa `content-g.json` con el proyecto real de fachada porque es el único con fotos
  verificadas de antes y después. No inventa un antes para el baño.
- Mismos datos de contacto, zonas y servicios.
- Contenido provisional marcado: alcance, proceso, materiales, garantía y preguntas del servicio;
  ubicación y foto del antes cuando falta.
- Sin reseñas, precios, garantías, certificaciones, ubicaciones ni fotos inventadas.
- Sin fuentes externas, CDNs, cookies, analítica ni formularios.

## 3. Verificación técnica

42 comprobaciones (7 propuestas × 3 rutas × 2 tamaños):

| Comprobación | A | B | C | D | E | F | G |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Build de 3 páginas | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| HTTP 200 | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| Un H1 por página | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| Sin desbordamiento 390/1440 | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| Imágenes cargadas y con alt | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| Sin peticiones externas | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| Sin errores de consola | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa | Pasa |
| Enlaces vacíos | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Scripts | 0 | 0 | 0 | 0 | 0 | 0 | 1 (slider) |

G usa un único script inline para el comparador antes/después; el resto es cero JavaScript.
El slider de G funciona con teclado, puntero y tiene fallback sin JS.

## 4. Lectura de cada dirección

### A — Artesano contemporáneo

**Idea:** papel cálido, verde bosque, terracota, serif editorial y fotografía grande.
**Fortalezas:** cercanía, oficio, calidad percibida, buena jerarquía y banda de contacto potente.
**Riesgos:** menos técnica; depende de fotos bien encuadradas.
**Público:** cliente particular que valora trato directo y trabajo cuidado.

### B — Precisión luminosa

**Idea:** caliza, azul marino, retícula, datos visibles y tipografía limpia.
**Fortalezas:** la más clara y rápida de escanear; orden, método y contacto inmediato.
**Riesgos:** la más corporativa y genérica; menos emocional.
**Público:** cliente que compara presupuestos y valora rapidez y profesionalidad.

### C — Cuaderno de obra

**Idea:** papel, grafito, naranja y azul de plano; fichas, figuras y anotaciones.
**Fortalezas:** la más diferenciada; convierte el portfolio en evidencia del proceso.
**Riesgos:** etiquetas pequeñas y densidad; más costosa de mantener.
**Público:** cliente que quiere ver cómo se trabaja y confía en lo documentado.

### D — Brutalista elegante

**Idea:** negro, papel, rojo señal, tipografía enorme y tablas.
**Fortalezas:** personalidad muy fuerte, contraste y jerarquía; memorable sin ser caótica.
**Riesgos:** puede sentirse demasiado severa para una marca doméstica; micro-etiquetas pequeñas.
**Público:** cliente que busca un proveedor con carácter y diseño diferencial.

### E — Editorial premium

**Idea:** blanco cálido, serif de revista, fotografía a sangre y silencio visual.
**Fortalezas:** percepción premium, fotografía protagonista y ritmo editorial.
**Riesgos:** menos directa para servicios; puede parecer más estudio de interiorismo que gremio.
**Público:** vivienda de gama alta y clientes sensibles al diseño.

### F — Mediterráneo contemporáneo

**Idea:** caliza, azul marino, arcos, luz y acentos serif.
**Fortalezas:** identidad mediterránea moderna y auténtica, sin tópicos; aire y claridad.
**Riesgos:** el arco puede volverse fórmula; necesita disciplina artística.
**Público:** cliente de Costa del Sol que quiere una marca local y contemporánea.

### G — Before/After driven

**Idea:** transformación real como argumento; comparador, etiquetas y proceso.
**Fortalezas:** prueba visual inmediata, honestidad y una interacción útil; la más orientada a
conversión por evidencia.
**Riesgos:** solo hay un proyecto con antes/después verificado; el ángulo del antes y el después
no coincide exactamente; la interacción añade un pequeño script.
**Público:** cliente que necesita ver resultados antes de llamar.

## 5. Matriz rápida

Valoración del coordinador, no una decisión. Escala 1–5.

| Criterio | A | B | C | D | E | F | G |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Claridad en 5 s | 4 | 5 | 4 | 4 | 4 | 5 | 5 |
| Confianza/cercanía | 5 | 4 | 4 | 3 | 4 | 5 | 5 |
| Calidad percibida | 5 | 4 | 4 | 4 | 5 | 5 | 4 |
| Contacto móvil | 4 | 5 | 4 | 4 | 4 | 4 | 5 |
| Diferenciación | 4 | 3 | 5 | 5 | 4 | 4 | 5 |
| Legibilidad | 5 | 5 | 3 | 3 | 5 | 5 | 4 |
| Mantenimiento | 4 | 5 | 3 | 4 | 4 | 4 | 3 |
| Encaje Costa del Sol | 5 | 5 | 4 | 3 | 4 | 5 | 4 |

## 6. Lectura del coordinador

- **A** sigue siendo la opción más equilibrada para una web corporativa cercana.
- **B** es la más segura y clara; buena si se prioriza orden sobre diferenciación.
- **C** y **G** son las más memorables; G añade prueba visual y C narrativa de proceso.
- **D** es la más audaz; funciona si el propietario quiere una marca con carácter.
- **E** es la más premium; encaja si el negocio quiere posicionarse en vivienda de gama alta.
- **F** es la más mediterránea y luminosa; buena síntesis de marca local contemporánea.

La decisión es del propietario, que puede elegir una o combinar elementos (por ejemplo, la
estructura de G con la tipografía de A). No se implementa nada hasta registrarlo.

## 7. Contenido pendiente de confirmar

- alcance real de cada servicio;
- proceso, materiales y garantía;
- ubicación del proyecto del baño y foto del antes;
- ubicación exacta del proyecto de fachada y confirmación de que el antes y el después son del
  mismo muro/ángulo;
- fotos originales y autorización de uso;
- logo vectorial;
- textos legales;
- reseñas reales y Google Business Profile.

## 8. Siguiente paso

1. El propietario elige una dirección o una combinación.
2. Se registra la decisión y la fecha.
3. Se extraen tokens de diseño y se conserva el prototipo elegido como referencia.
4. Se archivan o limpian los descartados.
5. Se pasa a la fase 3 (fundaciones y esqueleto) con la dirección aprobada.

Sin merge, staging, DNS, Hostinger, WordPress, redirecciones ni despliegue.
