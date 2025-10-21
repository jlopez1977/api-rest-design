# Introducción a MongoDB

MongoDB es una base de datos **NoSQL orientada a documentos** ampliamente utilizada en el desarrollo de APIs. A diferencia de las bases de datos relacionales (SQL), MongoDB no utiliza tablas ni filas, sino **colecciones** y **documentos JSON**.

---

## ¿Qué es una base de datos NoSQL?

NoSQL significa “Not Only SQL”. Se refiere a bases de datos que:

* No requieren esquemas rígidos
* Se adaptan fácilmente a datos flexibles
* Escalan horizontalmente
* Son ideales para aplicaciones modernas y APIs

---

## ¿Por qué usamos MongoDB en este curso?

MongoDB es muy popular en arquitectura de **microservicios** y APIs REST porque:
✔ Usa JSON (formato natural para APIs)
✔ Permite cambios rápidos en estructuras de datos
✔ Se integra fácilmente con Node.js
✔ Tiene un ecosistema moderno (Atlas, Compass, Docker, Mongoose, etc.)

---

## Comparativa: SQL vs MongoDB

| Característica   | SQL (Relacional)    | MongoDB (NoSQL Documentos) |
| ---------------- | ------------------- | -------------------------- |
| Estructura       | Tablas y filas      | Colecciones y documentos   |
| Formato de datos | Filas con columnas  | JSON/BSON                  |
| Esquema          | Rígido              | Flexible                   |
| Relaciones       | JOINs               | Referencias o embebidos    |
| Escalado         | Vertical            | Horizontal                 |
| Ideal para       | Datos estructurados | Datos flexibles y APIs     |

---

## Conceptos clave en MongoDB

| Concepto      | Descripción                                  |
| ------------- | -------------------------------------------- |
| Base de datos | Conjunto de colecciones                      |
| Colección     | Conjunto de documentos (similar a tabla)     |
| Documento     | Objeto JSON almacenado                       |
| `_id`         | Identificador único generado automáticamente |
| BSON          | Versión binaria optimizada de JSON           |

Ejemplo de documento en MongoDB:

```json
{
  "_id": "64a9f2172b8f0570d0a55012",
  "title": "Aprender MongoDB",
  "status": "todo",
  "priority": "high"
}
```

---

## Modelo de datos para APIs

MongoDB permite dos enfoques para estructurar datos:

### 1. Embebido (embedded)

Guardar dentro del mismo documento datos relacionados.

```json
{
  "title": "Reunión",
  "status": "todo",
  "tags": ["work", "urgent"]
}
```

### 2. Referenciado

Relacionar documentos por ID (similar a JOINs en SQL):

```json
{
  "title": "Proyecto Backend",
  "userId": "649f00bc1234abcd5678ef90"
}
```

---

## Ventajas de MongoDB para APIs REST

✅ Flexible: permite cambiar datos sin migraciones
✅ Rápido de desarrollar
✅ JSON nativo
✅ Fácil integración con Node.js
✅ Ideal para modelos simples como listas de tareas

---

## MongoDB en este curso

Usaremos MongoDB para **persistir las tareas** de la API. La integración será progresiva:

| Etapa        | Objetivo                            |
| ------------ | ----------------------------------- |
| LAB09        | Datos en memoria                    |
| ✅ LAB10      | Conectar con MongoDB usando Docker  |
| ✅ LAB10      | Implementar CRUD real               |
| Más adelante | Validación + autenticación opcional |

---

En el siguiente documento veremos **cómo dockerizar MongoDB** para usarlo en GitHub Codespaces.