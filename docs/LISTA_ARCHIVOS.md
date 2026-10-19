# Lista de archivos del repositorio

El repositorio contiene 22 archivos: seis en la raíz, tres en `docs/` y trece en `generated/`.

## Raíz

| Archivo | Función |
|---|---|
| `README.md` | Presentación, inicio rápido, conclusión, paper y Zenodo. |
| `CYBER.ipynb` | Notebook principal con sus salidas históricas. |
| `requirements.txt` | Dependencias del entorno. |
| `.gitignore` | Define los archivos que Git puede incorporar al repositorio. |
| `LICENSE` | Licencia MIT aplicable al código del proyecto, que establece las condiciones de uso, modificación y redistribución, conservando el reconocimiento de autoría. |
| `CITATION.cff` | Archivo de citación académica que identifica al autor, el artículo científico y su DOI, facilitando la generación de referencias bibliográficas en GitHub. |


## docs

| Archivo | Función |
|---|---|
| `USO_NOTEBOOK.md` | Instalación, rutas, flags y diferentes ejecuciones con y sin Drive. |
| `LISTA_ARCHIVOS.md` | Relación de archivos, dimensiones de los datos y uso de los resultados. |
| `Aprendizaje supervisado, clustering difuso e interpretabilidad con un enfoque KDD-XAI para la caracterización del cibertráfico IoT en un entorno controlado de forma privada.pdf` | Paper original. |

## generated

El notebook lee y escribe los datos y resultados bajo `path`, cuyo valor predeterminado es `./generated`. Xorg/yorg permiten utilizar el recorrido con intermedios sin volver a descargar el dataset.

| Archivo | Dimensiones | Función |
|---|---|---|
| `Xorg.csv` | 123.117 filas, 84 columnas | Exportación de las 83 variables predictoras originales más el índice guardado. Se carga con `fetchdataset=False`. |
| `yorg.csv` | 123.117 filas, 2 columnas | Etiqueta Attack_type más el índice guardado, alineada con Xorg. Se carga con `fetchdataset=False`. |
| `df_org_limpio.csv` | 117.922 filas, 113 columnas | Intermedio que se carga con `limpieza=False`. |
| `dfanálisis.csv` | 34.375 filas, 53 columnas | Variante analítica con reducción de dos_ddos: conserva 7.075 registros de ese grupo. El recorrido principal no la selecciona automáticamente. |
| `dfanálisis_alld2.csv` | 117.922 filas, 53 columnas | Conjunto analítico completo, con 90.622 registros dos_ddos. Se carga sin EDA y en clasificación. |
| `dftests_clusters_gruposk.csv` | 19 filas, 5 columnas | Inercia, silhouette, Davies-Bouldin y Calinski-Harabasz para K=2 a 20 sobre PCA. |
| `dftests_clusters_gruposk_xscaled.csv` | 19 filas, 5 columnas | Las mismas métricas sobre el espacio escalado sin PCA. |
| `centroskm_k17_25dpca.csv` | 17 filas, 50 columnas | Centroides K-Means reconstruidos desde PCA; salida de consulta, no carga de un modelo. |
| `centroides_fuzzy.csv` | 17 filas, 50 columnas | Centroides fuzzy reconstruidos; salida de consulta. |
| `resultados_clasificacion_attack_type.csv` | 10 filas, 12 columnas | Comparación agregada de diez algoritmos para el target de doce etiquetas. No es el informe final del test. |
| `resultados_clasificacion_folds_attack_type.csv` | 100 filas, 4 columnas | Diez particiones por algoritmo, con Algorithm, Fold, Accuracy y ROC AUC. |
| `eli5_weights_xgb_et_opt.csv` | 50 filas, 3 columnas | Importancia por permutación: feature, weight y std. Para interpretar los valores, ten en cuenta el modelo, el target y los datos utilizados. |
| `correlaciones_completas.xlsx` | Sheet1: 6.903 filas, 9 columnas | Pares de variables y correlaciones. La función exige col_x, col_y, best_corr, abs_best_corr y best_method. |

## Reutilización de resultados

Para utilizar una copia completa sin descargar datos ni repetir EDA, sigue el escenario B de [la guía de ejecución](USO_NOTEBOOK.md), con `fetchdataset=False`, `limpieza=False` y `analisis_eda=False`.

Las tablas de selección de K, comparación de algoritmos e importancias se pueden cargar mediante sus flags. Los centroides sirven para consultar resultados: no restauran el clustering. Las matrices PCA y de pertenencia fuzzy se exportan durante la ejecución, pero el notebook no las carga para omitir sus cálculos.

La configuración del escenario B recalcula SHAP con `shap_save=True`. Para reutilizar valores SHAP de una ejecución anterior, consulta el escenario D de la guía.

La fuente, la licencia y la atribución de RT-IoT2022 se indican en el [README principal](../README.md).
