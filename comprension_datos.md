# Comprensión de los datos y preparación para el modelado predictivo

*(Documento de apoyo técnico – Capstone Project)*

## 1. Contexto del problema

El objetivo del proyecto es **modelar y analizar la oferta productiva de aguacate en el estado de Jalisco**, utilizando información histórica oficial y técnicas de ciencia de datos. Desde una perspectiva aplicada, este problema busca entender **qué tan predecible es la producción agrícola** a partir de variables productivas, temporales y estructurales, y sentar las bases para modelos que apoyen la **planeación, análisis de escenarios y toma de decisiones**.

Una limitación central del problema es que la información productiva se encuentra **principalmente agregada a nivel anual**, lo cual restringe el análisis temporal y el uso de modelos de series de tiempo. Para abordar esta limitación, se construyó un **dataset mensual estimado** mediante desagregación temporal, preservando los totales anuales y apoyándose en un indicador externo de mayor frecuencia.

---

## 2. Qué aprendimos del Análisis Exploratorio de Datos (EDA)

El EDA permitió comprender la **estructura, calidad y comportamiento** de los datos antes de cualquier modelado. Entre los principales hallazgos destacan:

* El dataset presenta **alta consistencia y ausencia de valores faltantes estructurales** en las variables productivas principales.
* Las variables `Sembrada`, `Cosechada` y `Volumenproduccion` muestran **relaciones fuertes y coherentes**, esperables desde un punto de vista agronómico.
* Existen **valores extremos y distribuciones asimétricas** en varias variables numéricas, lo que motivó transformaciones para estabilizar la varianza.
* La variable categórica `Nommodalidad` (Riego / Temporal) está razonablemente balanceada a nivel anual, pero **presenta diferencias de frecuencia al trabajar con datos mensuales**, lo cual es consistente con la estacionalidad productiva.
* El paso de anual a mensual **no replica exactamente las distribuciones**, pero conserva patrones estructurales relevantes, lo cual es aceptable dado el carácter estimado del dataset mensual.

Estos hallazgos confirmaron que los datos contienen **señales útiles para predicción**, pero requieren preparación cuidadosa para evitar sesgos o fugas de información.

---

## 3. Decisiones clave de ingeniería de características

Durante la fase de preparación de datos se tomaron decisiones explícitas orientadas a **robustez, interpretabilidad y realismo predictivo**:

* **Eliminación de variables monetarias y derivadas** (`Valorproduccion`, `Precio`) para evitar fuga de información hacia la variable objetivo (`Volumenproduccion`).
* Eliminación de variables sin variabilidad (`Siniestrada`) y de metadatos del proceso (`TipoDato`, `Frecuencia`, `share_mes`).
* Conservación de variables productivas clave (`Sembrada`, `Cosechada`, `Rendimiento`) por su **sentido causal y agronómico**.
* Transformación de la variable temporal `Mes` mediante **codificación cíclica (seno y coseno)** para capturar estacionalidad sin introducir discontinuidades artificiales.
* Codificación binaria de la modalidad productiva (`Nommodalidad_one_hot`).

Estas decisiones permitieron construir un conjunto de características **coherente con un escenario real de predicción**, donde solo se utilizan variables observables o estimables.

---

## 4. Justificación metodológica

Las decisiones metodológicas del proyecto se guiaron por tres principios:

1. **Evitar fuga de información**
   Se excluyeron variables que dependen directa o indirectamente del volumen producido, aun cuando mostraran alta correlación, para evitar fuga de información y garantizar que las variables utilizadas sean observables antes de que ocurra la producción, y no consecuencias del propio resultado que se desea predecir.

2. **Preservar interpretabilidad**
   Dado el carácter aplicado del proyecto, se priorizó mantener variables comprensibles desde el dominio agrícola, aun cuando existiera colinealidad estructural entre ellas.

3. **Preparar los datos para múltiples enfoques de modelado**
   Las transformaciones y codificaciones realizadas permiten el uso posterior de modelos lineales, no lineales y de series de tiempo sin necesidad de rehacer la preparación de datos.

Técnicas estadísticas como Chi-cuadrada, ANOVA o Análisis Factorial no se emplearon, ya que el problema es de regresión con variables continuas y el número de características es reducido, haciendo innecesaria su aplicación en esta etapa.

---

## 5. Implicaciones para el modelado

El dataset final preparado:

* Está alineado con un **problema de regresión supervisada**.
* Contiene variables con **significado agronómico y temporal claro**.
* Permite evaluar modelos simples (baseline) y más complejos sin cambios estructurales.
* Reduce el riesgo de sobreajuste derivado de variables redundantes.

Además, la exploración diagnóstica mediante PCA mostró que **no existe una ganancia clara al reducir dimensionalidad**, reforzando la decisión de conservar las variables originales para la etapa de modelado.

---

## 6. Relación con CRISP-ML

Este trabajo cubre de forma explícita las fases de **Comprensión de los Datos** y **Preparación de los Datos** dentro del marco CRISP-ML:

* Se comprendió la procedencia, estructura y limitaciones de las fuentes.
* Se identificaron riesgos, supuestos y decisiones clave.
* Se preparó un conjunto de datos listo para modelar, trazable y documentado.

Con ello, el proyecto queda en condiciones de avanzar hacia la fase de **Modelado**, comenzando con la construcción de un **modelo baseline** que permita evaluar la viabilidad predictiva del problema.
