# Predicción del precio de publicaciones de Airbnb

**Trabajo Práctico Final – Ciencia de Datos (Curso I5521)**
Ingeniería Industrial – Universidad Tecnológica Nacional, Facultad Regional Buenos Aires

**Autor:** Facundo Del Bono

---

## Descripción

Airbnb pidió ayuda para predecir el precio por noche de los hospedajes en algunas ciudades de Estados Unidos. Para eso se trabajó con un dataset de **19.309 publicaciones y 29 variables** de San Francisco, Washington DC, Chicago y Boston.

El trabajo tiene tres partes:

1. **Análisis exploratorio (EDA):** limpieza de los datos, creación de nuevas variables y análisis de qué factores influyen en el precio.
2. **Machine Learning:** comparación de cinco modelos de regresión (regresión lineal, Ridge, árbol de decisión, Random Forest y LightGBM) contra un baseline, usando `Pipeline`, `ColumnTransformer` y búsqueda de hiperparámetros con validación cruzada.
3. **Reducción de la dimensionalidad:** aplicación de PCA y nueva predicción con los mismos modelos.

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| `clusterai_facundo_delbono_eda.ipynb` | Análisis exploratorio y limpieza de los datos. Al final genera `airbnb_limpio.csv` |
| `clusterai_facundo_delbono_machine_learning.ipynb` | Modelos de regresión, PCA y comparación de resultados, con la explicación de cada paso |
| `clusterai_facundo_delbono_pipeline.ipynb` | Todas las manipulaciones necesarias para aplicar el modelo, desde el CSV original hasta los resultados finales, sin comentarios |
| `clusterai_reporte_delbono.pdf` | Informe técnico del trabajo |
| `airbnb_us.csv` | Dataset original |
| `airbnb_limpio.csv` | Dataset limpio (generado por el notebook de EDA) |
| `requirements.txt` | Librerías necesarias |

## Cómo ejecutarlo

1. Clonar o descargar el repositorio y mantener todos los archivos en la misma carpeta (los notebooks leen los CSV con rutas relativas).
2. Instalar las librerías:

   ```
   pip install -r requirements.txt
   ```

   Si se usa Anaconda, pandas, numpy, matplotlib, seaborn y scikit-learn ya vienen instaladas; solo hace falta instalar LightGBM:

   ```
   pip install lightgbm
   ```

3. Abrir los notebooks en Jupyter o VS Code y ejecutarlos con **Restart + Run All**.

Orden de ejecución:

- `clusterai_facundo_delbono_eda.ipynb` genera `airbnb_limpio.csv`.
- `clusterai_facundo_delbono_machine_learning.ipynb` lee `airbnb_limpio.csv` (ya incluido en el repositorio, por lo que también puede ejecutarse sin correr antes el EDA).
- `clusterai_facundo_delbono_pipeline.ipynb` es independiente: lee directamente `airbnb_us.csv`.

Los notebooks de Machine Learning y de pipeline tardan alrededor de 5 minutos en ejecutarse, principalmente por la búsqueda de hiperparámetros del Random Forest y de LightGBM. Todos los resultados son reproducibles (`random_state=42`).

## Resultados principales

| Modelo | R² test | MAE (USD) |
|---|---|---|
| Baseline (mediana por ciudad y tipo de habitación) | 0,347 | 93,5 |
| Regresión lineal | 0,577 | 78,1 |
| Ridge | 0,577 | 78,1 |
| Árbol de decisión | 0,590 | 76,2 |
| Random Forest | 0,660 | 68,9 |
| **LightGBM** | **0,668** | **68,5** |

El R² se calcula sobre log(precio) y el MAE en dólares.

- El mejor modelo es **LightGBM**, que explica el 66,8% de la variación del precio con un error medio de 68,5 USD por noche.
- Las variables más importantes son el **tipo de habitación**, el **tamaño** de la propiedad y la **ubicación**, en línea con el análisis exploratorio.
- **PCA** redujo las variables de 54 a 37 manteniendo el 90% de la varianza, pero empeoró todos los modelos (LightGBM bajó de 0,668 a 0,599). Al ser un método no supervisado, descarta componentes de poca varianza que sí eran útiles para predecir el precio.

El análisis completo y la discusión de los resultados se encuentran en `clusterai_reporte_delbono.pdf`.

## Herramientas

Python 3, pandas, NumPy, matplotlib, seaborn, scikit-learn y LightGBM.
