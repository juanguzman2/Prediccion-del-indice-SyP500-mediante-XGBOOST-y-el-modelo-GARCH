# Prediccion del indice S&P 500 mediante XGBOOST y el modelo GARCH

**El índice S&P 500 de la Bolsa de Valores de Nueva York** es uno de los índices más importantes a nivel mundial, ya que reúne a las 500 principales empresas de la economía estadounidense. Por lo tanto, este índice proporciona una idea de cómo se encuentra la economía en uno de los países más influyentes en términos económicos. En este proyecto, se utilizarán dos métodos de predicción, como XGBOOST y el modelo GARCH, para hacer pronósticos de los retornos de esta inversión.

**¿Por qué se estiman los retornos del activo en lugar de los valores absolutos de la serie de tiempo?**

En primer lugar, es importante explicar qué son los retornos. Estos representan las ganancias o pérdidas generadas por una inversión durante un período de tiempo específico. Los retornos reflejan el rendimiento financiero obtenido en relación con la cantidad de dinero invertido.

Entonces, ¿por qué se utilizan los retornos en la predicción en lugar de los valores absolutos? Hay varios motivos:

1. Al modelar la tasa de retorno en lugar de los valores absolutos, se pueden comparar y analizar de manera más efectiva diferentes instrumentos financieros o activos. Las tasas de retorno proporcionan una medida relativa del desempeño entre diferentes inversiones, lo que permite evaluar qué activos tienen un mejor rendimiento relativo y tomar decisiones de inversión basadas en esa información.

2. Al modelar las tasas de retorno, se tiende a eliminar o reducir las tendencias generales en los datos, lo que permite identificar patrones o comportamientos específicos que pueden ser más relevantes para la toma de decisiones financieras.

4. Las tasas de retorno suelen exhibir una mayor estacionariedad que los valores absolutos en las series de tiempo financieras. La estacionariedad implica que las propiedades estadísticas de una serie, como la media y la varianza, no cambian con el tiempo. Esto facilita el análisis y la predicción de los retornos, ya que se pueden aplicar modelos estadísticos más robustos.

Para el proyecto se usaran los valores diarios del índice S&P 500 desde el 2021-01-01 hasta el 2023-10-15.

## Modelo GARCH

En muchas series, como en las financieras, el número de retardos a utilizar es muy elevado, lo cual dificulta su estimación. Es por esto que Bollerslev (1986) propone un modelo en el cual la varianza condicional no solo depende de los cuadrados de los errores, como en el modelo GARCH, sino que además depende de las varianzas condicionales de periodos anteriores.

### Estacionariedad

Se buscará si los retornos del activo son estacionarios, ya que esto es un supuesto esencial a la hora de aplicar el modelo GARCH. En caso de no ser estacionarios, se deberá hacer un tratamiento como la diferenciación con el fin de buscar la estacionariedad.

#### Grafica de los retornos

Inicialmente podemos observar la distribucion de los retornos acumuluados

