# TFM — Predicción de ventas para una cadena minorista (Store Sales – Time Series Forecasting)

Trabajo Fin de Máster (Big Data, Data Science & Inteligencia Artificial, UCM) sobre predicción de ventas diarias por tienda y familia de producto, a partir del dataset público de Kaggle [Store Sales – Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting), con el histórico real de ventas de **Corporación Favorita** (cadena minorista ecuatoriana).

## Estructura del repositorio

```
├── notebooks/          # Notebooks del análisis, en orden de ejecución (ver más abajo)
├── data/
│   ├── raw/             # CSVs originales de Kaggle (NO incluidos en el repo, ver "Datos")
│   ├── processed/       # Dataset final de modelado (df_model) y fechas de corte de la validación
│   └── results/         # Métricas y predicciones de cada modelo, generadas por los notebooks
├── figures/             # Figuras generadas por los notebooks (una carpeta por fase)
├── models/              # Modelos entrenados guardados (.joblib)
```

## Datos

Los archivos CSV originales **no están incluidos en este repositorio** por su tamaño (están en `.gitignore`). Para reproducir el análisis:

1. Descarga los datos desde la competición de Kaggle: **[Store Sales – Time Series Forecasting](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data)** (requiere cuenta de Kaggle y aceptar las reglas de la competición).
2. Coloca los siguientes archivos en `data/raw/`:
   - `train.csv`
   - `test.csv`
   - `stores.csv`
   - `oil.csv`
   - `holidays_events.csv`
   - `transactions.csv`
   - `sample_submission.csv`

> **Uso de los datos:** este dataset se distribuye bajo los términos de la competición de Kaggle, restringidos a **uso académico y no comercial** (sección 7.A, *Data Access and Use*, de las [reglas de la competición](https://www.kaggle.com/competitions/store-sales-time-series-forecasting/rules)). No contiene datos personales.

Las carpetas `data/processed/` y `data/results/` se generan automáticamente al ejecutar los notebooks 03, 04 y 05; no es necesario crearlas a mano.

## Requisitos

Python 3.10+ y las siguientes librerías:

```
pandas
numpy
matplotlib
scikit-learn
lightgbm
xgboost
catboost
statsmodels
shap
joblib
pyarrow
psutil
jupyter
```

Instalación rápida:

```bash
pip install pandas numpy matplotlib scikit-learn lightgbm xgboost catboost statsmodels shap joblib pyarrow psutil jupyter
```

## Cómo ejecutar

Los notebooks deben ejecutarse **en orden**, desde la carpeta `notebooks/` (usan rutas relativas del tipo `Path("..")` para acceder a `data/`, `figures/` y `models/`):

1. **`01_descripcion_datos.ipynb`** — Carga y descripción de las 7 fuentes de datos.
2. **`02_analisis_exploratorio_figuras.ipynb`** — Análisis exploratorio (EDA): variable objetivo, patrones temporales, tiendas, familias, promociones, festivos, petróleo.
3. **`03_preparacion_datos_figuras.ipynb`** — Limpieza, tratamiento de nulos/outliers e ingeniería de variables. Genera `data/processed/train_prepared.parquet`, el dataset que usan el resto de notebooks.
4. **`04_modelado_baseline_figuras.ipynb`** — Diseño de la validación temporal y modelos de referencia (baseline y series temporales clásicas).
5. **`05_modelos_machine_learning_v2_figuras.ipynb`** — Comparación de modelos de machine learning (Ridge, árboles, Random Forest, HistGradientBoosting, XGBoost, LightGBM, CatBoost) y ajuste de hiperparámetros. **Nota:** este notebook está pensado para ejecutarse un modelo por sesión (reiniciando el kernel entre modelos) por limitaciones de memoria — el modelo a entrenar se selecciona con una variable al inicio del notebook.
6. **`06_resultados_interpretabilidad_figuras.ipynb`** — Evaluación desagregada del modelo final e interpretabilidad (importancia de variables, permutation importance, SHAP).

### Antes de ejecutar 01 y 02

Estos dos notebooks tienen la ruta a los datos en bruto escrita como ruta absoluta de mi propio equipo. Si clonas este repositorio, cámbiala por una ruta relativa antes de ejecutar, por ejemplo:

```python
DATA_PATH = Path("../data/raw")
```

(Los notebooks 03-06 ya usan rutas relativas mediante `PROJECT_ROOT = Path("..")` y no necesitan este cambio.)
