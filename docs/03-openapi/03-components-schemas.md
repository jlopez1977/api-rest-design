# Components y Schemas en OpenAPI

En OpenAPI, la sección `components` se utiliza para definir **elementos reutilizables** dentro de la especificación de una API. Uno de los elementos más importantes son los **schemas**, que permiten modelar los datos que se envían y reciben en las peticiones.

Los `schemas` actúan como **definiciones de modelos**, equivalentes a clases o estructuras de datos en programación.

---

## ¿Qué es un schema?

Un **schema** describe la estructura de un objeto JSON: qué propiedades tiene, qué tipo de datos son, cuáles son obligatorias y qué restricciones existen.

Ejemplo básico de schema:

```yaml
components:
  schemas:
    Usuario:
      type: object
      properties:
        id:
          type: string
        nombre:
          type: string
        email:
          type: string
```

---

## Tipos de datos más comunes

OpenAPI soporta los siguientes tipos en `schemas`:

| Tipo      | Descripción        | Ejemplo     |
| --------- | ------------------ | ----------- |
| `string`  | Texto              | "David"     |
| `number`  | Número decimal     | 12.5        |
| `integer` | Número entero      | 42          |
| `boolean` | Verdadero/Falso    | true        |
| `array`   | Lista de elementos | [1, 2, 3]   |
| `object`  | Objeto JSON        | { "id": 1 } |

---

## Propiedades comunes en schemas

| Propiedad  | Uso                                            |
| ---------- | ---------------------------------------------- |
| `required` | Lista de campos obligatorios                   |
| `enum`     | Lista limitada de valores permitidos           |
| `format`   | Especifica formato (`date-time`, `email`, etc) |
| `example`  | Ejemplo útil para documentación                |
| `default`  | Valor por defecto                              |

Ejemplo:

```yaml
Task:
  type: object
  required: [title, status]
  properties:
    title:
      type: string
      example: "Comprar comida"
    status:
      type: string
      enum: [todo, in_progress, done]
      example: "todo"
```

---

## Uso de referencias con `$ref`

Para evitar repetir código, OpenAPI permite reutilizar definiciones mediante referencias:

```yaml
responses:
  '200':
    description: Lista de tareas
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Task'
```

`$ref` apunta a un schema ya definido en `components/schemas`.

---

## Ejemplo completo: Task (modelo real del curso)

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
          example: "Preparar ejercicio de OpenAPI"
        description:
          type: string
          example: "Definir models y responses con OpenAPI"
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
          example: "2025-10-25T10:00:00Z"
```

---

## Buenas prácticas

✔ Usar nombres en **singular** para los schemas (`Task`, `User`)
✔ Añadir siempre `example` para mejorar documentación
✔ Declarar `required` solo con lo mínimo necesario
✔ Usar `enum` cuando los valores estén limitados
✔ Evitar duplicar schemas, reutilizar con `$ref`

---

En el siguiente documento aprenderemos a usar los schemas dentro de **entradas y salidas de endpoints** utilizando `requestBody`, `parameters` y `responses`.