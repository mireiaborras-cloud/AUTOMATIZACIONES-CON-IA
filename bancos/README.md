# Bancos de ejercicios

Aquí van los bancos que usa la [app de práctica](../practica/). Cada banco es un archivo `NOMBRE.json` exportado desde el generador [Preguntas Test](../preguntas-test/) (botón «Descargar el banco»).

## Publicar un banco nuevo

Lo más cómodo es hacerlo desde la sección **Mis bancos** del generador, que sube el archivo, actualiza `index.json` y permite ocultar o eliminar bancos. A mano:

1. Sube el archivo a esta carpeta: **Add file → Upload files**.
2. Edita `index.json` y añade la entrada del banco dentro de la lista `bancos`. El generador te la da con el botón «Copiar entrada del catálogo»:

   ```json
   {"bancos": [
     {"archivo": "ejemplo-prl", "titulo": "Prevención de riesgos laborales", "descripcion": "Ejemplo · FOL", "icono": "🦺", "color": "#00A499"},
     {"archivo": "NOMBRE", "titulo": "…", "descripcion": "…", "icono": "📘", "color": "#26686D"}
   ]}
   ```

3. Comparte con los alumnos el enlace `https://mireiaborras-cloud.github.io/AUTOMATIZACIONES-CON-IA/practica/?banco=NOMBRE`.

El nombre del archivo solo puede llevar letras sin tilde, números y guiones. Si no añades el banco a `index.json`, el enlace directo funciona igualmente, pero no aparecerá en la biblioteca.

Para corregir un banco ya publicado, súbelo de nuevo con el mismo nombre. El progreso de los alumnos se conserva mientras no cambien los identificadores de los ejercicios, que el generador mantiene al editar.

`ejemplo-prl.json` es un banco de ejemplo escrito a mano (Prevención de Riesgos Laborales, módulo de FOL) con todos los tipos de ejercicio.

## Visibilidad

Cada entrada de `index.json` puede llevar `"visible": false`. Entonces el banco no aparece en la biblioteca y su enlace muestra «no disponible». La app de práctica solo abre los bancos que están en `index.json` y no están ocultos. El archivo sigue siendo descargable desde el repositorio, que es público.
