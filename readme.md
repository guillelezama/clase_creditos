# Simulacro de la Sesión 9 — Incumplimiento de créditos (clasificación)

## Archivos y cuándo van a GitHub

| Archivo | Filas | % incumple | ¿Cuándo al repo? |
|---|---|---|---|
| `proyecto_muestra.csv` | 500 | 18% | **Antes de la Sesión 9.** Es lo único que ven al comienzo. |
| `proyecto_nuevo.csv` | 1.500 | 15,5% | **Recién al final de la Sesión 9**, junto con `EECD_MLI_S9_celda_de_cierre.py`. |

(`proyecto_publico.csv` es del diseño anterior, de regresión con precios de autos. No se usa; no lo subas.)

## Pasos
1. Subir `proyecto_muestra.csv` y poner la URL real en `URL_BASE` del notebook.
2. Trabajan hasta las 10:00 con la muestra.
3. Al final, subir `proyecto_nuevo.csv` al mismo repo y compartir la celda de cierre. Calcula el AUC y el AP de su `modelo` sobre créditos nuevos y los compara con su AUC propio.
4. Reportan en el chat.

`proyecto_nuevo.csv` trae la respuesta: es un ensayo, no hay nota. No sirve para el examen real.
Para el examen: un conjunto reservado sin la columna de respuesta y corrección fuera de un repo público (por ejemplo, un pequeño servidor que reciba probabilidades y devuelva solo el puntaje).

## Diseño (para el docente)
- Baseline: regresión logística, AUC en créditos nuevos ≈ 0,78. Un Random Forest llega a ≈ 0,81. Un árbol profundo cae a ≈ 0,66 (sobreajuste); k-NN ≈ 0,74.
- Con ~18 positivos en el test propio, el AUC propio varía entre ≈ 0,68 y 0,92 según la partición: es la lección de ruido y de «decidir gasta datos».
- Trampas: `zona` es un código sin orden (conviene one-hot); la exactitud del baseline (≈ 0,87) apenas supera la de decir siempre «no incumple» (≈ 0,82); el desbalance sugiere `stratify`.

## Diccionario de datos
| Columna | Qué es |
|---|---|
| `edad` | Edad del cliente, en años. |
| `ingreso_mensual_usd` | Ingreso mensual, en dólares. |
| `antiguedad_laboral_anios` | Años en el empleo actual. |
| `deuda_sobre_ingreso` | Deuda total / ingreso (0 a 1). |
| `atrasos_previos` | Cantidad de atrasos en créditos anteriores. |
| `monto_credito_usd` | Monto del crédito. |
| `plazo_meses` | Plazo del crédito. |
| `tiene_garantia` | 1 si hay garantía. |
| `zona` | Código de zona (0 a 3), sin orden. |
| `incumple` | **Variable a predecir.** 1 si incumplió. |

Datos sintéticos, generados para este curso.
