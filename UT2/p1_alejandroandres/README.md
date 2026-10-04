# Práctica 01: Base de Datos Documental con MongoDB

**Alumno:** Alejandro Andrés Bermejo  
**Escenario elegido:** Plataforma multimedia (películas, series, episodios, géneros, usuarios y valoraciones).  
**Versión de MongoDB:** MongoDB 6.0 / MongoDB Compass 1.51.0

---

## 1. Definir el problema y los accesos

### 1.1 Contexto

He elegido el escenario de la plataforma de streaming. El objetivo es diseñar una base de datos en MongoDB para guardar el catálogo de películas y series, la información de episodios, los usuarios registrados y las valoraciones que dejan.

### 1.2 Usuarios del sistema

- **Usuario espectador:** Busca películas y series por género o duración, ve información de episodios y deja valoraciones.
- **Usuario administrador:** Añade y modifica el catálogo de películas, series y episodios.

### 1.3 Preguntas de negocio

1. ¿Cuáles son las 10 películas mejor valoradas?
2. ¿Qué series tienen más de 3 temporadas?
3. ¿Qué películas de "Ciencia ficción" duran más de 120 minutos?
4. ¿Qué series tienen 3 o más episodios y una valoración media menor a 5?
5. ¿Qué películas tienen menos de 10 valoraciones?
6. ¿Qué episodios han sido valorados con una nota media mayor o igual a 4?

### 1.4 Datos con mayor lectura y escritura

Búsquedas en el catálogo por género, filtro por duración, consulta de episodios de una serie y guardar las valoraciones y comentarios que van enviando los usuarios.

### 1.5 Matriz de accesos

| Pregunta | Colecciones                 | Filtros                                        | Ordenación             | Paginación  |
| -------- | --------------------------- | ---------------------------------------------- | ---------------------- | ----------- |
| **1**    | `peliculas`                 | `activo: true`                                 | `puntuacion_media: -1` | `limit(10)` |
| **2**    | `series`                    | `temporadas_totales > 3`, `activo: true`       | `titulo: 1`            | `limit(10)` |
| **3**    | `peliculas`                 | `generos: "Ciencia ficción"`, `duracion > 120` | `duracion: -1`         | `limit(10)` |
| **4**    | `series`, `episodios`       | `num_episodios >= 3`                           | `titulo: 1`            | `limit(10)` |
| **5**    | `peliculas`                 | `num_valoraciones < 10`, `activo: true`        | `num_valoraciones: 1`  | `limit(10)` |
| **6**    | `valoraciones`, `episodios` | `tipoContenido: "episodio"`, `puntuacion >= 4` | `puntuacion: -1`       | `limit(10)` |

### 1.6 Requisitos de seguridad, privacidad, disponibilidad y crecimiento

- **Seguridad:** Crear usuarios con permisos específicos para que la aplicación solo pueda consultar el catálogo y añadir valoraciones, evitando que se puedan borrar datos por error.
- **Privacidad:** Respetar la privacidad de los usuarios usando datos de ejemplo inventados.
- **Disponibilidad:** Mantener el servicio disponible 24/7 teniendo copias de la base de datos en más de un servidor para que si uno falla la página siga funcionando.
- **Crecimiento:** Diseñar la base de datos preparada para crecer, de forma que si entran miles de valoraciones se puedan repartir entre varios servidores para no ralentizarse.

---

## 2. Orden de ejecución y cómo reproducir las consultas

Para reproducir la práctica y ejecutar las consultas paso a paso:

1. **Creación de colecciones con validación:** Ejecuta el script [`scripts/01_colecciones_validacion.md`](./scripts/01_colecciones_validacion.md) para crear la base de datos `multimedia` y aplicar las reglas `$jsonSchema`. Puedes ver las evidencias visuales en [`docs/evidencias/evidencias.md`](./docs/evidencias/evidencias.md).
2. **Carga de datos de prueba:** Ejecuta la inserción con el script [`scripts/02_datos.md`](./scripts/02_datos.md) o importa los ficheros JSON situados en [`datos/`](./datos/).
3. **Creación de índices y explain:** Ejecuta el script [`scripts/03_indices.md`](./scripts/03_indices.md) para crear los 4 índices y verificar el plan de ejecución con `explain("executionStats")`.
4. **Consultas de negocio y CRUD:** Ejecuta el script [`scripts/04_consultas.md`](./scripts/04_consultas.md) para obtener las operaciones CRUD y los resultados de las 6 preguntas.
5. **Agregación compleja:** Ejecuta el pipeline por género incluido en [`scripts/04_consultas.md`](./scripts/04_consultas.md).
6. **Copias de seguridad y seguridad:** Consulta las sentencias y procedimientos en [`scripts/05_backup.md`](./scripts/05_backup.md).
