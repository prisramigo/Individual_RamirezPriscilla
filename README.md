# ¿Es el piloto o es el carro?

## Información del proyecto

**Autora:** Priscilla Ramírez Ramírez  
**Carné:** C4I820  
**Curso:** Herramientas de Ciencia de Datos I  
**Trabajo:** Proyecto individual, 2026

Este proyecto analiza los resultados de la Fórmula 1 de 2024 para estudiar las diferencias de competitividad entre pilotos y constructores, explorar cambios hipotéticos de carro y comparar la información predictiva del equipo con la posición de salida.

El análisis, la especificación del modelo y el reporte están integrados en `cod/Individual_Ramirez_HerramientasDatosI_2026.qmd`. El archivo `_quarto.yml` establece la carpeta principal del proyecto como directorio de ejecución y envía el reporte a `res/reporte/`.

## Datos y alcance

La temporada objetivo es **2024**: 24 Grandes Premios y 479 inscripciones de pilotos en carreras. El análisis principal conserva 425 participaciones según el criterio de clasificación implementado en el código. Cada observación corresponde a un piloto en un Gran Premio.

Los resultados de **2014–2023** se utilizan como apoyo para conectar comparaciones entre pilotos y equipos. El análisis excluye carreras sprint y temporadas posteriores a 2024.

- **Insumo principal:** `data/originales/base_piloto_carrera.csv`, con los resultados de 2024 previamente recopilados de Jolpica.
- **Variables utilizadas:** temporada, ronda, circuito, piloto, constructor, posición final, posición de salida, puntos, vueltas y estado de finalización.
- **Apoyo histórico:** el código consulta la API de Jolpica y guarda las respuestas en `data/originales/historico/`. Si esas respuestas ya están guardadas, las reutiliza.
- **Base integrada:** se genera en `data/limpios/base_historico_y_objetivo.csv`.

El archivo `data/originales/calendario_2024.csv` se conserva como insumo complementario; el Quarto lee directamente la base de piloto y carrera.

## Metodología

Se utiliza un modelo **Plackett–Luce bayesiano multinivel** para el orden de llegada. La competitividad de cada combinación se representa mediante efectos de piloto, piloto–temporada, constructor y constructor–temporada. La estimación se realiza con RStan.

El proyecto genera rankings con incertidumbre, compara la dispersión de los efectos de pilotos y constructores en 2024 y calcula probabilidades de que un participante termine delante de otro en escenarios hipotéticos.

Además, evalúa la información de la parrilla mediante validación que reserva Grandes Premios completos, realiza una sensibilidad a la escala de los supuestos previos y un contraste gaussiano inspirado en Bell et al. (2016).

## Organización de archivos

| Ruta | Contenido |
|---|---|
| `cod/Individual_Ramirez_HerramientasDatosI_2026.qmd` | Código de preparación, modelos, diagnósticos, resultados y texto del reporte. |
| `data/originales/` | Insumos de 2024 y respuestas históricas descargadas. |
| `data/limpios/` | Base integrada generada por el análisis. |
| `config/parametros.yml` | Años, semilla, opciones de estimación y criterios de diagnóstico. |
| `info/` | Referencias, documentación metodológica, controles y registro de versiones de R y paquetes. |
| `res/analisis/` | Tablas de resultados en CSV. |
| `res/diagnosticos/` | Diagnósticos, controles, registro de avance y exportación de resultados. |
| `res/modelos/` | Ajustes guardados para su reutilización local. |
| `res/reporte/` | Reporte generado por Quarto. |
| `F1_2024_Bell_Bayes.Rproj` | Proyecto para abrir la carpeta en RStudio. |
| `_quarto.yml` | Configuración de ejecución y salida de Quarto. |
| `.gitignore` | Reglas que excluyen archivos auxiliares del control de versiones. |

Las carpetas de resultados y datos procesados se crean durante la ejecución. Los gráficos del informe se producen dentro del Quarto.

## Requisitos e instalación

Se necesitan R, Quarto y una distribución de LaTeX con XeLaTeX para generar el PDF. RStudio facilita la ejecución, pero también se puede trabajar desde la terminal. Para estimar los modelos con RStan se necesita un compilador C++ compatible.

Abrir `F1_2024_Bell_Bayes.Rproj` y ejecutar una vez este bloque en **Console de RStudio** para instalar los paquetes faltantes:

```r
paquetes <- c(
  "dplyr", "tidyr", "tibble", "ggplot2", "httr2", "yaml",
  "knitr", "rmarkdown", "digest", "rstan", "posterior", "lme4", "kableExtra")
faltantes <- paquetes[
  !vapply(paquetes, requireNamespace, logical(1), quietly = TRUE)
]

if (length(faltantes) > 0) {
  install.packages(faltantes, repos = "https://cloud.r-project.org")
}
```

En **Terminal**, comprobar Quarto:

```bash
quarto --version
```

