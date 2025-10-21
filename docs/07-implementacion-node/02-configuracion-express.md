# Configuración de Express en un proyecto Node.js

Express es el framework más popular para construir APIs REST con Node.js. Es ligero, flexible y fácil de extender con middlewares y librerías. A partir de este punto empezaremos a construir nuestra API Task Manager utilizando Express como framework base.

---

## ¿Por qué usar Express?

✔ Sencillo de aprender
✔ Flexible y minimalista
✔ Ecosistema maduro y compatible con OpenAPI
✔ Soporta middlewares y arquitectura modular
✔ Perfecto para construir APIs profesionales

---

## Instalación de Express

Dentro del proyecto (que inicializaremos en el próximo laboratorio), Express se instala con:

```bash
npm install express
```

---

## Crear un servidor básico

Un servidor Express mínimo luce así:

```js
// app.js
import express from "express";
const app = express();
app.use(express.json()); // Habilita JSON en peticiones

export default app;
```

Y en un archivo separado lanzamos el servidor:

```js
// server.js
import app from "./app.js";

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`API escuchando en http://localhost:${PORT}`);
});
```

---

## Endpoint de prueba `/health`

Es una buena práctica incluir un endpoint de salud para verificar que el servidor responde:

```js
app.get("/health", (req, res) => {
  res.json({ status: "ok" });
});
```

---

## ¿Por qué separar `app.js` y `server.js`?

| Motivo        | Beneficio                                               |
| ------------- | ------------------------------------------------------- |
| Modulación    | Código más ordenado                                     |
| Testeabilidad | Supertest puede importar `app.js` sin arrancar servidor |
| Escalabilidad | Permite organizar middlewares y rutas                   |

---

## Organización recomendada

A medida que el proyecto crezca, **Express debe organizarse modularmente**:

```
src/
  app.js          # Configuración Express
  server.js       # Arranque del servidor
  routes/         # Rutas
  controllers/    # Controladores
  services/       # Lógica de negocio
  middlewares/    # Middlewares
```

---

## Buenas prácticas básicas

✔ Separar responsabilidades (configuración vs servidor)
✔ Habilitar `express.json()` para parsear JSON
✔ Usar `PORT` desde entorno con fallback
✔ Añadir endpoint `/health`
✔ Exportar la app para testing futuro

---

En el siguiente documento veremos cómo añadir **middlewares básicos** para mejorar nuestra API desde el inicio.