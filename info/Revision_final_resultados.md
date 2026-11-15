# Revisión final: piloto, constructor y parrilla — Fórmula 1 de 2024

## Material revisado y alcance

Fuentes: `resultados_bell_bayes_2024.zip`, `reporte_f1-2.pdf` y el código `reporte_f1.qmd` entregado previamente. La revisión utiliza los resultados del nuevo modelo Bell–Bayes, no los del prototipo de tiempos de vuelta.

Se inspeccionaron visualmente las seis páginas del PDF. Se cotejaron las tablas exportadas, los resúmenes de los seis ajustes MCMC y sus diagnósticos por parámetro. Se recalcularon en Python los errores individuales y por carrera, los promedios de las cuatro reglas de información y la comparación de puestos entre el modelo principal y el contraste inspirado en Bell. No se recibieron las extracciones posteriores completas ni se repitió el muestreo R/Stan; por tanto, esta revisión no reconstruye desde cadenas los intervalos posteriores.

## Dictamen

El PDF coincide con las tablas para los resultados principales. El proceso completó los seis ajustes y todos superaron las condiciones numéricas programadas. Esto respalda su uso como estimaciones del modelo ajustado; no equivale a validar causalmente la separación piloto–constructor ni a medir mecánicamente los carros.

La entrega cumple el formato observado: cinco páginas de contenido, dos de Resultados (3 y 4), tres ítems de conclusión y una página adicional de bibliografía. No se imprime código. La nueva edición del Quarto conserva los cálculos de ajuste sin modificaciones; únicamente mejora terminología, interpretación y presentación.

## 1. Diagnósticos numéricos

| Ajuste | R-hat máximo | ESS central mínimo | Divergencias | Máxima profundidad alcanzada | BFMI mínimo |
|---|---:|---:|---:|---:|---:|
| Principal | 1,0034 | 1364,94 | 0 | 0 | 0,7763 |
| Prior alternativo | 1,0045 | 1299,50 | 0 | 0 | 0,7145 |
| Parrilla, pliegue 1 | 1,0061 | 1133,77 | 0 | 0 | 0,7594 |
| Parrilla, pliegue 2 | 1,0062 | 1260,28 | 0 | 0 | 0,6950 |
| Parrilla, pliegue 3 | 1,0036 | 1137,98 | 0 | 0 | 0,7259 |
| Parrilla, pliegue 4 | 1,0035 | 1254,19 | 0 | 0 | 0,7456 |

Fuentes: `mcmc_principal.csv`, `mcmc_prior_alternativo.csv` y `mcmc_cv_parrilla_1.csv` a `mcmc_cv_parrilla_4.csv`. Los resúmenes y los diagnósticos por parámetro coinciden. Los controles se interpretan según la documentación oficial de Stan: https://mc-stan.org/learn-stan/diagnostics-warnings.html.

## 2. Conclusión central: predominio del componente constructor

La mediana de la proporción de dispersión latente atribuida al componente constructor en 2024 es **82,7128 %**, con intervalo creíble de 95 % **69,6062–90,3504 %**. La mediana complementaria del piloto es **17,2872 %**, con intervalo **9,6496–30,3938 %**.

Es una comparación de la dispersión de los efectos bajo un escenario equilibrado, no el porcentaje de victorias del mejor carro, ni el porcentaje de variabilidad total de las posiciones explicado por piezas mecánicas. Tampoco significa que cada victoria de Verstappen se reparta en esas proporciones.

Todas las extracciones posteriores retenidas sitúan la proporción del constructor por encima del 50 %. La frecuencia empírica exportada es 1; es preferible comunicarlo así en vez de convertir el «100,0 %» redondeado en una afirmación de certeza absoluta.

La segunda medida exportada, basada en hiperparámetros de todo el período, es **90,2861 %** [80,8228–95,5020 %]. No debe confundirse con la de 2024 ni compararse con porcentajes de artículos sin verificar que coincidan su definición, muestra y denominador.

Fuente: `descomposicion_variacion.csv`.

## 3. Rankings: qué muestran y qué no

Max Verstappen encabeza la media posterior de pilotos, con probabilidad de primer puesto **84,4333 %**. Ferrari encabeza la de constructores, con probabilidad **73,5333 %**. Son rankings de efectos medios anuales estimados, no pruebas de quién tuvo el carro mecánicamente más rápido en cada Gran Premio.

El orden por media no es una clasificación cierta. Franco Colapinto aparece quinto, pero tiene **6 participaciones modeladas**, rango posterior medio **8,74** e intervalo de rango **2–21**. No sería correcto describirlo sin más como el quinto mejor conductor de 2024. Tampoco debe interpretarse el segundo puesto por media de Norris como un segundo puesto conocido sin incertidumbre: su intervalo de rango va de 1 a 15.

El cambio de escala del prior conserva casi todo el orden: correlaciones 0,9983 (pilotos) y 1,0000 (constructores). En cambio, el contraste gaussiano inspirado en Bell produce una correlación de 0,8557 en pilotos y 0,9273 en constructores. Algunas diferencias relevantes son:

| Piloto | Puesto por media: principal | Puesto en adaptación Bell |
|---|---:|---:|
| Verstappen | 1 | 1 |
| Norris | 2 | 2 |
| Leclerc | 6 | 3 |
| Hamilton | 12 | 4 |
| Russell | 7 | 14 |
| Colapinto | 5 | 9 |

No es una contradicción que deba borrarse: cambian la respuesta modelada, la especificación y el tratamiento de retiros. No se puede atribuir toda la diferencia a una sola decisión. La coincidencia de líderes tiene más respaldo que un orden exhaustivo e inequívoco de todos los puestos intermedios.

