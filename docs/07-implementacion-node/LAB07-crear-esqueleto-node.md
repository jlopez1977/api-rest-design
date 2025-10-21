# LAB07 – Crear el esqueleto del proyecto Node.js + Express

## Objetivo

Crear la estructura inicial del backend de la API **Task Manager** usando **Node.js + Express** con JavaScript moderno (**ES Modules**) y arquitectura modular profesional.

---

## Requisitos previos

* Haber completado LAB01−LAB06
* GitHub Codespaces activo con este repositorio
* Node.js ya disponible en Codespaces
* Express será instalado durante el lab

---

## Instrucciones paso a paso

### 1. Crear la carpeta del proyecto backend

En la raíz del repositorio, crea una carpeta para el backend si no existe:

```bash
mkdir src
```

---

### 2. Inicializar proyecto Node.js

Ejecuta:

```bash
npm init -y
```

Esto creará un archivo `package.json`.

---

### 3. Configurar ES Modules

Abre `package.json` y añade:

```json
"type": "module",
```

Ejemplo:

```json
{
  "name": "taskmanager-api",
  "version": "1.0.0",
  "type": "module",
  "main": "src/server.js",
  "license": "MIT"
}
```

---

### 4. Instalar Express y Nodemon

```bash
npm install express
npm install --save-dev nodemon
```

---

### 5. Configurar scripts en package.json

```json
"scripts": {
  "start": "node src/server.js",
  "dev": "nodemon src/server.js"
}
```

---

### 6. Crear estructura base del proyecto

```bash
mkdir -p src/routes src/controllers src/services src/middlewares src/config src/utils
touch src/app.js src/server.js
```

---

### 7. Configurar Express en `app.js`

```js
// src/app.js
import express from "express";

const app = express();
app.use(express.json()); // Habilitar JSON

app.get("/health", (req, res) => {
  res.json({ status: "ok" });
});

export default app;
```

---

### 8. Crear servidor en `server.js`

```js
// src/server.js
import app from "./app.js";

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Servidor escuchando en puerto ${PORT}`));
```

---

### 9. Añadir middlewares básicos

Crea el archivo `src/middlewares/basic.js`:

```js
export const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

export const notFound = (req, res, next) => {
  res.status(404).json({ error: "Endpoint not found" });
};

export const errorHandler = (err, req, res, next) => {
  console.error("Error:", err.message);
  res.status(500).json({ error: "Internal server error" });
};
```

Activa los middlewares en `app.js`:

```js
import { logger, notFound, errorHandler } from "./middlewares/basic.js";

app.use(logger);
app.use(notFound);
app.use(errorHandler);
```

---

### 10. Ejecutar el proyecto en modo desarrollo

```bash
npm run dev
```

Abre en el navegador:

```
http://localhost:3000/health
```

✅ Si ves `{ "status": "ok" }`, ¡todo está funcionando!

---

## Reto del alumno 💡

Crea una nueva ruta GET `/welcome` que devuelva:

```json
{ "message": "Welcome to Task Manager API" }
```

---

## ✅ Solución del reto

Añadir en `app.js`:

```js
app.get("/welcome", (req, res) => {
  res.json({ message: "Welcome to Task Manager API" });
});
```

---

## Entrega (commit sugerido)

```bash
git add .
git commit -m "lab07: crear esqueleto inicial Node.js con Express"
```

---

## Tiempo estimado

35–45 minutos