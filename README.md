# Generacion y evaluacion de datos sinteticos con Iris

**Universidad EAFIT**  
**Asignatura:** Aprendizaje Automatico  
**Semestre:** 2026-2  
**Autor:** Vladlen Shatunov

## 1. Descripcion del proyecto

Este proyecto explora la generacion de datos sinteticos y su utilidad para entrenar modelos de aprendizaje automatico.

Los datos sinteticos son observaciones creadas artificialmente mediante reglas, simulaciones o modelos estadisticos que buscan reproducir determinadas caracteristicas de los datos reales.

Para desarrollar el experimento se utiliza el conjunto de datos Iris, disponible en Scikit-learn. A partir de sus observaciones de entrenamiento se generan nuevos registros mediante un metodo estadistico basado en distribuciones normales multivariadas.

Posteriormente, se compara el rendimiento de dos modelos de clasificacion entrenados por separado con datos reales y sinteticos.

## 2. Objetivo

Generar datos sinteticos a partir del dataset Iris y evaluar su utilidad en un problema de clasificacion, comparando su comportamiento estadistico y el rendimiento de modelos entrenados con datos reales y sinteticos.

## 3. Dataset utilizado

El dataset Iris contiene 150 observaciones correspondientes a tres especies de flores:

- Setosa.
- Versicolor.
- Virginica.

Cada observacion incluye cuatro caracteristicas numericas:

- Longitud del sepalo.
- Anchura del sepalo.
- Longitud del petalo.
- Anchura del petalo.

Las tres especies cuentan con 50 observaciones cada una, por lo que el conjunto original presenta una distribucion equilibrada de clases.

El dataset se obtiene directamente mediante `load_iris` de Scikit-learn y no requiere descargar archivos adicionales.

## 4. Metodologia

El experimento se desarrolla mediante las siguientes etapas:

### 4.1. Preparacion de los datos

Se separan las variables predictoras (X) de la variable objetivo (y).

Posteriormente, se realiza una division estratificada de los datos:

- Entrenamiento: 120 observaciones (80 %).
- Prueba: 30 observaciones (20 %).

Se utiliza una semilla fija para permitir la reproduccion del procedimiento.

### 4.2. Generacion de datos sinteticos

Se calculan las medias y matrices de covarianza de las cuatro caracteristicas para cada especie, utilizando exclusivamente el conjunto de entrenamiento.

Estos parametros se utilizan para generar nuevas observaciones mediante distribuciones normales multivariadas.

Se generan 40 observaciones sinteticas por especie, para un total de 120 registros artificiales.

### 4.3. Comparacion estadistica

Se comparan los datos reales de entrenamiento y los sinteticos mediante:

- Medias y desviaciones estandar.
- Graficos de dispersion.
- Histogramas por especie.

Estas comparaciones permiten observar si los datos generados conservan aproximadamente las distribuciones y relaciones presentes en el conjunto original.

### 4.4. Entrenamiento y evaluacion

Se entrenan dos modelos independientes utilizando K-Nearest Neighbors (KNN) con tres vecinos:

- Modelo A: entrenado con datos reales.
- Modelo B: entrenado con datos sinteticos.

Ambos modelos utilizan `StandardScaler` mediante un Pipeline y se evaluan sobre las mismas 30 observaciones reales reservadas.

Las metricas utilizadas son Accuracy, F1-score macro y matrices de confusion.

### 4.5. Evaluacion de estabilidad

Adicionalmente, se repite el procedimiento con 30 divisiones estratificadas mediante `StratifiedShuffleSplit`.

En cada repeticion se generan nuevamente los datos sinteticos y se entrenan ambos modelos utilizando solamente las observaciones de entrenamiento correspondientes.

Esto permite estudiar la variabilidad del rendimiento bajo diferentes particiones de los datos.

## 5. Resultados principales

### Evaluacion inicial

