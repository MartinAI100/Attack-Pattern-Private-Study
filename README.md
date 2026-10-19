# Attack-pattern-private-study

Estudio de patrones de tráfico IoT con RT-IoT2022: análisis exploratorio, K-Means, Fuzzy C-Means, clasificación supervisada e interpretabilidad con SHAP, LIME y ELI5.

El notebook estudia estadísticas de los flujos sin inspeccionar el contenido de los paquetes. Los resultados corresponden a un entorno controlado; no constituyen una validación de un detector en producción ni una garantía formal de privacidad. Las clases minoritarias tienen pocos ejemplos, lo que limita la solidez de sus métricas.

## Contenido

- `CYBER.ipynb`: notebook principal, con las salidas históricas conservadas.
- El PDF incluido en la raíz: manuscrito asociado, conservado sin modificaciones.
- `requirements.txt`: dependencias para preparar un entorno local.
- `generated/`: carpeta predeterminada para datos descargados, CSV intermedios, resultados y cachés.
- `docs/PREPARAR_REPO.md`: selección de archivos y pasos para preparar el primer commit.

## Preparar el entorno local en VS Code

Abre esta carpeta como carpeta de trabajo e instala las extensiones Python y Jupyter de Microsoft. En la terminal PowerShell de VS Code:

```powershell
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Abre `CYBER.ipynb` y selecciona `.venv` como kernel. Las dependencias no están fijadas a las versiones de la ejecución histórica: esta instalación no garantiza reproducir exactamente sus resultados. No se ha repetido el entrenamiento completo durante esta preparación.

Después de instalar `requirements.txt`, configura `installs_flag=False` en la celda de flags. Así no se vuelve a ejecutar el bloque histórico de instalaciones, que contiene una instrucción inválida para instalar `sklearn.utils._test_common`. Mantén `imports_flag=True` e `installs_imports_flag=True`.

## Elegir la carpeta de trabajo

En la primera celda que define `path`, escribe la carpeta donde quieres leer y guardar los archivos. No es la ruta al propio `.ipynb`.

```python
path = "./generated"
```

También puedes usar una ruta absoluta, con espacios y sin barra final:

```python
path = "C:/Users/TU_USUARIO/Documents/Cyber/generated"
```

En Linux o Colab, por ejemplo:

```python
path = "/content/generated"
```

El notebook normaliza la ruta y crea la carpeta. Las rutas relativas se resuelven desde el directorio de trabajo del kernel; en VS Code, inicia el notebook desde esta carpeta o utiliza una ruta absoluta para evitar ambigüedad. Los ejemplos anteriores son alternativas: elige una sola asignación y conserva debajo la línea que normaliza `path`.

Para guardar en Google Drive desde Colab:

```python
path = "/content/drive/MyDrive/CYBER_TRABAJO"
```

El montaje de Drive solo se ejecuta cuando la ruta apunta a `/content/drive/`. Ejecutar localmente con una ruta de Windows no requiere `google.colab`. Si el notebook está abierto desde GitHub en Colab, los demás archivos del repositorio no aparecen automáticamente en la sesión: debes copiar `requirements.txt` e instalarlo allí, o preparar las dependencias antes de continuar.

## Primera ejecución sin CSV ni cachés

Instala las dependencias y configura estos flags antes de ejecutar las celdas en orden:

```python
installs_flag = False
imports_flag = True
installs_imports_flag = True
fetchdataset = True
analisis_eda = True
limpieza = True
siluette_innertia = True
siluette_innertia2 = True
clasificacion_accdf_res = True
shap_save = True
csv_eli5_weights = True
```

Para el escenario principal conserva `alld2_et=True` y `target=y_12type.copy()`. Puedes dejar `data_prof=False`, `graficos_pca_clusters=False`, `graficos_xscaled_clusters=False`, `best_params_xgb_et=False` y `exportar_html_lime_caso=False`: no son necesarios para generar las entradas del recorrido principal.

El código usa `dfanálisis_alld2.csv` para clustering y clasificación, es decir, el dataset completo preparado. También genera `dfanálisis.csv`, con reducción del grupo dos_ddos, pero no lo selecciona automáticamente para el recorrido principal. `alld2_et` elige el modelo; no cambia por sí mismo el dataset. El flag histórico `clustering` no controla toda la sección de clustering.

Durante esta primera ejecución:

1. `fetchdataset=True` descarga RT-IoT2022 desde UCI y genera `Xorg.csv` e `yorg.csv`.
2. `limpieza=True` prepara y guarda `df_org_limpio.csv`.
3. El EDA genera los datasets de análisis. Si falta `correlaciones_completas.xlsx`, se calcula a partir del dataframe disponible y se guarda en `path`.
4. Los flags `siluette_innertia` y `siluette_innertia2` generan las tablas para elegir K.
5. `clasificacion_accdf_res=True` compara modelos y guarda el resumen y los resultados por fold para el target elegido.
6. `shap_save=True` calcula y guarda SHAP; `csv_eli5_weights=True` calcula las importancias por permutación.
7. Al final se muestran las métricas del test y la interfaz Gradio. Esta interfaz requiere ejecutar el notebook; no funciona como aplicación dentro de la vista de GitHub.

El tiempo guardado en el notebook corresponde a una ejecución que reutiliza varios archivos previos. Descargar, calcular todos los tests, comparar modelos y obtener SHAP desde cero puede tardar bastante más y requiere suficiente memoria. No uses la duración histórica como estimación del recorrido completo.

## Ejecuciones posteriores con los archivos ya generados

Puedes volver a poner `fetchdataset=False`, `limpieza=False`, `siluette_innertia=False`, `siluette_innertia2=False`, `clasificacion_accdf_res=False`, `shap_save=False` y `csv_eli5_weights=False`. Mantén inicialmente `analisis_eda=True` para seguir el mismo recorrido.

Estos valores cargan archivos guardados y no omiten todos los entrenamientos: K-Means, Fuzzy C-Means, validación y el modelo final siguen teniendo bloques que se ejecutan. Si cambias de carpeta sin copiar las cachés, vuelve a usar la configuración de primera ejecución. Si cambias datos, target, modelo o particiones, regenera los resultados correspondientes y evita reutilizar cachés de otro experimento.

## Datos y manuscrito

Fuente de los datos: [RT-IoT2022, UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/942/rt-iot2022), identificador `942`, DOI `10.24432/C5P338`, licencia CC BY 4.0 según UCI. Se mantiene la atribución a sus autores. No es necesario incluir los CSV originales en Git, ya que el notebook permite descargarlos.

El manuscrito PDF se conserva como documento asociado. El tratamiento de datos, el análisis y las conclusiones del notebook no se han revisado metodológicamente en esta preparación: las modificaciones se limitan a la configuración de rutas, el montaje de Drive y la generación del Excel de correlaciones cuando falta. Las salidas existentes se conservan como resultados históricos, no como una nueva ejecución.
