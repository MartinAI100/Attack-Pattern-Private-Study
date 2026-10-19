# Preparar Attack-pattern-private-study

Repositorio indicado: https://github.com/MartinAI100/Attack-pattern-private-study

## Alcance de esta preparación

Se conservan el PDF, los CSV locales, las salidas del notebook y la metodología existente. No se ha modificado el tratamiento de duplicados, el balanceo, los modelos ni las conclusiones. Se permite elegir una sola carpeta `path`, usar el notebook fuera de Colab y generar el Excel de correlaciones si no existe. No se ha ejecutado el entrenamiento completo.

La selección siguiente se basa en las referencias del notebook y en los nombres y tamaños de las capturas. Los archivos extra de las capturas no están disponibles en esta carpeta: no se ha comprobado su contenido, su correspondencia con el paper ni que dos archivos con igual nombre sean idénticos.

## Archivos principales para el primer commit

- `CYBER.ipynb`.
- El PDF original de la raíz, conservando su contenido.
- `README.md`, `requirements.txt`, `.gitignore`.
- `generated/README.md` y esta guía.

No hace falta subir CSV para ejecutar desde cero con los flags documentados en el README. Puedes añadir las tablas pequeñas de referencia de la siguiente sección para que los resultados se puedan consultar sin volver a entrenar.

## Archivos de las capturas: selección

| Archivo | Papel según el notebook actual | Qué haría |
|---|---|---|
| `CYBER.ipynb` | Trabajo principal. | Subir. |
| `Xorg.csv`, `yorg.csv` | Entradas originales exportadas tras descargar UCI. | No subir; se regeneran con `fetchdataset=True`. |
| `df_org_limpio.csv` | Intermedio de preparación; se lee si `limpieza=False`. | No subir; generar con `limpieza=True`. |
| `dfanálisis_alld2.csv` | Dataset preparado completo, leído por la clasificación actual. | No subir; se genera en el EDA. |
| `dfanálisis.csv` | Variante con reducción de dos_ddos; no es el dataset principal seleccionado actualmente. | No subir; se genera en el EDA. |
| `df_processed.csv` | No aparece como entrada o salida en el código actual. | Dejar fuera del primer commit; origen no verificado. |
| `df_processed_alld2.csv` | No aparece como entrada o salida en el código actual. | Dejar fuera del primer commit; no sustituir automáticamente a `dfanálisis_alld2.csv`. |
| `df_processed_noflag.csv` | No aparece como entrada o salida en el código actual. | Dejar fuera; no se puede determinar su función solo por el nombre. |
| `dfc_pca_25d_comp.csv` | Matriz PCA exportada; el recorrido actual la calcula en memoria. | No subir; matriz grande regenerable. |
| `dfc_pca_25d_comp_nod2.csv` | No se referencia con este nombre. | No subir; variante histórica sin contenido verificado. |
| `pertenencia_memb_fuzzy.csv` | Matriz de pertenencias exportada, no una entrada necesaria. | No subir; regenerable y grande. |
| `pertenencia_memb_fuzzy_nod2.csv` | No se referencia con este nombre. | No subir. |
| `centroskm_k17_25dpca.csv` | Centroides K-Means exportados, pequeños y útiles para consulta. | Opcional: subir a `generated/`. Hay dos entradas con ese nombre en la captura; comprobar cuál corresponde al experimento antes de elegir. |
| `centroides_fuzzy.csv` | Centroides fuzzy exportados, pequeños. | Opcional: subir a `generated/`. |
| `centroides_fuzzy_nod2.csv` | No se lee con ese nombre. | Opcional solo como evidencia histórica identificada, fuera de la caché principal. |
| `dftests_clusters_gruposk.csv` | Métricas por K sobre PCA; se carga si `siluette_innertia=False`. | Recomendado como resultado pequeño en `generated/`; se puede regenerar. |
| `dftests_clusters_gruposk_xscaled.csv` | Métricas por K sin PCA; se carga si `siluette_innertia2=False`. | Recomendado en `generated/`; se puede regenerar. |
| `dftests_clusters_gruposk_xscaled_nod2.csv` | No se lee con ese nombre. | Opcional como tabla de un experimento histórico, si se identifica. |
| `resultados_clasificacion_attack_type.csv` | Resumen de comparación de modelos para el target actual. | Recomendado en `generated/`. |
| `resultados_clasificacion_folds_attack_type.csv` | Resultados por partición de esa comparación. | Recomendado junto al resumen en `generated/`. |
| `resultados_clasificacion_attack_type_nod2.csv` | No se carga automáticamente; variante histórica. | Guardar solo si respalda un experimento del paper, con descripción. |
| `resultados_clasificacion_folds_attack_type_nod2.csv` | Particiones de esa variante histórica. | Mantener junto al resumen anterior si se decide conservarlo. |
| `resultados_clasificacion_attack_groupstr_nod2.csv` | Resultado histórico para el target de grupos; el flujo actual usa `attack_type`. | Opcional si respalda el paper; no sirve como caché del target actual. |
| `resultados_clasificacion_folds_attack_groupstr_nod2.csv` | Particiones del resultado de grupos. | Conservar junto al resumen si se incluye el experimento; aparece en ambas fotos. |
| `resultados_clasificacion_attack_flag_nod2.csv` | Resultado histórico binario. | Opcional si respalda el paper, identificado como experimento separado. |
| `resultados_clasificacion_folds_attack_flag_nod2.csv` | Particiones del experimento binario. | Conservar junto al resumen si se incluye. |
| `eli5_weights_xgb_et_opt.csv` | Tabla de importancia por permutación; se lee si `csv_eli5_weights=False`. | Recomendado en `generated/`, tras comprobar a qué modelo corresponde. |
| `eli5_weights_xgb_et_opt_nod2.csv` | Variante histórica no cargada por el código actual. | Opcional como resultado documentado de otro experimento. |
| `explainer_attack_type_multiclase.pkl` | Caché del explicador SHAP; hay una reconstrucción si no está. | No subir a Git; regenerar. |
| `shap_values_attack_type_multiclase.pkl` | Valores SHAP; se requieren si `shap_save=False`. | No subir a Git normal: la captura indica 148,9 MB. Generarlos con `shap_save=True`. |
| `shap_values_attack_type_multiclase_nod2.pkl` | Variante histórica no leída con ese nombre. | No subir; regenerar o conservar externamente para archivo personal. |
| `profiling_report.html` | Informe EDA opcional; si falta, `data_prof=False` solo informa de su ausencia. | Dejar fuera del primer commit; generar con `data_prof=True` si se desea. |
| `profiling_only.ipynb` | Notebook adicional no invocado por el principal. | Dejar fuera hasta revisar su contenido y confirmar que aporta algo distinto. |
| `lime_attack_type_multiclase_fila_1200.html` | Explicación local exportada de otra selección de fila. | Opcional como ejemplo, tras comprobar su contexto. No es una entrada del notebook. |
| `lime_attack_type_multiclase_fila_0.html` | Otra explicación local histórica. | Igual que la anterior; basta un ejemplo bien identificado. |
| `lime_explicacion.html` | Salida exportable de la interfaz LIME. | Opcional; puede corresponder al último caso interactivo. |
| `eda_relaciones_importantes.xlsx` | No se referencia con este nombre. | Dejar fuera del primer commit; contenido no verificado. |
| `correlaciones_completas.xlsx` | Tabla usada por el bloque de correlaciones. | Opcional en `generated/`: ahora se genera si falta. Para preservar exactamente la tabla histórica, conservarla con su procedencia. |

