# Prediccion del indice S&P 500 mediante XGBOOST y el modelo GARCH

**El índice S&P 500 de la Bolsa de Valores de Nueva York** es uno de los índices más importantes a nivel mundial, ya que reúne a las 500 principales empresas de la economía estadounidense. Por lo tanto, este índice proporciona una idea de cómo se encuentra la economía en uno de los países más influyentes en términos económicos. En este proyecto, se utilizarán dos métodos de predicción, como XGBOOST y el modelo GARCH, para hacer pronósticos de los retornos de esta inversión.

**¿Por qué se estiman los retornos del activo en lugar de los valores absolutos de la serie de tiempo?**

En primer lugar, es importante explicar qué son los retornos. Estos representan las ganancias o pérdidas generadas por una inversión durante un período de tiempo específico. Los retornos reflejan el rendimiento financiero obtenido en relación con la cantidad de dinero invertido.

Entonces, ¿por qué se utilizan los retornos en la predicción en lugar de los valores absolutos? Hay varios motivos:

1. Al modelar la tasa de retorno en lugar de los valores absolutos, se pueden comparar y analizar de manera más efectiva diferentes instrumentos financieros o activos. Las tasas de retorno proporcionan una medida relativa del desempeño entre diferentes inversiones, lo que permite evaluar qué activos tienen un mejor rendimiento relativo y tomar decisiones de inversión basadas en esa información.

2. Al modelar las tasas de retorno, se tiende a eliminar o reducir las tendencias generales en los datos, lo que permite identificar patrones o comportamientos específicos que pueden ser más relevantes para la toma de decisiones financieras.

4. Las tasas de retorno suelen exhibir una mayor estacionariedad que los valores absolutos en las series de tiempo financieras. La estacionariedad implica que las propiedades estadísticas de una serie, como la media y la varianza, no cambian con el tiempo. Esto facilita el análisis y la predicción de los retornos, ya que se pueden aplicar modelos estadísticos más robustos.

Para el proyecto se usaran los valores diarios del índice S&P 500 desde el 2021-01-01 hasta el 2023-10-15.

