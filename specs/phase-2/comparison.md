# Comparación A–S — Fase 2

Estado: **diecinueve prototipos listos para decisión o combinación**. Fecha: 2026-11-03.
Autorización: directa del propietario para cuatro tandas (A/B/C, D/E/F/G, H–M y N–R) más la
variante S de G.
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
| N | Before/After con portada editorial | `prototypes/n` | <http://localhost:4351/> | Fachada con antes/después real |
| O | Neo-psicodelia esmeralda | `prototypes/o` | <http://localhost:4352/> | Baño |
| P | Neo-psicodelia nocturna | `prototypes/p` | <http://localhost:4353/> | Baño |
| Q | Neo-psicodelia rosa | `prototypes/q` | <http://localhost:4354/> | Baño |
| R | Psicodélica solar | `prototypes/r` | <http://localhost:4355/> | Baño |
| S | G con portada de marca | `prototypes/s` | <http://localhost:4356/> | Fachada con antes/después real |

**Sala de comparación:** <http://localhost:4330/> (`prototypes/index.html`).
**Captura de conjunto:** [`screenshots/comparison.png`](screenshots/comparison.png).

Cada una incluye portada, una página de servicio y un caso de estudio, en móvil y escritorio.
A–F, H–M y O–R usan el baño; G, N y S usan la fachada con antes/después real.

## 2. Base común

- A–F usan el mismo `content.json` (hash idéntico) y las mismas fotos.
- G, N y S usan `content-g.json` con el proyecto real de fachada porque es el único con fotos
  verificadas de antes y después. No inventan un antes para el baño.
- S es G con la portada cambiada: titular de servicio y foto de la furgoneta; el comparador se
  queda en `02 / Transformaciones`, donde G ya lo tenía.
- O, P y Q son la misma estructura de K con tres paletas distintas; R es la estructura de J
  con otra paleta. Cambian el color, no el contenido ni la jerarquía.
- Mismos datos de contacto, zonas y servicios.
- Contenido provisional marcado: alcance, proceso, materiales, garantía y preguntas del servicio;
  ubicación y foto del antes cuando falta.
- Sin reseñas, precios, garantías, certificaciones, ubicaciones ni fotos inventadas.
- Sin fuentes externas, CDNs, cookies, analítica ni formularios.

## 3. Verificación técnica

114 comprobaciones (19 propuestas × 3 rutas × 2 tamaños). Las diecinueve pasan build, HTTP 200,
un H1, sin overflow, imágenes con alt, sin peticiones externas, sin errores de consola y sin
enlaces vacíos. Diecisiete tienen cero scripts; G, N y S tienen un único script inline para el
comparador antes/después.

| Comprobación | Resto (16) | G, N y S |
| --- | --- | --- |
| Build de 3 páginas | Pasa | Pasa |
| HTTP 200 | Pasa | Pasa |
| Un H1 por página | Pasa | Pasa |
| Sin desbordamiento 390/1440 | Pasa | Pasa |
| Imágenes cargadas y con alt | Pasa | Pasa |
| Sin peticiones externas | Pasa | Pasa |
| Sin errores de consola | Pasa | Pasa |
| Enlaces vacíos | 0 | 0 |
| Scripts | 0 | 1 (comparador) |

G, N y S usan un único script inline para el comparador antes/después; el resto es cero JavaScript.
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

### N — Before/After con portada editorial

**Idea:** la prueba real de G, pero con portada de verdad: cubierta oscura, titular grande y
foto de fachada a sangre; el comparador se convierte en la sección `03 / Evidencia`.
**Fortalezas:** conserva la prueba visual y gana presencia de marca; jerarquía clara y orden
editorial; distinta de G (que abre con el comparador) y de D (brutalista sin evidencia).
**Riesgos:** la prueba sigue dependiendo de un único caso y de un antes/después con ángulos
distintos; la portada alta retrasa la foto en pantallas de 900 px.
**Público:** profesional con trabajo real que quiere una presencia más premium y editorial que
una plantilla de servicios.

### O — Neo-psicodelia esmeralda

