# Copias de Seguridad y Seguridad

## 1. Copia de seguridad y restauración

### Copia de seguridad (`mongodump`)

Para hacer una copia de seguridad de la base de datos desde la consola:

```bash
mongodump --db="multimedia" --gzip --out="./backup"
```

### Restauración (`mongorestore`)

Para restaurar la copia de seguridad si hemos borrado la base de datos:

```bash
mongorestore --db="multimedia" --gzip --drop "./backup/multimedia"
```

---

## 2. Usuarios y permisos

En MongoDB he definido estos permisos para mantener la seguridad:

```javascript
use admin;

// Usuario para la aplicación
db.createUser({
  user: "app_user",
  pwd: "Password123!",
  roles: [
    { role: "read", db: "multimedia" },
    { role: "readWrite", db: "multimedia", collection: "valoraciones" }
  ]
});

// Usuario administrador
db.createUser({
  user: "admin_user",
  pwd: "AdminPassword123!",
  roles: [
    { role: "readWrite", db: "multimedia" }
  ]
});
```

---

## 3. Seguridad y privacidad de los datos

- **Anonimización:** No se usan datos personales reales. Los correos de los usuarios son ficticios (`ejemplo@example.com`).
- **Seguridad en red:** En un entorno real se habilitaría conexión cifrada TLS.

---

## 4. Cuándo MongoDB no es la mejor opción

1. **Guardar los archivos de vídeo:** MongoDB es perfecto para guardar los datos de las películas y series, pero no es una buena idea guardar los archivos de vídeo reales dentro de la base de datos porque ocupan demasiado.
2. **Edición de vídeos:** Si nuestra aplicación necesitara editar los vídeos o aplicarles filtros, MongoDB no nos serviría, ya que no está pensado para modificar ni procesar los vídeos internamente.

---

## 5. Datos históricos y limpieza

- **Borrado automático (Índice TTL):** Para borrar automáticamente logs antiguos después de 30 días:
  ```javascript
  db.logs.createIndex({ fecha: 1 }, { expireAfterSeconds: 2592000 });
  ```
- **Archivo de valoraciones:** Las valoraciones antiguas se pueden exportar a ficheros de respaldo para no llenar el espacio principal.
