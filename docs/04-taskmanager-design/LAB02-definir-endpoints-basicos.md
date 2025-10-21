# LAB02 – Definir endpoints básicos de la API

## Objetivo

Añadir los primeros endpoints de la API **Task Manager** siguiendo el enfoque API First. En este laboratorio definiremos las operaciones básicas sobre tareas:

* `GET /tasks` → Obtener lista de tareas
* `POST /tasks` → Crear una nueva tarea

---

## Requisitos previos

* Haber completado LAB01 con el archivo `openapi/taskmanager.yaml` creado.
* Conocer la estructura de `paths` en OpenAPI.
* Saber documentar métodos HTTP básicos (`GET` y `POST`).

---

## Instrucciones paso a paso

### 1. Abrir el archivo OpenAPI

Edita el archivo creado en LAB01:

```
openapi/taskmanager.yaml
```

---

### 2. Añadir el endpoint GET /tasks

Debajo de `paths: {}`, reemplázalo con el siguiente contenido:

```yaml
paths:
  /tasks:
    get:
      tags:
        - Tasks
      summary: Obtener todas las tareas
      description: Devuelve una lista de tareas registradas
      responses:
        '200':
          description: Lista de tareas obtenida correctamente
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    id:
                      type: string
                      example: "1"
                    title:
                      type: string
                      example: "Aprender OpenAPI"
                    status:
                      type: string
                      example: "todo"
```

---

### 3. Añadir el endpoint POST /tasks

Añade justo debajo del `get:` lo siguiente:

```yaml
    post:
      tags:
        - Tasks
      summary: Crear una nueva tarea
      description: Crea una nueva tarea a partir de los datos enviados
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - title
                - status
              properties:
                title:
                  type: string
                  example: "Preparar curso de OpenAPI"
                status:
                  type: string
                  example: "todo"
      responses:
        '201':
          description: Tarea creada correctamente
```

---

### 4. Validar archivo

Usa Swagger Editor para copiar y pegar el contenido actualizado. Debe mostrarse sin errores.

---

## Resultado esperado

El archivo debe incluir ya los endpoints básicos dentro de la sección `paths`:

✔ `/tasks`
✔ GET → listar
✔ POST → crear

---

## Validación

Comprueba que:

* Ambos métodos aparecen correctamente en Swagger Editor.
* No hay errores de indentación.
* Se visualiza la sección **Tasks** como etiqueta (tag).

---

## Reto del alumno 💡

Añade una descripción clara al método `POST /tasks` indicando que devuelve el ID de la tarea creada.

---

## Solución del reto ✅

```yaml
responses:
  '201':
    description: Tarea creada correctamente. El ID de la nueva tarea es devuelto en la respuesta.
```

---

## Entrega (commit sugerido)

Ejecuta:

```
git add openapi/taskmanager.yaml
git commit -m "lab02: añadir endpoints básicos GET y POST para /tasks"
```

---

## Tiempo estimado

20 – 25 minutos