Fuentes: `ranking_pilotos_2024.csv`, `ranking_constructores_2024.csv`, `bell_ranking_pilotos_2024.csv`, `sensibilidad_prior_correlaciones.csv` y `sensibilidad_inspirada_en_Bell.csv`. La comparación completa se añade en `comparacion_rankings_PL_Bell.csv`.

## 4. Intercambios de carro

| Comparación: A frente a B | P(A delante), media posterior | ICr 95 % |
|---|---:|---:|
| Verstappen–Norris, carro común | 65,7 % | 44,4–83,0 % |
| Verstappen–Red Bull / Norris–McLaren | 70,0 % | 55,2–82,2 % |
| Verstappen–McLaren / Norris–Red Bull | 60,5 % | 22,3–90,2 % |
| Verstappen–Sauber / Zhou–Red Bull | 22,3 % | 3,9–58,3 % |

Estos valores son probabilidades predictivas de un orden entre dos combinaciones, promediadas sobre el posterior. No son probabilidades de ganar a todo el pelotón ni la probabilidad posterior de que un efecto sea superior a otro.

Ejemplo de la distinción: en carro común, la probabilidad media de Verstappen delante de Norris es 65,7 %, mientras que la proporción de extracciones con ventaja latente de Verstappen es 92,7 %. No son cantidades intercambiables. En el escenario Sauber–Red Bull, el modelo favorece en promedio a Zhou–Red Bull, pero el intervalo de la probabilidad de Verstappen aún incluye el 50 %.

Fuentes: `contrafactuales_2024.csv` y fórmula de escenarios del Quarto.

## 5. Constructor y posición de salida

Se verificaron las **1.700 filas de predicción** (425 participantes por cuatro reglas) y se recalculó el error de cada regla dentro de cada Gran Premio. Los resultados coinciden con los archivos exportados:

| Regla de información | MAE medio por GP |
|---|---:|
| Piloto + constructor + parrilla | 2,002192 |
| Constructor + parrilla | 2,030621 |
| Parrilla | 2,698088 |
| Constructor | 2,778235 |

La regla completa tiene el menor error medio y gana en 15 de 24 GP; constructor–parrilla gana en 7; parrilla gana en 2. El incremento de precisión medio al añadir piloto sobre constructor–parrilla es **0,028429 puestos**, pequeño en esta medida. Esto no demuestra que el piloto carezca de efecto: se están suprimiendo términos de una misma estructura ajustada conjuntamente, no comparando efectos causales aislados.

La diferencia **MAE constructor menos MAE parrilla** es **0,080147 puestos**, con intervalo bootstrap pareado por carrera **[−0,151569; 0,295921]**. La parrilla tiene menor error puntual, pero el intervalo incluye cero: no se ha establecido una ventaja clara entre las dos reglas de información por separado. El contraste más nítido es el menor error conjunto.

En Mónaco, la regla constructor–parrilla obtiene 1,430, frente a 1,452 de la completa; en Hungría, obtiene 1,920, frente a 2,076 de la completa. No se atribuyen esas diferencias exclusivamente a la geometría del circuito.

Fuentes: `parrilla_predicciones_fuera_de_muestra.csv`, `parrilla_errores_por_gp.csv`, `parrilla_resumen.csv` y `parrilla_diferencia_mae_bootstrap.csv`. Las evaluaciones usan el rango dentro del conjunto de clasificados seleccionado, no el orden completo con fiabilidad modelada.

## 6. Comprobación predictiva y alcance

Las réplicas de victorias suman 24 por temporada simulada. Para Verstappen el promedio replicado es 5,517 frente a 9 observadas; las nueve están dentro del intervalo central reportado [2; 10,025]. El modelo no reproduce exactamente la concentración observada de victorias. Que un conteo caiga dentro de ese intervalo no valida por sí solo todas las partes del modelo.

El grafo histórico tiene dos componentes. Ese conteo no demuestra por sí solo que los componentes que afectan a las comparaciones objetivo estén aislados, ni acredita identificación completa; la comparación solo con 2024 tenía nueve componentes. No se ha cambiado ninguna categoría o añadido un enlace ficticio para lograr conectividad.

El constructor agrega carro y organización, condicionado al modelo anual. No se ha medido potencia ni aerodinámica. Hay efectos anuales, pero no efectos libres por cada actualización de carro. El análisis principal se condiciona a la selección de clasificados y no modela averías. La prueba Bell incluye registros con vueltas completadas positivas y no todos los inscritos indiscriminadamente. Estas limitaciones delimitan el resultado sin anularlo.

## 7. Cambios editoriales incorporados en el Quarto adjunto

- Se aclara que se trata de logit de rangos Plackett–Luce.
- Se evita presentar 100 % empírico como certeza absoluta.
- Se muestra el tamaño muestral junto a los nombres del gráfico de pilotos.
- Se elimina «realmente» de la probabilidad de primer puesto latente.
- Se añade el intervalo bootstrap que impide declarar una superioridad inequívoca de parrilla frente a constructor.
- Se incluyen en las conclusiones la sensibilidad a la especificación Bell y la incertidumbre de los sustitutos.
- Se sustituye el párrafo sobre el prototipo por alcance científico de esta versión.

El prefijo que contiene todos los cálculos y el ajuste estadístico es idéntico, byte por byte, al del Quarto previamente entregado. No se han alterado priors, datos, iteraciones, modelos Stan ni asignación de pliegues. La versión revisada no fue ejecutada con R en este entorno; al renderizar en el proyecto existente debe reutilizar los ajustes cuando coincidan sus huellas. No se garantiza la paginación revisada hasta ese render, aunque se conservan los saltos que organizan cinco páginas y bibliografía aparte.
