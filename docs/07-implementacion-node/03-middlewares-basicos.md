# Middlewares básicos en Express

Los **middlewares** son funciones que se ejecutan **antes** o **después** de que una petición llegue a su destino final en la API. Son una pieza fundamental en Express, ya que permiten añadir funcionalidades como autenticación, validación, logging o manejo de errores.

---

## ¿Qué es un middleware?

Un middleware es simplemente una función con esta firma:

```js
(req, res, next) => { ... }
```

Donde:

* `req` = información de la petición
* `res` = respuesta al cliente
* `next()` = llama al siguiente middleware

---

## ¿Cómo funciona la cadena de middlewares?

Cuando llega una petición a Express, pasa por una **cadena** de middlewares:

```
Request -> Middleware A -> Middleware B -> Controller -> Response
```

Cada middleware decide si:
✅ llama a `next()` → continúa al siguiente
⛔ o responde directamente → corta la cadena

---

## Ejemplos de middlewares básicos

### 1. Middleware de log (registro de peticiones)

```js
export const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};
```

---

### 2. Middleware para JSON obligatorio

```js
export const requireJson = (req, res, next) => {
  if (!req.is("application/json")) {
    return res.status(415).json({ error: "Content-Type must be application/json" });
  }
  next();
};
```

---

### 3. Middleware 404 Not Found

```js
export const notFound = (req, res, next) => {
  res.status(404).json({ error: "Endpoint not found" });
};
```

---

### 4. Middleware de manejo de errores centralizado

```js
export const errorHandler = (err, req, res, next) => {
  console.error("Error:", err.stack);
  res.status(500).json({ error: "Internal server error" });
};
```

> Los errores lanzados con `throw new Error()` pasarán automáticamente por este middleware si lo declaramos correctamente al final del `app.js`.

---

## Cómo usar middlewares en Express

```js
import express from "express";
import { logger, notFound, errorHandler } from "./middlewares/basic.js";

const app = express();

app.use(logger); // Se ejecuta para todas las rutas
app.use(express.json()); // Middleware nativo de Express
app.get("/health", (req, res) => res.json({ status: "ok" }));

app.use(notFound); // Para rutas inexistentes
app.use(errorHandler); // Manejo global de errores

export default app;
```

---

## Buenas prácticas con middlewares

✔ Colocar `express.json()` antes de usar `req.body`
✔ Usar `app.use()` para middlewares globales
✔ Colocar `notFound` **después de las rutas**
✔ Colocar `errorHandler` **al final de todo**
✔ No duplicar lógica → reutilizar middlewares

---

Con esto ya tenemos la base para manejar flujos completos y controlados en Express. En el próximo laboratorio crearemos el esqueleto del servidor y añadiremos estos middlewares iniciales.