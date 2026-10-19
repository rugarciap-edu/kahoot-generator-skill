---
name: generador-kahoot
description: Genera cuestionarios de repaso de Kahoot listos para importar, en el formato Excel oficial de la plataforma (plantilla KahootQuizTemplate.xlsx). Úsala siempre que el usuario pida un Kahoot, un cuestionario, un test tipo quiz, preguntas de repaso o de opción múltiple a partir de un texto, PDF, apuntes, temario o documentación, aunque no mencione Kahoot ni Excel explícitamente (p. ej. "hazme preguntas de repaso de este tema", "un quiz para mis alumnos sobre este PDF").
---

# Generador de Kahoots

Convierte cualquier material (texto pegado, PDF, Word, apuntes, documentación técnica…) en un Kahoot de repaso en formato Excel, listo para importar en Kahoot mediante "Importar desde hoja de cálculo".

## 1. Recoger los datos antes de empezar

Necesitas tres cosas. Pide solo las que falten, en un único mensaje breve:

1. **Título del Kahoot.**
2. **Número de preguntas.** Por defecto 20. El usuario puede pedir más o menos, con un máximo de 200, que es lo que admite la plantilla.
3. **Contenido** sobre el que redactar las preguntas (texto, PDF, documento, enlace…).

Si el usuario ya ha adjuntado contenido y dado el título, no vuelvas a preguntar: asume 20 preguntas y dilo en una línea al entregar. Si no da título pero sí contenido, pregunta solo el título (o, si prefiere no decidir, propón uno a partir del tema).

## 2. Localizar la plantilla

Usa siempre la plantilla oficial de Kahoot, no crees un Excel desde cero: Kahoot rechaza archivos cuya estructura no coincide exactamente. Búscala en este orden:

1. `assets/KahootQuizTemplate.xlsx` dentro de esta skill.
2. Archivos subidos por el usuario o del proyecto (`/mnt/user-data/uploads/`, o cualquier `.xlsx` cuyo nombre contenga "kahoot" o "template").

Si no aparece en ningún sitio, pide al usuario que la adjunte (se descarga desde Kahoot al elegir "Importar desde hoja de cálculo").

Estructura de la plantilla (hoja `Sheet1`; el script detecta las cabeceras automáticamente):

- Filas 2–6: instrucciones de Kahoot. No se tocan.
- Fila 8: cabeceras en las columnas B–H.
- Fila 9: pregunta de ejemplo de Kahoot. El script la sobrescribe con la primera pregunta. Si quedara en el archivo, se importaría como una pregunta más.
- Columna A: numeración 1–200 ya rellenada. La plantilla admite como máximo 200 preguntas.

| Columna | Cabecera | Contenido | Límite |
|---|---|---|---|
| B | Question - 120 max length | Enunciado | 120 caracteres |
| C–F | Answer 1 … Answer 4 - 75 max length | Respuestas. Las respuestas 3 y 4 son opcionales | 75 caracteres |
| G | Time limit | Segundos: 5, 10, 15, 20, 30, 45, 60, 90, 120, 180 o 240 | — |
| H | Correct answer(s) | Número de la respuesta correcta (`3`) o varias separadas por comas (`1,4`) | — |

La plantilla incluye validaciones de datos y formato condicional: el texto se pone en rojo si se pasa de longitud. El script los conserva.

## 3. Redactar las preguntas

Lee el contenido completo antes de escribir. Objetivo: un repaso que ayude a fijar lo importante, no a pillar al alumno.

- **Cobertura:** reparte las preguntas por todo el material, priorizando conceptos clave, definiciones, relaciones causa-efecto, datos relevantes y aplicación práctica. Evita detalles triviales y no repitas el mismo concepto dos veces.
- **Solo del contenido:** cada respuesta correcta debe poder justificarse con el material aportado. No introduzcas datos externos que contradigan o amplíen la fuente.
- **Enunciados:** claros, en el idioma del material (o el que pida el usuario), sin dobles negaciones y de ≤120 caracteres. En Kahoot se leen deprisa en una pantalla proyectada: cuanto más directos, mejor.
- **Respuestas:** 4 opciones por defecto, ≤75 caracteres, de longitud y estilo parecidos (que la correcta no sea delatada por ser la más larga o la más precisa). Distractores plausibles, basados en errores típicos o conceptos cercanos del propio temario. Evita "Todas las anteriores" / "Ninguna de las anteriores", porque al barajar las posiciones pierden sentido.
- **Verdadero/Falso:** puedes incluir alguna pregunta de 2 opciones (Verdadero / Falso) si encaja, pero que no superen el 20 % del total.
- **Una sola correcta** salvo que el usuario pida preguntas de respuesta múltiple.
- **Tiempo:** 20 s por defecto. Usa 30–45 s para preguntas que requieren cálculo o una lectura más larga, y 10–15 s para preguntas muy rápidas o de Verdadero/Falso. Usa solo los valores permitidos: 5, 10, 15, 20, 30, 45, 60, 90, 120, 180 o 240.
- **Dificultad:** progresiva, de más sencillas a más complejas.

## 4. Posición aleatoria de las respuestas correctas

Es un requisito esencial: si la correcta está siempre en la misma columna, los jugadores lo detectan enseguida y el repaso deja de servir. Por eso no coloques tú las respuestas a mano; escribe en el JSON la correcta aparte de las incorrectas y deja que el script las baraje. El script:

- reparte las posiciones correctas de forma equilibrada entre las columnas disponibles (en 20 preguntas de 4 opciones, unas 5 por columna),
- evita que la correcta caiga en la misma columna más de 2 veces seguidas,
- y verifica el resultado al final, fallando si la distribución no es válida.

## 5. Generar el Excel

1. Escribe las preguntas en un JSON en `/home/claude/preguntas.json` con este formato:

```json
{
  "titulo": "Fotosíntesis - Repaso tema 4",
  "preguntas": [
    {
      "pregunta": "¿En qué orgánulo se realiza la fotosíntesis?",
      "correcta": "Cloroplasto",
      "incorrectas": ["Mitocondria", "Ribosoma", "Aparato de Golgi"],
      "tiempo": 20
    },
    {
      "pregunta": "La clorofila absorbe sobre todo la luz verde.",
      "correcta": "Falso",
      "incorrectas": ["Verdadero"],
      "tiempo": 10
    }
  ]
}
```

Para respuesta múltiple usa `"correcta": ["A", "B"]`.

2. Ejecuta el script:

```bash
python scripts/build_kahoot.py \
  --plantilla assets/KahootQuizTemplate.xlsx \
  --json /home/claude/preguntas.json \
  --salida "/mnt/user-data/outputs/<Título>.xlsx"
```

Si el script informa de textos demasiado largos, reescribe esas preguntas o respuestas (no las recortes a lo bruto) y vuelve a ejecutarlo.

3. Revisa el resumen que imprime (distribución de correctas por columna) y comprueba que el número de preguntas coincide con el solicitado.

## 6. Entregar

Presenta el archivo `.xlsx` con `present_files` y añade un mensaje breve con:

- título y número de preguntas,
- la distribución de respuestas correctas por posición (p. ej. "1: 5 · 2: 5 · 3: 5 · 4: 5"),
- recordatorio de cómo importarlo: en Kahoot, *Crear → Añadir pregunta → Importar desde hoja de cálculo* y subir el archivo.

No listes todas las preguntas en el chat salvo que el usuario lo pida; ya están en el Excel.
