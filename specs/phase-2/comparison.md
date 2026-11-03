# Comparación A–M — Fase 2

Estado: **trece prototipos listos para decisión o combinación**. Fecha: 2026-11-03.
Autorización: directa del propietario para tres rondas (A/B/C, D/E/F/G y H–M).
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
| H | Habana Art Déco | `prototypes/h` | <http://localhost:4341/> | Baño |
| I | Cuba elegante contemporánea | `prototypes/i` | <http://localhost:4342/> | Baño |
| J | Psicodélica abstracta | `prototypes/j` | <http://localhost:4343/> | Baño |
| K | Neo-psicodelia premium | `prototypes/k` | <http://localhost:4344/> | Baño |
| L | Wabi-sabi textural | `prototypes/l` | <http://localhost:4345/> | Baño |
| M | Retro signage artesanal | `prototypes/m` | <http://localhost:4346/> | Baño |

**Sala de comparación:** <http://localhost:4330/> (`prototypes/index.html`).
**Captura de conjunto:** [`screenshots/comparison.png`](screenshots/comparison.png).

Cada una incluye portada, una página de servicio y un caso de estudio, en móvil y escritorio.
A–F y H–M usan el baño; G usa la fachada con antes/después real.

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

78 comprobaciones (13 propuestas × 3 rutas × 2 tamaños). Las trece pasan build, HTTP 200, un H1,
sin overflow, imágenes con alt, sin peticiones externas, sin errores de consola y sin enlaces
vacíos. A–F y H–M tienen cero scripts; G tiene un único script inline para el comparador.

| Comprobación | A–F y H–M | G |
| --- | --- | --- |
| Build de 3 páginas | Pasa | Pasa |
| HTTP 200 | Pasa | Pasa |
| Un H1 por página | Pasa | Pasa |
| Sin desbordamiento 390/1440 | Pasa | Pasa |
| Imágenes cargadas y con alt | Pasa | Pasa |
| Sin peticiones externas | Pasa | Pasa |
| Sin errores de consola | Pasa | Pasa |
| Enlaces vacíos | 0 | 0 |
| Scripts | 0 | 1 (slider) |

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

### H — Habana Art Déco

**Idea:** arquitectura Art Déco habanera: simetría, arcos, abanicos, esquinas escalonadas,
coral/menta/crema/teal y líneas doradas. Sin banderas ni símbolos políticos.
**Fortalezas:** elegancia, distinción y coherencia; la fotografía queda enmarcada con intención.
**Riesgos:** más boutique/formal que una web de gremio; puede leerse como caro; tracking serif
variable según sistema.
**Público:** cliente que busca distinción y un acabado boutique.

### I — Cuba elegante contemporánea

**Idea:** color blocking cálido, ritmo de lamas, patio y arquitectura; tabaco, turquesa, coral y
verde profundo. Sin tópicos turísticos ni símbolos políticos.
**Fortalezas:** calidez, ritmo y carácter local sin literalidad; buena jerarquía y navegación.
**Riesgos:** paleta de contraste medio; la referencia puede malinterpretarse si el copy fuerza;
las lamas pueden volverse fórmula.
**Público:** estudio de reforma/interiorismo de la Costa del Sol, residentes y segundas residencias.

### J — Psicodélica abstracta

**Idea:** maximalismo abstracto: violeta eléctrico, verde ácido, rosa y naranja; formas fluidas,
marquesinas CSS y color inmersivo.
**Fortalezas:** personalidad inmediata y memorable; mucha energía; navegación y CTAs claros.
**Riesgos:** puede cansar en sesiones largas; exige disciplina de contraste; no encaja en un
cliente conservador.
**Público:** cliente joven/creativo o negocio que quiere romper visualmente.

### K — Neo-psicodelia premium

**Idea:** psicodelia refinada: burdeos, lila, teal, crema y oro; marcos líquidos, serif de alto
contraste y capas translúcidas.
**Fortalezas:** lujo surrealista, diferenciación y legibilidad; fotografía real como protagonista.
**Riesgos:** las máscaras orgánicas recortan fotos; el lila es decorativo y puede necesitar
contención; no es una opción conservadora.
**Público:** vivienda premium y clientes internacionales que valoran diseño.

### L — Wabi-sabi textural

**Idea:** cal, yeso, arcilla y piedra; texturas CSS/SVG, marcos irregulares y estética de taller.
**Fortalezas:** tacto, calma y honestidad; los servicios se leen como muestras de material.
**Riesgos:** home larga; textura sutil que puede parecer plana; menos directa comercialmente.
**Público:** propietario que valora craft, durabilidad y trato directo.

### M — Retro signage artesanal

**Idea:** rotulación pintada a mano: crema, azul esmalte, rojo, mostaza y verde; paneles, estrellas,
flechas y bordes gruesos.
**Fortalezas:** calidez comercial, oficio y personalidad; hero y contacto muy claros.
**Riesgos:** la pila condensada depende de fuentes del sistema; puede rozar lo vintage si se
exagera; etiquetas pequeñas.
**Público:** autónomo o taller pequeño que vende confianza, proximidad y trabajo bien hecho.

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

### Segunda y tercera ronda

| Criterio | H | I | J | K | L | M |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Claridad en 5 s | 4 | 4 | 3 | 4 | 4 | 5 |
| Confianza/cercanía | 4 | 5 | 3 | 4 | 5 | 5 |
| Calidad percibida | 5 | 4 | 3 | 5 | 4 | 4 |
| Contacto móvil | 4 | 4 | 3 | 4 | 4 | 5 |
| Diferenciación | 5 | 4 | 5 | 5 | 4 | 4 |
| Legibilidad | 4 | 4 | 3 | 4 | 4 | 4 |
| Mantenimiento | 4 | 4 | 3 | 3 | 3 | 4 |
| Encaje Costa del Sol | 4 | 5 | 3 | 4 | 5 | 5 |

## 6. Lectura del coordinador

- **A** sigue siendo la opción más equilibrada para una web corporativa cercana.
- **B** es la más segura y clara; buena si se prioriza orden sobre diferenciación.
- **C** y **G** son las más memorables; G añade prueba visual y C narrativa de proceso.
- **D** es la más audaz; funciona si el propietario quiere una marca con carácter.
- **E** es la más premium; encaja si el negocio quiere posicionarse en vivienda de gama alta.
- **F** es la más mediterránea y luminosa; buena síntesis de marca local contemporánea.
- **H** aporta elegancia Art Déco y distinción; para un posicionamiento boutique.
- **I** es la más cálida y rítmica; buena opción local sin tópicos.
- **J** es la más rompedora; funciona si se busca impacto y un público creativo.
- **K** es la psicodelia elegante; atrevida pero contenida y legible.
- **L** es la más táctil y humana; encaja con el oficio y la calma.
- **M** es la más comercial y de taller; rotulación con carácter y contacto claro.

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
