# Parámetros, Query Params y Request Body en OpenAPI

En una API REST, los datos que recibe un endpoint pueden llegar por **diferentes partes de la petición**. OpenAPI permite documentar correctamente estos datos para que otros desarrolladores sepan cómo usar la API.

---

## Tipos de parámetros en OpenAPI

### 1. Parámetros de ruta (`path`)

Son obligatorios y forman parte de la URL. Ejemplo: `/tasks/{id}`

```yaml
parameters:
  - in: path
    name: id
    required: true
    schema:
      type: string
    description: Identificador único de la tarea
```

---

### 2. Parámetros de consulta (`query`)

Se envían en la URL como filtros u opciones. Ejemplo: `/tasks?status=done&page=2`

```yaml
parameters:
  - in: query
    name: status
    schema:
      type: string
      enum: [todo, in_progress, done]
    description: Filtrar por estado
```

---

### 3. Parámetros de cabecera (`header`)

Se usan para información adicional como autenticación o metadatos.

```yaml
parameters:
  - in: header
    name: X-Request-ID
    schema:
      type: string
    required: false
    description: ID de seguimiento de la petición
```

---

## Cuerpo de la petición (`requestBody`)

Cuando un endpoint recibe datos estructurados (ej: crear tarea), se usa **requestBody** en lugar de `parameters`.

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        $ref: '#/components/schemas/Task'
```

---

## Ejemplo completo aplicado (POST /tasks)

```yaml
paths:
  /tasks:
    post:
      summary: Crear nueva tarea
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [title, status]
              properties:
                title:
                  type: string
                  example: "Aprender OpenAPI"
                description:
                  type: string
                status:
                  type: string
                  enum: [todo, in_progress, done]
      responses:
        '201':
          description: Tarea creada
```

---

## Resumen

| Tipo          | Uso                           | Ejemplo                       |
| ------------- | ----------------------------- | ----------------------------- |
| `path`        | Identificar recursos          | `/tasks/{id}`                 |
| `query`       | Filtros y opciones            | `/tasks?status=done`          |
| `header`      | Metadatos de la petición      | `Authorization: Bearer token` |
| `requestBody` | Datos enviados por el cliente | JSON en POST/PUT              |

---

Con esto ya podemos construir endpoints completos en OpenAPI. En el siguiente contenido aprenderemos a **documentar respuestas utilizando `responses` y códigos de estado HTTP**.
