# ¿Qué es API First?

**API First** es un enfoque de desarrollo que establece que el diseño y definición de la API debe realizarse **antes de escribir cualquier línea de código de implementación**. Este enfoque da protagonismo a la API como **contrato** entre equipos, servicios o sistemas externos, garantizando claridad y consistencia desde el inicio del proyecto.

---

## ¿Qué significa diseñar primero la API?

Tradicionalmente, muchos equipos comienzan programando directamente el backend y **documentan al final** (si es que lo hacen). Esto genera APIs inconsistentes, difíciles de mantener y problemáticas para los equipos de frontend o integradores.

El enfoque API First invierte este proceso:

| Desarrollo clásico                | API First                     |
| --------------------------------- | ----------------------------- |
| Se empieza programando            | Se empieza diseñando          |
| API improvisada                   | API consensuada               |
| Documentación al final (si acaso) | Documentación desde el inicio |
| Cambios frecuentes y caóticos     | Contrato estable              |
| Rompe integraciones               | Facilita colaboración         |

---

## API como contrato

API First considera la API como un **contrato formal** que define:

* Qué recursos expone el servicio
* Cómo se accede a esos recursos (endpoints)
* Qué datos se envían y reciben (modelos)
* Qué reglas de validación existen
* Qué errores puede devolver
* Qué seguridad aplica

---

## Ventajas clave del enfoque API First

✔ Mejora la comunicación entre equipos
✔ Reduce cambios y retrabajos
✔ Asegura consistencia en endpoints y modelos
✔ Permite mockear y probar la API antes de programarla
✔ Facilita el trabajo en paralelo entre frontend y backend
✔ Permite generar código y documentación automáticamente

---

### Herramientas habituales en API First

El enfoque API First suele trabajarse junto con estándares y herramientas como:

* OpenAPI 3
* Swagger Editor
* Swagger UI
* Prism / Mock servers
* Stoplight / Postman Collections
* OpenAPI Generator

---

En el siguiente contenido veremos el **por qué** adoptar API First en proyectos reales.