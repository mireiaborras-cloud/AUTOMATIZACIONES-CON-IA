# Práctica · app tipo Duolingo para los alumnos

Una sola app para todos los bancos de ejercicios que genera [Preguntas Test](../preguntas-test/). No usa IA ni servidor: lee un banco JSON de la carpeta [`bancos`](../bancos/) y guarda el progreso de cada alumno en su navegador. Se adapta al móvil y al ordenador.

## Enlaces

- **Biblioteca** (todos los bancos del catálogo): `https://mireiaborras-cloud.github.io/AUTOMATIZACIONES-CON-IA/practica/`
- **Un banco concreto:** `…/practica/?banco=NOMBRE`, que carga `bancos/NOMBRE.json`. Por ejemplo, `…/practica/?banco=anatomia-aparato-locomotor`.
- **Vista previa** desde el generador, sin publicar: `…/practica/?vista=previa`. Solo funciona en el mismo navegador y sitio que el generador.

## Cómo funciona para el alumno

- **Tests por tema.** Cada test tiene 15 preguntas al azar del tema (configurable en el generador con «Preguntas por test»).
  - La selección va por niveles: primero salen, al azar, las preguntas que el alumno aún no ha visto; después, las que ha fallado; y por último, las menos repetidas. Con 200 preguntas por tema, en unos 14 tests de 15 han salido todas.
  - Se corrige cada pregunta al momento, con la solución y la explicación.
- **Modelo de examen.** De 20, 40 o 60 preguntas (o todas), sacadas de todos los temas en proporción a su tamaño.
  - Por defecto se corrige **al final, como en un examen real**. Desmarcando la casilla, se corrige al momento.
- **Resultado:** nota sobre 10, aciertos, tiempo y **revisión de errores** con tu respuesta, la correcta y la explicación.
- **Repasa tus fallos:** 15 preguntas falladas cada vez; salen de la lista cuando se aciertan a la primera.
- **Seguimiento:** en cada tema, preguntas dominadas, mejor nota y número de tests; también los últimos resultados, la XP y la racha de días.
- Seis tipos de ejercicio: opción múltiple, con imagen, verdadero o falso, completar huecos, emparejar y ordenar pasos. Las opciones se barajan cada vez.
- **Teclado:** números para elegir y Enter para comprobar.

## Resultados para el profesor (opcional)

Si en el generador escribes una dirección en «Dirección para enviar resultados», el banco la incluye. La app pide el nombre al alumno la primera vez y, al terminar cada test o examen, envía un `POST` con este contenido:

```json
{"banco": "…", "titulo": "…", "alumno": "…", "modo": "tema|examen|fallos", "tema": "…",
 "preguntas": 15, "aciertos": 12, "nota": 8, "fallados": ["id…"], "xp": 24, "segundos": 310, "fecha": "…"}
```

Para recogerlos con Power Automate:
1. Crea un flujo con el desencadenador **«Cuando se recibe una solicitud HTTP»**.
2. Añade **«Analizar JSON»** sobre `json(triggerBody())`, porque llega como texto.
3. Añade una fila a un Excel o a una lista de SharePoint, y desde ahí puedes llevarlo a Power BI.
4. Pega la URL del desencadenador en el generador y vuelve a publicar el banco.

Si no pones dirección, los alumnos solo practican y no se envía nada.