![Retornos_acumulados](https://github.com/juanguzman2/Trading-algoritmico/blob/master/Imagenes/retornos_acumulados.png?raw=true)

En donde podmeos observar lo siguiente:  

* Podemos ver que no muestra tendencias.
* Se observa una media constante alrededor de 0.
* También parece tener una varianza constante a lo largo del tiempo.

Estos son buenos indicios, ya que cumplen con los supuestos del modelo GARCH.

#### Test de estacionariedad

Se aplicaran los test ADF, Phillips-Perron y KPSS

Los resultados son los siguientes:

|ADF|PP|KPSS|
|---|---|---|
|0.0|0.0|0.4373|

Como el p-valor es menor que $\alpha$, se rechaza $H_0$ para las pruebas de ADF y PP, y se acepta la prueba de KPSS. No parece haber presencia de unidad raíz en los retornos del activo. Por lo tanto, podemos concluir que los retornos son estacionarios.

#### Grafica ACF y PACF

Otra forma de verificar la estacionariedad en los modelos es mediante las gráficas ACF y PACF. Si los rezagos de la serie convergen rápidamente al intervalo, podemos decir que la serie es estacionaria. Sin embargo, es importante mencionar que la convergencia rápida de los rezagos en las gráficas ACF y PACF no es suficiente para afirmar la estacionariedad, y que se deben realizar pruebas estadísticas adicionales para confirmarla.

![grafica_acf_pacf](https://github.com/juanguzman2/Trading-algoritmico/blob/master/Imagenes/acf_pacf.png?raw=true)

En conclusion se puede decir que los retornos del S&P500 son estacionarios

## Estimacion del modelo

Se hara una combinacion de los parametros del modelo (p,o,q) con el fin de encontrar los valores minimos de las metricas de evaluacion como el criterio de informacion de akaike (AIC) y el bayesiano (BIC) se tiene GARCH(p=1,o=1,q=1)

A continuacion observamos el precio el modelo GARCH contra la volatilidad móvil de los retornos diarios que se refiere al cálculo de la volatilidad de los cambios diarios en una serie de datos financieros.

![GARCH_vs_retornos de volatilidad](https://github.com/juanguzman2/Trading-algoritmico/blob/master/Imagenes/garch_vs_rolling.png?raw=true)

Luego podmeos observar una prediccion de la volatilidad para los proximos 90 dias de negociacion

![prediccion_de_volatilidad](https://github.com/juanguzman2/Trading-algoritmico/blob/master/Imagenes/pronostico_movil.png?raw=true)

## XGBOOST

XGBoost se basa en el concepto de Gradient Boosting, que es una técnica de ensamblaje que combina varios modelos débiles (normalmente árboles de decisión) para formar un modelo más robusto. El algoritmo entrena los árboles de forma secuencial, enfocándose en los errores residuales de los modelos anteriores

XGBoost es conocido por su excelente rendimiento en términos de precisión y velocidad. Dado que el S&P500 es un índice ampliamente seguido y se negocia activamente en los mercados financieros, la capacidad de predecir los retornos con alta precisión y en tiempo real es fundamental para los inversores y operadores.

De este modelo podemos observar la siguiente grafica que muestra las predicciones del XGBOOST para los datos de entrenamiento entrenamiento

![prediccion_de_volatilidad](https://github.com/juanguzman2/Trading-algoritmico/blob/master/Imagenes/XGBOOST_vs_GARCH%20_train.png?raw=true)

Esto nos muestra que tan bien se ajusto el modelo para los datos de entrenamiento

Luego observamos la prediccion con los datos reales:

![prediccion_de_volatilidad](https://github.com/juanguzman2/Trading-algoritmico/blob/master/Imagenes/volatilidad_XGBOOST.png?raw=true)

# Evaluacion de los modelos

La evaluacion se hara con 2 metricas:

* Root Mean Squared Error (RMSE):

El RMSE es una métrica comúnmente utilizada para medir la precisión de un modelo de regresión. Se calcula tomando la raíz cuadrada del error cuadrático medio, que a su vez se obtiene promediando los errores cuadrados entre los valores predichos y los valores reales.

El RMSE se expresa en las mismas unidades que la variable objetivo y proporciona una medida de la diferencia promedio entre los valores predichos y los valores reales. Un RMSE más bajo indica una mayor precisión del modelo, ya que implica que los valores predichos se ajustan mejor a los valores reales.

* Mean Absolute Percentage Error (MAPE):

El MAPE es una métrica utilizada para evaluar la precisión de un modelo en términos de porcentaje de error absoluto promedio en relación con los valores reales. Se calcula tomando el promedio de los errores absolutos porcentuales entre los valores predichos y los valores reales, y multiplicándolo por 100 para obtener un porcentaje.

El MAPE proporciona una medida relativa del error promedio en términos porcentuales, lo que permite comparar la precisión entre diferentes modelos y conjuntos de datos. Un MAPE más bajo indica una mayor precisión del modelo, ya que implica que los valores predichos tienen un menor porcentaje de diferencia con respecto a los valores reales.

Los resultados son:

El RMSE del modelo GARCH en los datos de la serie completa es: 0.1898

El MAPE del modelo GARCH en los datos de la serie completa es: 15.1482%

El RMSE del modelo GARCH en los datos predichos es: 0.1195

El MAPE del modelo GARCH en los datos predichos es: 10.9324%

El RMSE del modelo XGBOOST en los datos de la serie completa es: 0.0338

El MAPE del modelo XGBOOST en los datos de la serie completa es: 2.2842%

El RMSE del modelo XGBOOST en los datos predichos es: 0.0467

El MAPE del modelo XGBOOST en los datos pedichos es: 3.5515%

# Conslusion

La investigación ha evidenciado que el modelo GARCH tiene la capacidad de predecir de manera efectiva la variabilidad mensual de las acciones. No obstante, otros métodos más modernos, como el XGBOOST, pueden arrojar resultados superiores, como se ha observado previamente en las métricas de evaluación. Este proyecto demuestra la utilidad del Machine Learning y GARCH en la estimación futura de la volatilidad de las acciones.