# Proyecto de Análisis de Datos evento_2 1 — Supervivencia en el Titanic

**Instituto Tecnológico Metropolitano (ITM)** · Ingeniería de Sistemas · Curso: Análisis de Datos
**Docente:** Daniel Alexis Nieto Mora · **Semestre:** 2026-2 · **Evento evaluativo 2**

## Integrantes

| Nombre | Usuario GitHub | Responsabilidad principal |
|--------|----------------|---------------------------|
| Darío Montoya | [@dariomontoya0505](https://github.com/dariomontoya0505) | _Creador del repositorio y cargue de informacion_ |
| _2_ | _@Casper128_ , _@Franyelica_ | _Tema 2_ |
| _3_ | _@santiagopabon15_ | _Tema 3_ |

## Objetivo

Explorar varias bases de datos de distintos tipos, seleccionar una con criterios explícitos, realizar un **Análisis Exploratorio de Datos (EDA)** completo y preparar los datos para un futuro modelado mediante **preprocesamiento y reducción de dimensionalidad (PCA y t-SNE)**.

## Bases de datos exploradas

| Base | Tipo | Origen | Registros × atributos | Resultado |
|------|------|--------|-----------------------|-----------|
| **Titanic** | Tabular | Secundaria | 891 × 15 |  **Seleccionada** (24/25) |
| Taxis NYC (marzo 2019) | Tabular + tiempo | Secundaria | 6 433 × 14 | 21/25 |
| Digits (dígitos 8×8) | Imágenes | Secundaria/terciaria | 1 797 × 64 | 20/25 |
| SMS Spam Collection | Texto | Secundaria | 5 572 × 2 | 19/25 |

Criterios: completitud, relevancia, documentación, manejabilidad y riqueza para el EDA. Titanic se eligió porque combina variables numéricas y categóricas, tiene **valores faltantes reales de distinta magnitud** (`deck` 77 %, `age` 20 %, `embarked` 0.2 %), **outliers legítimos** (`fare`) y una variable objetivo clara para plantear hipótesis.

## Estructura del repositorio

```
proyecto-analisis-datos/
├── README.md
├── requirements.txt
├── data/
│   ├── titanic.csv               # base seleccionada
│   ├── taxis.csv                 # base explorada
│   ├── sms_spam.tsv              # base explorada (texto)
│   └── titanic_procesado.csv     # salida de la Fase 3
├── notebooks/
│   ├── 01_exploracion_bases.ipynb      # Fase 1
│   ├── 02_eda.ipynb                    # Fase 2
│   └── 03_preprocesamiento_pca.ipynb   # Fase 3
├── figures/                      # gráficas exportadas por los notebooks
└── docs/
    └── guion_video.md            # guion del video (máx. 8 min)
```

La base de imágenes (Digits) se carga directamente desde `scikit-learn`, por eso no está en `data/`.

## Cómo ejecutar

```bash
git clone https://github.com/dariomontoya0505/proyecto-analisis-datos.git
cd proyecto-analisis-datos
pip install -r requirements.txt
jupyter notebook notebooks/
```

Ejecutar los notebooks en orden (01 → 02 → 03). También se pueden abrir en Google Colab subiendo la carpeta `data/`.

## Resumen de cada fase

### Fase 1 — Exploración (`01_exploracion_bases.ipynb`)
Carga y caracterización de 4 bases de 3 tipos (tabular, texto, imágenes): fuente, tipo de origen, tamaño, faltantes, documentación y aplicaciones. Matriz de criterios y justificación de la elección.

### Fase 2 — EDA (`02_eda.ipynb`)
Calidad (duplicados, columnas redundantes), valores faltantes y su mecanismo, outliers (IQR, z-score, boxplots), distribuciones y asimetría, análisis univariado, tablas cruzadas, correlaciones de Pearson y Spearman, pairplot, y **5 hipótesis verificadas con pruebas estadísticas** (chi-cuadrado y Mann-Whitney).

### Fase 3 — Preprocesamiento y reducción (`03_preprocesamiento_pca.ipynb`)
Eliminación de columnas redundantes, indicadores de faltantes, imputación por mediana agrupada, transformación `log1p`, codificación binaria y One-Hot, estandarización, **PCA** (varianza explicada, cargas, biplot) y comparación con **t-SNE**.

## Hallazgos principales

1. **El sexo es el factor más determinante:** sobrevivió el 74 % de las mujeres y el 19 % de los hombres (V de Cramér = 0.54).
2. **La clase social influyó:** 63 % de supervivencia en 1.ª clase, 47 % en 2.ª y 24 % en 3.ª.
3. **Sexo y clase interactúan:** mujeres de 1.ª y 2.ª clase > 90 %; hombres de 2.ª y 3.ª < 16 %.
4. **Los niños (≤ 12 años) sobrevivieron más** (58 % frente a 39 %).
5. **Relación no lineal con la familia:** solos 30 %, familias de 2–4 personas 58 %, familias de 5 o más 16 %.
6. **El faltante de `deck` es informativo:** con cubierta registrada sobrevivió el 67 %; sin ella, el 30 %.
7. **Cherbourg tiene mayor supervivencia por composición:** el 51 % de quienes embarcaron allí eran de 1.ª clase.
8. **PCA:** se requieren 7 de 11 componentes para el 90 % de la varianza; PC1 resume familia y gasto, PC2 clase y edad, PC4 el sexo. t-SNE separa grupos claros por sexo y clase.

## Problemas de calidad y tratamiento

| Problema | Tratamiento |
|----------|-------------|
| `deck` 77 % faltante (no aleatorio) | Se elimina y se crea el indicador `tiene_cubierta` |
| `age` 20 % faltante | Mediana por `pclass` y `sex` + indicador `edad_faltante` |
| `embarked` 2 faltantes | Moda (`S`) |
| Columnas redundantes (`alive`, `class`, `embark_town`, `alone`, `adult_male`, `who`) | Eliminadas (`alive` causaría fuga de información) |
| `fare` sesgada (asimetría 4.8) con outliers reales | Se conservan y se aplica `log1p` (asimetría 0.39) |
| Escalas distintas | `StandardScaler` |

## Herramientas

Python 3, pandas, NumPy, Matplotlib, Seaborn, SciPy y scikit-learn.

## Fuentes de datos

- Titanic y Taxis NYC: repositorio [`mwaskom/seaborn-data`](https://github.com/mwaskom/seaborn-data).
- Fuente original: Kaggle, Titanic – Machine Learning from Disaster: https://www.kaggle.com/competitions/titanic/data (archivo train.csv). Se usó la versión derivada del repositorio seaborn-data, que contiene los mismos 891 pasajeros.
- SMS Spam Collection: Almeida, T. A. & Gómez Hidalgo, J. M. (2011), UCI Machine Learning Repository.
- Digits: *Optical Recognition of Handwritten Digits*, UCI Machine Learning Repository (vía `sklearn.datasets.load_digits`).

## Video

🎥 Enlace al video explicativo:
