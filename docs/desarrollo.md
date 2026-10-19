# Desarrollo

## Estructura

```text
generador-kahoot/
  .claude-plugin/plugin.json
  skills/generador-kahoot/
    SKILL.md
    assets/KahootQuizTemplate.xlsx
    scripts/build_kahoot.py
claude-desktop/generador-kahoot/SKILL.md
dist/
  generador-kahoot-v0.0.1.zip
  generador-kahoot-claude-desktop.zip
docs/
```

`generador-kahoot/skills/generador-kahoot/SKILL.md` es el original y no se debe
editar para preparar la variante de Claude Desktop. Las instrucciones adaptadas
para importar se mantienen en `claude-desktop/generador-kahoot/SKILL.md`.

## Generador Excel

`scripts/build_kahoot.py` carga la plantilla con `openpyxl`, localiza sus
cabeceras, valida cantidad de preguntas, longitudes, respuestas y tiempos, coloca
las respuestas correctas en posiciones equilibradas y guarda el resultado.
Admite una semilla opcional con `--semilla` para repetir la aleatoriedad.

Desde `generador-kahoot/skills/generador-kahoot`, instalar la dependencia y
ejecutar una comprobación básica:

```powershell
python -m pip install openpyxl
python scripts\build_kahoot.py --plantilla assets\KahootQuizTemplate.xlsx --json preguntas.json --salida "prueba.xlsx" --semilla 42
```

Comprueba que el resultado indique el número de preguntas y la distribución de
correctas. Importa el `.xlsx` en un Kahoot de prueba para verificar que Kahoot lo
acepta y que la vista previa coincide.

## Actualizar paquetes

Al cambiar el generador o su plantilla, actualiza el paquete original desde la
carpeta `generador-kahoot` y vuelve a construir el ZIP de importación de Claude
Desktop, incorporando dentro de su carpeta raíz el `SKILL.md` adaptado, la
plantilla y el script. Mantén los dos paquetes separados: sus instrucciones de
uso y rutas no son intercambiables.