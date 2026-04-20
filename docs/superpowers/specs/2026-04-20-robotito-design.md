# Robotito Intérprete — app cozy de mood

**Fecha:** 2026-04-20
**Estado:** draft

Mini web app "cozy": un robotito (entrenado por Mario) interpreta el estado de ánimo de su novia a partir de cartitas de mood que ella elige, y responde con un diagnóstico cariñoso-gracioso + un mensaje personalizado de Mario.

Sorpresa puntual inspirada en el meme "mi novio no entiende cuándo me pasa algo... quizás sea capaz de construir una app para esto".

## Objetivos

- Sorpresa emocional + chiste autoconsciente: carta de amor disfrazada de mini-app.
- ~2 minutos de jugar, rejugable varias veces sin agotarse.
- Deployable en minutos a Netlify/Vercel/GitHub Pages.

## No-objetivos (YAGNI)

- Persistencia, login, base de datos, backend.
- IA real, NLP, ML — el "análisis" es un diccionario escrito a mano.
- Multiusuario, compartir resultados, stats.
- Multi-idioma (solo ES).
- Desktop optimizado (prioridad móvil; desktop debe verse decente pero no es foco).
- Accesibilidad avanzada (cumplir lo básico, no WCAG AA).

## User flow

1. **Intro** — pantalla de bienvenida con el robotito y saludo corto tipo "Hola 💛 Me ha entrenado Mario durante mucho tiempo. Cuéntame cómo estás y te ayudo." + botón "Empezar".
2. **Selección de mood** — grid de 9 cartitas. Ella elige 2 o 3. Contador visible ("Elige 2 o 3"). Botón "Analizar" se activa con ≥2 seleccionadas.
3. **Análisis** — ~2 segundos. Robot con animación de "pensando" (wobble + puntitos) y texto cambiante ("analizando... cruzando datos... consultando mis notas...").
4. **Resultado** — diagnóstico del robot + mensaje personalizado de Mario. Botones: "Otra vez" (vuelve al paso 2, resetea selección) y "Volver al inicio".

## Cartitas de mood (9 fijas)

Propuesta inicial, ajustable por Mario:

1. 🌧️ Día gris
2. 😩 Agotada
3. 🫂 Necesito mimos
4. 🍫 Antojo
5. 🎬 Plan sofá
6. 🔥 Cabreada
7. 🥰 Tierna
8. 💭 Pensativa
9. ✨ Feliz

## Lógica de "adivinación"

No es IA. Es un diccionario de combinaciones escritas a mano.

- **Combos priorizados**: Mario escribe 12-15 combinaciones concretas (ej. "gris + antojo", "agotada + mimos", "cabreada + pensativa") con su diagnóstico + mensaje.
- **Fallback**: 3-4 respuestas cariñosas genéricas al azar cuando la combinación no está en el diccionario.
- **Normalización**: las combinaciones se tratan como sets — "gris+antojo" == "antojo+gris". Orden de selección irrelevante.
- **Selección múltiple** = combinación única. No se suman diagnósticos.

Formato de cada respuesta:

```js
{
  moods: ["gris", "antojo"],
  diagnostico: "Detecto día de lluvia interior con antojo de algo dulce. Protocolo: mantita, peli tonta, y chocolate sin remordimiento.",
  mensaje: "Si estás así y yo no estoy contigo ahora, acuérdate de que te quiero un montón. Y de que mañana te compro un Kinder Bueno."
}
```

`diagnostico` = voz del robot (cozy, gracioso, tono de "escáner cariñoso"). `mensaje` = voz de Mario (directo, cariñoso, personal).

## Estética

- **Paleta**: crema de fondo (`#FBF6EE`), terracota (`#D98B6F`), salvia (`#9BB2A1`), lavanda (`#C9B8DA`), texto marrón oscuro (`#3A2E28`). Definida en variables CSS al inicio para ajustar fácil.
- **Tipografía**: Nunito o Quicksand vía Google Fonts. Pesos 400 y 700.
- **Robot**: SVG inline, redondeado, ojitos grandes, antenita. Estilo "cute starter" — suficiente para funcionar, reemplazable si Mario quiere mejorarlo.
- **Microanimaciones (CSS puro)**:
  - Robot pestañea cada 4-6s.
  - Hover en cartas: leve wobble + elevación.
  - Al seleccionar: check animado + borde destacado.
  - Pantalla de análisis: wobble del robot + 3 puntitos que pulsan + texto que cambia.
  - Transiciones entre pantallas: fade + slide corto.
- **Layout móvil-first**, ancho máximo 480px centrado en desktop.

## Tech & estructura

- **Stack**: 1 archivo `index.html` con Tailwind via CDN + vanilla JS inline. Cero build, cero dependencias instaladas. Funciona abriéndolo con doble click.
- **Estructura del repo**:
  ```
  robotito/
    index.html          ← toda la app
    README.md           ← instrucciones para Mario
    docs/superpowers/specs/2026-04-20-robotito-design.md
  ```
- **JavaScript**: `<script>` al final del body. Estado global simple (`state` con `screen`, `selectedMoods`). Función `render()` única que pinta según `state.screen`. Sin frameworks, ~200-300 líneas.
- **Deploy**: arrastrar carpeta a Netlify Drop, o `vercel --prod`, o push a GitHub Pages. 1-2 minutos.

## Arquitectura JS

- `MOODS` — array de las 9 cartitas (`{id, emoji, label}`).
- `RESPONSES` — array de combos personalizados + fallbacks.
- `state` — `{screen, selectedMoods}`.
- `render()` — switch sobre `state.screen` (intro | select | analyzing | result).
- `findResponse(selectedIds)` — busca combo exacto (comparando sets), si no, fallback aleatorio.
- Handlers pequeños para click de cartas, "analizar", "otra vez", "volver".

## Contenido que aporta Mario

Placeholders bien marcados en el código para que Mario rellene:

1. Saludo inicial (1-2 frases).
2. 12-15 combos personalizados (2 moods + diagnóstico + mensaje cada uno).
3. 3-4 mensajes fallback cariñosos genéricos.
4. (Opcional) Ajustar labels/emojis de las 9 cartitas.

El plan de implementación construirá la app funcional con contenido placeholder, y marcará claramente las secciones a personalizar.

## Criterios de éxito

- Se abre en el móvil de la novia, se ve cozy, se juega en ~2 minutos.
- Los 12-15 combos cubren la mayoría de selecciones probables — rara vez cae en fallback.
- Al menos un diagnóstico la hace reír y al menos un mensaje la emociona.
- URL pública compartible en menos de 1 hora de trabajo total.

## Riesgos y mitigaciones

- **Que no se sienta personal** → Mario escribe los mensajes, no se dejan placeholders genéricos al publicar.
- **Combos insuficientes** → fallback cubre huecos. Con 9 cartas, 36 combos de 2 + 84 de 3; no hace falta cubrir todos, solo los probables para *su* novia.
- **Estética "de plantilla"** → microanimaciones + paleta cuidada + robot con personalidad.
