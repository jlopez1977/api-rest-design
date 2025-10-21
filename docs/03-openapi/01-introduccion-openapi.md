# Introducción a OpenAPI

**OpenAPI** es una especificación abierta y estandarizada para describir, documentar y definir APIs REST.
Permite representar de manera formal cómo funciona una API: qué endpoints existen, qué parámetros reciben, qué datos devuelven, qué errores pueden producirse y qué modelos utilizan.

El uso de OpenAPI se ha convertido en una práctica fundamental dentro del enfoque **API First**, ya que permite diseñar y comunicar APIs de manera clara antes de escribir código.

---

## ¿Qué es OpenAPI?

OpenAPI define un formato legible tanto por humanos como por máquinas, que describe todos los elementos de una API REST.

Con una definición OpenAPI se pueden:

* **Documentar** la API de forma automática.
* **Validar** contratos entre backend y frontend.
* **Generar** código cliente o servidor.
* **Simular** el comportamiento con servidores mock.
* **Automatizar** pruebas y despliegues.

---

## Breve historia: Swagger → OpenAPI

El formato fue originalmente creado por **Swagger** (SmartBear).
En 2016, Swagger fue adoptado por la **OpenAPI Initiative**, pasando a ser un estándar abierto mantenido por la **Linux Foundation**.

Actualmente:

* **Swagger** es el conjunto de herramientas (Editor, UI, Codegen).
* **OpenAPI** es la especificación (el formato estándar).

**En resumen:**

> Swagger = herramientas
> OpenAPI = estándar

---

## Estructura general de un documento OpenAPI

Un archivo OpenAPI puede escribirse en **YAML** o **JSON**, y se compone de varias secciones principales.

Ejemplo mínimo:

```yaml
openapi: 3.0.3
info:
  title: Ejemplo de API
  version: 1.0.0
  description: API de ejemplo para ilustrar la estructura básica de OpenAPI.
servers:
  - url: https://api.example.com/v1
paths:
  /usuarios:
    get:
      summary: Lista de usuarios
      responses:
        '200':
          description: Respuesta exitosa
```

---

## Secciones principales del documento

| Sección          | Descripción                                                  |
| ---------------- | ------------------------------------------------------------ |
| **openapi**      | Versión de la especificación (ej. `3.0.3` o `3.1.0`)         |
| **info**         | Metadatos de la API (nombre, versión, descripción, contacto) |
| **servers**      | URLs base donde se despliega la API                          |
| **paths**        | Endpoints de la API y sus operaciones (GET, POST, etc.)      |
| **components**   | Elementos reutilizables (schemas, responses, parameters…)    |
| **tags**         | Agrupación lógica de endpoints                               |
| **security**     | Configuración de autenticación                               |
| **externalDocs** | Enlaces externos de documentación                            |

---

## Ejemplo ampliado con respuesta JSON

```yaml
openapi: 3.0.3
info:
  title: Ejemplo extendido API de productos
  version: 1.0.0
  description: API REST para gestionar productos.
servers:
  - url: https://api.ejemplo.com/v1
paths:
  /productos:
    get:
      summary: Listar productos
      description: Devuelve la lista de productos disponibles.
      responses:
        '200':
          description: Lista de productos
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    id:
                      type: integer
                      example: 1
                    nombre:
                      type: string
                      example: "Portátil 15 pulgadas"
                    precio:
                      type: number
                      format: float
                      example: 899.99
```

---

## Ventajas de trabajar con OpenAPI

✔ Facilita la **comunicación** entre equipos
✔ Permite **generar documentación automática**
✔ Mejora la **consistencia** en el desarrollo de APIs
✔ Posibilita **mockear** y **probar** APIs antes de tener el backend
✔ Estándar soportado por herramientas en todos los lenguajes

---

En el siguiente documento veremos con detalle **cómo se estructura un archivo OpenAPI**, y cómo definir cada sección (info, servers, paths, components) siguiendo las convenciones más utilizadas.