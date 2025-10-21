# LAB01 – Creación del proyecto OpenAPI (API First)

## Objetivo

Crear la definición inicial de la API **Task Manager** siguiendo el enfoque **API First** utilizando **OpenAPI 3**. El resultado será nuestro primer archivo de contrato API (`taskmanager.yaml`) con metadatos y estructura base.

---

## Requisitos previos

* Comprender qué es API First y la importancia del contrato API (visto en teoría).
* Conocer la estructura básica de OpenAPI 3.
* Tener acceso a Swagger Editor (web).

---

## Instrucciones paso a paso

### 1. Abrir Swagger Editor

Accede desde el navegador a:

```
https://editor.swagger.io
```

Borra cualquier contenido inicial del editor.

---

### 2. Definir la estructura mínima del proyecto OpenAPI

Copia este contenido inicial:

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  description: API para la gestión de tareas del curso Diseño de APIs REST
  version: 1.0.0
servers:
  - url: http://localhost:3000/v1
    description: Servidor local de desarrollo
paths: {}
```

Verifica que no haya errores en la barra inferior del editor.

---

### 3. Añadir metadatos opcionales (mejor documentación)

Completa la sección `info` con información adicional:

```yaml
contact:
  name: Equipo del curso
  email: soporte@taskmanager.dev
license:
  name: MIT
  url: https://opensource.org/licenses/MIT
```

---

### 4. Añadir etiqueta de organización (`tags`)

Añade esta sección al final:

```yaml
tags:
  - name: Tasks
    description: Operaciones relacionadas con la gestión de tareas
```

---

### 5. Guardar el archivo en el repositorio

Descarga el archivo desde Swagger Editor usando **File → Save as YAML**
Guárdalo dentro del repositorio en esta ruta:

```
openapi/taskmanager.yaml
```

---

## Resultado esperado

Tu archivo `taskmanager.yaml` debe tener, como mínimo:

✔ Sección `openapi`
✔ Sección `info` con título, descripción y versión
✔ Sección `servers`
✔ Sección `paths` vacía
✔ Sección `tags`

---

### Ejemplo de resultado completo mínimo

```yaml
openapi: 3.0.3
info:
  title: Task Manager API
  description: API para la gestión de tareas del curso Diseño de APIs REST
  version: 1.0.0
  contact:
    name: Equipo del curso
    email: soporte@taskmanager.dev
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT
servers:
  - url: http://localhost:3000/v1
    description: Servidor local de desarrollo

tags:
  - name: Tasks
    description: Operaciones relacionadas con la gestión de tareas

paths: {}
```

---

## Validación

Antes de continuar:

* Asegúrate de que Swagger Editor no muestra errores.
* Verifica indentación y espaciado (OpenAPI es sensible a YAML).
* Haz commit del archivo en tu repositorio.

---

## Reto del alumno 💡

Añade un nuevo servidor alternativo de pruebas llamado `"Servidor de staging"` con URL:

```
https://staging.api.taskmanager.dev/v1
```

---

## Solución del reto ✅

```yaml
servers:
  - url: http://localhost:3000/v1
    description: Servidor local de desarrollo
  - url: https://staging.api.taskmanager.dev/v1
    description: Servidor de staging
```

---

## Entrega (commit sugerido)

Ejecuta:

```
git add openapi/taskmanager.yaml
git commit -m "lab01: definición inicial OpenAPI del proyecto Task Manager API"
```

---

## Tiempo estimado

15 – 20 minutos