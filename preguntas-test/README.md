# Preguntas Test · generador de ejercicios

Genera bancos de ejercicios tipo Duolingo a partir de un libro, eligiendo cuántos ejercicios quieres en cada tema. Está pensado para alumnado de **Formación Profesional**. Es una aplicación web de un solo fichero (`index.html`), sin servidor: funciona en el navegador con tu propia clave de IA.

Es la mitad del sistema. Los bancos que genera los usan los alumnos en la [app de práctica](../practica/).

## Cómo usarla

- **En GitHub Pages:** `https://mireiaborras-cloud.github.io/AUTOMATIZACIONES-CON-IA/preguntas-test/`
- **En local:** abre `index.html` con Chrome o Edge.

Sin clave funciona en **modo demostración**, con ejercicios simulados hechos con frases del libro, y trae un **libro de ejemplo** de Prevención de Riesgos Laborales con señales de seguridad. Para usarla de verdad, pulsa **IA** y elige OpenAI, Google Gemini o Anthropic Claude. Para los ejercicios con imagen, el modelo debe admitir imágenes; los modelos por defecto de los tres proveedores lo hacen.

## Recorrido

1. **Libro.** Sube un PDF, un Word (.docx) o un texto (.txt, .md). El texto y las **imágenes del libro** se extraen en el navegador:
   - **PDF:** página a página, sin cabeceras repetidas ni números de página. Se guardan las figuras con su pie y se descartan los logotipos que se repiten.
   - **Word y texto:** se agrupa en bloques de unas 450 palabras, y cada título de tema abre un bloque nuevo.

   En una galería puedes descartar las imágenes que no sirvan.
2. **Temas.** Se detectan por:
   - los **marcadores** del PDF o los **títulos** del Word o del Markdown;
   - los **encabezados** del tipo «Tema 1», «Unidad 2» (saltando el índice);
   - o la **IA leyendo el índice**, y el código busca cada título en el texto para fijar dónde empieza.

   Escribes **cuántos ejercicios quieres en cada tema**, o repartes un total según la extensión de cada uno.
3. **Generar.** Eliges:
   - el nivel (por defecto, ciclo formativo de FP);
   - la dificultad (por defecto 50 % fácil, 40 % media y 10 % difícil);
   - el idioma;
   - la **mezcla de ejercicios**: opción múltiple, con imagen, completar huecos, emparejar, ordenar pasos y verdadero o falso.

Después, en **Banco**:
- revisas y editas cada ejercicio;
- lo **publicas en la app de práctica** con título, icono, color y número de ejercicios por lección, o lo pruebas antes con «Vista previa»;
- o lo exportas a CSV (Excel) y GIFT (Moodle). GIFT no admite los ejercicios de ordenar ni los de imagen, así que esos se omiten.

## Cómo se comprueban los ejercicios

La IA propone y el código comprueba.

- El texto de cada tema se trocea en fragmentos numerados `[F1]…[FN]` y se pide por **lotes repartidos por todo el tema**.
- Cada ejercicio debe indicar su fragmento y una **frase literal** que lo justifique. Si no aparece en el libro, se descarta.
- En **completar**, la frase con el hueco relleno con la respuesta correcta debe ser **idéntica** a una frase del libro.
- El código descarta además:
  - las preguntas sin una única respuesta válida o con opciones repetidas;
  - las que usan «todas/ninguna de las anteriores»;
  - los ejercicios de emparejar con pares repetidos y los de ordenar con menos de 3 pasos;
  - los repetidos o casi iguales a otro del banco.
- **Imágenes:** se envían a un modelo con visión, de cuatro en cuatro, con su pie y el texto de esas páginas. La IA descarta las decorativas. Si la respuesta no se apoya en el pie ni en el texto, el ejercicio se marca para que lo revises.
- Si un tema no tiene imágenes o procesos que ordenar, esos ejercicios se convierten en opción múltiple. Si faltan para llegar al número pedido, se hacen hasta tres rondas más con los fragmentos menos usados.
- La posición de la respuesta correcta se reparte entre A, B, C y D, y se avisa de pistas involuntarias (respuesta mucho más larga, o que aparece en el enunciado).
- Un **revisor** opcional comprueba cada solución contra su fragmento.

Los prompts de los cuatro agentes (`indexador`, `generador`, `imagenes`, `revisor`) se pueden editar en **Agentes**.

## Formato del banco (JSON, versión 2)

```json
{
  "formato": "banco-preguntas", "version": 2,
  "presentacion": {"titulo": "…", "descripcion": "…", "icono": "🦺", "color": "#00A499", "porLeccion": 8},
  "resultados": {"url": "https://…"},
  "temas": [{"id": "…", "titulo": "Tema 1. …", "paginas": [5, 22], "ejercicios": [
    {"tipo": "opcion_multiple", "enunciado": "…", "opciones": ["…"], "correcta": 2},
    {"tipo": "imagen", "imagen": "I3", "enunciado": "…", "opciones": ["…"], "correcta": 0},
    {"tipo": "completar", "frase": "La ___ es …", "opciones": ["…"], "correcta": 1},
    {"tipo": "emparejar", "pares": [{"a": "término", "b": "definición"}]},
    {"tipo": "ordenar", "pasos": ["primero", "segundo", "tercero"]},
    {"tipo": "verdadero_falso", "enunciado": "…", "opciones": ["Verdadero", "Falso"], "correcta": 1}
  ]}],
  "imagenes": {"I3": "data:image/jpeg;base64,…"}
}
```

Cada ejercicio lleva además `id`, `explicacion`, `dificultad`, `pagina`, `cita`, `verificada`, `revisada` y `avisos`. `correcta` es la posición de la opción correcta, empezando en 0. En «ordenar», los pasos van en el orden correcto y la app de práctica los baraja. `resultados` solo aparece si has puesto una dirección para recibir las notas. El banco se puede volver a subir en el paso «Libro» para seguir editándolo; también acepta los bancos de la versión 1.

## Límites

- Que la cita exista prueba que el ejercicio sale del libro, no que esté bien planteado. **Revisa los ejercicios antes de usarlos en una evaluación.**
- Los fragmentos y las imágenes del libro se envían al proveedor de IA elegido. Usa libros cuyo uso tengas permitido y comprueba que tu centro admite ese proveedor.
- Los PDF escaneados necesitan OCR. Los esquemas dibujados con vectores en el PDF (no como imagen) no se extraen como figura.

## Créditos

La arquitectura (fragmentos numerados, cita por índice y validación determinista, capa de IA multiproveedor) y la identidad visual se basan en [Doctoralia](https://github.com/fborrasumh/doctoralia), de Fernando Borrás Rocher (UMH), publicado con licencia MIT. El fichero `THIRD_PARTY_LICENSE_doctoralia.txt` contiene esa licencia y debe acompañar a `index.html` si lo publicas o distribuyes.
