# Gestión de errores en APIs REST

La gestión de errores es fundamental en el diseño de una API. Una respuesta de error bien definida ayuda a los integradores a entender **qué ha salido mal** y **cómo solucionarlo**. El objetivo es que todos los errores sigan una estructura **clara, consistente y predecible**.

---

## ¿Qué NO debemos hacer?

❌ Enviar errores con código 200:

```json
{
  "status": "error",
  "message": "No se encontró el usuario"
}
```

❌ Devolver mensajes poco informativos:

```
Error 500
```

❌ Cambiar el formato según el endpoint:

```
{ "error": "not found" }
```

```
{ "message": "No existe" }
```

---

## Buenas prácticas básicas

✔ Usar **códigos HTTP adecuados**
✔ Devolver **JSON estructurado**
✔ **No exponer información sensible**
✔ Proveer **mensajes claros para cliente**
✔ Mantener **un formato consistente en toda la API**

---

## Estructura básica recomendada

```json
{
  "error": "Invalid input",
  "details": "The field 'email' is required"
}
```

---

## Evolución del diseño de errores

Modelo inicial (suficiente para muchos proyectos):

```json
{
  "error": "Not Found",
  "statusCode": 404,
  "message": "The requested resource does not exist"
}
```

---

## Diseño profesional basado en estándar RFC 7807

La **RFC 7807 (Problem Details for HTTP APIs)** define un formato estándar para errores en APIs.

Ejemplo:

```json
{
  "type": "https://api.ejemplo.com/errors/invalid-data",
  "title": "Invalid request data",
  "status": 400,
  "detail": "The field 'title' is required",
  "instance": "/tasks"
}
```

### Campos recomendados

| Campo      | Descripción                               |
| ---------- | ----------------------------------------- |
| `type`     | URL única que identifica el tipo de error |
| `title`    | Resumen del error                         |
| `status`   | Código HTTP                               |
| `detail`   | Explicación más concreta                  |
| `instance` | Ruta o recurso relacionado con el error   |

---

## Ejemplos de errores estándar

### Error 400 – Datos inválidos

```json
{
  "type": "https://api.ejemplo.com/errors/validation",
  "title": "Validation error",
  "status": 400,
  "detail": "The field 'status' must be one of [todo, done]"
}
```

### Error 404 – No encontrado

```json
{
  "type": "https://api.ejemplo.com/errors/not-found",
  "title": "Resource not found",
  "status": 404,
  "detail": "Task with id 123 not found"
}
```

### Error 500 – Error interno

```json
{
  "type": "https://api.ejemplo.com/errors/server",
  "title": "Internal server error",
  "status": 500,
  "detail": "Unexpected server error"
}
```

---

## Resumen de buenas prácticas

| Recomendación                 | ✅ Correcto          | ❌ Incorrecto        |
| ----------------------------- | ------------------- | ------------------- |
| Código HTTP adecuado          | `404 Not Found`     | `200 OK` con error  |
| Formato JSON estándar         | Claro y consistente | JSON variable       |
| Información útil              | Explica qué pasó    | Mensaje genérico    |
| No filtrar detalles sensibles | Sin stacktrace      | Exponer stack o SQL |

---

En el siguiente documento veremos cómo aplicar **paginación y filtros** correctamente en APIs REST para controlar grandes volúmenes de datos.