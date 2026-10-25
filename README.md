# 🏋️ WODPredict-AI: Evaluación y Validación del Modelo de Sobrecarga

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rick-lastra/Actividad_6/blob/main/evaluacion.ipynb)

## Título y Descripción
**Nombre del proyecto:** WODPredict-AI (Evaluación y Validación del Modelo de Sobrecarga)
Este repositorio contiene la Actividad 6 del proyecto WODPredict-AI para la red de centros **UnderCover Fitness**. Evaluación y validación de modelos. Se evalúa la regresión logística con matriz de confusión, métricas, ajuste de umbral, curva ROC, validación cruzada, comparación contra baselines con pruebas de significancia y una prueba A/B simulada. Todos los experimentos se registran en **MLflow**.

**Objetivo General:**
Detectar en atletas nuevos las sesiones con riesgo de **sobrecarga al día siguiente** (`overreaching_label` = 1) con **recall ≥ 0.70**, **precisión ≥ 0.35** y **ROC-AUC ≥ 0.80**, con una desviación del ROC-AUC **menor a 0.03** en validación cruzada, antes del piloto con datos reales de Wodify (noviembre de 2026).

**Contexto Profesional:**
Iniciativa alineada al *Business Case* de UnderCover Fitness para reducir la deserción de atletas en un 25% derivada de lesiones por acumulación de fatiga. El error más caro es el falso negativo: una sobrecarga que el coach no ve.

**Organización del notebook:** `evaluacion.ipynb` tiene una celda de texto y una de código por cada punto del reporte técnico.

| Sección | Punto del reporte |
|---|---|
| 0 | Preparación del entorno (versiones, semilla y MLflow) |
| 1 | Definición del problema, objetivo SMART y datos |
| 2 | Código del modelo (split por atleta, 3 modelos y 2 baselines) |
| 3 | Matriz de confusión |
| 4 | Métricas (exactitud, precisión, recall, F1 y ROC-AUC) |
| 5 | Ajuste de umbral |
| 6 | Curva ROC, AUC e intervalo de confianza |
| 7 | Validación cruzada de 10 folds por atleta |
| 8 | Comparación con baselines y significancia estadística |
| 9 | Prueba A/B simulada |
| 10 | Evidencia de experimentos en MLflow |
| 11 | Tablero visual de resultados |
| 12 | Justificación técnica e impacto en el negocio |
| 13 | Conclusiones |

## 🤗 Ecosistema Hugging Face
| Componente | Uso en el proyecto |
|---|---|
| **Datasets** | Los datos vienen del dataset **público** `rick-lastra/wodpredict-synth` (versión `v1.0`). El notebook lee la versión limpia (`sesiones_limpias.csv`) desde el tag `v1.0` del repositorio de la Actividad 5, así que siempre usa el mismo archivo |
| **Versionado (Hub)** | Cada run de MLflow guarda el parámetro `dataset = rick-lastra/wodpredict-synth@v1.0 (limpio: Actividad_5@v1.0)` |
| **Models** | Las variables `prob_fatiga_*` reproducen la salida del modelo elegido en la Actividad 4 (`cardiffnlp/twitter-xlm-roberta-base-sentiment`) sobre los comentarios de los atletas |
| **Spaces** | Despliegue futuro: demo en Gradio para que el coach capture la carga de la semana y vea el riesgo de sobrecarga |

## 🛠️ Tecnologías Utilizadas
*   **Lenguaje:** Python 3.12
*   **Entorno:** VS Code (venv o conda) y Google Colab. No requiere GPU; la ejecución completa tarda unos 3 minutos.
*   **Librerías principales (versiones fijas en `requirements.txt`):** `numpy 2.0.2`, `pandas 2.2.2`, `scikit-learn 1.6.1`, `xgboost 3.0.5`, `scipy 1.16.3`, `statsmodels 0.14.5`, `matplotlib 3.10.0`, `mlflow 3.4.0`
*   **Modelos evaluados (clasificación binaria):**
    * Regresión logística (`sklearn.linear_model.LogisticRegression`), modelo seleccionado en la Actividad 5
    * Random Forest (`sklearn.ensemble.RandomForestClassifier`)
    * XGBoost (`xgboost.XGBClassifier`)
    * Baseline 1: clase mayoritaria (`sklearn.dummy.DummyClassifier`)
    * Baseline 2: regla actual del box, alerta si ACWR > 1.5
*   **Seguimiento de experimentos:** MLflow con base de datos local SQLite (`mlflow.db`), experimento `actividad6-evaluacion`
*   **Reproducibilidad:** semilla 42 en todo el notebook y versiones de las librerías impresas y guardadas en cada run de MLflow

## 📁 Estructura del Proyecto

