# Estructura de un archivo OpenAPI

Un documento OpenAPI describe una API REST en un archivo de texto estructurado escrito en formato **YAML** o **JSON**. Su estructura sigue un conjunto de secciones bien organizadas que permiten definir todos los aspectos de una API: información general, endpoints, modelos de datos, tipos de respuesta y seguridad.

En esta sección aprenderemos la **estructura base** de un archivo OpenAPI.

---

## Estructura general

A continuación se presenta la mínima estructura de un archivo OpenAPI:

```yaml
openapi: 3.0.3
info:
  title: Nombre de la API
  version: 1.0.0
servers:
  - url: https://api.example.com
paths: {}
```

Esta definición es válida, pero incompleta. Una definición real suele incluir más secciones como `paths`, `components` y documentación descriptiva.

---

## Secciones principales

| Sección        | Descripción                                  |
| -------------- | -------------------------------------------- |
| `openapi`      | Versión de la especificación utilizada       |
| `info`         | Información descriptiva de la API            |
| `servers`      | Servidores en los que está desplegada la API |
| `paths`        | Endpoints y operaciones                      |
| `components`   | Elementos reutilizables como modelos         |
| `security`     | Configuración de autenticación (opcional)    |
| `tags`         | Organización de endpoints (opcional)         |
| `externalDocs` | Documentación adicional (opcional)           |

---

### 1. Sección `info`

Define información descriptiva y metadatos de la API.

```yaml
info:
  title: Task Manager API
  description: API para gestionar tareas de usuarios.
  version: 1.0.0
  contact:
    name: Soporte Técnico
    email: soporte@example.com
```

---

### 2. Sección `servers`

Permite indicar las URLs base desde las que estará disponible la API.

```yaml
servers:
  - url: https://api.example.com/v1
    description: Servidor de producción
  - url: http://localhost:3000/v1
    description: Servidor de desarrollo
```

---

### 3. Sección `paths`

Define los **endpoints** de la API y las operaciones disponibles con sus métodos HTTP.

```yaml
paths:
  /tasks:
    get:
      summary: Listar tareas
      responses:
        '200':
          description: Lista de tareas
```

Cada operación dentro de `paths` puede incluir parámetros, request body, respuestas, ejemplos y más.

---

### 4. Sección `components`

Permite definir estructuras reutilizables dentro del documento:

```yaml
components:
  schemas:
    Task:
      type: object
      properties:
        id:
          type: string
        title:
          type: string
```

Elementos reutilizables comunes:

* `schemas`: Esquemas de datos
* `responses`: Respuestas compartidas
* `parameters`: Parámetros reutilizables
* `securitySchemes`: Autenticación
* `requestBodies`: Cuerpos de petición

---

## Ejemplo completo simplificado

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  version: 1.0.0
servers:
  - url: http://localhost:3000/v1

paths:
  /tasks:
    get:
      summary: Obtener tareas
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
        title:
          type: string
```

---

Con esta base ya podemos comenzar a describir correctamente una API utilizando OpenAPI. En el siguiente documento aprenderemos a construir **modelos reutilizables con `components` y `schemas`** para estructurar correctamente los datos.