# ¿Qué es un Mock Server?

Un **Mock Server** (servidor simulado) es una herramienta que **emula el comportamiento de una API real** antes de que exista su implementación.
Permite que los equipos de desarrollo (por ejemplo, frontend y backend) trabajen en paralelo sin depender de que la API esté ya programada.

---

## ¿Por qué usar un Mock Server?

En el enfoque **API First**, el contrato OpenAPI se define antes de escribir código.
A partir de ese contrato, podemos generar un servidor falso que responde como si la API ya existiera.

Esto ofrece grandes ventajas:

* Permite **probar la API** desde Postman o desde el frontend antes de tener backend real.
* Facilita la **detección temprana de errores de diseño**.
* Permite **validar flujos funcionales** y **documentación viva**.
* Mejora la colaboración entre equipos.

---

## Ejemplo conceptual

Supón que hemos definido el endpoint `GET /tasks` en nuestro archivo `taskmanager.yaml`.

Podemos levantar un mock que devuelva respuestas automáticas según los ejemplos definidos en el contrato.

**Petición simulada:**

```
GET https://mock.taskmanager.dev/v1/tasks
```

**Respuesta mock JSON:**

```json
[
  {
    "id": "1",
    "title": "Preparar práctica de OpenAPI",
    "status": "in_progress",
    "priority": "high"
  },
  {
    "id": "2",
    "title": "Leer documentación de Swagger UI",
    "status": "todo",
    "priority": "medium"
  }
]
```

👉 Esta respuesta no viene de un backend real, sino de un **mock server** configurado automáticamente a partir del contrato OpenAPI.

---

## Mock vs Stub vs Fake

| Término  | Descripción                                            | Uso principal                  |
| -------- | ------------------------------------------------------ | ------------------------------ |
| **Mock** | Simula comportamiento real con respuestas predefinidas | Pruebas y desarrollo temprano  |
| **Stub** | Implementación mínima con valores fijos                | Aislar una parte del sistema   |
| **Fake** | Implementación parcial con cierta lógica real          | Pruebas de integración o demos |

En este curso trabajaremos con **Mocks**, no con código ni lógica real.

---

## Herramientas comunes para crear Mock Servers

| Herramienta                     | Descripción                                                |
| ------------------------------- | ---------------------------------------------------------- |
| **Swagger Editor / SwaggerHub** | Generan mocks automáticos a partir de un archivo OpenAPI   |
| **Prism (Stoplight)**           | CLI profesional para simular APIs REST desde YAML          |
| **Redocly CLI**                 | Permite previsualizar documentación y servir mocks básicos |
| **Postman Mock Server**         | Crea endpoints simulados directamente desde Postman        |

---

## Enfoque del curso

En los siguientes laboratorios:

1. Usaremos **Swagger Editor** para generar un mock automático.
2. Montaremos **Prism** en Codespaces para un mock local.
3. Validaremos que las respuestas concuerdan con los ejemplos del contrato.

Esto nos permitirá **probar nuestra API Task Manager sin backend real**.