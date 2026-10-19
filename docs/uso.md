# Uso

## Generar el cuestionario

1. En Claude, pide que prepare un Kahoot e incluye o adjunta el texto, apuntes o
   documento de referencia.
2. Indica un título y el número de preguntas. Si no indicas la cantidad, se
   preparan 20; la plantilla admite hasta 200.
3. Indica si quieres otro idioma, dificultad o preguntas de respuesta múltiple.
   Si no especificas esos detalles, se usa una respuesta correcta por pregunta y
   dificultad progresiva.
4. Revisa el Excel que Claude genere y descarga/guarda el `.xlsx`.

El generador comprueba los límites de Kahoot (120 caracteres por enunciado y 75
por respuesta), utiliza la plantilla incluida y distribuye las respuestas
correctas entre las posiciones. Si una pregunta excede un límite, hay que
reformularla y generar el archivo de nuevo; no se deben truncar las respuestas.

## Importar el Excel en Kahoot

1. Abre Kahoot e inicia sesión.
2. Crea un Kahoot o abre uno para editarlo.
3. En el editor, selecciona **Añadir pregunta**.
4. En el selector de tipos de pregunta, ve a la pestaña/opción del final:
   **Importar desde hoja de cálculo** (*Import spreadsheet*).
5. Selecciona **Subir archivo** y elige el `.xlsx` generado. Si Kahoot ofrece una
   plantilla descargable, la plantilla usada por este skill ya sigue el formato
   de importación de Kahoot.
6. Comprueba la vista previa y confirma la importación. Revisa en el editor las
   preguntas, los tiempos y las respuestas correctas antes de guardar/publicar.

La opción de importar una hoja de cálculo está en el flujo de añadir preguntas
dentro del editor del Kahoot; no es una importación desde el panel principal de
Kahoot.

## Generar manualmente desde la línea de comandos

El archivo de entrada es un JSON UTF-8 con un título y una lista de preguntas:

```json
{
  "titulo": "Fotosíntesis - Repaso",
  "preguntas": [
    {
      "pregunta": "¿En qué orgánulo se realiza la fotosíntesis?",
      "correcta": "Cloroplasto",
      "incorrectas": ["Mitocondria", "Ribosoma", "Aparato de Golgi"],
      "tiempo": 20
    }
  ]
}
```

Ejecuta el script desde la carpeta del skill:

```powershell
python scripts\build_kahoot.py --plantilla assets\KahootQuizTemplate.xlsx --json preguntas.json --salida "Mi Kahoot.xlsx"
```

Para una pregunta de respuesta múltiple, `correcta` puede ser una lista, por
ejemplo `"correcta": ["Opción A", "Opción B"]`. Usa entre 2 y 4 respuestas en
total y uno de los tiempos permitidos: 5, 10, 15, 20, 30, 45, 60, 90, 120, 180 o
240 segundos.