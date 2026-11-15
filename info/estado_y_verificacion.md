# Estado verificable de esta entrega

## Realizado

- Revisión de las dos publicaciones, su estructura estadística y el código público del modelo de referencia.
- Preparación del proyecto Quarto: los modelos, preparación, tablas, gráficos y redacción dependiente de resultados están incluidos en un solo QMD.
- Inclusión sin alteración de los CSV de resultados y calendario 2024 aportados por la estudiante.
- Verificación independiente de 479 registros, 24 carreras, 24 pilotos, 10 constructores y 425 registros en la selección principal de 2024.
- Verificación independiente de normalización de probabilidades Plackett–Luce, invariancia a desplazamiento, rangos esperados y escenarios de igualdad.
- Comprobaciones léxicas de delimitadores del código y validación YAML; no equivalen a ejecutar ni analizar sintácticamente R/Stan.
- Prueba privada de maquetación con marcadores explícitos sin resultados: cinco páginas de contenido y una de bibliografía. No se distribuye esa maqueta como reporte científico.

## Pendiente de ejecución en RStudio

- Primera descarga del apoyo histórico 2014–2023.
- Compilación y ajuste RStan, diagnósticos, validación, contraste lme4 y resultados numéricos.
- Render Quarto con resultados reales y comprobación de paginación final.

No hay nuevos porcentajes ni rankings empíricos bayesianos calculados en este paquete. No se han reutilizado como resultados propios las cifras publicadas ni los resultados del modelo de ritmo anterior. El entorno de construcción no disponía de R/Stan y no permitió obtener el histórico desde el contenedor. Los resultados se producirán únicamente al ejecutar y superar los controles definidos.
