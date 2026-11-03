# Comparación A/B/C — Fase 2

Estado: **prototipos listos para decisión del propietario**. Fecha: 2026-11-03.
Autorización: directa del propietario para tres direcciones A/B/C.
Salida pendiente: elección de la dirección definitiva (D7).

## 1. Prototipos

| ID | Dirección | Directorio | URL local | Capturas |
| --- | --- | --- | --- | --- |
| A | Artesano contemporáneo | `prototypes/a` | <http://localhost:4321/> | [`a/screenshots`](../../prototypes/a/screenshots) |
| B | Precisión luminosa | `prototypes/b` | <http://localhost:4322/> | [`b/screenshots`](../../prototypes/b/screenshots) |
| C | Cuaderno de obra | `prototypes/c` | <http://localhost:4323/> | [`c/screenshots`](../../prototypes/c/screenshots) |

Cada una incluye `/`, `/servicios/reforma-de-banos/` y
`/trabajos/remodelacion-integral-de-bano/`.

**Sala de comparación local:** <http://localhost:4330/> (`prototypes/index.html`).
**Captura de conjunto:** [`screenshots/comparison.png`](screenshots/comparison.png).

## 2. Base común

- Mismo `content.json` en las tres (hash idéntico).
- Mismas ocho fotos reales.
- Mismo orden de servicios y proyectos.
- Contenido provisional marcado de forma visible: alcance, proceso, materiales, garantía y
  preguntas del servicio; ubicación y foto del antes del proyecto.
- Sin reseñas, precios, garantías, certificaciones, ubicaciones ni fotos inventadas.
- Sin fuentes externas, CDNs, cookies, analítica ni formularios.

## 3. Verificación técnica

Ejecutada por el coordinador con Chrome headless y un script común:

| Comprobación | A | B | C |
| --- | --- | --- | --- |
| Build de 3 páginas | Pasa | Pasa | Pasa |
| HTTP 200 en las 3 rutas | Pasa | Pasa | Pasa |
| Un solo H1 por página | Pasa | Pasa | Pasa |
| Cero `<script>` | Pasa | Pasa | Pasa |
| Sin desbordamiento a 390/1440 | Pasa | Pasa | Pasa |
| Imágenes cargadas y con alt | Pasa | Pasa | Pasa |
| Sin peticiones externas | Pasa | Pasa | Pasa |
| Sin errores de consola | Pasa | Pasa | Pasa |
| Enlaces vacíos | 0 | 0 | 0 |

## 4. Comparación cualitativa

Valoración del coordinador, no una decisión. Escala 1–5.

| Criterio | A | B | C |
| --- | ---: | ---: | ---: |
| Claridad en 5 segundos | 4 | 5 | 4 |
| Confianza y cercanía | 5 | 4 | 4 |
| Calidad percibida del trabajo | 5 | 4 | 4 |
| Contacto desde móvil | 4 | 5 | 4 |
| Diferenciación local | 4 | 3 | 5 |
| Legibilidad y accesibilidad | 5 | 5 | 3 |
| Facilidad de mantenimiento | 4 | 5 | 3 |
| Encaje Costa del Sol / oficio | 5 | 5 | 4 |

## 5. Fortalezas y riesgos

### A — Artesano contemporáneo

**Fortalezas:** identidad cálida y humana; fotografía grande con encuadre editorial; jerarquía
muy clara; banda de contacto potente; servicios como lista numerada fácil de escanear. Es la
dirección que mejor equilibra oficio, cercanía y calidad percibida.

**Riesgos:** la tipografía serif y el papel cálido pueden leerse como menos técnicos; depende de
fotos bien encuadradas; el contacto no tiene botón flotante persistente.

### B — Precisión luminosa

**Fortalezas:** la más clara y rápida de escanear; grid y datos explícitos; teléfono y WhatsApp
con enorme peso; excelente para transmitir orden, método y respuesta. La más fácil de mantener y
extender a más servicios.

**Riesgos:** es la más corporativa y la menos diferencial; puede parecer un estudio genérico si
la fotografía no sostiene la credibilidad; pierde parte del componente emocional.

### C — Cuaderno de obra

**Fortalezas:** la más diferenciada; convierte el portfolio en evidencia (fichas, figuras,
anotaciones, antes/después); tono honesto y de proceso; muy memorable. Cuenta el trabajo, no sólo
lo decora.

**Riesgos:** las etiquetas mono de 9–10 px son pequeñas; la densidad visual exige simplificar en
móvil; es la más costosa de mantener y la que más se aleja de una web corporativa convencional.

## 6. Lectura del coordinador

- **A** es la opción más equilibrada para captar contactos y transmitir cercanía sin perder
  calidad percibida.
- **B** es la más segura y clara; buena si el propietario prioriza orden y rapidez sobre
  diferenciación.
- **C** es la más memorable y narrativa; si se elige, hay que subir tamaños de etiqueta y
  simplificar la densidad antes de implementar.

La decisión es del propietario. No se implementa ninguna dirección hasta que la elija.

## 7. Contenido pendiente de confirmar

Antes de la fase 3/4, independientemente de la dirección:

- alcance real de cada servicio;
- proceso, materiales y garantía;
- ubicación del proyecto del baño y foto del antes;
- fotos originales y autorización de uso;
- logo vectorial;
- textos legales;
- reseñas reales y Google Business Profile.

## 8. Siguiente paso

1. El propietario elige A, B o C.
2. Se registra la decisión y la fecha.
3. Se extraen tokens de diseño y se conserva el prototipo elegido como referencia.
4. Se archivan o limpian los descartados.
5. Se pasa a la fase 3 (fundaciones y esqueleto) con la dirección aprobada.

Sin merge, staging, DNS, Hostinger, WordPress, redirecciones ni despliegue.
