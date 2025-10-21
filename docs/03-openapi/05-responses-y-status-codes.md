# Responses y Códigos de Estado HTTP en OpenAPI

En OpenAPI, las **respuestas (`responses`)** definen lo que devuelve un endpoint, incluyendo códigos HTTP, ejemplos y estructuras de datos. Documentar correctamente las respuestas es esencial para que la API sea clara y predecible para los consumidores.

---

## Estructura básica de una respuesta

Cada operación debe incluir al menos una respuesta:

```yaml
responses:
  '200':
    description: Operación exitosa
```

El código de estado debe escribirse **entre comillas** (`'200'`) porque YAML no permite claves numéricas sin comillas.

---

## Respuestas con contenido JSON

Cuando una respuesta devuelve datos, se debe documentar el esquema del contenido:

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

---

## Ejemplo completo: respuestas de GET /tasks

```yaml
responses:
  '200':
    description: Lista de tareas obtenida con éxito
    content:
      application/json:
        schema:
          type: array
          items:
            $ref: '#/components/schemas/Task'
  '400':
    description: Parámetros de consulta inválidos
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Error'
  '500':
    description: Error interno del servidor
```

---

## Respuesta con `example`

El campo `example` permite incluir una muestra real del JSON de respuesta, lo que mejora mucho la documentación.

```yaml
responses:
  '200':
    description: Una tarea encontrada
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Task'
        example:
          id: "1"
          title: "Aprender OpenAPI"
          status: "todo"
```

---

## Buenas prácticas con respuestas

✔ Incluir al menos un código de éxito y códigos de error comunes
✔ Usar siempre el código correcto (201 para creación, 204 para borrado)
✔ Documentar errores con schema claro
✔ Usar `application/json` como tipo de contenido principal
✔ Incluir ejemplos reales siempre que sea posible

---

## Tabla de respuestas recomendadas

| Operación | Código principal | Otros códigos |
| --------- | ---------------- | ------------- |
| GET       | 200 OK           | 400, 404, 500 |
| POST      | 201 Created      | 400, 409, 500 |
| PUT       | 200 OK           | 400, 404, 500 |
| DELETE    | 204 No Content   | 404, 500      |

---

En el siguiente documento veremos cómo usar **Swagger Editor** para crear y editar especificaciones OpenAPI de forma práctica.