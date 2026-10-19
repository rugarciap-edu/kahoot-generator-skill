# Generador de Kahoots

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

- **Claude Desktop:** en Configuración, abre **Personalización > Skills** y usa la
  opción para subir un skill. Selecciona
  `dist/generador-kahoot-v0.0.1.zip`.

<img width="789" height="495" alt="image" src="https://github.com/user-attachments/assets/afcf2ce8-0f82-496b-8efd-d2294343732f" />

Los nombres de las opciones pueden variar ligeramente según la versión de la
aplicación. Consulta [la guía de instalación](docs/instalacion.md).

## Uso

Pide un Kahoot, adjunta el material y especifica el título y la cantidad de
preguntas (por defecto, 20; máximo, 200). El skill genera el Excel listo para
importar. 

Como cualquier otra skill, la puedes invocar directamente:

<img width="693" height="164" alt="image" src="https://github.com/user-attachments/assets/351c0c33-d6ad-4348-862f-e0bb7e109f80" />

Para cargar el resultado en Kahoot, edita o crea uno nuevo, añade una pregunta y, finalmente,
elige la pestaña/opción **Importar desde hoja de cálculo** (al final de las
opciones de tipos de pregunta) y selecciona el `.xlsx`.

<img width="621" height="621" alt="image" src="https://github.com/user-attachments/assets/2c7eb812-8957-4422-a765-b980b9bcad73" />

Kahoot! y sus marcas y logotipos son propiedad de sus respectivos titulares.
Este proyecto es independiente y no está afiliado, patrocinado ni avalado por Kahoot!.

Consulta [la guía de uso](docs/uso.md) para el flujo completo.
