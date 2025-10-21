# Testing de APIs con Postman Web

El testing es un elemento fundamental en el ciclo de vida de una API profesional. Permite validar que los endpoints funcionan correctamente, devolver respuestas consistentes y cumplir con la definición del contrato (**OpenAPI**). Además, reduce errores antes de desplegar y facilita la colaboración entre equipos.

---

## ¿Por qué testear APIs?

Testear una API garantiza:

* ✅ Que los endpoints funcionan como se espera
* ✅ Que no se rompen funcionalidades al añadir cambios
* ✅ Que los errores se manejan correctamente
* ✅ Que se cumple el contrato definido en OpenAPI
* ✅ Que podemos automatizar pruebas del backend

---

## Tipos de pruebas de API

| Tipo de prueba      | Objetivo                          | Ejemplo                         |
| ------------------- | --------------------------------- | ------------------------------- |
| Prueba manual       | Verificar rápidamente un endpoint | GET `/tasks` devuelve 200       |
| Prueba automatizada | Validar con scripts ejecutables   | Verificar JSON response         |
| Prueba de contrato  | Cumplimiento OpenAPI              | Respuesta coincide con esquema  |
| Prueba funcional    | Validar una operación real        | Crear + consultar + eliminar    |
| Prueba de error     | Validar casos negativos           | POST `/tasks` sin `title` → 400 |

---

## Herramientas para testing de APIs (comparativa)

| Herramienta                       | Instalación            | Scripts    | CI/CD        | Facilidad            |
| --------------------------------- | ---------------------- | ---------- | ------------ | -------------------- |
| **Postman Web** ✅ (usaremos esta) | ❌ No requiere instalar | ✅ Sí       | ✅ Compatible | ⭐ Muy fácil          |
| Bruno                             | ⚠️ Requiere instalar   | ✅ Sí       | ✅            | ⭐ Ligero             |
| Insomnia                          | ⚠️ Requiere instalar   | ✅ Sí       | ✅            | ⭐ Profesional        |
| Hoppscotch                        | ✅ Web                  | ❌ Limitado | ❌            | ⭐ Rápido pero básico |
| Thunder Client (VSCode)           | ⚠️ Plugin              | ✅ Sí       | ❌            | ⭐ Para devs          |

> 🎯 **Elegimos Postman Web** porque no requiere instalación, es potente y permite trabajar 100% en navegador, ideal para formación y entornos cloud.

---

## ¿Qué es Postman Web?

Postman Web es la versión online de Postman accesible desde navegador:
👉 [https://www.postman.com/](https://www.postman.com/)

Permite:
✅ Enviar peticiones HTTP (GET, POST, PUT, DELETE…)
✅ Probar APIs como la nuestra
✅ Crear colecciones reutilizables
✅ Guardar entornos y variables
✅ Añadir **tests automatizados** con JavaScript

---

## Conceptos clave en Postman

| Concepto                   | Descripción                                     |
| -------------------------- | ----------------------------------------------- |
| **Colección**              | Conjunto organizado de peticiones API           |
| **Workspace**              | Área de trabajo colaborativa                    |
| **Variables**              | Datos reutilizables (URLs, IDs)                 |
| **Ambiente (Environment)** | Configuración para desarrollo/prod              |
| **Tests**                  | Scripts para validar respuestas automáticamente |

---

## Ejemplo real con nuestra API: `/tasks`

**Petición GET**:

```
GET http://localhost:3000/tasks
```

**Respuesta esperada**:

```json
{
  "items": [],
  "page": 1,
  "limit": 10,
  "totalItems": 0,
  "totalPages": 1
}
```

**Primer test en Postman:**

```js
pm.test("Código 200 correcto", function () {
  pm.response.to.have.status(200);
});
```

---

## Variables útiles en Postman

| Nombre      | Valor (ejemplo)                                |
| ----------- | ---------------------------------------------- |
| `baseUrl`   | [http://localhost:3000](http://localhost:3000) |
| `version`   | v1                                             |
| `authToken` | (más adelante opcional)                        |

Ejemplo usando variable:

```
GET {{baseUrl}}/tasks
```

---

## Scripts de test básicos (Postman)

Los scripts permiten validar respuestas automáticamente:

```js
pm.test("Respuesta en JSON", () => {
  pm.response.to.be.json;
});

pm.test("Lista de tareas existe", () => {
  const body = pm.response.json();
  pm.expect(body).to.have.property("items");
});
```

---

## Buenas prácticas de testing de API

✔ Crear una colección con los endpoints de la API
✔ Añadir tests simples a cada endpoint
✔ Probar casos **felices** (success) y **errores** (fail)
✔ Usar variables para la base URL
✔ Guardar ejemplos de respuestas válidas
✔ Documentar pruebas junto al proyecto

---

## Preparación para el laboratorio

En el **LAB11**:
✅ Crearemos **una colección Postman Web**
✅ Importaremos **todos los endpoints de la Task Manager API**
✅ Añadiremos **tests automáticos básicos**
✅ Exportaremos la colección al repositorio del curso