**Idea:** la estructura de K en bosque, salvia, marfil y cobre; atmósfera líquida pero terrosa.
**Fortalezas:** identidad completamente distinta con la misma composición; encaja con la
fotografía real (madera, jardín, cal); contraste AA resuelto con dos tonos de cobre.
**Riesgos:** el verde puede derivar en «eco/rustic»; el cobre de texto pequeño necesita su
variante oscura; los orbes oscuros van más contenidos que en K.
**Público:** estudio de reforma premium con discurso material y arquitectónico.

### P — Neo-psicodelia nocturna

**Idea:** K en índigo, medianoche, arena, coral y jade; noche arquitectónica y serena.
**Fortalezas:** la más sobria de las tres paletas; buena legibilidad y bandas oscuras potentes;
el coral queda como acento y línea, sin volver al burdeos.
**Riesgos:** más fría y severa que K; el salto arena→medianoche debe validarse con el cliente;
los orbes de jade son sutiles.
**Público:** vivienda premium y clientes internacionales que prefieren discreción.

### Q — Neo-psicodelia rosa

**Idea:** K en perla, rosa, ciruela y bronce; la paleta más suave y femenina.
**Fortalezas:** cálida y contemporánea sin perder el aire premium; contraste resuelto usando
ciruela y carbón para texto, rosa solo en formas.
**Riesgos:** el rosa sobre perla es 1,7:1 y nunca puede usarse como texto; puede leerse menos
técnica que O o P.
**Público:** interioristas, segundas residencias y clientes que buscan una marca cálida.

### R — Psicodélica solar

**Idea:** la energía de J en cobalto, azafrán, coral y aguamarina; sol y color sin postal.
**Fortalezas:** personalidad inmediata y cálida; CTAs muy visibles; conserva la legibilidad de J
con una paleta completamente nueva.
**Riesgos:** el cobalto con azafrán polariza; exige disciplina de contraste (texto tinta sobre
azafrán, nunca crema); sigue siendo la opción más rupturista.
**Público:** cliente que quiere destacar y un público menos conservador.

### S — G con portada de marca

**Idea:** G sin tocar, salvo la portada: «Pintura y mantenimiento en general» y la foto real de la
furgoneta; el comparador sigue en `02 / Transformaciones`.
**Fortalezas:** presenta el negocio y el oficio antes de pedir contexto para una comparativa;
conserva el argumento de evidencia de G y todo su sistema visual, así que el mantenimiento es
mínimo.
**Riesgos:** la foto de la furgoneta no demuestra trabajo; la prueba real queda más abajo; sigue
dependiendo de un único caso y de ángulos que no coinciden.
**Público:** profesional que quiere una portada de marca reconocible sin renunciar a la prueba
antes/después.

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

### Cuarta tanda

| Criterio | N | O | P | Q | R | S |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Claridad en 5 s | 5 | 4 | 4 | 4 | 3 | 5 |
| Confianza/cercanía | 4 | 4 | 4 | 5 | 4 | 5 |
| Calidad percibida | 5 | 5 | 5 | 4 | 4 | 4 |
| Contacto móvil | 5 | 4 | 4 | 4 | 4 | 5 |
| Diferenciación | 4 | 4 | 4 | 4 | 5 | 4 |
| Legibilidad | 5 | 4 | 4 | 4 | 3 | 4 |
| Mantenimiento | 4 | 2 | 2 | 2 | 3 | 5 |
| Encaje Costa del Sol | 5 | 4 | 4 | 4 | 4 | 5 |

S es G con otra portada: hereda sus notas salvo en diferenciación (misma base que G) y
mantenimiento (no añade sistema visual nuevo). O, P y Q comparten estructura, así que su coste de
mantenimiento es el mismo que K: una sola paleta que hay que respetar en cada edición.

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
- **N** es G con portada de verdad; la opción si gusta la prueba real pero se quiere una
  cabecera con presencia.
- **O, P y Q** son la misma K en tres climas: esmeralda/material, nocturna/sobria y
  rosa/cálida. Eligen paleta, no estructura.
- **R** es J en versión solar; misma energía, color más cálido y menos «rave».
- **S** es G con la portada de marca; si gusta G pero su apertura resulta demasiado
  «comparativa», S es el cambio mínimo.

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