Si Quarto no está instalado, seguir la [guía de instalación](https://quarto.org/docs/get-started/). Si falta una distribución de LaTeX, instalar TinyTeX desde **Terminal**:

```bash
quarto install tinytex
```

La configuración del compilador depende del sistema operativo. Consultar la [guía oficial de RStan](https://github.com/stan-dev/rstan/wiki/RStan-Getting-Started), especialmente si aparecen errores de compilación en macOS.

## Cómo ejecutar el proyecto

1. Abrir la carpeta completa del proyecto. Debe contener `_quarto.yml`, `config/parametros.yml`, `cod/Individual_Ramirez_HerramientasDatosI_2026.qmd`, `info/referencias.bib` y `data/originales/base_piloto_carrera.csv`.
2. Instalar las dependencias descritas arriba.
3. Desde **Terminal**, situada en la carpeta principal, ejecutar:

```bash
quarto render
```

También se puede abrir `cod/Individual_Ramirez_HerramientasDatosI_2026.qmd` en RStudio y pulsar **Render**.

El proceso lee los insumos, obtiene el apoyo histórico que falte, prepara la base, estima o recupera los modelos, comprueba sus diagnósticos y genera las tablas y el PDF. No requiere ejecutar scripts previos ni restaurar una sesión `.RData`.

El PDF se guarda dentro de `res/reporte/`. Si se cambia el nombre o la ubicación del `.qmd`, también debe actualizarse la entrada `render` de `_quarto.yml`.

### Primera ejecución y modelos guardados

Una ejecución sin datos históricos guardados requiere conexión a internet. Si faltan los ajustes, se compila Stan y se realiza el muestreo; su duración depende del equipo y de la configuración.

Con `reajustar: false`, el programa reutiliza un ajuste cuando encuentra el archivo correspondiente en `res/modelos/` y coincide la huella de sus datos, especificación y parámetros de muestreo. Si falta el archivo o cambia esa huella, vuelve a estimarlo. Una copia del repositorio sin los ajustes guardados deberá calcularlos.

Para consultar el progreso durante la ejecución, abrir otra terminal en la carpeta del proyecto y ejecutar:

```bash
tail -n 20 res/diagnosticos/avance.txt
```

El mensaje `Reutilizando ajuste` indica que se está leyendo un modelo guardado; `Muestreo MCMC` indica una nueva estimación.

## Configuración

Los parámetros se encuentran en `config/parametros.yml`. La configuración compartida utiliza:

| Parámetro | Valor |
|---|---|
| Temporada objetivo | 2024 |
| Apoyo histórico | 2014–2023 |
| Semilla | 20241024 |
| Cadenas | 4 |
| Iteraciones por cadena del ajuste principal | 3000, incluidas 1500 de calentamiento |
| Iteraciones por cadena en validación | 2500, incluidas 1250 de calentamiento |
| Pliegues de validación | 4 |
| `reajustar` | `false` |
| `exigir_diagnosticos` | `true` |

También están activadas la sensibilidad al prior, la comparación inspirada en Bell y la validación de parrilla. La configuración y los datos deben conservarse para reproducir el análisis; el archivo `info/sessionInfo_ejecucion.txt` registra las versiones de R y de los paquetes utilizados en cada ejecución.

## Resultados y comprobaciones

Las principales tablas se guardan en `res/analisis/`:

- `ranking_pilotos_2024.csv` y `ranking_constructores_2024.csv`: estimaciones e incertidumbre de los rankings.
- `descomposicion_variacion.csv`: comparación de la dispersión de los componentes.
- `contrafactuales_2024.csv`: probabilidades en escenarios de intercambio de carro.
- `companeros_2024.csv`: comparaciones entre compañeros de equipo.
- `parrilla_resumen.csv` y `parrilla_errores_por_gp.csv`: errores de predicción en la validación por carrera.
- `sensibilidad_inspirada_en_Bell.csv`: contraste con el modelo gaussiano.

En `res/diagnosticos/`, `revision_final.csv` resume la muestra y los diagnósticos principales. Los archivos `mcmc_*.csv` detallan los controles de los ajustes. La exportación conjunta de tablas se guarda como `resultados_bell_bayes_2024.zip` en esa misma carpeta.

Con los controles activados, la ejecución se detiene si un ajuste no cumple los criterios establecidos. Debe revisarse el diagnóstico correspondiente antes de interpretar sus resultados.

## Interpretación y limitaciones

El componente del constructor representa el conjunto competitivo del equipo, incluido el carro y su organización. El porcentaje de variación compara la dispersión de efectos latentes; no mide un porcentaje de mérito ni de victorias.

Los escenarios de intercambio comparan dos participantes bajo los supuestos del modelo. No incorporan adaptación a otro carro ni averías, y no representan la probabilidad de ganar un Gran Premio completo. Las conclusiones dependen de la selección de participantes, la conectividad entre pilotos y equipos y los supuestos estadísticos.

## Fuentes

- [Jolpica: documentación de la API](https://github.com/jolpica/jolpica-f1/tree/main/docs).
- Bell, A., Smith, J., Sabel, C. E. y Jones, K. (2016). *Formula for success: Multilevel modelling of Formula One driver and constructor performance, 1950–2014*. https://doi.org/10.1515/jqas-2015-0050
- van Kesteren, E.-J. y Bergkamp, T. (2023). *Bayesian analysis of Formula One race results: Disentangling driver skill and constructor advantage*. https://doi.org/10.1515/jqas-2022-0021

Las referencias del reporte están en `info/referencias.bib`. La documentación de la adaptación y las condiciones del código de referencia se conservan en `info/`.
