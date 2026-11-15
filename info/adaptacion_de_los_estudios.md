# Qué procede de los estudios y qué es adaptación de este proyecto

## Fuentes principales

Bell, A., Smith, J., Sabel, C. E. y Jones, K. (2016). Formula for success: Multilevel modelling of Formula One Driver and Constructor performance, 1950–2014. *Journal of Quantitative Analysis in Sports*, 12(2), 99–112. DOI: 10.1515/jqas-2015-0050.

Van Kesteren, E.-J. y Bergkamp, T. (2023). Bayesian analysis of Formula One race results: disentangling driver skill and constructor advantage. *Journal of Quantitative Analysis in Sports*, 19(4), 273–293. DOI: 10.1515/jqas-2022-0021.

Código de los autores: https://github.com/vankesteren/f1model (licencia MIT; se reconoce especialmente `basic_model.stan`). No se incorpora como si fuera trabajo original la estimación numérica de sus publicaciones.

## Bell: elementos adoptados

Separación cruzada piloto–equipo; variación del equipo entre temporadas; regularización; rankings condicionales al equipo y descomposición de variación. Como contraste se ajusta una respuesta rankit con puntuación constante (10, 6, 4, 3, 2, 1; debajo del sexto, `(0.1^0.2)^(posición-6)`). La transformación es monotónica, de modo que su propósito es una escala normalizada, no otorgar puntos oficiales.

**Cambios declarados:** período histórico auxiliar 2014–2023 y foco 2024; inclusión del efecto piloto-año adicional; no se replica la modelización temporal 1950–2014 ni las funciones complejas de varianza por lluvia/tipo de pista. Se ajusta por REML mediante lme4. Se informa si hay singularidad. No se convierte la concordancia con el modelo principal en una prueba de verdad.

## Van Kesteren–Bergkamp: elementos adoptados

Verosimilitud Plackett–Luce/logit de orden completo por carrera; efectos piloto, constructor y formas anuales; priors jerárquicos; simulación posterior; probabilidades de enfrentamiento e intercambios hipotéticos. Se conserva una selección condicional a clasificación siguiendo el último participante activo; no se predice fiabilidad.

**Cambios declarados:** soporte 2014–2023 y foco 2024; codificación solo de niveles piloto-año/equipo-año observados; desviaciones estándar positivas; inferencia mediante RStan; ICr del 95 %; sensibilidad de escala de prior; extensión de parrilla con pendientes regularizadas por circuito. Los efectos no varían dentro de una temporada. Se conservan los identificadores oficiales distintos de constructores (sin unir automáticamente marcas históricas).

## Dos porcentajes que no deben confundirse

1. Principal: dispersión de los efectos combinados de 2024. En cada draw, se calcula la varianza poblacional (divisor n) de los 24 efectos piloto y de los 10 efectos constructor, con peso igual dentro de cada conjunto. Se expresa la parte del constructor en su suma. Este es un escenario equilibrado latente; no reproduce la asignación real piloto-equipo ni incluye la perturbación de la verosimilitud.
2. Comparable metodológicamente con el segundo artículo: `(sigma_equipo² + sigma_equipo_año²) / suma de las cuatro varianzas`. Es una estimación de población jerárquica que usa toda la ventana de apoyo, no un porcentaje exclusivamente de 2024.

El principal NO se interpreta como el 86 % o el 88 % publicados. No se impone ninguna cifra ni ranking.

## Comparación de parrilla

Se realizan cuatro pliegues por GP, distribuidos por ronda. Cada ajuste incluye el apoyo histórico y los GP de 2024 no reservados. En la muestra reservada se calculan rangos esperados para: (a) sólo constructor; (b) sólo parrilla; (c) constructor y parrilla; (d) piloto, constructor y parrilla. Son ablaciones de puntuación de una misma estructura probabilística, no cuatro modelos independientes ni efectos causales. Los efectos del GP reservado no se recuperan del ajuste completo. El MAE se calcula por GP y se promedia con igual peso. Se exporta una diferencia pareada y un bootstrap por GP.

El término de circuito afecta la asociación con parrilla. No representa toda la geometría ni los efectos de tráfico. La selección de clasificados utiliza información posterior: la validación es retrospectiva, no un pronóstico anterior a la salida.

## Mecánica frente a constructor

Los datos no contienen una prueba mecánica controlada. La palabra carro en el título se operacionaliza como ventaja estadística del constructor. El reporte evita convertir esa aproximación en potencia de motor, carga aerodinámica o velocidad máxima.
