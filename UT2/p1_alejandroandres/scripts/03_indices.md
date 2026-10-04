# 03 - Creación de Índices e Inspección del Plan (explain)

## Creación de Índices Justificados

- **Índice 1 (Compuesto):** `{ activo: 1, puntuacion_media: -1, titulo: 1 }` --> Acelera ranking Top 10 películas.
- **Índice 2 (Compuesto):** `{ generos: 1, duracion: 1 }` --> Acelera filtro Ciencia Ficción > 120 minutos.
- **Índice 3 (Simple):** `{ serieId: 1 }` --> Acelera los joins con episodios.
- **Índice 4 (Texto):** `{ titulo: "text", descripcion: "text" }` --> Acelera búsquedas de texto.

```javascript
// 1. Índice compuesto para ranking de películas
db.peliculas.createIndex({ activo: 1, puntuacion_media: -1, titulo: 1 });

// 2. Índice compuesto para filtro por género y duración
db.peliculas.createIndex({ generos: 1, duracion: 1 });

// 3. Índice en episodios por serieId
db.episodios.createIndex({ serieId: 1 });

// 4. Índice de texto en películas
db.peliculas.createIndex({ titulo: "text", descripcion: "text" });
```

## Plan de ejecución (explain)

```javascript
// Comprobación de consulta utilizando índice
db.peliculas
  .find({ generos: "Ciencia ficción", duracion: { $gt: 120 } })
  .explain("executionStats");
```
