# 03 - Creación de Índices e Inspección del Plan (explain)

## Capturas en [/docs/evidencias](../docs/evidencias/evidencias.md)

## Creación de Índices Justificados

- **Índice 1 (Compuesto):** `{ activo: 1, puntuacion_media: -1, titulo: 1 }` --> Acelera ranking Top 10 películas.
- **Índice 2 (Compuesto):** `{ generos: 1, duracion: 1 }` --> Acelera filtro Ciencia Ficción > 120 min.
- **Índice 3 (Simple):** `{ serieId: 1 }` --> Acelera joins con episodios.
- **Índice 4 (Texto):** `{ titulo: "text", descripcion: "text" }` --> Acelera búsquedas de texto.

```javascript
// 1. Índice compuesto para ranking de películas
db.peliculas.createIndex(
  { activo: 1, puntuacion_media: -1, titulo: 1 },
  { name: "idx_peliculas_activo_puntuacion_titulo" },
);

// 2. Índice compuesto para filtro por género y duración
db.peliculas.createIndex(
  { generos: 1, duracion: 1 },
  { name: "idx_peliculas_generos_duracion" },
);

// 3. Índice en episodios por serieId
db.episodios.createIndex({ serieId: 1 }, { name: "idx_episodios_serieId" });

// 4. Índice de texto en películas
db.peliculas.createIndex(
  { titulo: "text", descripcion: "text" },
  {
    name: "idx_peliculas_texto_titulo_descripcion",
    default_language: "spanish",
  },
);
```

## Plan de ejecución

```javascript
// Comprobación de consulta utilizando índice
db.peliculas
  .find({ generos: "Ciencia ficción", duracion: { $gt: 120 } })
  .explain("executionStats");
```
