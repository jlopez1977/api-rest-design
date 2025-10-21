# LAB06 – Aplicar buenas prácticas REST + OpenAPI al contrato

## Objetivo

Mejorar el contrato **OpenAPI** de la API Task Manager incorporando buenas prácticas de diseño:
✔ Paginación y filtros
✔ Respuestas consistentes
✔ Manejo estándar de errores
✔ Reutilización con `components`
✔ Aplicar reglas de calidad con Redocly

---

## Requisitos previos

* Haber completado LAB01–LAB05
* Tener `openapi/taskmanager.yaml` definido con `/tasks` (GET y POST)
* Redocly CLI instalado (`openapi lint`)
* Prism Mock funcionando (opcional)

---

## Instrucciones paso a paso

### ✅ Paso 1: Añadir parámetros de paginación y filtros al GET /tasks

Edita `/tasks → get` para que acepte `page`, `limit`, `status`, `priority`:

```yaml
parameters:
  - in: query
    name: page
    schema:
      type: integer
      default: 1
      minimum: 1
    description: Página de resultados
  - in: query
    name: limit
    schema:
      type: integer
      default: 10
      minimum: 1
      maximum: 100
    description: Número de resultados por página
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
```

---

### ✅ Paso 2: Mejorar la respuesta de `GET /tasks` con paginación

Reemplaza la respuesta actual del 200 por esta:

```yaml
responses:
  '200':
    description: Lista paginada de tareas
    content:
      application/json:
        schema:
          type: object
          required: [items, page, limit, totalItems]
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
            totalItems:
              type: integer
              example: 42
            totalPages:
              type: integer
              example: 5
```

---

### ✅ Paso 3: Mejorar respuestas de error

Añade en `components/responses`:

```yaml
components:
  responses:
    ErrorResponse:
      description: Error genérico
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
```

Y reutiliza en los endpoints:

```yaml
'400':
  $ref: '#/components/responses/ErrorResponse'
'500':
  $ref: '#/components/responses/ErrorResponse'
```

---

### ✅ Paso 4: (Opcional avanzado) Añadir respuesta RFC 7807

Añade también en `components/schemas`:

```yaml
ProblemDetails:
  type: object
  properties:
    type:
      type: string
      example: "https://api.ejemplo.com/errors/validation"
    title:
      type: string
      example: "Validation error"
    status:
      type: integer
      example: 400
    detail:
      type: string
      example: "Missing field 'title'"
    instance:
      type: string
      example: "/tasks"
```

---

### ✅ Paso 5: Mejorar POST /tasks con ejemplos

Añade ejemplo de request y response:

```yaml
requestBody:
  content:
    application/json:
      example:
        title: "Revisar errores"
        status: "todo"
```

---

### ✅ Paso 6: Ejecutar validación con Redocly

```
openapi lint openapi/taskmanager.yaml
```

Corrige advertencias si las hay.

---

## Resultado esperado

✔ Contrato mejorado con paginación y filtros
✔ Mejores respuestas
✔ Errores reutilizables
✔ Listo para pasar a implementación

---

## Reto del alumno 💡

Añade un parámetro opcional `search` al endpoint `/tasks` para permitir buscar por texto libre en el título de la tarea.

Ejemplo:

```
GET /tasks?search=demo
```

---

## ✅ Solución del reto

```yaml
- in: query
  name: search
  schema:
    type: string
  description: Buscar texto en el título de la tarea
```

---

## Entrega (commit sugerido)

```bash
git add openapi/taskmanager.yaml
git commit -m "lab06: aplicar buenas prácticas REST y mejora de contrato OpenAPI"
```

---

## Tiempo estimado

30 – 40 minutos