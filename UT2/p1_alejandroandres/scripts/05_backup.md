# Copias de Seguridad y Seguridad

## 1. Copia de seguridad y restauración

### Copia de seguridad (`mongodump`)

Para hacer una copia de seguridad de la base de datos desde la consola:

```bash
mongodump --db="multimedia" --out="./backup" --gzip
```

### Restauración (`mongorestore`)

Para restaurar la copia de seguridad si hemos borrado la base de datos:

```bash
mongorestore --db="multimedia" --drop --gzip "./backup/multimedia"
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
- **Seguridad en red:** En un entorno real se habilitaría conexión cifrada SSL/TLS.

---

## 4. Cuándo MongoDB no es la mejor opción

1. **Sistemas bancarios o contables:** Para transacciones financieras complejas con muchas tablas relacionadas, una base de datos relacional tradicional (como PostgreSQL) es mejor opción por las restricciones de clave foránea estrictas.
2. **Búsqueda de texto avanzada:** Si se necesita un motor de búsqueda muy avanzado con sinónimos o correctores ortográficos, es mejor usar un motor especializado como Elasticsearch.

---

## 5. Datos históricos y limpieza

- **Borrado automático (Índice TTL):** Para borrar automáticamente logs antiguos después de 30 días:
  ```javascript
  db.logs.createIndex({ fecha: 1 }, { expireAfterSeconds: 2592000 });
  ```
- **Archivo de valoraciones:** Las valoraciones antiguas se pueden exportar a ficheros de respaldo para no llenar el espacio principal.
