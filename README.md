# Generador de Kahoot

Skill para convertir apuntes, documentos o texto en un cuestionario y un Excel
compatible con la importación de hojas de cálculo de Kahoot. Incluye la plantilla
oficial y un generador que valida límites y baraja las respuestas correctas.

## Qué incluye

- `generador-kahoot/skills/generador-kahoot/SKILL.md`: instrucciones originales
  del skill. 
- `generador-kahoot/skills/generador-kahoot/assets/KahootQuizTemplate.xlsx`:
  plantilla de Kahoot oficial a fecha de creación del skill.
- `generador-kahoot/skills/generador-kahoot/scripts/build_kahoot.py`: generador
  del libro Excel.
- `dist/generador-kahoot-v0.0.1.zip`: Zip que puedes importar directamente en Claude.
- `docs/`: instalación, uso y desarrollo.

## Instalación rápida

- **Claude Desktop:** en Configuración, abre **Capabilities > Skills** y usa la
  opción para cargar/importar un skill. Selecciona
  `dist/generador-kahoot-v0.0.1.zip`.

Los nombres de las opciones pueden variar ligeramente según la versión de la
aplicación. Consulta [la guía de instalación](docs/instalacion.md).

## Uso

Pide un Kahoot, adjunta el material y especifica el título y la cantidad de
preguntas (por defecto, 20; máximo, 200). El skill genera el Excel listo para
importar. 

Para cargarlo en Kahoot, edita o crea un Kahoot, añade una pregunta,
elige la pestaña/opción **Importar desde hoja de cálculo** (al final de las
opciones de tipos de pregunta) y selecciona el `.xlsx`.

Consulta [la guía de uso](docs/uso.md) para el flujo completo.