```
Actividad_6/
├── evaluacion.ipynb      # notebook de Colab (evaluación y validación)
├── requirements.txt      # versiones fijas de las librerías
├── resultados/           # gráficas y tablas que genera el notebook
├── tablero/              # tablero de resultados (Word y PDF) y capturas de MLflow
├── README.md             # descripción y guía de uso
└── .gitignore            # excluye mlflow.db y mlruns/
```

*   **`evaluacion.ipynb`:** notebook principal. Contiene toda la evaluación, con una sección por punto del reporte técnico (0 a 13). Se guarda con sus salidas para ver los resultados sin ejecutarlo.
*   **`requirements.txt`:** versiones exactas con las que se ejecutó el notebook (atiende la retroalimentación de la Actividad 5).
*   **`resultados/`:** gráficas (matriz de confusión, ajuste de umbral, curva ROC, validación cruzada, prueba A/B y tablero) y tablas en CSV. El notebook las vuelve a generar al ejecutarse.
*   **`tablero/`:** tablero visual de resultados (`tablero_resultados.docx` y su PDF) y capturas de MLflow en `capturas_mlflow/`.
*   **Al ejecutar** se crean además `mlflow.db` y `mlruns/` con el registro de los experimentos de MLflow (métricas, gráficas y modelo guardado).

## ⚙️ Instrucciones de Uso

1. **Ejecutar en Google Colab:** da clic en el botón **Abrir en Colab** y luego en *Entorno de ejecución > Ejecutar todas*. La primera celda instala `mlflow==3.4.0`; los datos se descargan del tag `v1.0` de la Actividad 5, así que no hay que subir archivos.
2. **Clonar el repositorio e instalar dependencias (local):**
   ```bash
   git clone https://github.com/rick-lastra/Actividad_6.git
   cd Actividad_6
   python3.12 -m venv .venv
   source .venv/bin/activate          # Windows: .\.venv\Scripts\Activate.ps1
   pip install -r requirements.txt
   ```
   Con conda: `conda activate <ambiente>` (Python 3.12) y `pip install -r requirements.txt`.
3. **Ejecutar en local (VS Code):** abre la carpeta con `code .`, abre `evaluacion.ipynb`, selecciona el kernel `.venv` (o tu ambiente de conda) y ejecuta todas las celdas en orden. Las rutas son relativas al repositorio. Se necesita internet para descargar los datos.
4. **Ver los experimentos en MLflow:** desde la carpeta del repositorio:
   ```bash
   mlflow ui --backend-store-uri sqlite:///mlflow.db --port 5000
   ```
   y abre http://127.0.0.1:5000. El experimento se llama `actividad6-evaluacion` y tiene 6 runs: uno por modelo o baseline y uno de comparación estadística y prueba A/B.

## 🧪 Metodología
*   **Datos:** `sesiones_limpias.csv` de la Actividad 5: 81,901 sesiones de 280 atletas sintéticos (novato, intermedio y élite) entre octubre de 2024 y septiembre de 2026. El 12.5% de las sesiones termina en sobrecarga al día siguiente. Solo se usan 23 variables que se conocen **antes** de entrenar (cargas y frecuencias de los últimos días, ACWR, RPE de la sesión anterior y `prob_fatiga` de los comentarios).
*   **Generación del dataset sintético** (paquete `wodpredict.synth`, semilla 42):
    1. 280 atletas (94 novato, 93 intermedio y 93 élite) con edad, sexo, idioma, capacidad y frecuencia según su nivel; 25% entra tarde y 20% asiste de forma irregular.
    2. Un WOD diario común al box; la dificultad se calibró con las marcas reales del atleta de SugarWOD (Actividad 4).
    3. Fatiga oculta = carga que decae + estrés y sueño con choques. Nunca se entrega al modelo.
    4. Resultado = dificultad × capacidad × fatiga × ruido; RPE según fatiga y estrés; carga Foster = RPE × minutos.
    5. Etiqueta: la fatiga supera el umbral individual del atleta; prevalencia calibrada al 15% con 5% de ruido de etiqueta.
    6. Notas tomadas del banco de 84 comentarios etiquetados por el coach, con `prob_fatiga` del modelo XLM-R (Actividad 4).
    7. Variables previas (ACWR, cargas y frecuencias) calculadas solo con datos hasta el día anterior.
    8. Errores de captura agregados para practicar la limpieza (de 87,258 a 81,901 sesiones en la Actividad 5).

    Validación: mediana de For Time sintética de 783 s contra 780 s reales (KS p = 0.97); techo teórico del ROC-AUC ≈ 0.91.
