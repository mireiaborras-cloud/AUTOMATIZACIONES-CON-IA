# Práctica · app tipo Duolingo para los alumnos

Una sola app para todos los bancos de ejercicios que genera [Preguntas Test](../preguntas-test/). No usa IA ni servidor: lee un banco JSON de la carpeta [`bancos`](../bancos/) y guarda el progreso de cada alumno en su navegador.

## Enlaces

- **Biblioteca** (todos los bancos del catálogo): `https://mireiaborras-cloud.github.io/AUTOMATIZACIONES-CON-IA/practica/`
- **Un banco concreto:** `…/practica/?banco=NOMBRE`, que carga `bancos/NOMBRE.json`. Por ejemplo, `…/practica/?banco=ejemplo-prl`.
- **Vista previa** desde el generador, sin publicar: `…/practica/?vista=previa`. Solo funciona en el mismo navegador y sitio que el generador.

## Cómo funciona para el alumno

- Cada **tema** es una unidad del camino, con sus **lecciones** (8 ejercicios por defecto, configurable en el banco) y un **repaso** final con trofeo.
- Las lecciones se desbloquean en orden dentro de cada tema y van de lo fácil a lo difícil. Los temas están todos abiertos.
- Cada lección empieza con **5 vidas**. Un fallo quita una vida y el ejercicio vuelve al final de la lección. Sin vidas, se reintenta.
- **XP** por lección (+5 si es perfecta), **racha** de días seguidos y **«Repasa tus fallos»** con los ejercicios fallados. Un fallo sale de esa lista cuando se acierta a la primera en otra lección.
- Seis tipos de ejercicio: opción múltiple, con imagen, verdadero o falso, completar huecos, emparejar y ordenar pasos. Las opciones se barajan cada vez.
- Funciona en el móvil y admite teclado: números para elegir y Enter para comprobar.

## Resultados para el profesor (opcional)

Si en el generador escribes una dirección en «Dirección para enviar resultados», el banco la incluye. Entonces la app pide el nombre al alumno la primera vez y, al terminar cada lección, envía un `POST` con este contenido:

```json
{"banco": "…", "titulo": "…", "alumno": "…", "tipo": "leccion", "tema": "…", "leccion": 1,
 "ejercicios": 8, "aciertosPrimera": 7, "fallados": ["id…"], "xp": 10, "segundos": 95, "fecha": "…"}
```

Para recogerlos con Power Automate:
1. Crea un flujo con el desencadenador **«Cuando se recibe una solicitud HTTP»**.
2. Añade **«Analizar JSON»** sobre el cuerpo de la solicitud (`json(triggerBody())`, porque llega como texto).
3. Añade una fila a un Excel o a una lista de SharePoint, y desde ahí puedes llevarlo a Power BI.
4. Pega la URL del desencadenador en el generador y vuelve a publicar el banco.

La app envía los datos como texto plano y sin esperar respuesta, para que funcione desde GitHub Pages sin problemas de CORS.

Si no pones dirección, los alumnos solo practican y no se envía nada.
