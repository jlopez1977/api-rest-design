# DEMO – API First 

Objetivo de la demo: crear el **primer contrato OpenAPI** de la Task Manager API siguiendo el enfoque API First, sin escribir código de servidor.

Al finalizar tendrás un archivo `openapi/taskmanager.yaml` con:

* Información del API
* Servidores
* Esquemas `Task` y `Error`
* Endpoints iniciales: `GET /tasks` y `POST /tasks`
* Ejemplos de request/response y códigos de estado

---

## Paso 0. Preparación

1. Abre en el navegador Swagger Editor (online).
2. Crea un archivo nuevo (vacío) y selecciona versión `OpenAPI 3.0.x`.

Sugerencia de ruta en el repo para guardar el resultado:

* `openapi/taskmanager.yaml`

---

## Paso 1. Definir metadatos del API (info + version)

Copia este bloque inicial:

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  description: API para gestionar tareas con enfoque API First (demo del curso).
  version: 1.0.0
servers:
  - url: https://api.example.com/v1
    description: Producción (ejemplo)
  - url: http://localhost:3000/v1
    description: Desarrollo local
tags:
  - name: Tasks
    description: Operaciones sobre tareas
```

Comprueba que no hay errores en el panel de validación.

---

## Paso 2. Definir esquemas base (components/schemas)

Añadimos el modelo `Task` y un esquema estándar de error.

```yaml
components:
  schemas:
    Task:
      type: object
      required: [title, status]
      properties:
        id:
          type: string
          example: "64f1b2c9a8e5f9a12c34d567"
        title:
          type: string
          example: "Preparar demo API First"
        description:
          type: string
          example: "Crear contrato OpenAPI y escenarios de prueba"
        status:
          type: string
          enum: [todo, in_progress, done]
          example: "todo"
        priority:
          type: string
          enum: [low, medium, high]
          example: "medium"
        dueDate:
          type: string
          format: date-time
          example: "2025-10-21T12:00:00Z"
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time

    Error:
      type: object
      required: [error]
      properties:
        error:
          type: string
          example: "Validation failed"
        details:
          type: string
          example: "The field 'title' is required"
