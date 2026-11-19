# Datos del curso «Validación y Monitoreo de Modelos de Riesgo de Crédito»

Datos sintéticos de **Vicuña Financiera**, una financiera ficticia de créditos de consumo en cuotas (montos en USD). Son del curso de Academia Bayes y los notebooks los leen directo desde aquí. Los datos son ficticios y no describen a ninguna persona ni empresa real.

## Cómo leerlos

```python
import pandas as pd

BASE = "https://raw.githubusercontent.com/nexolabs-gh/datos-validacion/main/v1"
produccion = pd.read_parquet(f"{BASE}/a_produccion.parquet")
```

Si GitHub no responde, el espejo es `https://cdn.jsdelivr.net/gh/nexolabs-gh/datos-validacion@main/v1`.

## Versiones

- **`v1/` es inmutable:** ningún archivo cambia; solo se agregan archivos nuevos, y cada agregado se avisa.
- **`v1/manifiesto.json`** trae el sha256 y el tamaño de cada archivo, para comprobar que lees los mismos bytes:

```python
import hashlib, json, urllib.request

BASE = "https://raw.githubusercontent.com/nexolabs-gh/datos-validacion/main/v1"
manifiesto = json.load(urllib.request.urlopen(f"{BASE}/manifiesto.json"))
datos = urllib.request.urlopen(f"{BASE}/a_produccion.parquet").read()
assert hashlib.sha256(datos).hexdigest() == manifiesto["a_produccion.parquet"]["sha256"]
```

## Dos carteras

- **A** (prefijo `a_`): la de las clases.
- **B** (prefijo `b_`): la de las entregas evaluadas. Cada alumno trabaja una muestra personal de B que sale de su correo.

## El caso

Vicuña usa el **Score de Admisión Consumo v1.2** desde 2024-01. Hoy es el **corte 1 (2026-06-30)** y tú haces la revisión periódica independiente. El **corte 2 (2026-09-30)** se usa en la última clase y en la entrega final.

| Archivo | Contenido |
|---|---|
| `desarrollo` | Solicitudes 2021-01 → 2022-06 con las que se construyó el modelo, su mora a 12 meses y la partición del desarrollador |
| `produccion`, `produccion_corte2` | Solicitudes 2024-01 → 2026-06 y 2026-07 → 2026-09 tal como llegaron al motor, con el score y la decisión registrados |
| `desempeno_corte1`, `desempeno_corte2` | Mora observada de los créditos aprobados a cada corte |
| `tabla_puntajes.json` | La especificación aprobada: tramos, puntos, cortes de grado y PD por grado |
| `monitor_oficial_2026T2` | El informe trimestral del dueño del modelo |
| `inventario`, `usos_observados`, `hallazgos_previos`, `cambios_motor`, `politica.json` | El expediente de gobierno del modelo |
| `genai_eval_interno`, `genai_eval_cliente`, `genai_acciones` | Registros de evaluación de los dos asistentes de IA generativa |

Los umbrales de `politica.json` son convenciones de la entidad ficticia, no exigencias de un regulador.

## Formato

Las tablas son Parquet y la especificación y la política, JSON en UTF-8. Cada archivo lleva el prefijo de su cartera: `a_produccion.parquet`, `b_tabla_puntajes.json`. Los montos están en USD.

## Uso

Material docente de Academia Bayes.
