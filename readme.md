# Simulacro de la Sesión 9 — Incumplimiento de créditos (clasificación)

## Archivos y cuándo van a GitHub

| Archivo | Filas | % incumple | ¿Cuándo al repo? |
|---|---|---|---|
| `proyecto_muestra.csv` | 500 | 19,4% | **Antes de la Sesión 9.** Es lo único que ven al comienzo. |
| `proyecto_nuevo.csv` | 1.500 | 17,3% | **Recién al final de la Sesión 9**, junto con `EECD_MLI_S9_celda_de_cierre.py`. |

(`proyecto_publico.csv` es del diseño anterior, de regresión con precios de autos. No se usa; no lo subas.)

## Pasos
1. Subir `proyecto_muestra.csv` y poner la URL real en `URL_BASE` del notebook.
2. Trabajan hasta las 10:00 con la muestra.
3. Al final, subir `proyecto_nuevo.csv` al mismo repo y compartir la celda de cierre. Calcula el AUC y el AP de su `modelo` sobre créditos nuevos y los compara con su AUC propio.
4. Reportan en el chat.

`proyecto_nuevo.csv` trae la respuesta: es un ensayo, no hay nota. No sirve para el examen real.
Para el examen: un conjunto reservado sin la columna de respuesta y corrección fuera de un repo público (por ejemplo, un pequeño servidor que reciba probabilidades y devuelva solo el puntaje).

## Diseño (para el docente)
- 41 variables + `incumple`. Hay variables casi duplicadas (`ingreso_anual_usd`, `cuota_mensual_usd`, `cuota_sobre_ingreso`), variables relacionadas (`score_buro`, `utilizacion_tarjetas`, `deuda_sobre_ingreso`), códigos sin orden (`zona`, `estado_civil`, `sector_empleo`) y 15 columnas de puro ruido (`indicador_01` a `indicador_15`).
- Baseline: regresión logística con `C=1_000_000` (sin regularización), entrenando con 150 casos. AUC propio 0,69; AUC en créditos nuevos 0,67. Exactitud 0,75, peor que decir siempre «no incumple» (0,80).
- Con regularización sobre la misma partición: L2 con `C=0,01` llega a 0,81 en créditos nuevos; L1 con `C=0,1` a 0,82. Con `C` muy chico en L1 (0,05) se pasa y cae a 0,74. Un árbol sin podar da 0,63 y un Random Forest 0,80.
- Con split 80/20 estratificado y L1 el AUC en nuevos sube a ≈ 0,83.
- Lección: con muchas variables y pocos casos, regularizar es la mejora más grande.

## Diccionario de datos
| Grupo | Columnas |
|---|---|
| Cliente | `edad`, `ingreso_mensual_usd`, `ingreso_anual_usd`, `antiguedad_laboral_anios`, `horas_trabajo_semana`, `sector_empleo` (código 0-7), `estado_civil` (código 0-3), `nivel_educativo` (1-5), `dependientes`, `vivienda_propia`, `meses_residencia`, `zona` (código 0-3) |
| Historial | `deuda_sobre_ingreso`, `atrasos_previos`, `score_buro`, `consultas_buro_6m`, `cantidad_tarjetas`, `utilizacion_tarjetas`, `saldo_tarjetas_usd`, `antiguedad_cuenta_anios`, `saldo_promedio_cuenta_usd` |
| Préstamo | `monto_credito_usd`, `plazo_meses`, `cuota_mensual_usd`, `cuota_sobre_ingreso`, `tiene_garantia` |
| Anónimas | `indicador_01` a `indicador_15` |
| **A predecir** | `incumple` (1 si incumplió) |

Datos sintéticos, generados para este curso.
