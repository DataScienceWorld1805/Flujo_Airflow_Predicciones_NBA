# 🏀 NBA Predicciones Temporada 25 - Airflow DAG

[![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-2.x-blue.svg)](https://airflow.apache.org/)
[![Python](https://img.shields.io/badge/Python-3.7+-green.svg)](https://www.python.org/)
[![Machine Learning](https://img.shields.io/badge/ML-XGBoost%20%7C%20LightGBM-orange.svg)](https://xgboost.readthedocs.io/)

Este proyecto contiene un **DAG de Apache Airflow** que automatiza el proceso completo de análisis de datos y predicción de resultados de partidos de la NBA utilizando técnicas de Machine Learning. El DAG está diseñado para ejecutar el pipeline de análisis del repositorio relacionado de forma automatizada y programada.

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Repositorio Relacionado](#-repositorio-relacionado)
- [Características](#-características)
- [Arquitectura del DAG](#-arquitectura-del-dag)
- [Requisitos](#-requisitos)
- [Instalación](#-instalación)
- [Configuración](#-configuración)
- [Uso](#-uso)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Tareas del DAG](#-tareas-del-dag)
- [Troubleshooting](#-troubleshooting)
- [Contribuciones](#-contribuciones)
- [Licencia](#-licencia)

## 🎯 Descripción

Este DAG de Airflow automatiza el flujo completo de trabajo del proyecto de predicciones de NBA, ejecutando:

1. **Verificación de dependencias** - Asegura que todas las librerías necesarias estén instaladas
2. **Verificación de archivos** - Valida que todos los archivos del proyecto estén presentes
3. **Ejecución del notebook principal** - Procesa los datos y entrena los modelos de ML
4. **Análisis estadístico** - Genera informes y visualizaciones del resumen estadístico
5. **Análisis de predicciones** - Analiza y reporta las predicciones para la temporada 25
6. **Análisis de clasificación** - Genera la clasificación predicha de equipos
7. **Notificación de finalización** - Confirma la finalización exitosa del proceso

## 🔗 Repositorio Relacionado

Este DAG está diseñado para trabajar con el proyecto completo de análisis y predicciones de NBA:

**Repositorio:** [NBA_PrediccionesTemporada_25-ML-Implementación-de-XGBoost-y-LightGBM](https://github.com/DataScienceWorld1805/NBA_PrediccionesTemporada_25-ML-Implementaci-n-de-XGBoost-y-LightGBM-.git)

El repositorio contiene:
- Notebook principal con EDA completo y modelado (`NBA_Predicciones_Partidos.ipynb`)
- Scripts de análisis (`analizar_resumen_estadistico.py`, `analizar_predicciones_temporada_25.py`, `analizar_clasificacion_temporada_25.py`)
- Datos históricos de partidos de NBA
- Modelos entrenados (XGBoost y LightGBM)
- Informes y visualizaciones generadas

## ✨ Características

- ✅ **Automatización completa** del pipeline de ML
- ✅ **Verificación automática** de dependencias y archivos
- ✅ **Ejecución programada** diaria del análisis
- ✅ **Manejo de errores** robusto con reintentos
- ✅ **Logging detallado** para debugging
- ✅ **Integración con Papermill** para ejecutar notebooks
- ✅ **Generación automática** de informes y visualizaciones

## 🏗️ Arquitectura del DAG

El DAG sigue una arquitectura secuencial donde cada tarea depende de la anterior:

```
┌─────────────────────────┐
│ verificar_dependencias  │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│ verificar_archivos_     │
│      proyecto           │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│ ejecutar_notebook_      │
│      principal          │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│ analizar_resumen_       │
│    estadistico          │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│ analizar_predicciones_  │
│    temporada_25         │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│ analizar_clasificacion_ │
│    temporada_25         │
└───────────┬─────────────┘
            │
┌───────────▼─────────────┐
│ notificacion_           │
│    finalizacion         │
└─────────────────────────┘
```

## 📦 Requisitos

### Software Base

- **Apache Airflow** 2.x o superior
- **Python** 3.7 o superior
- **Docker** (opcional, si usas Airflow en contenedores)

### Dependencias Python

El DAG requiere las siguientes librerías (se verifican e instalan automáticamente):

- `pandas` - Manipulación de datos
- `numpy` - Computación numérica
- `matplotlib` - Visualización básica
- `seaborn` - Visualizaciones estadísticas
- `scikit-learn` - Preprocesamiento y métricas
- `xgboost` - Modelo de Gradient Boosting
- `lightgbm` - Modelo de Gradient Boosting rápido
- `papermill` - Ejecución de notebooks Jupyter
- `scipy` - Estadísticas y análisis científico

### Archivos del Proyecto NBA

El DAG espera encontrar los siguientes archivos en la ruta configurada:

- `final_data.csv` - Dataset principal con datos históricos
- `NBA_Predicciones_Partidos.ipynb` - Notebook principal
- `analizar_resumen_estadistico.py` - Script de análisis estadístico
- `analizar_predicciones_temporada_25.py` - Script de análisis de predicciones
- `analizar_clasificacion_temporada_25.py` - Script de análisis de clasificación

## 🚀 Instalación

### 1. Clonar el Repositorio Relacionado

Primero, clona el repositorio principal de NBA en la ubicación esperada por Airflow:

```bash
# Dentro del contenedor o entorno de Airflow
cd /opt/airflow/PROYECTOS
git clone https://github.com/DataScienceWorld1805/NBA_PrediccionesTemporada_25-ML-Implementaci-n-de-XGBoost-y-LightGBM-.git NBA_Predicciones_Temporada_25
```

### 2. Copiar el DAG a Airflow

Copia el archivo DAG al directorio de DAGs de Airflow:

```bash
# Si usas Docker
docker cp nba_predicciones_dag.py <container_name>:/opt/airflow/dags/

# O si tienes acceso directo al sistema de archivos
cp nba_predicciones_dag.py /opt/airflow/dags/
```

### 3. Instalar Dependencias en el Entorno de Airflow

```bash
# Si usas Docker
docker exec -it <container_name> pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm papermill scipy

# O directamente en el entorno
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm papermill scipy
```

### 4. Verificar la Configuración

Asegúrate de que la ruta `PROJECT_BASE` en el DAG coincida con la ubicación real del proyecto:

```python
PROJECT_BASE = '/opt/airflow/PROYECTOS/NBA_Predicciones_Temporada_25'
```

Si tu proyecto está en otra ubicación, modifica esta variable en el archivo `nba_predicciones_dag.py`.

## ⚙️ Configuración

### Variables del DAG

El DAG tiene las siguientes configuraciones por defecto:

```python
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}
```

### Programación

El DAG está configurado para ejecutarse **diariamente**:

```python
schedule_interval=timedelta(days=1)  # Ejecutar diariamente
start_date=datetime(2024, 1, 1)
catchup=False
```

Puedes modificar estos valores según tus necesidades:
- `schedule_interval`: Cambiar la frecuencia (horas, días, semanas, etc.)
- `start_date`: Ajustar la fecha de inicio
- `catchup`: Activar/desactivar la ejecución de DAGs pasados

### Rutas

Asegúrate de que las siguientes rutas sean correctas en tu entorno:

- **PROJECT_BASE**: `/opt/airflow/PROYECTOS/NBA_Predicciones_Temporada_25`
- **NOTEBOOK_PATH**: `{PROJECT_BASE}/NBA_Predicciones_Partidos.ipynb`
- **Output logs**: `/opt/airflow/logs/nba_notebook_output_{date}.ipynb`

## 📖 Uso

### Ejecución Manual

1. **Accede a la interfaz web de Airflow** (generalmente en `http://localhost:8080`)

2. **Encuentra el DAG** `nba_predicciones_temporada_25` en la lista

3. **Activa el DAG** usando el toggle switch

4. **Ejecuta manualmente** haciendo clic en "Trigger DAG"

5. **Monitorea la ejecución** en la vista de Graph o Tree

### Ejecución Programada

El DAG se ejecutará automáticamente según el `schedule_interval` configurado (diariamente por defecto).

### Verificación de Logs

Puedes ver los logs de cada tarea:

1. Haz clic en la tarea en la vista Graph
2. Selecciona "Log" para ver los logs detallados
3. Revisa los mensajes de verificación, ejecución y errores

### Salidas Generadas

Después de una ejecución exitosa, encontrarás:

- **Notebook ejecutado**: `/opt/airflow/logs/nba_notebook_output_{date}.ipynb`
- **CSV de predicciones**: `{PROJECT_BASE}/Informes/predicciones_temporada_25_completa.csv`
- **CSV de clasificación**: `{PROJECT_BASE}/Informes/clasificacion_predicha_temporada_25.csv`
- **Informes Markdown**: En `{PROJECT_BASE}/Informes/`
- **Gráficos**: En `{PROJECT_BASE}/Informes/Graficos/`

## 📁 Estructura del Proyecto

```
Script_Aiflow_Predicciones_NBA/
│
├── nba_predicciones_dag.py          # DAG principal de Airflow
├── repo_relacionado.txt             # URL del repositorio relacionado
└── README.md                         # Este archivo

/opt/airflow/PROYECTOS/NBA_Predicciones_Temporada_25/
│
├── final_data.csv                    # Dataset principal
├── NBA_Predicciones_Partidos.ipynb  # Notebook principal
├── analizar_resumen_estadistico.py   # Script de análisis estadístico
├── analizar_predicciones_temporada_25.py
├── analizar_clasificacion_temporada_25.py
├── Informes/
│   ├── predicciones_temporada_25_completa.csv
│   ├── clasificacion_predicha_temporada_25.csv
│   ├── resumen_estadistico.csv
│   ├── INFORME_RESUMEN_ESTADISTICO.md
│   ├── INFORME_PREDICCIONES_TEMPORADA_25.md
│   ├── INFORME_CLASIFICACION_TEMPORADA_25.md
│   └── Graficos/
│       └── [27 visualizaciones]
└── Graficos (EDA + ML + Comparacion de Modelos ML)/
    └── [17 gráficos de análisis]
```

## 🔧 Tareas del DAG

### 1. `verificar_dependencias`

**Tipo**: PythonOperator  
**Descripción**: Verifica que todas las dependencias necesarias estén instaladas. Si faltan, intenta instalarlas automáticamente.

**Dependencias verificadas**:
- pandas, numpy, matplotlib, seaborn
- scikit-learn, xgboost, lightgbm
- papermill, scipy

### 2. `verificar_archivos_proyecto`

**Tipo**: PythonOperator  
**Descripción**: Valida que todos los archivos necesarios del proyecto existan en las rutas esperadas.

**Archivos verificados**:
- `final_data.csv`
- `NBA_Predicciones_Partidos.ipynb`
- Scripts de análisis Python

### 3. `ejecutar_notebook_principal`

**Tipo**: PythonOperator  
**Descripción**: Ejecuta el notebook principal usando Papermill. Este notebook realiza:
- Carga y exploración de datos
- Análisis exploratorio (EDA)
- Preprocesamiento
- Entrenamiento de modelos (XGBoost y LightGBM)
- Generación de predicciones base

**Output**: Notebook ejecutado guardado en `/opt/airflow/logs/`

### 4. `analizar_resumen_estadistico`

**Tipo**: PythonOperator  
**Descripción**: Ejecuta el script que genera el análisis estadístico completo del dataset.

**Genera**:
- `resumen_estadistico.csv`
- `INFORME_RESUMEN_ESTADISTICO.md`
- Gráficos estadísticos

### 5. `analizar_predicciones_temporada_25`

**Tipo**: PythonOperator  
**Descripción**: Analiza las predicciones generadas para la temporada 25.

**Genera**:
- `INFORME_PREDICCIONES_TEMPORADA_25.md`
- Visualizaciones de predicciones

### 6. `analizar_clasificacion_temporada_25`

**Tipo**: PythonOperator  
**Descripción**: Analiza y genera la clasificación predicha de equipos para la temporada 25.

**Genera**:
- `INFORME_CLASIFICACION_TEMPORADA_25.md`
- Visualizaciones de clasificación

### 7. `notificacion_finalizacion`

**Tipo**: BashOperator  
**Descripción**: Muestra un mensaje de confirmación de finalización exitosa del proceso.

## 🐛 Troubleshooting

### Error: "ModuleNotFoundError"

**Problema**: Faltan dependencias Python.

**Solución**:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm papermill scipy
```

O verifica que el DAG las instale automáticamente en la tarea `verificar_dependencias`.

### Error: "FileNotFoundError"

**Problema**: Los archivos del proyecto no se encuentran en la ruta esperada.

**Solución**:
1. Verifica que el repositorio esté clonado en `/opt/airflow/PROYECTOS/NBA_Predicciones_Temporada_25`
2. Ajusta la variable `PROJECT_BASE` en el DAG si tu proyecto está en otra ubicación
3. Verifica que todos los archivos requeridos estén presentes

### Error: "OSError: libgomp.so.1"

**Problema**: LightGBM requiere una dependencia del sistema.

**Solución**:
```bash
# En sistemas basados en Debian/Ubuntu
apt-get update && apt-get install -y libgomp1

# En sistemas basados en RedHat/CentOS
yum install -y libgomp
```

**Nota**: Este error no es crítico si LightGBM no es esencial para tu caso de uso.

### Error: "Notebook execution failed"

**Problema**: El notebook falla durante la ejecución.

**Solución**:
1. Revisa los logs de la tarea `ejecutar_notebook_principal`
2. Verifica que el notebook se ejecute correctamente de forma manual
3. Asegúrate de que `final_data.csv` esté presente y tenga el formato correcto
4. Verifica que el kernel de Jupyter esté disponible: `kernel_name='python3'`

### Error: "Script execution failed"

**Problema**: Los scripts Python fallan durante la ejecución.

**Solución**:
1. Revisa los logs de la tarea correspondiente
2. Ejecuta el script manualmente para identificar el error:
   ```bash
   cd /opt/airflow/PROYECTOS/NBA_Predicciones_Temporada_25
   python analizar_resumen_estadistico.py
   ```
3. Verifica que los archivos de entrada generados por el notebook estén presentes

### El DAG no aparece en la interfaz

**Problema**: El DAG no se muestra en Airflow.

**Solución**:
1. Verifica que el archivo esté en el directorio de DAGs (`/opt/airflow/dags/`)
2. Revisa que no haya errores de sintaxis:
   ```bash
   python -m py_compile nba_predicciones_dag.py
   ```
3. Reinicia el scheduler de Airflow si es necesario
4. Revisa los logs del scheduler para errores

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

### Áreas de Mejora Potenciales

- [ ] Agregar notificaciones por email en caso de fallos
- [ ] Implementar alertas a Slack/Teams
- [ ] Agregar validación de calidad de datos
- [ ] Implementar versionado de modelos
- [ ] Agregar métricas de rendimiento del DAG
- [ ] Crear tests unitarios para las funciones
- [ ] Agregar soporte para múltiples temporadas
- [ ] Implementar paralelización de tareas independientes

## 📝 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo LICENSE para más detalles.

## 👤 Autor

**DataScienceWorld1805**

- GitHub: [@DataScienceWorld1805](https://github.com/DataScienceWorld1805)
- Email: datascienceworld1805@gmail.com

## 🙏 Agradecimientos

- [Repositorio Principal de NBA Predicciones](https://github.com/DataScienceWorld1805/NBA_PrediccionesTemporada_25-ML-Implementaci-n-de-XGBoost-y-LightGBM-.git) - Por el proyecto base de ML
- Comunidad de Apache Airflow
- Comunidad de ciencia de datos y machine learning
- Librerías open-source que hacen posible este proyecto

## 📚 Referencias

- [Documentación de Apache Airflow](https://airflow.apache.org/docs/)
- [Repositorio Principal del Proyecto NBA](https://github.com/DataScienceWorld1805/NBA_PrediccionesTemporada_25-ML-Implementaci-n-de-XGBoost-y-LightGBM-.git)
- [Papermill Documentation](https://papermill.readthedocs.io/)
- [XGBoost Documentation](https://xgboost.readthedocs.io/)
- [LightGBM Documentation](https://lightgbm.readthedocs.io/)

---

⭐ Si este proyecto te resultó útil, considera darle una estrella en GitHub!

Para preguntas, sugerencias o colaboraciones, por favor abre un issue en el repositorio o contacta al autor.
