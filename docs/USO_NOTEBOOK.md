# Guía de ejecución de CYBER.ipynb

## 1. Preparar el entorno

### VS Code o Jupyter en tu ordenador

Abre la raíz del proyecto e instala las dependencias en un entorno:

```powershell
py -3 -m venv .venv
./.venv/Scripts/python.exe -m pip install -r requirements.txt
```

En Linux/macOS puedes crear el entorno con `python3 -m venv .venv` e instalar con `.venv/bin/python -m pip install -r requirements.txt`. Selecciona el kernel de ese entorno en el notebook. Configura `installs_flag=False` después de instalar las dependencias; mantén activos los imports.

### Colab sin Drive

Copia o descarga el repositorio en la sesión, de modo que el notebook, `requirements.txt` y los archivos de `generated/` estén disponibles. Abrir solo el notebook desde GitHub no descarga sus archivos auxiliares.

Puedes instalar las dependencias ejecutando en una celda:

```python
%pip install -r /content/Attack-pattern-private-study/requirements.txt
```

Adapta esa ruta al nombre real de la carpeta. Después configura `installs_flag=False` y utiliza como `path` la carpeta `generated` de la sesión. También es posible dejar `installs_flag=True` y utilizar las instalaciones incluidas en el notebook.

Los archivos de `/content` pertenecen a la sesión temporal. Descarga los resultados que quieras conservar antes de cerrarla.

### Colab con tu propio Drive

Copia el proyecto o sus archivos de ejecución a tu unidad. En la celda de ruta escribe, por ejemplo:

```python
path = "/content/drive/MyDrive/Attack-pattern-private-study/generated"
```

Ejecuta la celda siguiente: solicita el montaje de Drive dentro de Colab y crea la carpeta si falta. Autoriza el acceso a tu cuenta de forma interactiva. Si has copiado solo el notebook, utiliza la configuración sin archivos de la primera ejecución.

Fuera de Colab, una ruta que empieza por `/content/drive/` produce un mensaje para elegir una carpeta local. Si utilizas Drive sincronizado en Windows o macOS, escribe su ruta local y se tratará como una carpeta normal.

## 2. Configurar una sola ruta

La primera celda de ruta contiene:

```python
path = "./generated"
```

Sustituye únicamente esa asignación por tu carpeta. Conserva la línea que la normaliza. No pongas la ruta al archivo `.ipynb`; pon la carpeta de datos y resultados. Se aceptan rutas absolutas, espacios y una barra final opcional.

Una ruta relativa depende del directorio de trabajo del kernel. Si no sabes cuál es, usa una ruta absoluta. Todos los archivos de ejecución se buscan o escriben bajo `path`; el PDF y esta guía no son entradas de cálculo.

## 3. Elegir la ejecución

Reinicia el kernel si has cambiado datos, target o modelo y ejecuta las celdas en orden. Mantén `imports_flag=True` e `installs_imports_flag=True`. Tras instalar las dependencias por separado, usa `installs_flag=False`.

| Flag | A: calcular desde cero | B: clon completo con los trece archivos | C: intermedios sin Xorg/yorg | D: todo, incluidos pickles compatibles |
|---|---|---|---|---|
| `fetchdataset` | True | False | True | False |
| `analisis_eda` | True | False | False | False |
| `limpieza` | True | False | False | False |
| `siluette_innertia` | True | False | False | False |
| `siluette_innertia2` | True | False | False | False |
| `clasificacion_accdf_res` | True | False | False | False |
| `csv_eli5_weights` | True | False | False | False |
| `shap_save` | True | True | True | False |

Para el recorrido principal conserva `alld2_et=True` y `target=y_12type.copy()`. Puedes dejar en False `data_prof`, `graficos_pca_clusters`, `graficos_xscaled_clusters`, `best_params_xgb_et` y `exportar_html_lime_caso`.

### A. Sin ningún archivo, o recalcular todo

Se descargan los originales, se prepara el limpio, se realiza EDA, se generan los datasets analíticos y se calculan los demás resultados. Las tablas generadas se guardan en `path`. Una carpeta de ejecución distinta permite conservar intactas las referencias publicadas.

Si ya existe `correlaciones_completas.xlsx`, se reutiliza incluso con EDA activo. Para recalcular también ese Excel, utiliza una carpeta sin ese archivo o aparta previamente la copia anterior.

### B. Clon nuevo con los trece archivos publicados

Carga Xorg/yorg con fetchdataset=False, y el limpio y `dfanálisis_alld2.csv` para omitir EDA. Reutiliza las tablas de selección de K, comparación e importancias. Todos esos archivos se incluyen en el repositorio: no hace falta descargar datos de UCI ni montar Drive si utilizas una copia local completa. La instalación de dependencias es independiente y puede requerir conexión.

Sigue calculando PCA, K-Means, Fuzzy C-Means, validación posterior, modelo final y SHAP. Los centroides publicados sirven para consultar resultados, no para restaurar el clustering.

### C. Tienes los intermedios, pero te faltan Xorg.csv e yorg.csv

Es igual que B, pero fetchdataset=True obtiene los originales de UCI y requiere conexión. También puedes copiar los dos CSV publicados a path y utilizar False. Leer las exportaciones históricas y obtener datos directamente de UCI son vías de entrada distintas: no des por supuesto que todas las etapas reproducirán exactamente las mismas filas o resultados.

### D. También conservas los pickles de esa ejecución

