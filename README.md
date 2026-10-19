# Attack-pattern-private-study

Estudio de patrones de tráfico IoT con RT-IoT2022 mediante análisis exploratorio, PCA, K-Means, Fuzzy C-Means, clasificación supervisada e interpretabilidad con SHAP, LIME y ELI5.

El análisis utiliza estadísticas de los flujos, como tiempos, volumen y banderas, sin inspeccionar el contenido de los paquetes. Los resultados corresponden al laboratorio del dataset. Las clases con pocos ejemplos requieren una interpretación especialmente cuidadosa.

## Qué incluye

- `CYBER.ipynb`: notebook principal, con sus resultados históricos.
- `generated/`: trece archivos de datos y resultados, incluidos Xorg/yorg y los datasets preparados para evitar repetir el EDA.
- [Guía de ejecución](docs/USO_NOTEBOOK.md): instalación, rutas, flags, funciones y escenarios de reutilización.
- [Lista de archivos e inventario de resultados](docs/LISTA_ARCHIVOS.md): contenido, dimensiones y función de cada archivo.
- [Paper completo](docs/Aprendizaje%20supervisado%2C%20clustering%20difuso%20e%20interpretabilidad%20con%20un%20enfoque%20KDD-XAI%20para%20la%20caracterizaci%C3%B3n%20del%20cibertr%C3%A1fico%20IoT%20en%20un%20entorno%20controlado%20de%20forma%20privada.pdf): recopilación del estudio.

## Empezar en VS Code

Descarga o clona este repositorio y abre su carpeta. Instala las extensiones Python y Jupyter de Microsoft y prepara un entorno desde PowerShell:

```powershell
py -3 -m venv .venv
./.venv/Scripts/python.exe -m pip install -r requirements.txt
```

Abre `CYBER.ipynb`, selecciona `.venv` como kernel y deja `path = "./generated"` si el kernel trabaja desde la raíz del proyecto. También puedes usar una ruta absoluta a esa carpeta.

Para utilizar los intermedios publicados sin repetir EDA, ajusta las flags antes de ejecutar:

```python
installs_flag = False
imports_flag = True
installs_imports_flag = True
fetchdataset = False
analisis_eda = False
limpieza = False
siluette_innertia = False
siluette_innertia2 = False
clasificacion_accdf_res = False
csv_eli5_weights = False
shap_save = True
```

`fetchdataset=False` lee Xorg/yorg desde la carpeta configurada. No necesitas descargar los datos de UCI ni montar Drive para utilizar una copia local completa. Si falta alguno de los dos archivos o quieres obtener otra copia desde UCI, usa `True`; la descarga no requiere una clave de API. Esta configuración recalcula SHAP. Se siguen ejecutando PCA, clustering y los modelos posteriores: reutilizar tablas no equivale a cargar un modelo ya entrenado.

Elige la configuración adecuada de la [guía](docs/USO_NOTEBOOK.md) antes de ejecutar todas las celdas en orden.

## Con Drive y sin Drive

El montaje de Google Drive es opcional. Elige la carpeta que contiene los archivos de ejecución:

| Entorno | Ejemplo de path | Drive |
|---|---|---|
| Windows, Linux o macOS | Una ruta local absoluta, o `./generated` desde la raíz | No se importa ni monta Google Drive. |
| Colab sin Drive | `/content/Attack-pattern-private-study/generated` | Los archivos deben estar en la sesión; no se monta Drive. |
| Colab con Drive | `/content/drive/MyDrive/Attack-pattern-private-study/generated` | El notebook solicita el montaje mediante Colab. |
| Drive sincronizado en tu ordenador | La ruta local de esa carpeta | Se utiliza como cualquier carpeta local, sin montar Drive con Colab. |

Abrir un notebook de GitHub en Colab no copia automáticamente los demás archivos. Descarga el repositorio en la sesión o copia los datos y resultados a la carpeta elegida. Para trabajar con Drive, copia el proyecto a tu propia unidad.

Si eliges Drive, autoriza el acceso a tu cuenta de forma interactiva dentro de Colab. Consulta la [guía de uso](docs/USO_NOTEBOOK.md) para los pasos de instalación y la primera ejecución.

## Datos y resultados

Fuente: [RT-IoT2022 en UCI](https://archive.ics.uci.edu/dataset/942/rt-iot2022), identificador `942`, DOI [10.24432/C5P338](https://doi.org/10.24432/C5P338), con licencia CC BY 4.0 según UCI.

Se incluyen Xorg/yorg, el limpio, los dos datasets analíticos, las métricas por K, centroides, comparación de modelos, resultados por fold, importancias ELI5 y correlaciones. El [inventario](docs/LISTA_ARCHIVOS.md) distingue entradas reutilizables de salidas de consulta.

PCA, clustering y SHAP se calculan durante la ejecución del escenario B. Las dependencias no están fijadas a las versiones históricas, por lo que repetir una ejecución puede producir diferencias.


## Licencias y citación

Este repositorio contiene diferentes materiales
de investigación sujetos a sus respectivas licencias.

- Artículo científico: CC BY 4.0.
  © 2026 Martín José García Navarro.
- Código del notebook: MIT License.
  © 2026 Martín José García Navarro.
- Dataset RT-IoT2022: CC BY 4.0.
  © Sus respectivos autores originales.
- Datasets derivados: CC BY 4.0, manteniendo
  la atribución a RT-IoT2022 e identificando
  las modificaciones realizadas.

La licencia MIT del archivo LICENSE principal
se aplica exclusivamente al código del proyecto,
no al artículo ni a los datasets.

Para citar la investigación, consultar
CITATION.cff y el registro del artículo en Zenodo.


## Conclusión y recopilación del estudio

El trabajo reúne preparación de datos, exploración de patrones, agrupamiento clásico y difuso, clasificación y explicación de las decisiones. Los resultados guardados muestran una elevada capacidad de discriminación global en condiciones controladas, con limitaciones importantes en algunas clases minoritarias. La interpretación se apoya en patrones temporales, volumétricos y de control del flujo y debe distinguirse de una explicación causal.

El paper recopila el planteamiento, la metodología, los resultados, la discusión, las limitaciones y el trabajo futuro:

**García Navarro, M. J. (2026). Aprendizaje supervisado, clustering difuso e interpretabilidad con un enfoque KDD-XAI para la caracterización del cibertráfico IoT en un entorno controlado de forma privada.**

[Consultar en Zenodo](https://zenodo.org/records/20131778) · [DOI: 10.5281/zenodo.20131778](https://doi.org/10.5281/zenodo.20131778) · [Leer el PDF incluido](docs/Aprendizaje%20supervisado%2C%20clustering%20difuso%20e%20interpretabilidad%20con%20un%20enfoque%20KDD-XAI%20para%20la%20caracterizaci%C3%B3n%20del%20cibertr%C3%A1fico%20IoT%20en%20un%20entorno%20controlado%20de%20forma%20privada.pdf)

El notebook permite explorar el estudio y sus resultados; el manuscrito reúne su exposición completa.
