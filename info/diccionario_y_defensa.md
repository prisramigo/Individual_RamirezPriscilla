# Diccionario y guía para explicar el proyecto

## Variables observadas

`season`, `round`: temporada y ronda, clave de carrera.
`driver_id`: piloto; `constructor_id`: identificador oficial del constructor.
`position`: posición en los resultados recibidos; `status`: estado reportado.
`grid`: parrilla original. El código no trata el cero como pole.
`clasificado`: participante incluido en el orden de la verosimilitud principal.
`rank_modelo`: posición relativa entre los participantes de esa muestra.
`grid_ventaja`: ventaja de parrilla normalizada; una salida mejor produce un valor mayor.

## Efectos que estima el modelo

`a_d` / `d`: componente del piloto de largo plazo en la ventana de datos.
`a_ds` / `dy`: desviación del piloto para una temporada.
`b_c` / `t`: componente de largo plazo del constructor.
`b_cs` / `ty`: desviación anual del constructor.
`D2024`: suma de ambos componentes del piloto para 2024, centrada para graficar.
`C2024`: suma de ambos componentes del constructor para 2024, centrada para graficar.

Una puntuación mayor representa una ventaja en la escala log-odds. Las diferencias, no los ceros arbitrarios, determinan las probabilidades. El modelo no interpreta un tiempo rápido como habilidad del piloto sin separar el constructor.

## Cinco explicaciones que debes poder dar

**¿Por qué no sólo 2024?** Porque cada piloto conduce casi siempre para un mismo equipo. Los cambios históricos crean comparaciones adicionales. En el insumo 2024 se verificaron nueve componentes en la red piloto-equipo. El histórico reduce este problema, pero no elimina la dependencia de los priors ni todos los supuestos.

**¿Qué significa un ranking ajustado?** El orden de las medias posteriores de efectos estimados después de considerar los otros componentes. Hay incertidumbre: se exportan intervalos de puntuación, de rango y probabilidades de ser primero.

**¿Qué significa el porcentaje?** Dispersión de efectos latentes bajo el modelo. No es porcentaje de mérito, victorias, energía ni R². Sus intervalos se calculan en cada draw; no se crea un valor fijo de 80/20.

**¿Qué significa el intercambio?** Se conserva el efecto estimado de cada piloto y se cambia el efecto del constructor que se le suma. La probabilidad se refiere a terminar delante de otro participante, condicionada al modelo. No predice adaptación ni garantiza el resultado real de un cambio de equipo.

**¿Qué muestra la validación?** Los errores se calculan en GP no utilizados para ese ajuste. No se reparten al azar las filas de una misma carrera entre entrenamiento y test. La comparación de parrilla es retrospectiva y condicional a clasificación.

## Diagnósticos

Rhat cercano a 1: las cadenas dan distribuciones compatibles. ESS: información efectiva, no cantidad de filas ni carreras. Divergencia: advertencia geométrica del muestreador; no equivale a un simple dato atípico. BFMI y profundidad de árbol se revisan adicionalmente. Si el código se detiene por diagnóstico no se deben borrar las comprobaciones para imprimir un ranking.

## Entrega y honestidad académica

Lee los dos artículos y la explicación de cada bloque antes de defenderlo. Comprueba las políticas del curso sobre uso de IA y herramientas de apoyo; documenta la asistencia si corresponde. La autoría del razonamiento defendido debe ser comprendida y revisada por quien presenta. No presentes el código compilado o números que aún no se hayan ejecutado como resultados verificados.