`shap_save=False` carga los valores guardados. Deben corresponder al mismo modelo, datos, target y orden del test. Si cambias algo de eso, recalcula con True. Esta vía requiere generar previamente los pickles con `shap_save=True`.

El explicador se intenta reconstruir si falta, pero el pickle de valores sigue siendo obligatorio para la vía de carga. No cargues pickles de procedencia desconocida.

### Si solo tienes algunos archivos

Decide cada bloque por separado:

| Archivos presentes en path | Para reutilizarlos | Si faltan |
|---|---|---|
| Xorg e yorg | `fetchdataset=False` | True para descargar ambos. |
| `df_org_limpio.csv` y `dfanálisis_alld2.csv` | `limpieza=False`, `analisis_eda=False` | Sigue el escenario A si necesitas generarlos. |
| Tests PCA | `siluette_innertia=False` | True. |
| Tests sin PCA | `siluette_innertia2=False` | True. |
| Resumen y folds para el target | `clasificacion_accdf_res=False` | True si falta cualquiera. |
| CSV ELI5 | `csv_eli5_weights=False` | True. |
| Valores SHAP compatibles | `shap_save=False` | True. |

Tener solo el dataset analítico no basta para ejecutar desde el principio: las primeras celdas obtienen los originales y la preparación también carga el limpio cuando `limpieza=False`. Los nombres y los acentos deben coincidir.

## 4. Todas las flags

| Flag | Efecto |
|---|---|
| `installs_imports_flag` | Interruptor maestro de instalación e imports. En un kernel limpio, no lo desactives si necesitas los imports. |
| `installs_flag` | Instala dependencias dentro del notebook cuando el maestro está activo. Usa False si ya instalaste requirements. |
| `imports_flag` | Ejecuta imports y temporizador cuando el maestro está activo. |
| `fetchdataset` | True descarga RT-IoT2022 y exporta Xorg/yorg; False lee ambos desde path. |
| `analisis_eda` | True ejecuta EDA y prepara ambos analíticos; False carga el analítico completo más adelante. |
| `limpieza` | True guarda el limpio; False lo carga. No evita todas las operaciones iniciales. |
| `clustering` | Variable histórica: no hay un condicional que desactive toda la sección con ella. |
| `data_prof` | True genera profiling HTML; False muestra el existente o informa de su ausencia y continúa. |
| `siluette_innertia` | Calcula o carga las métricas por K sobre PCA. |
| `siluette_innertia2` | Calcula o carga las métricas por K sin PCA. |
| `graficos_pca_clusters` | Activa entrenamientos y gráficos adicionales por K sobre PCA. |
| `graficos_xscaled_clusters` | Activa entrenamientos y gráficos adicionales por K sin PCA. |
| `clasificacion_accdf_res` | Compara algoritmos y guarda resumen/folds, o carga ambos. No evita entrenar el modelo final. |
| `alld2_et` | True elige Extra Trees en los bloques finales; False XGBoost. No cambia el dataset ni la lista general de comparación. |
| `best_params_xgb_et` | Ejecuta GridSearchCV. La celda posterior vuelve a construir el estimador con los parámetros escritos en ella; no adopta automáticamente cualquier nuevo ganador. |
| `shap_save` | Calcula y guarda SHAP, o intenta cargar sus cachés. |
| `csv_eli5_weights` | Calcula y guarda importancia por permutación, o carga su CSV. |
| `exportar_html_lime_caso` | Guarda explicaciones HTML; False no desactiva las explicaciones ni Gradio. |

No actives todas las flags por sistema: algunas repiten cálculos opcionales costosos. Selecciona el escenario antes de ejecutar.

## 5. Funciones y archivos

| Función | Papel |
|---|---|
| `proptrafico` | Conteos, porcentajes y gráfico de distribución por grupo. |
| `direccion_q012` | Crea categorías de conteo cero y cuantiles positivos, y opcionalmente suma PSH/URG. Modifica el dataframe y devuelve las columnas creadas. |
| `direccion_xpkts` | Cruza esas categorías con los grupos de tráfico. |
| `bruteforce_flags_bar` | Representa la presencia de flags en fuerza bruta. |
| `d2_top_combos_pretty_short` | Resume combinaciones frecuentes de flags. |
| `juntarto` | Cruza variables, discretizadas por cuantiles si son numéricas, con grupos de tráfico. |
| `importantes_corr` | Calcula o carga el Excel, filtra pares y devuelve la selección y la ruta. |
| `preparar_df_v2` | Se define dos veces: primero prepara la variante reducida y después el conjunto completo. Cada llamada devuelve dataframe intermedio, analítico e informe. La segunda definición sustituye a la primera en el kernel. |
| `partir12` | Divide nombres largos para presentar los gráficos. |
| `explain_instance_with_lime` | Recibe valores de Gradio, predice con el modelo en memoria y prepara la explicación local. |

La [lista de archivos e inventario](LISTA_ARCHIVOS.md) explica cada CSV y el Excel. Las matrices PCA y fuzzy son exportaciones: el código no las carga para evitar el cálculo. Tampoco carga un modelo final persistido. `interface.launch()` inicia Gradio sin un flag general de desactivación.

## 6. Recursos de ejecución

Instala requirements, selecciona el kernel y path, configura A para calcular todo o B/C para reutilizar referencias y ejecuta todas las celdas en orden. Ese recorrido puede requerir bastante tiempo y memoria; las dependencias no están fijadas a la versión de la ejecución histórica.

Drive es opcional; la descarga desde UCI requiere conexión, pero no una clave de API.
