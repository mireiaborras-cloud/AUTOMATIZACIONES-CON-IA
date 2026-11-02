# Memoria I+D

Redacción asistida de memorias técnicas para convocatorias autonómicas de I+D (IVACE+i y AVI). Es una aplicación web de un solo fichero (`index.html`), sin servidor: funciona en el navegador con tu propia clave de IA.

## Cómo usarla

- **En local:** abre `index.html` con Chrome o Edge.
- **En GitHub Pages:** sube `index.html` a la raíz de un repositorio y activa Pages en *Settings → Pages* (rama `main`, carpeta `/`).

Sin clave funciona en **modo demostración**, con un ejemplo y respuestas simuladas, para ver el recorrido completo. Para usarla de verdad, pulsa **IA** y elige OpenAI, Google Gemini o Anthropic Claude.

## Recorrido

1. **Convocatoria.** Elige el programa y sube las bases, la ficha o el modelo de memoria (PDF, DOCX o TXT). El texto se divide en fragmentos numerados [B1]…[BN]. Un agente extrae apartados, criterios y requisitos, y el código comprueba que cada uno aparece literalmente en las bases.
2. **Proyecto.** Título, idea, objetivos, novedad, resultados, S3 y entidades. Es la única fuente admitida para nombres y cifras.
3. **Presupuesto y plan.** Personal, servicios externos, otros gastos, indirectos y paquetes de trabajo. El código aplica las reglas del programa (coste por hora, porcentajes de gestión y difusión, topes por entidad, reparto en consorcio…).
4. **Estado del arte.** Búsqueda en OpenAlex, CrossRef, Semantic Scholar, Europe PMC, PubMed y arXiv, deduplicación por DOI y ranking. Los papers que marques forman el corpus [P1]…[PN].
5. **Generar.** Estructura, redacción por secciones citando [P n] y [B n], evaluación con los criterios de la convocatoria y rondas de mejora que solo se conservan si la nota total sube.

Después, **Memoria** (editar, reescribir con indicaciones y exportar a Word, Markdown o traza JSON) y **Cumplimiento** (comprobaciones calculadas por código).

## Principio

La IA propone y el código comprueba: citas fuera de rango o inventadas se eliminan, las cifras que no están en los datos, las bases o el corpus se señalan, y el presupuesto se valida con reglas explícitas.

## Límites

- Una cita válida demuestra que el paper o el fragmento existe, no que respalde la frase.
- La evaluación simulada es una opinión del modelo, no una predicción de la nota real.
- Las reglas de presupuesto de los programas IVACE+i y AVI vienen de fichas públicas de convocatorias anteriores: confírmalas con las bases vigentes.
- Los datos del proyecto, los fragmentos de las bases y los resúmenes de los papers se envían al proveedor de IA elegido. Si el proyecto es confidencial, comprueba que tu organización lo permite.

## Créditos

La arquitectura (corpus numerado, citación por índice y validación determinista, capa de red para las bases académicas y generación de .docx sin librerías) se basa en [Doctoralia](https://github.com/fborrasumh/doctoralia), de Fernando Borrás Rocher (UMH), publicado con licencia MIT.

El fichero `THIRD_PARTY_LICENSE_doctoralia.txt` contiene la licencia MIT original de Doctoralia y debe acompañar a `index.html` si lo publicas o distribuyes.
