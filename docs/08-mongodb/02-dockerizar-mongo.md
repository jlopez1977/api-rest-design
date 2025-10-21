# Dockerizar MongoDB para desarrollo

En esta sección prepararemos MongoDB usando **Docker Compose** para disponer de una base de datos sin necesidad de instalar nada en local. Además incluiremos **mongo-express**, una interfaz web muy sencilla para navegar las colecciones mientras desarrollamos.

---

## Objetivo

✅ Levantar MongoDB en local con Docker
✅ Añadir panel visual con mongo-express
✅ Crear volumen persistente para no perder datos
✅ Preparar conexión para Node.js (LAB10)

---

## Requisitos

* Tener Docker instalado o usar GitHub Codespaces (ya viene configurado)
* Archivo del curso clonado
* Puerto 27017 libre en Codespaces/local

---

## 1. Crear archivo docker-compose.yml

En la raíz del proyecto, crea este archivo:

```yaml
version: "3.8"

services:
  mongodb:
    image: mongo:6
    container_name: taskmanager-mongo
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
    volumes:
      - mongo-data:/data/db

  mongo-express:
    image: mongo-express:1.0.0-alpha
    container_name: taskmanager-mongo-express
    restart: always
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_SERVER: mongodb
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: example
      ME_CONFIG_BASICAUTH_USERNAME: admin
      ME_CONFIG_BASICAUTH_PASSWORD: admin
    depends_on:
      - mongodb

volumes:
  mongo-data:
```

---

## 2. Levantar MongoDB y mongo-express

Ejecuta:

```bash
docker compose up -d
```

Verifica que los contenedores están arriba:

```bash
docker ps
```

---

## 3. Probar conexión

| Servicio                     | URL                                            |
| ---------------------------- | ---------------------------------------------- |
| MongoDB                      | `mongodb://root:example@localhost:27017`       |
| Mongo Express (interfaz web) | [http://localhost:8081](http://localhost:8081) |

Cuando entres a mongo-express, usa estas credenciales:

```
Usuario: admin
Contraseña: admin
```

---

## 4. Notas importantes para Codespaces

Si trabajas en Codespaces:

* Abre el puerto **8081** como `Public`
* MongoDB seguirá accesible localmente

---

## 5. Detener o reiniciar

```bash
docker compose stop     # Detener
docker compose start    # Arrancar de nuevo
docker compose down     # Apagar y borrar contenedores
```

> **Importante**: gracias al volumen `mongo-data`, los datos no se borran con `down`.

---

## Resumen

✔ MongoDB funcionando en Docker
✔ mongo-express accesible vía navegador
✔ Datos persistentes en volumen
✔ Preparado para conectar desde Node.js