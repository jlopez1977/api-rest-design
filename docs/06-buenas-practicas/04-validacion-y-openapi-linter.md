# Validación y linters en APIs con OpenAPI

La **validación** en APIs es el proceso que garantiza que los datos enviados y recibidos cumplen con unas reglas bien definidas. Esto hace que las APIs sean **predecibles, seguras y confiables**.

---

## Tipos de validación

### ✅ 1. Validación de entrada (Request Validation)

Consiste en comprobar que los datos enviados por el cliente cumplen con lo esperado:

* Campos obligatorios presentes (`required`)
* Tipos correctos (`string`, `number`, etc.)
* Longitud mínima/máxima (`minLength`, `maxLength`)
* Valores permitidos (`enum`)
* Formatos válidos (`email`, `uuid`, `date-time`)

Ejemplo en OpenAPI:

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        type: object
        required: [title]
        properties:
          title:
            type: string
            minLength: 3
            example: "Comprar leche"
```

---

### ✅ 2. Validación de salida (Response Validation)

Garantiza que **la API devuelve siempre datos bien formados**, útiles y coherentes con su contrato.

Ejemplo:

```yaml
responses:
  '201':
    description: Tarea creada
    content:
      application/json:
        schema:
          $ref: '#/components/schemas/Task'
```

---

## Validación con JSON Schema

OpenAPI usa **JSON Schema** internamente para validar datos. Permite definir reglas como:

```yaml
Task:
  type: object
  required: [title, status]
  properties:
    title:
      type: string
      minLength: 3
    status:
      type: string
      enum: [todo, in_progress, done]
```

---

## ¿Qué es un OpenAPI Linter?

Un **linter** comprueba automáticamente la calidad del archivo OpenAPI. No solo detecta errores técnicos, sino también:

✔ Inconsistencias
✔ Falta de documentación
✔ Falta de ejemplos
✔ Buenas prácticas REST
✔ Esquemas incompletos

---

## Redocly OpenAPI CLI (linter profesional)

En LAB04 ya usamos una validación con Redocly CLI.
Ejemplo básico:

```bash
openapi lint openapi/taskmanager.yaml
```

---

## Reglas de calidad recomendadas

Archivo `.redocly.yaml`:

```yaml
lint:
  extends:
    - recommended
  rules:
    operation-description: error
    no-empty-servers: warn
    info-contact: warn
```

---

## ¿Por qué usar validación automática?

| Beneficio            | Impacto                           |
| -------------------- | --------------------------------- |
| Mejora consistencia  | API clara                         |
| Evita errores        | Menos fallos antes de implementar |
| Facilidad en equipo  | Mismo estilo para todos           |
| Preparado para CI/CD | Automatizable                     |

---

## Resumen

✔ Validar datos es crítico en APIs REST
✔ OpenAPI permite validar request y response
✔ JSON Schema define reglas de datos
✔ Redocly permite aplicar buenas prácticas automáticamente
✔ El contrato debe ser **de calidad antes de escribir código**

---

En el siguiente paso aplicaremos **buenas prácticas directamente sobre nuestra API Task Manager** usando OpenAPI.