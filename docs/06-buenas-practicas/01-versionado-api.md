# Versionado de APIs

El **versionado de APIs** es una práctica esencial para permitir la evolución de un servicio sin romper la compatibilidad con clientes o sistemas que dependen de versiones anteriores. Cada vez que realizamos cambios incompatibles con versiones previas (*breaking changes*), debemos publicar una nueva versión de la API.

---

## ¿Por qué versionar una API?

El versionado permite:

* Evolucionar una API sin interrumpir servicios existentes.
* Añadir nuevas funcionalidades de forma ordenada.
* Corregir errores de diseño sin romper integraciones anteriores.
* Mantener compatibilidad entre distintos consumidores.

---

## ¿Cuándo cambiar de versión?

Según las reglas del versionado semántico, debemos incrementar la versión cuando realizamos cambios como:

| Cambio                                    | ¿Nueva versión?           |
| ----------------------------------------- | ------------------------- |
| Añadir propiedad opcional                 | ❌ No rompe compatibilidad |
| Corregir error tipográfico en descripción | ❌ No                      |
| Cambiar estructura de respuesta           | ✅ Sí                      |
| Eliminar un atributo                      | ✅ Sí                      |
| Cambiar formato de datos                  | ✅ Sí                      |
| Renombrar un endpoint                     | ✅ Sí                      |

---

## Estrategias de versionado

Existen diferentes formas de versionar una API:

### 1. Versionado en la URL (recomendado)

Es la forma más clara y fácil de gestionar:

```
/api/v1/products
/api/v2/products
```

### 2. Versionado en cabecera HTTP (header)

Menos visible pero más flexible:

```
GET /products
Accept: application/vnd.company.v2+json
```

### 3. Versionado por parámetro de consulta (menos recomendable)

```
GET /products?version=2
```

---

## Buenas prácticas para versionado

✔ Empieza siempre con `v1`
✔ Mantén versiones antiguas durante un tiempo antes de retirarlas
✔ Documenta qué ha cambiado entre versiones
✔ Evita hacer cambios rompientes sin actualizar la versión
✔ Notifica siempre a los consumidores antes de eliminar una versión

---

## Ejemplo de versionado correcto

```
/api/v1/users      # Versión estable actual
/api/v2/users      # Nueva versión con cambios importantes
```

---

## Ejemplo de breaking change que obliga a versionar

Antes:

```json
{
  "id": 1,
  "name": "Laptop"
}
```

Después:

```json
{
  "productId": 1,
  "name": "Laptop",
  "price": 999.99
}
```

✅ Cambió la estructura → se debe publicar como **nueva versión**.

---

Versionar correctamente es clave para mantener APIs estables y confiables a largo plazo. En el siguiente contenido veremos **cómo manejar errores en APIs de forma estándar**.