# Guion del video (máximo 8 minutos)

Tiempo total estimado: **7 min 30 s**, con margen de 30 s. Repartan los bloques entre los integrantes para que todos hablen.

---

## Bloque 1 · Presentación (0:00 – 0:45) — Integrante 1
**En pantalla:** README del repositorio en GitHub.

> "Hola, somos [nombres], del curso de Análisis de Datos del ITM. En este proyecto exploramos cuatro bases de datos, elegimos la del Titanic, hicimos un análisis exploratorio completo y preparamos los datos con técnicas de preprocesamiento y reducción de dimensionalidad. Todo está en nuestro repositorio de GitHub, organizado en tres notebooks."

Mostrar rápidamente la estructura de carpetas y el historial de commits.

---

## Bloque 2 · Exploración y selección (0:45 – 2:00) — Integrante 1
**En pantalla:** `01_exploracion_bases.ipynb` → tabla resumen y gráfica `01_criterios_seleccion.png`.

> "Exploramos cuatro bases de tres tipos: dos tabulares (Titanic y viajes de taxi en Nueva York), una de texto (mensajes SMS spam) y una de imágenes (dígitos escritos a mano). Todas son fuentes secundarias.
> Las calificamos en completitud, relevancia, documentación, manejabilidad y riqueza para el EDA. Ganó Titanic con 24 de 25 puntos.
> ¿Por qué no las otras? Taxis casi no tiene faltantes; SMS es texto libre y habría que convertirlo a números antes; Digits son 64 píxeles sin significado individual. Titanic, en cambio, tiene variables numéricas y categóricas, faltantes reales y outliers, justo lo que pide el EDA."

---

## Bloque 3 · Calidad: faltantes y outliers (2:00 – 3:30) — Integrante 2
**En pantalla:** `02_eda.ipynb` → `02_valores_faltantes.png`, tabla de faltantes por clase, `02_outliers_fare_por_clase.png`.

> "Encontramos tres variables con faltantes. `deck` le falta al 77 %, pero no al azar: falta casi siempre en segunda y tercera clase, y quienes la tienen sobrevivieron 67 % contra 30 %. Por eso no la imputamos, sino que creamos un indicador. A `age` le falta el 20 %, y la imputamos con la mediana por clase y sexo. `embarked` solo tiene dos faltantes, que llenamos con la moda.
> En outliers, `fare` tiene 116 según el IQR, pero al ver el boxplot por clase descubrimos que son tarifas reales de primera clase. No los eliminamos: aplicamos logaritmo, que baja la asimetría de 4.8 a 0.4.
> También detectamos columnas redundantes, como `alive`, que es lo mismo que `survived` y causaría fuga de información."

**Código a explicar (10 s):** la función `outliers_iqr`.

---

## Bloque 4 · Patrones e hipótesis (3:30 – 5:15) — Integrante 2 / 3
**En pantalla:** `02_supervivencia_por_categoricas.png`, `02_heatmap_clase_sexo.png`, `02_heatmap_correlaciones.png`, tabla de hipótesis.

> "El factor más fuerte es el sexo: sobrevivió el 74 % de las mujeres y solo el 19 % de los hombres. Luego la clase: 63 % en primera, 24 % en tercera. Y hay interacción: las mujeres de primera y segunda superan el 90 %, pero en tercera bajan al 50 %.
> Los niños menores de 12 años sobrevivieron más, y el tamaño de la familia tiene una relación no lineal: viajar solo o en familias grandes redujo la supervivencia.
> Planteamos cinco hipótesis y las verificamos con chi-cuadrado y Mann-Whitney. Todas resultaron significativas; la del sexo tiene el efecto más grande, con una V de Cramér de 0.54."

---

## Bloque 5 · Preprocesamiento y PCA (5:15 – 6:45) — Integrante 3
**En pantalla:** `03_preprocesamiento_pca.ipynb` → celdas de imputación, codificación y escalado; `03_pca_varianza_explicada.png`, `03_pca_cargas.png`, `03_pca_proyeccion.png`, `03_tsne.png`.

> "Eliminamos las columnas redundantes, creamos los indicadores, imputamos, aplicamos logaritmo a la tarifa, codificamos el sexo como binaria y el puerto con One-Hot, y estandarizamos todo con StandardScaler, porque PCA es sensible a la escala.
> Con PCA necesitamos 7 de 11 componentes para explicar el 90 % de la varianza. El último componente explica 0 % porque el tamaño de familia es la suma exacta de `sibsp` y `parch`; PCA detectó esa redundancia.
> Según las cargas, PC1 representa familia y gasto, PC2 la clase y la edad, y PC4 el sexo. En el plano PC1–PC2 los grupos se superponen, porque PCA maximiza varianza, no separación. Con t-SNE sí aparecen grupos claros por sexo y clase, lo que confirma el EDA."

**Código a explicar (15 s):** la imputación con `groupby(...).transform("median")` y el `PCA().fit_transform`.

---

## Bloque 6 · Dificultades y conclusiones (6:45 – 7:30) — Todos
> "Dificultades: decidir qué hacer con `deck` (77 % faltante), distinguir outliers reales de errores, y notar que varias columnas estaban duplicadas con otro nombre.
> Conclusión: en el Titanic sobrevivir dependió sobre todo del sexo y la clase social. Los datos quedaron limpios, codificados y escalados en `titanic_procesado.csv`, listos para entrenar modelos de clasificación en la siguiente fase. Gracias."

---

### Consejos de grabación
- Usen OBS Studio, Loom o la grabación de Zoom/Teams compartiendo pantalla.
- Tengan los notebooks ya ejecutados y las pestañas abiertas en orden antes de grabar.
- Ensayen una vez con cronómetro; si se pasan, recorten el Bloque 4.
