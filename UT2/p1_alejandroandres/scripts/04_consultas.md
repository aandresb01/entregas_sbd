# 04 - Consultas de Negocio, CRUD y Agregación Compleja

## Operaciones CRUD

```javascript
// Inserción
db.peliculas.insertOne({
  titulo: "Nebulosa Solar",
  anio: 2025,
  duracion: 140,
  generos: ["Ciencia ficción"],
  puntuacion_media: 5.0,
  num_valoraciones: 1,
  activo: true
});

// Actualización parcial
db.peliculas.updateOne(
  { titulo: "Nebulosa Solar" },
  { $inc: { num_valoraciones: 1 } }
);

// Borrado lógico
db.peliculas.updateOne(
  { titulo: "El Último Café" },
  { $set: { activo: false } }
);
```

---

## Consultas para las 6 preguntas de negocio

### 1. Top 10 películas mejor valoradas
```javascript
db.peliculas.find({ activo: true })
  .sort({ puntuacion_media: -1, titulo: 1 })
  .limit(10);
```

### 2. Series con más de 3 temporadas
```javascript
db.series.find({ temporadas_totales: { $gt: 3 }, activo: true })
  .sort({ titulo: 1 })
  .limit(10);
```

### 3. Películas de Ciencia Ficción > 120 min
```javascript
db.peliculas.find({ generos: "Ciencia ficción", duracion: { $gt: 120 }, activo: true })
  .sort({ duracion: -1 })
  .limit(10);
```

### 4. Series con episodios usando $lookup
```javascript
db.series.aggregate([
  { $match: { activo: true } },
  {
    $lookup: {
      from: "episodios",
      localField: "_id",
      foreignField: "serieId",
      as: "episodios"
    }
  },
  {
    $project: {
      titulo: 1,
      num_episodios: { $size: "$episodios" }
    }
  }
]);
```

### 5. Películas con menos de 10 valoraciones
```javascript
db.peliculas.find({ num_valoraciones: { $lt: 10 }, activo: true })
  .sort({ num_valoraciones: 1 })
  .limit(10);
```

### 6. Episodios mejor valorados
```javascript
db.valoraciones.aggregate([
  { $match: { tipoContenido: "episodio", puntuacion: { $gte: 4 } } },
  {
    $lookup: {
      from: "episodios",
      localField: "contenidoId",
      foreignField: "_id",
      as: "episodio"
    }
  },
  { $unwind: "$episodio" },
  { $sort: { puntuacion: -1 } }
]);
```

---

## Agregación Compleja (5 etapas)

```javascript
db.peliculas.aggregate([
  // Etapa 1: Filtrar activos
  { $match: { activo: true } },

  // Etapa 2: Descomponer array de géneros
  { $unwind: "$generos" },

  // Etapa 3: Cruzar con valoraciones
  {
    $lookup: {
      from: "valoraciones",
      localField: "_id",
      foreignField: "contenidoId",
      as: "resenas"
    }
  },

  // Etapa 4: Agrupar por género y calcular promedios
  {
    $group: {
      _id: "$generos",
      total_peliculas: { $sum: 1 },
      duracion_media: { $avg: "$duracion" },
      nota_media: { $avg: "$puntuacion_media" },
      total_resenas: { $sum: { $size: "$resenas" } }
    }
  },

  // Etapa 5: Ordenar por nota media
  { $sort: { nota_media: -1 } }
]);
```
