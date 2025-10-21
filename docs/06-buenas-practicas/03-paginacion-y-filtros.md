# Paginación y filtros en APIs REST

En una API profesional es importante evitar devolver grandes volúmenes de datos en una sola respuesta. Para mejorar el rendimiento, reducir carga de red y dar control al cliente, se utilizan **paginación y filtros**.

---

## ¿Qué es la paginación?

La paginación permite devolver **resultados en bloques** en lugar de toda la colección. Esto mejora rendimiento y escalabilidad.

---

## Estrategias de paginación

### 1. Paginación con `page` y `limit` (recomendada)

Usa número de página y tamaño de página:

```
GET /tasks?page=2&limit=10
```

Ventajas:

* Fácil de entender
* Muy extendida en APIs modernas

---

### 2. Paginación con `offset` y `limit`

Usa desplazamiento desde el inicio de la colección:

```
GET /tasks?offset=20&limit=10
```

Ventajas:

* Natural para bases de datos SQL
* Útil para recorrer colecciones grandes

---

## Ejemplo de respuesta paginada (bien diseñada)

```json
{
  "items": [
    { "id": "1", "title": "Tarea 1" },
    { "id": "2", "title": "Tarea 2" }
  ],
  "page": 1,
  "limit": 2,
  "totalItems": 50,
  "totalPages": 25
}
```

---

## ¿Qué son los filtros?

Los filtros permiten restringir resultados mediante parámetros de consulta (`query params`).

Ejemplos:

```
GET /tasks?status=done
GET /products?category=electronics&brand=sony
GET /orders?from=2024-01-01&to=2024-01-31
```

---

## Ordenación (`sort`)

Permite ordenar resultados por campo:

```
GET /tasks?sort=title
GET /tasks?sort=-createdAt   # - indica descendente
```

---

## Combinar paginación + filtros + ordenación

```
GET /tasks?page=1&limit=5&status=todo&sort=-priority
```

---

## Buenas prácticas

| Recomendación           | ✅ Correcto       | ❌ Incorrecto  |
| ----------------------- | ---------------- | ------------- |
| Usar query params       | /tasks?page=2    | /tasks/page/2 |
| No usar nombres raros   | limit=10         | l=10          |
| Límite máximo razonable | limit <= 100     | sin límite    |
| Respuesta clara         | items + metadata | solo array    |
| Devolver metadata útil  | totalPages       | NONE          |

---

## Qué evitar

❌ No expongas lógica SQL en la API (`limit 10 offset 20` en rutas)
❌ No mezclar filtros con rutas (`/tasks/done`)
❌ No devolver listas sin límite (⚠️ riesgo rendimiento)

---

En los **laboratorios** usaremos **paginación con `page` y `limit`** como estándar para nuestra API.