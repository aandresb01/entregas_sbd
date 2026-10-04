# 01 - Creación de Colecciones con Validación ($jsonSchema)

```javascript
use multimedia;

// Limpiar colecciones anteriores
db.valoraciones.drop();
db.episodios.drop();
db.series.drop();
db.peliculas.drop();
db.usuarios.drop();
db.generos.drop();

// 1. Colección peliculas
db.createCollection("peliculas", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["titulo", "anio", "duracion", "generos", "activo"],
      properties: {
        titulo: { bsonType: "string" },
        descripcion: { bsonType: "string" },
        anio: { bsonType: "int", minimum: 1888, maximum: 2030 },
        duracion: { bsonType: "int", minimum: 1 },
        generos: { bsonType: "array", minItems: 1, items: { bsonType: "string" } },
        puntuacion_media: { bsonType: "double", minimum: 0.0, maximum: 5.0 },
        num_valoraciones: { bsonType: "int", minimum: 0 },
        activo: { bsonType: "bool" }
      }
    }
  }
});

// 2. Colección series
db.createCollection("series", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["titulo", "anioInicio", "temporadas_totales", "generos", "activo"],
      properties: {
        titulo: { bsonType: "string" },
        anioInicio: { bsonType: "int", minimum: 1950 },
        anioFin: { bsonType: ["int", "null"] },
        temporadas_totales: { bsonType: "int", minimum: 1 },
        generos: { bsonType: "array", items: { bsonType: "string" } },
        activo: { bsonType: "bool" }
      }
    }
  }
});

// 3. Colección episodios
db.createCollection("episodios", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["serieId", "temporada", "numero", "titulo", "duracion"],
      properties: {
        serieId: { bsonType: "objectId" },
        temporada: { bsonType: "int", minimum: 1 },
        numero: { bsonType: "int", minimum: 1 },
        titulo: { bsonType: "string" },
        duracion: { bsonType: "int", minimum: 1 }
      }
    }
  }
});

// 4. Colección usuarios
db.createCollection("usuarios", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["nombre", "email", "rol", "activo"],
      properties: {
        nombre: { bsonType: "string" },
        email: { bsonType: "string", pattern: "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$" },
        rol: { enum: ["admin", "editor", "espectador"] },
        activo: { bsonType: "bool" }
      }
    }
  }
});

// 5. Colección valoraciones
db.createCollection("valoraciones", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["usuarioId", "contenidoId", "tipoContenido", "puntuacion"],
      properties: {
        usuarioId: { bsonType: "objectId" },
        contenidoId: { bsonType: "objectId" },
        tipoContenido: { enum: ["pelicula", "serie", "episodio"] },
        puntuacion: { bsonType: "int", minimum: 1, maximum: 5 },
        comentario: { bsonType: "string" }
      }
    }
  }
});
```