| Metrica | Datos reales | Datos sinteticos |
|---|---:|---:|
| Accuracy | 90,00 % | 93,33 % |
| F1 macro | 0,8997 | 0,9327 |
| Predicciones correctas | 27/30 | 28/30 |

### Evaluacion de estabilidad: 30 repeticiones

| Metrica | Datos reales | Datos sinteticos |
|---|---:|---:|
| Accuracy promedio | 93,33 % | 94,56 % |
| F1 macro promedio | 0,9332 | 0,9451 |
| Desviacion estandar de Accuracy | 0,0401 | 0,0376 |

En las 30 repeticiones, el modelo entrenado con datos reales obtuvo mayor Accuracy en 3 ocasiones, el modelo sintetico en 12 y ambos empataron en 15.

Los resultados muestran que los datos sinteticos permitieron entrenar un clasificador con un rendimiento comparable al obtenido mediante datos reales.

Aunque el modelo sintetico alcanzo un rendimiento promedio ligeramente superior, las diferencias observadas no permiten establecer una superioridad estadistica general.

## 6. Limitaciones

El experimento presenta algunas limitaciones:

- El dataset Iris tiene un tamaño reducido.
- El metodo de generacion supone distribuciones normales multivariadas por especie.
- Las observaciones sinteticas pueden presentar diferencias respecto a las distribuciones originales.
- Las divisiones repetidas pueden compartir observaciones, por lo que no constituyen pruebas independientes.
- La generacion de datos sinteticos no garantiza automaticamente la privacidad de los registros originales.

Los resultados deben interpretarse dentro de las condiciones especificas del experimento.

## 7. Requisitos y ejecucion

El proyecto fue desarrollado utilizando Python 3.13.16 y las siguientes librerias:

- NumPy 2.1.3.
- Pandas 2.2.3.
- Matplotlib 3.10.0.
- Scikit-learn 1.6.1.

Las dependencias se encuentran especificadas en `requirements.txt`.

### 7.1. Clonar el repositorio

Clonar este repositorio mediante Git o descargar su contenido como archivo ZIP.

### 7.2. Instalar las dependencias

Desde una terminal ubicada en la carpeta del proyecto, ejecutar:

```bash
python -m pip install -r requirements.txt
```

### 7.3. Ejecutar el notebook

Abrir el archivo `Aprendizaje_Automatico_Synthetic_Data.ipynb` utilizando Jupyter Notebook o Visual Studio Code con soporte para notebooks de Python.

Si Jupyter Notebook no se encuentra instalado, se puede instalar mediante:

```bash
python -m pip install notebook
```

Para iniciarlo:

```bash
python -m notebook
```

Una vez abierto el archivo, seleccionar la opcion para ejecutar todas las celdas en orden.

El experimento utiliza semillas fijas para controlar la aleatoriedad y facilitar la reproduccion de sus resultados.

## 8. Referencias

- [Scikit-learn - Iris Dataset](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_iris.html)
- [Scikit-learn - KNeighborsClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)
- [Scikit-learn - StratifiedShuffleSplit](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedShuffleSplit.html)
- [NumPy - Multivariate Normal Distribution](https://numpy.org/doc/stable/reference/random/generated/numpy.random.Generator.multivariate_normal.html)
- [NIST - SDNist Synthetic Data Report Tool](https://www.nist.gov/services-resources/software/sdnist-synthetic-data-report-tool)

## 9. Conclusiones

El experimento permitio generar observaciones sinteticas que conservaron aproximadamente algunas propiedades estadisticas del conjunto Iris.

Los modelos entrenados con datos sinteticos alcanzaron un rendimiento comparable al de los modelos entrenados con observaciones reales.

Estos resultados muestran el potencial de los datos sinteticos como complemento para determinadas tareas de aprendizaje automatico.

Sin embargo, su utilidad depende del metodo de generacion, las caracteristicas de los datos originales y la evaluacion de su fidelidad, rendimiento y posibles riesgos.
