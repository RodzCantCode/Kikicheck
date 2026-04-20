# Robotito 💛

Mini web app cozy: tu novia elige 2-3 cartitas de mood, el robotito "analiza" y le devuelve un diagnóstico cariñoso-gracioso + un mensaje tuyo personalizado.

Un único archivo `index.html`. Cero build, cero dependencias instaladas.

## Probar en local

Doble clic en `index.html`, o servirlo rápido con:

```bash
python -m http.server 5173
# o
npx serve .
```

Y abre `http://localhost:5173`.

## Personalizar (lo importante)

Todo lo editable está agrupado al principio del `<script>` en `index.html`, marcado con:

```
🛠️  CONTENIDO PERSONALIZABLE — Mario, edita esta sección a tu gusto
```

Cuatro cosas que puedes cambiar:

1. **`TRAINER_NAME`** — tu nombre. Aparece en el saludo y en las etiquetas de mensaje.
2. **`GREETING`** — título y cuerpo de la pantalla de intro. Usa `{name}` donde quieras que aparezca tu nombre.
3. **`MOODS`** — las 9 cartitas (emoji + label). Los `id` solo se usan internamente, pero si los cambias, acuérdate de actualizar también los `moods` de `RESPONSES`.
4. **`RESPONSES`** — la lista de combos personalizados. Cada uno:
   - `moods`: array de 2 o 3 ids de los moods que hacen match. El orden no importa.
   - `diagnostico`: la voz del robot (cozy, un punto gracioso, tono de "escáner cariñoso").
   - `mensaje`: tu voz directa para ella — esto es lo que hace que la app se sienta personal. Cambia TODOS los mensajes por cosas tuyas reales.
5. **`FALLBACKS`** — respuestas genéricas cuando la combinación que ella elige no está en `RESPONSES`. Se elige una al azar.

### Consejos de contenido

- Cuantos más combos personales escribas, menos veces caerá en el fallback. Con 12-15 ya va sobrada.
- Piensa en cosas suyas concretas: comidas que le gustan, bromas internas vuestras, momentos compartidos. Eso es lo que va a hacer gracia.
- Los fallbacks también son tuyos — cámbialos por 3-4 mensajes cariñosos genéricos pero que suenen a ti.

## Deploy rápido

Cualquiera de estos funciona con la carpeta tal cual (no hay build):

### Netlify Drop (más rápido, 30 segundos)

1. Ve a [app.netlify.com/drop](https://app.netlify.com/drop)
2. Arrastra la carpeta `robotito/` al navegador.
3. Te da una URL pública. Fin.

### Vercel CLI

```bash
npm i -g vercel
vercel --prod
```

### GitHub Pages

1. Crea un repo, push de esta carpeta.
2. Settings → Pages → Branch: `main`, folder: `/ (root)`.
3. En 1-2 minutos tienes `https://<usuario>.github.io/<repo>/`.

## Estructura

```
robotito/
├── index.html                                  ← toda la app
├── README.md                                   ← este archivo
└── docs/superpowers/specs/
    └── 2026-04-20-robotito-design.md           ← spec original
```

## Stack

- HTML + CSS + vanilla JS en un único archivo.
- Google Fonts (Nunito) cargada por CDN.
- Sin frameworks, sin build, sin dependencias.
