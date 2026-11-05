# Preguntas Test

Genera bancos de preguntas tipo test a partir de un libro en PDF, eligiendo cuántas preguntas quieres en cada tema. Es una aplicación web de un solo fichero (`index.html`), sin servidor: funciona en el navegador con tu propia clave de IA.

Es la primera pieza de una futura aplicación de práctica para alumnos (estilo Duolingo): aquí se generan y revisan las preguntas; la exportación JSON es la que usará esa aplicación.

## Cómo usarla

- **En GitHub Pages:** `https://mireiaborras-cloud.github.io/AUTOMATIZACIONES-CON-IA/preguntas-test/`
- **En local:** abre `index.html` con Chrome o Edge.

Sin clave funciona en **modo demostración** (preguntas simuladas de completar huecos hechas con frases del libro) para ver el recorrido completo. Para usarla de verdad, pulsa **IA** y elige OpenAI, Google Gemini o Anthropic Claude.

## Recorrido

1. **Libro.** Sube el PDF. El texto se extrae en el navegador con pdf.js, página a página, y se quitan las cabeceras y pies repetidos y los números de página. Si el PDF es un escaneo sin texto, avisa de que necesita OCR.
2. **Temas.** Se detectan por este orden:
   - los **marcadores** del PDF;
   - los **encabezados** del tipo «Tema 1», «Unidad 2», «Capítulo 3» (las páginas del índice se saltan);
   - o, si lo pides, la **IA lee el índice** y el código busca cada título en el texto para fijar su página real.

   Puedes corregir las páginas, desmarcar temas, añadirlos a mano y escribir **cuántas preguntas quieres en cada tema**, o repartir un total en proporción a la extensión de cada uno.
3. **Generar.** Idioma, nivel de los alumnos, número de opciones (3, 4 o 5), porcentaje de fácil/media/difícil, verdadero o falso opcional e indicaciones propias.

Después, en **Banco**, revisas y editas cada pregunta y exportas a:
- **JSON**, con temas y preguntas, para la futura app;
- **CSV**, para Excel (separador `;`);
- **GIFT**, para importar en Moodle con una categoría por tema.

## Cómo se generan y se comprueban las preguntas

La IA propone y el código comprueba, como en Doctoralia y Memoria I+D.

- El texto de cada tema se trocea en fragmentos numerados `[F1]…[FN]`. Las preguntas se piden **por lotes repartidos por todo el tema**, para que no se concentren en las primeras páginas.
- Cada pregunta debe indicar su fragmento y una **frase literal** que la justifique. El código busca esa frase en el libro, ignorando tildes y puntuación, y **descarta** la pregunta si no aparece.
- El código también descarta:
  - las preguntas sin una única respuesta válida;
  - las que tienen opciones repetidas;
  - las que usan «todas/ninguna de las anteriores»;
  - las repetidas o casi iguales a otra del banco.
- Si faltan preguntas para llegar al número pedido, hace hasta tres rondas extra con los fragmentos menos usados.
- La posición de la respuesta correcta se reparte entre A, B, C… para que no haya patrones.
- Avisa cuando la opción correcta es mucho más larga que las demás o aparece en el enunciado.
- Opcionalmente, un **agente revisor** comprueba cada respuesta contra su fragmento y marca las dudosas.

Los prompts de los tres agentes (`indexador`, `generador`, `revisor`) se pueden editar en **Agentes**.

## Formato JSON exportado

```json
{
  "formato": "banco-preguntas", "version": 1,
  "libro": {"titulo": "…", "paginas": 240}, "idioma": "castellano", "nivel": "2.º de ESO",
  "temas": [{
    "id": "t…", "orden": 1, "titulo": "Tema 1. …", "paginas": [5, 22],
    "preguntas": [{
      "id": "q…", "tipo": "opcion_multiple", "enunciado": "…",
      "opciones": ["…", "…", "…", "…"], "correcta": 2,
      "explicacion": "…", "dificultad": "media", "pagina": 12,
      "cita": "frase literal del libro", "verificada": true, "revisada": true, "avisos": []
    }]
  }]
}
```

`correcta` es la posición de la opción correcta, empezando en 0. En verdadero o falso, `opciones` es `["Verdadero", "Falso"]`. El mismo JSON se puede volver a subir en el paso «Libro» para seguir editando el banco.

## Límites

- Que la cita exista prueba que la pregunta sale del libro, no que esté bien planteada. El revisor es otro modelo y también puede equivocarse: **revisa las preguntas antes de usarlas en una evaluación**.
- Los fragmentos del libro se envían al proveedor de IA elegido. Usa libros cuyo uso tengas permitido y comprueba que tu centro admite ese proveedor.
- Los PDF escaneados necesitan OCR antes de subirlos.

## Créditos

La arquitectura (fragmentos numerados, cita por índice y validación determinista, capa de IA multiproveedor) y la identidad visual se basan en [Doctoralia](https://github.com/fborrasumh/doctoralia), de Fernando Borrás Rocher (UMH), publicado con licencia MIT. El fichero `THIRD_PARTY_LICENSE_doctoralia.txt` contiene esa licencia y debe acompañar a `index.html` si lo publicas o distribuyes.
