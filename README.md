# Análisis de Evasión de Clientes (Churn) – Telecom X

Proyecto end-to-end de exploración, limpieza, transformación y análisis de la evasión (*churn*) de clientes para el reto Telecom X (LATAM). Incluye extracción desde fuente pública (GitHub Raw), control de calidad de datos, ingeniería ligera de variables, análisis exploratorio, generación de artefactos y modelo baseline opcional.

## 🎯 Objetivos
- Comprender el comportamiento de los clientes y factores asociados a la evasión.
- Construir un pipeline reproducible (Extracción → Transformación → Análisis → Informe → Baseline ML).
- Generar artefactos reutilizables (parquets, métricas, reportes CSV, checklist de requisitos).

## 📁 Estructura Principal del Repo
```
├─ TelecomX_LATAM.ipynb          # Notebook principal (flujo completo)
├─ README.md                     # Este documento
└─ data/                         # Artefactos y datasets derivados
   ├─ TelecomX_Data_local.parquet
   ├─ TelecomX_transformado.parquet
   ├─ TelecomX_limpio.parquet
   ├─ TelecomX_model_ready.parquet
   ├─ dictionary_variables.csv
   ├─ summary_columns.csv
   ├─ relevance_simple.csv
   ├─ relevance_mutual_info.csv (si sklearn disponible)
   ├─ quality_nulls.csv
   ├─ quality_overview.csv
   ├─ descriptive_numeric.csv
   ├─ churn_categorical_spread.csv
   ├─ churn_numeric_diff.csv
   ├─ correlation_matrix.csv (opcional)
   ├─ baseline_metrics.json (opcional – modelo)
   ├─ model_prep_metadata.json (opcional – metadata modelado)
   └─ requisitos_checklist.csv
```

## 🔄 Flujo Metodológico
| Etapa | Descripción | Resultado Clave |
|-------|-------------|-----------------|
| 1. Extracción | Carga desde JSON remoto (API Raw GitHub) | `TelecomX_Data_local.parquet` |
| 2. Transformación 2.1–2.5 | Exploración, diccionario, control de incoherencias, limpieza, derivaciones, estandarización opcional | `TelecomX_transformado.parquet`, `TelecomX_limpio.parquet` |
| 3. Análisis (EDA) | Descriptivo, distribución de churn, categóricas, numéricas, correlaciones | CSVs de métricas + gráficos en notebook |
| 4. Modelado Baseline (Opcional) | Regresión logística sobre dataset preparado | `TelecomX_model_ready.parquet`, `baseline_metrics.json` |
| 5. Informe Final | Síntesis narrativa de pasos, hallazgos y recomendaciones | Sección “Informe Final” en el notebook |
| 6. Checklist | Verificación automática de requisitos | `requisitos_checklist.csv` |

## 🧪 Principales Transformaciones
- Normalización de strings (espacios, casing, variantes Yes/No/Sí → 1/0).
- Conversión de columnas *object* numéricas usando heurística (>95% parseable).
- Identificación y posible conversión de fechas.
- Derivación de `Cuentas_Diarias` (MonthlyCharges / 30) si existe columna mensual.
- Creación de dataset listo para modelado (one-hot para baja cardinalidad + escalado numérico).

## 📊 Análisis Exploratorio Destacado
- Descriptivos para variables numéricas (`describe`).
- Distribución absoluta y porcentual de churn.
- Tasa de churn por categoría (spread para priorizar variables).
- Diferencias de medias y test t para numéricas clave.
- Heatmap de correlaciones (opcional, guardado como CSV).

## 🤖 Modelo Baseline (Opcional)
- Regresión Logística con división estratificada 75/25.
- Métricas guardadas: ROC-AUC, precision, recall, F1, soporte.
- Artefactos: `baseline_metrics.json`, matriz de confusión, curva ROC (en notebook).

## ✅ Checklist de Requisitos
La última celda del notebook genera un resumen y persiste `data/requisitos_checklist.csv` con el estado (True/False) de cada punto del flujo solicitado.

## 🚀 Cómo Ejecutar (Local)
1. Crear entorno (opcional pero recomendado):
   ```bash
   python -m venv .venv
   # Windows PowerShell
   .venv\Scripts\Activate.ps1
   ```
2. Instalar dependencias mínimas:
   ```bash
   pip install pandas numpy pyarrow fastparquet seaborn matplotlib scikit-learn scipy
   ```
3. Abrir y ejecutar el notebook `TelecomX_LATAM.ipynb` en orden (o usar “Run All”).
4. Revisar artefactos en `data/` y completar los placeholders del Informe Final con hallazgos concretos.

## 🧷 Reproducibilidad
- Se emplean formatos Parquet para eficiencia y consistencia de tipos.
- Limpieza y derivaciones idempotentes: al re-ejecutar, los archivos se sobre-escriben de forma controlada.
- Para congelar el entorno: `pip freeze > requirements-lock.txt`.

## 🔐 Consideraciones de Calidad
- Conversión robusta de tipos con manejo de errores.
- Detección de cardinalidad alta para evitar sobrecodificación.
- Separación clara de dataset “limpio” vs “model ready”.

## 📈 Posibles Mejores Futuras
- Feature engineering temporal (cohortes, antigüedad segmentada).
- Modelos avanzados (Gradient Boosting, Random Forest, XGBoost, LightGBM).
- Interpretabilidad (SHAP, Permutation Importance).
- Ajuste de umbrales según costo de falsos negativos (churn real no detectado).
- Pipeline formal (scikit-learn `Pipeline` + validación cruzada estratificada).
- Integración continua de métricas (GitHub Actions).

## 🗂️ Data Privacy
El dataset es público (fuente del challenge). No se incluyen datos personales sensibles.

## 📜 Licencia
Agregar una licencia adecuada (ej.: MIT) en `LICENSE` si el repositorio se hace público.

## 🙌 Agradecimientos
- Fuente del reto: Alura / Challenge Data Science LATAM.
- Comunidad open-source (pandas, scikit-learn, seaborn, etc.).

---
Si necesitas un `requirements.txt` generado o adaptación para despliegue (API / dashboard), abre un issue o añade la solicitud.
