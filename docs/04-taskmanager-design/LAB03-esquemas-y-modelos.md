# LAB03 – Definición de modelos con OpenAPI (components/schemas)

## Objetivo

Definir modelos reutilizables en la API usando **components/schemas** para evitar repetir estructuras JSON en los endpoints y mejorar la consistencia del diseño. Crearemos los modelos:

* `Task` → representación de una tarea
* `Error` → respuesta estándar para errores

---

## Requisitos previos

* Haber completado LAB01 y LAB02.
* Conocer qué son los `schemas` en OpenAPI.
* Tener el archivo `openapi/taskmanager.yaml` actualizado.

---

## Instrucciones paso a paso

### 1. Abrir el archivo OpenAPI

Edita el archivo:

```
openapi/taskmanager.yaml
```

---

### 2. Añadir la sección `components`

Al final del archivo, añade la estructura base si no existe:

```yaml
components:
  schemas: {}
```

---

### 3. Definir el modelo Task

Añade este schema dentro de `components/schemas`:

```yaml
components:
  schemas:
    Task:
      type: object
      required:
        - title
        - status
      properties:
        id:
          type: string
          example: "64ab32cd9f0c1e12a45b78d9"
        title:
          type: string
          example: "Preparar práctica de OpenAPI"
        description:
          type: string
          example: "Definir modelos y esquemas en OpenAPI"
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
          example: "2025-10-30T14:00:00Z"
```

---

### 4. Definir el modelo de error

Añade este modelo para respuestas de error reutilizables:

```yaml
    Error:
      type: object
      required:
        - error
      properties:
        error:
          type: string
          example: "Validation error"
        details:
          type: string
          example: "The field 'title' is required"
```

---

### 5. Usar los modelos en los endpoints

Modifica el endpoint `GET /tasks` para usar `$ref`:

```yaml
responses:
  '200':
    description: Lista de tareas
    content:
      application/json:
        schema:
          type: array
          items:
            $ref: '#/components/schemas/Task'
```

Modifica también la respuesta del POST:

```yaml
responses:
  '201':
    description: Tarea creada correctamente
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Task'
```

---

## Resultado esperado

Tu archivo OpenAPI ahora debe incluir:

✔ Componentes reutilizables (`Task` y `Error`)
✔ Referencias usando `$ref`
✔ Estructura más limpia y mantenible

---

## Validación

En Swagger Editor verifica:

* No hay errores en el panel de validación.
* El esquema `Task` aparece en la sección **Schemas**.
* Las respuestas de `GET` y `POST` usan `$ref`.

---

## Reto del alumno 💡

Añade una nueva propiedad opcional llamada `tags` al modelo `Task`. Debe ser un **array de strings** y permitir valores como `"home"` o `"work"`.

---

## Solución del reto ✅

```yaml
tags:
  type: array
  items:
    type: string
  example: ["work", "urgent"]
```

---

## Entrega (commit sugerido)

```
git add openapi/taskmanager.yaml
git commit -m "lab03: añadir modelos Task y Error con schemas"
```

---

## Tiempo estimado

25 – 30 minutos