```

---

## Paso 3. Definir `GET /tasks` (listar tareas)

Creamos el endpoint de lectura con respuesta `200` (lista de `Task`) y errores comunes.

```yaml
paths:
  /tasks:
    get:
      tags: [Tasks]
      summary: Listar tareas
      description: Obtiene la lista de tareas. Permite filtros mediante query params.
      parameters:
        - in: query
          name: status
          schema:
            type: string
            enum: [todo, in_progress, done]
          description: Filtrar por estado
        - in: query
          name: priority
          schema:
            type: string
            enum: [low, medium, high]
          description: Filtrar por prioridad
        - in: query
          name: page
          schema:
            type: integer
            minimum: 1
            default: 1
          description: Página de resultados
        - in: query
          name: limit
          schema:
            type: integer
            minimum: 1
            maximum: 100
            default: 10
          description: Tamaño de página
      responses:
        '200':
            description: Lista paginada de tareas
            content:
              application/json:
                schema:
                  type: object
                  required: [items, page, limit, total]
                  properties:
                    items:
                      type: array
                      items:
                        $ref: '#/components/schemas/Task'
                    page:
                      type: integer
                      example: 1
                    limit:
                      type: integer
                      example: 10
                    total:
                      type: integer
                      example: 37
                examples:
                  ejemplo:
                    value:
                      items:
                        - id: "64f1b2c9a8e5f9a12c34d567"
                          title: "Preparar demo API First"
                          status: "in_progress"
                          priority: "high"
                          dueDate: "2025-10-21T12:00:00Z"
                      page: 1
                      limit: 10
                      total: 1
        '400':
          description: Parámetros de consulta inválidos
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
```

---

## Paso 4. Definir `POST /tasks` (crear tarea)

Añadimos el endpoint de creación con `201 Created` y ejemplo de request/response.

```yaml
    post:
      tags: [Tasks]
      summary: Crear nueva tarea
      description: Crea una tarea a partir de los datos enviados.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              allOf:
                - $ref: '#/components/schemas/Task'
              required:
                - title
                - status
            examples:
              ejemplo:
                value:
                  title: "Leer especificación OpenAPI"
                  description: "Repasar secciones components y paths"
                  status: "todo"
                  priority: "medium"
                  dueDate: "2025-10-25T09:00:00Z"
      responses:
        '201':
          description: Tarea creada
          headers:
            Location:
              description: URL del recurso creado
              schema:
                type: string
              example: "/v1/tasks/64f1b2c9a8e5f9a12c34d567"
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
              examples:
                ejemplo:
                  value:
                    id: "64f1b2c9a8e5f9a12c34d567"
                    title: "Leer especificación OpenAPI"
                    status: "todo"
                    priority: "medium"
                    dueDate: "2025-10-25T09:00:00Z"
                    createdAt: "2025-10-21T10:00:00Z"
                    updatedAt: "2025-10-21T10:00:00Z"
        '400':
          description: Datos de entrada inválidos
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '409':
          description: Conflicto (por ejemplo, duplicado)
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Error interno del servidor
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
```

Nota: Este bloque `post:` va justo debajo de `/tasks` junto al `get:` (misma indentación).

---

## Paso 5. (Opcional) Añadir `GET /tasks/{id}` básico

Para completar el flujo inicial, añadimos la consulta por id. Puedes hacerlo ahora o en el siguiente laboratorio.

```yaml
  /tasks/{id}:
    get:
      tags: [Tasks]
      summary: Obtener una tarea por id
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Tarea encontrada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
        '404':
          description: No existe una tarea con ese id
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
```

---

## Paso 6. Validar el contrato en Swagger Editor

1. Observa el panel de errores/advertencias.
2. Corrige indentaciones o referencias `$ref` si apareciera algún aviso.
3. Verifica que las respuestas tienen `content` y `schema` adecuados.

---

## Paso 7. Probar ejemplos desde la vista “Try it out” (UI)

Si tu editor ofrece previsualización (o usando Swagger UI más adelante):

* Lanza `GET /tasks` y revisa el ejemplo de respuesta.
* Comprueba que los parámetros de consulta aparecen con sus descripciones.
* Revisa el `201 Created` del `POST /tasks` y el header `Location`.

---

## Paso 8. Guardar el contrato en el repositorio

1. Crea (si no existe) la carpeta `openapi/` en el repo.
2. Guarda el contenido como: `openapi/taskmanager.yaml`.
3. Haz commit con un mensaje claro, por ejemplo:
   `feat(openapi): contrato inicial Task Manager API (GET/POST /tasks)`

---

## Paso 9. Checklist mínimo de calidad (contrato inicial)

* [ ] `info` con `title`, `description`, `version`
* [ ] `servers` con al menos local y prod (ejemplo)
* [ ] `tags` para organizar operaciones
* [ ] `components/schemas` con `Task` y `Error`
* [ ] `GET /tasks` con paginación y ejemplos
* [ ] `POST /tasks` con `201 Created` y `Location`
* [ ] Códigos `400/409/500` documentados
* [ ] Validación sin errores en el editor

---

## Paso 10. Resultado completo (merge de los pasos)

Si prefieres copiar el YAML completo ya consolidado, aquí lo tienes:

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  description: API para gestionar tareas con enfoque API First (demo del curso).
  version: 1.0.0
servers:
  - url: https://api.example.com/v1
    description: Producción (ejemplo)
  - url: http://localhost:3000/v1
    description: Desarrollo local
tags:
  - name: Tasks
    description: Operaciones sobre tareas

paths:
  /tasks:
    get:
      tags: [Tasks]
      summary: Listar tareas
      description: Obtiene la lista de tareas. Permite filtros mediante query params.
      parameters:
        - in: query
          name: status
          schema: { type: string, enum: [todo, in_progress, done] }
          description: Filtrar por estado
        - in: query
          name: priority
          schema: { type: string, enum: [low, medium, high] }
          description: Filtrar por prioridad
        - in: query
          name: page
          schema: { type: integer, minimum: 1, default: 1 }
          description: Página de resultados
        - in: query
          name: limit
          schema: { type: integer, minimum: 1, maximum: 100, default: 10 }
          description: Tamaño de página
      responses:
        '200':
          description: Lista paginada de tareas
          content:
            application/json:
              schema:
                type: object
                required: [items, page, limit, total]
                properties:
                  items:
                    type: array
                    items: { $ref: '#/components/schemas/Task' }
                  page: { type: integer, example: 1 }
                  limit: { type: integer, example: 10 }
                  total: { type: integer, example: 37 }
              examples:
                ejemplo:
                  value:
                    items:
                      - id: "64f1b2c9a8e5f9a12c34d567"
                        title: "Preparar demo API First"
                        status: "in_progress"
                        priority: "high"
                        dueDate: "2025-10-21T12:00:00Z"
                    page: 1
                    limit: 10
                    total: 1
        '400':
          description: Parámetros de consulta inválidos
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }
        '500':
          description: Error interno del servidor
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }

    post:
      tags: [Tasks]
      summary: Crear nueva tarea
      description: Crea una tarea a partir de los datos enviados.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              allOf:
                - $ref: '#/components/schemas/Task'
              required: [title, status]
            examples:
              ejemplo:
                value:
                  title: "Leer especificación OpenAPI"
                  description: "Repasar secciones components y paths"
                  status: "todo"
                  priority: "medium"
                  dueDate: "2025-10-25T09:00:00Z"
      responses:
        '201':
          description: Tarea creada
          headers:
            Location:
              description: URL del recurso creado
              schema: { type: string }
              example: "/v1/tasks/64f1b2c9a8e5f9a12c34d567"
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Task' }
              examples:
                ejemplo:
                  value:
                    id: "64f1b2c9a8e5f9a12c34d567"
                    title: "Leer especificación OpenAPI"
                    status: "todo"
                    priority: "medium"
                    dueDate: "2025-10-25T09:00:00Z"
                    createdAt: "2025-10-21T10:00:00Z"
                    updatedAt: "2025-10-21T10:00:00Z"
        '400':
          description: Datos de entrada inválidos
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }
        '409':
          description: Conflicto (por ejemplo, duplicado)
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }
        '500':
          description: Error interno del servidor
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }

  /tasks/{id}:
    get:
      tags: [Tasks]
      summary: Obtener una tarea por id
      parameters:
        - in: path
          name: id
          required: true
          schema: { type: string }
      responses:
        '200':
          description: Tarea encontrada
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Task' }
        '404':
          description: No existe una tarea con ese id
          content:
            application/json:
              schema: { $ref: '#/components/schemas/Error' }

components:
  schemas:
    Task:
      type: object
      required: [title, status]
      properties:
        id: { type: string, example: "64f1b2c9a8e5f9a12c34d567" }
        title: { type: string, example: "Preparar demo API First" }
        description: { type: string, example: "Crear contrato OpenAPI y escenarios de prueba" }
        status: { type: string, enum: [todo, in_progress, done], example: "todo" }
        priority: { type: string, enum: [low, medium, high], example: "medium" }
        dueDate: { type: string, format: date-time, example: "2025-10-21T12:00:00Z" }
        createdAt: { type: string, format: date-time }
        updatedAt: { type: string, format: date-time }

    Error:
      type: object
      required: [error]
      properties:
        error: { type: string, example: "Validation failed" }
        details: { type: string, example: "The field 'title' is required" }
```