# Modelo de datos: Task Manager API

Antes de integrar MongoDB en el backend, debemos definir con claridad **el modelo de datos** que representará una **tarea (Task)** en nuestra API. Este modelo debe ser **consistente** con:

✔ La definición OpenAPI
✔ La lógica de negocio
✔ La estructura JSON utilizada en la API
✔ MongoDB como base de datos
✔ Futuras necesidades del proyecto

---

## Requisitos del modelo

Una tarea (**Task**) debe almacenar:

| Campo         | Tipo   | Obligatorio | Descripción          |
| ------------- | ------ | ----------- | -------------------- |
| `title`       | string | ✅ Sí        | Título de la tarea   |
| `status`      | string | ✅ Sí        | Estado actual        |
| `priority`    | string | ❌ No        | Prioridad            |
| `description` | string | ❌ No        | Detalle opcional     |
| `dueDate`     | date   | ❌ No        | Fecha límite         |
| `createdAt`   | date   | ✅ Sí        | Fecha de creación    |
| `updatedAt`   | date   | ✅ Sí        | Última actualización |

---

## Estados permitidos (`status`)

Para mantener orden y consistencia:

| Estado        | Descripción |
| ------------- | ----------- |
| `todo`        | Pendiente   |
| `in_progress` | En curso    |
| `done`        | Completada  |

---

## Prioridades (`priority`)

| Prioridad | Uso   |
| --------- | ----- |
| `low`     | Baja  |
| `medium`  | Media |
| `high`    | Alta  |

---

## Representación JSON del documento Task

Ejemplo:

```json
{
  "_id": "64b5f8e2c02c4b8a4c8a1d42",
  "title": "Diseñar contrato OpenAPI",
  "status": "in_progress",
  "priority": "high",
  "description": "Definir endpoints y modelos iniciales",
  "dueDate": "2025-01-30T10:00:00.000Z",
  "createdAt": "2025-01-25T15:12:00.000Z",
  "updatedAt": "2025-01-25T15:12:00.000Z"
}
```

📌 MongoDB añade el campo `_id` automáticamente a cada documento.

---

## Diseño orientado a MongoDB

✔ Modelo simple y directo
✔ No requiere relaciones complejas
✔ ✅ No se necesitan referencias ni colecciones adicionales
✔ Perfecto para CRUD REST
✔ Compatible con OpenAPI y Swagger UI

---

## Coherencia entre OpenAPI y MongoDB

⚠ Importante:
Este modelo **debe coincidir** con el esquema definido en **`components/schemas/Task`** dentro del archivo `openapi/taskmanager.yaml`.

Si el modelo cambia aquí → debe actualizarse también en OpenAPI para mantener la **alineación API First** ✅

---

## Conclusión

El modelo Task queda definido así:

```plaintext
Task
 ├─ _id: ObjectId
 ├─ title: String *
 ├─ status: String *
 ├─ priority: String
 ├─ description: String
 ├─ dueDate: Date
 ├─ createdAt: Date *
 └─ updatedAt: Date *
```

* Campos obligatorios

---

En el siguiente laboratorio **LAB10** conectaremos MongoDB al proyecto Node.js para reemplazar la lista en memoria por persistencia real.