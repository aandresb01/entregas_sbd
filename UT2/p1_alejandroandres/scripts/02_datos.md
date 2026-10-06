# 02 - Inserción de Datos de Prueba

## Capturas en [/docs/evidencias](../docs/evidencias/evidencias.md)

```javascript
// Usuarios
const u1 = new ObjectId();
const u2 = new ObjectId();
const u3 = new ObjectId();

db.usuarios.insertMany([
  {
    _id: u1,
    nombre: "Ana Martínez",
    email: "ana.martinez@example.com",
    rol: "espectador",
    activo: true,
  },
  {
    _id: u2,
    nombre: "Carlos López",
    email: "carlos.lopez@example.com",
    rol: "espectador",
    activo: true,
  },
  {
    _id: u3,
    nombre: "Elena Gómez",
    email: "elena.gomez@example.com",
    rol: "admin",
    activo: true,
  },
]);

// Películas
const p1 = new ObjectId();
const p2 = new ObjectId();
const p3 = new ObjectId();

db.peliculas.insertMany([
  {
    _id: p1,
    titulo: "Horizonte Cero",
    descripcion: "Una tripulación viaja a un planeta desconocido.",
    anio: 2024,
    duracion: 135,
    generos: ["Ciencia ficción", "Acción"],
    puntuacion_media: 4.8,
    num_valoraciones: 15,
    activo: true,
  },
  {
    _id: p2,
    titulo: "El Último Café",
    descripcion: "Dos desconocidos se encuentran en una cafetería.",
    anio: 2023,
    duracion: 105,
    generos: ["Drama"],
    puntuacion_media: 3.5,
    num_valoraciones: 4,
    activo: true,
  },
  {
    _id: p3,
    titulo: "Matrix Inception",
    descripcion: "Aventura futurista en un mundo cibernético.",
    anio: 2022,
    duracion: 148,
    generos: ["Ciencia ficción"],
    puntuacion_media: 4.9,
    num_valoraciones: 120,
    activo: true,
  },
]);

// Series
const s1 = new ObjectId();
const s2 = new ObjectId();

db.series.insertMany([
  {
    _id: s1,
    titulo: "Ciudad Oscura",
    anioInicio: 2021,
    anioFin: null,
    temporadas_totales: 4,
    generos: ["Ciencia ficción"],
    activo: true,
  },
  {
    _id: s2,
    titulo: "Vecinos del Caos",
    anioInicio: 2022,
    anioFin: 2023,
    temporadas_totales: 2,
    generos: ["Comedia"],
    activo: true,
  },
]);

// Episodios
const ep1 = new ObjectId();
const ep2 = new ObjectId();
const ep3 = new ObjectId();

db.episodios.insertMany([
  {
    _id: ep1,
    serieId: s1,
    temporada: 1,
    numero: 1,
    titulo: "La Llegada",
    duracion: 52,
  },
  {
    _id: ep2,
    serieId: s1,
    temporada: 1,
    numero: 2,
    titulo: "La Señal",
    duracion: 48,
  },
  {
    _id: ep3,
    serieId: s1,
    temporada: 1,
    numero: 3,
    titulo: "El Enigma",
    duracion: 55,
  },
]);

// Valoraciones
db.valoraciones.insertMany([
  {
    usuarioId: u1,
    contenidoId: p1,
    tipoContenido: "pelicula",
    puntuacion: 5,
    comentario: "Muy buena película.",
  },
  {
    usuarioId: u2,
    contenidoId: p1,
    tipoContenido: "pelicula",
    puntuacion: 4,
    comentario: "Efectos espectaculares.",
  },
  {
    usuarioId: u1,
    contenidoId: s1,
    tipoContenido: "serie",
    puntuacion: 4,
    comentario: "Gran serie.",
  },
  {
    usuarioId: u3,
    contenidoId: ep1,
    tipoContenido: "episodio",
    puntuacion: 5,
    comentario: "Piloto excelente.",
  },
]);
```
