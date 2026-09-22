# Práctica 1

## 1. Comprender el problema

- **a)** El ayuntamiento será quien utilice el sistema.
- **b)** Gracias a los datos van a poder saber si hacer saltar una alerta y además prevenir problemas que puedan llegar a pasar en un futuro.
- **c)** Una alerta inmediata es para avisar del problema que hay ahora mismo en este momento y un informe histórico es para analizar a través de varios resultados en diferentes momentos para sacar conclusiones.

## 2. Analizar cobertura y calidad

- **a)**
  - Valores extremos: esto puede hacer que den resultados falsos y de esta manera dé errores al resto del análisis.
  - Lecturas congeladas: por este problema no van a poder saber si hay picos de contaminación en un momento dado de los registros de la zona.
- **b)** D4 Sur Industrial: porque hay muchos habitantes en esta zona y pocos sensores comparados con el resto de zonas.
- **c)** Unidades incorrectas: yo corregiría este problema haciendo una conversión de Fahrenheit a Celsius para obtener todos los datos en un mismo formato y así poder usarlos para generar alertas.

## 3. Comparar arquitecturas

- **a)** Rapidez para generar alertas:
  - Batch: Lento
  - Streaming: Rápido
- **b)** Coste y complejidad:
  - Batch: Menores
  - Streaming: Mayores
- **c)** Informes históricos:
  - Batch: Fácil de generar
  - Streaming: Difícil de generar
- **d)** Picos de datos:
  - Batch: los guarda y procesa más tarde sin saturarse
  - Streaming: los procesa al instante, pero puede saturarse si hay muchos

Para alertas usaría streaming y para informes históricos usaría batch ya que para cada uso los pros y contras de cada uno encajan mejor.

## 4. Elaborar una recomendación

El riesgo más urgente es no detectar a tiempo picos de contaminación en D4 Sur Industrial por falta de sensores y que afecte a la salud de la gente. Recomiendo poner más sensores en esta zona para poder detectar los picos a tiempo y tomar medidas de movilidad. Dos razones basadas en el dossier serían que es la zona con más habitantes con mucho tráfico e industria, y que tiene la peor proporción de sensores. Un problema que seguiría pendiente es que en D6 Parque Natural no hay ningún sensor. Y como medida de privacidad se deberían anonimizar las ubicaciones de los sensores móviles para no rastrear los movimientos o rutas de los ciudadanos.