Los sufijos `_nod2`, `_noflag` y `_processed` no bastan para establecer el contenido. En particular, la variante reducida del código conserva parte de dos_ddos: no elimina necesariamente todo ese grupo. Algunas matrices de las fotos con y sin sufijo tienen tamaños parecidos. No renombres variantes para que el notebook las acepte sin comprobar filas, columnas y escenario.

Si decides conservar tablas históricas que respaldan el paper, puedes colocarlas en `results/paper/` manteniendo sus nombres y añadiendo un README que explique cada experimento. Esa carpeta no se usa automáticamente como caché. No hace falta incluir esas tablas para el primer commit mínimo.

## Primer commit desde VS Code

Abre la carpeta `proyecto cyberseguridad` en VS Code, no la carpeta que también contiene Chesscraping. Abre una terminal PowerShell. Si Git solicita identidad al hacer el commit, configura tu nombre y el correo que quieras asociar a tus commits, pudiendo utilizar el correo privado de GitHub.

Esta secuencia supone que el repositorio remoto está vacío. Si ya tiene un README u otros commits, utiliza la alternativa del apartado siguiente.

```powershell
Set-Location -LiteralPath 'C:\Users\mjpj0\OneDrive\Escritorio Samgsung\git proyectos subida\proyecto cyberseguridad'
git init -b main
git config core.longpaths true
git remote add origin https://github.com/MartinAI100/Attack-pattern-private-study.git
git add -- CYBER.ipynb README.md requirements.txt .gitignore generated/README.md docs/PREPARAR_REPO.md
git add -- '*.pdf'
git status
git diff --cached --stat
git diff --cached --name-only
```

Comprueba la lista: no deberían aparecer `Xorg.csv`, `yorg.csv`, `.venv`, los datasets intermedios ni los pickles. El notebook contiene salidas históricas conservadas; no se han recalculado.

Si has copiado las tablas pequeñas recomendadas a `generated/`, añádelas explícitamente antes del commit:

```powershell
git add -- generated/dftests_clusters_gruposk.csv generated/dftests_clusters_gruposk_xscaled.csv
git add -- generated/resultados_clasificacion_attack_type.csv generated/resultados_clasificacion_folds_attack_type.csv
git add -- generated/eli5_weights_xgb_et_opt.csv
```

Ejecuta esas líneas opcionales solo para los archivos que realmente hayas copiado. Después:

```powershell
git commit -m "Prepare notebook paths and document usage"
git push -u origin main
```

La autenticación dependerá de tu configuración de GitHub. También puedes revisar los archivos preparados desde el panel Control de código fuente de VS Code antes de pulsar Commit. Esta preparación no ha ejecutado `git init`, creado commits ni publicado archivos.

## Si el remoto ya contiene commits

Clona el remoto en una carpeta nueva desde VS Code mediante Git: Clone, o desde la terminal:

```powershell
git clone https://github.com/MartinAI100/Attack-pattern-private-study.git Attack-pattern-private-study
```

Abre la carpeta clonada y copia allí los archivos seleccionados de esta preparación. Conserva o integra los documentos que ya existan en el remoto. Revisa `git status`, añade los archivos, haz el commit y utiliza `git push`. Así partes del historial existente y evitas necesitar un push forzado.
