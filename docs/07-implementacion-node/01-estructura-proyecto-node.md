# Estructura de un proyecto Node.js para una API REST

Antes de comenzar la implementación de la API Task Manager, es importante definir una **arquitectura de proyecto clara y modular** para garantizar escalabilidad, mantenibilidad y facilidad de pruebas.

---

## ¿Por qué es importante estructurar el proyecto?

Una mala estructura lleva a código desordenado y difícil de mantener. Una buena organización permite:

✔ Separar responsabilidades
✔ Reutilizar código
✔ Facilitar pruebas y documentación
✔ Preparar el proyecto para crecer

---

## Enfoque recomendado: Arquitectura modular

Dividiremos el backend en capas con responsabilidades claras:

| Carpeta       | Responsabilidad                                |
| ------------- | ---------------------------------------------- |
| `routes`      | Define las rutas/endpoints                     |
| `controllers` | Maneja la lógica de cada endpoint              |
| `services`    | Contiene la lógica de negocio                  |
| `models`      | Representa los datos (se usará con MongoDB)    |
| `middlewares` | Reglas antes/después de peticiones             |
| `config`      | Configuración general (puerto, DB…)            |
| `utils`       | Funciones auxiliares reutilizables             |
| `docs`        | Documentación y OpenAPI (ya existe en el repo) |

---

## Estructura inicial del proyecto

Cuando comencemos la implementación, nuestra estructura será:

```
src/
  routes/
  controllers/
  services/
  middlewares/
  config/
  utils/
openapi/
docs/
```

Luego la iremos completando progresivamente con MongoDB + Swagger UI + lógica.

---

## Uso de ES Modules (import/export)

Usaremos JavaScript moderno en lugar de CommonJS. Ejemplo de módulo:

```js
// controllers/taskController.js
export const getTasks = (req, res) => {
  res.json([{ id: 1, title: "Demo task" }]);
};
```

```js
// routes/taskRoutes.js
import { Router } from "express";
import { getTasks } from "../controllers/taskController.js";

const router = Router();
router.get("/", getTasks);
export default router;
```

```js
// app.js
import express from "express";
import taskRoutes from "./routes/taskRoutes.js";

const app = express();
app.use(express.json());
app.use("/tasks", taskRoutes);
export default app;
```

---

## Buenas prácticas de organización

✅ Código dividido por funcionalidad
✅ Nada de rutas largas en `app.js`
✅ Nada de lógica dentro de las rutas
✅ Todo reutilizable y fácil de testear

---

## Conclusión

Ya tenemos la **arquitectura base** para comenzar a construir nuestra API real. En el siguiente laboratorio crearemos el proyecto Node.js desde cero en Codespaces con esta estructura base.