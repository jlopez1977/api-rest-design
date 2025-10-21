# DEMO – Uso de Swagger Editor para crear una API paso a paso

En esta demostración vamos a aprender a usar **Swagger Editor**, la herramienta oficial para trabajar con **OpenAPI**. Swagger Editor permite definir APIs en formato YAML, detectar errores y ver los endpoints documentados en tiempo real.

---

## Objetivo de la demo

Aprender a:
✔ Crear una especificación OpenAPI básica
✔ Añadir un endpoint en `/tasks`
✔ Definir un esquema simple
✔ Validar la estructura del archivo

---

## Paso 1: Abrir Swagger Editor

1. Ve a la versión online oficial:
   [https://editor.swagger.io](https://editor.swagger.io)
2. Borra todo el contenido de ejemplo.
3. Asegúrate de que el modo está en **YAML** (no JSON).

---

## Paso 2: Crear la estructura base

Copia este contenido inicial:

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  version: 1.0.0
  description: API para gestionar tareas del curso
servers:
  - url: http://localhost:3000/v1
    description: Servidor local
paths: {}
```

Deberías ver este mensaje en Swagger Editor:
✓ No errors

---

## Paso 3: Definir el primer endpoint

Vamos a añadir `GET /tasks` justo dentro de `paths`:

```yaml
paths:
  /tasks:
    get:
      summary: Listar todas las tareas
      responses:
        '200':
          description: Respuesta exitosa
```

Swagger Editor ya mostrará el endpoint en la parte derecha como documentación.

---

## Paso 4: Añadir el esquema de datos

Creamos un modelo `Task` dentro de `components/schemas`:

```yaml
components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
          example: "abc123"
        title:
          type: string
          example: "Aprender OpenAPI"
```

---

## Paso 5: Usar el schema en la respuesta

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

Ahora la respuesta aparece documentada.

---

## Paso 6: Validar errores en Swagger Editor

Comprueba:
✔ No hay errores en la barra inferior
✔ Los bloques YAML están bien indentados
✔ `$ref` apunta correctamente a `#/components/schemas/Task`

---

## Paso 7: Guardar el contrato

1. En el editor, haz clic en **File → Save as YAML**.
2. Guarda el archivo como:

```
openapi/taskmanager.yaml
```

3. Sube el archivo a tu repositorio Git si aún no lo has hecho.

---

## Resultado final esperado

Tu archivo OpenAPI mínimo debería parecerse a esto:

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  version: 1.0.0
  description: API para gestionar tareas del curso
servers:
  - url: http://localhost:3000/v1
paths:
  /tasks:
    get:
      summary: Listar todas las tareas
      responses:
        '200':
          description: Lista de tareas
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Task'

components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
          example: "abc123"
        title:
          type: string
          example: "Aprender OpenAPI"
```

---

## Conclusión

✔ Swagger Editor permite construir APIs visualmente paso a paso
✔ Valida errores automáticamente
✔ Es ideal para el enfoque **API First**