*   **Split:** 80% entrenamiento y 20% prueba **por atleta** (`GroupShuffleSplit`). Prueba: 16,498 sesiones de 56 atletas que el modelo nunca vio.
*   **Ajuste de umbral:** se eligió el umbral más alto que cumple recall ≥ 0.70 con predicciones fuera del fold (`cross_val_predict` con `GroupKFold` de 5 folds) **solo en entrenamiento**; el conjunto de prueba no se usa para elegirlo.
*   **Curva ROC:** AUC en prueba con intervalo de confianza del 95% por remuestreo de atletas (500 repeticiones).
*   **Validación cruzada:** `GroupKFold` de 10 folds por atleta, con los mismos folds para todos los modelos.
*   **Significancia:** t pareada y Wilcoxon sobre los 10 folds con corrección de Bonferroni (α = 0.05 / 4) y prueba de McNemar en prueba contra la regla ACWR.
*   **Prueba A/B simulada:** 28 atletas de prueba al grupo A (umbral 0.5) y 28 al grupo B (umbral ajustado), asignados al azar; prueba z de dos proporciones, prueba de permutación por atleta y cálculo del tamaño de muestra para el piloto real.
*   **MLflow:** un run por modelo y baseline con parámetros, versiones de librerías, métricas de prueba (umbral por defecto y ajustado) y de validación cruzada; el modelo seleccionado guarda además sus gráficas y el modelo con firma. Un run adicional guarda la comparación estadística y la prueba A/B.

## 📊 Resultados Obtenidos (prueba, 16,498 sesiones de 56 atletas nuevos)

| Modelo | Umbral | Precisión | Recall | F1 | ROC-AUC | ROC-AUC CV (10 folds) |
|---|---|---|---|---|---|---|
| **Regresión logística** | **0.17** | 0.378 | **0.732** | 0.498 | **0.859** | 0.853 ± 0.005 |
| Random Forest | 0.50 | 0.383 | 0.719 | 0.500 | 0.854 | 0.852 ± 0.006 |
| XGBoost | 0.17 | 0.388 | 0.722 | 0.504 | 0.855 | 0.852 ± 0.005 |
| Regla ACWR > 1.5 (baseline) | 1.5 | 0.313 | 0.297 | 0.305 | 0.741 | 0.728 ± 0.012 |
| Clase mayoritaria (baseline) | — | 0.000 | 0.000 | 0.000 | 0.500 | 0.500 ± 0.000 |

**Metas SMART** (regresión logística, umbral 0.17):

| Meta | Valor | ¿Cumple? |
|---|---|---|
| Recall ≥ 0.70 | 0.732 | Sí |
| Precisión ≥ 0.35 | 0.378 | Sí |
| ROC-AUC ≥ 0.80 | 0.859 (IC 95%: 0.846 a 0.871) | Sí |
| Desviación del ROC-AUC en CV < 0.03 | 0.005 | Sí |

*   **Ajuste de umbral:** al bajar el umbral de 0.5 a 0.17, el recall sube de 0.27 a 0.73 con precisión de 0.38. El modelo ya ordenaba bien el riesgo; el problema era el umbral.
*   **Baselines:** la regresión logística supera a la clase mayoritaria y a la regla ACWR > 1.5 con diferencias significativas (t pareada y Wilcoxon con Bonferroni; McNemar p = 4.6 × 10⁻⁵). Frente a Random Forest y XGBoost no hay diferencia significativa, así que se mantiene el modelo más simple e interpretable: **regresión logística**.
*   **Prueba A/B simulada:** la versión B (umbral 0.17) detecta el 69.8% de las sobrecargas contra el 30.4% de la versión A (umbral 0.5), con p < 0.001.
*   **Impacto en el negocio:** por cada 100 atletas a la semana se detectan 29 de 39 sobrecargas (hoy, con la regla ACWR, se detectan 12), a cambio de unos 152 minutos de revisión del coach.

## ⚠️ Limitaciones
*   Los datos son sintéticos.
*   El recall es menor en los atletas élite (0.62), porque en ellos la sobrecarga es menos frecuente.
*   El umbral 0.17 genera más alertas (≈ 24% de las sesiones), así que el coach debe revisar cada alerta: el modelo recomienda y el coach decide.

## 🔗 Enlaces
*   Repositorio: https://github.com/rick-lastra/Actividad_6
*   Notebook en Google Colab: https://colab.research.google.com/github/rick-lastra/Actividad_6/blob/main/evaluacion.ipynb
*   Dataset sintético en Hugging Face (público, versión `v1.0`): https://huggingface.co/datasets/rick-lastra/wodpredict-synth


## 👥 Contacto y Créditos
*   **Autor:** Ricardo Alejandro Lastra Dueñas
*   **Empresa (Caso de Uso):** UnderCover Fitness
*   **Fecha:** Octubre 2026
*   **Institución:** Universidad Tecmilenio (Gestión de Proyectos de IA)
