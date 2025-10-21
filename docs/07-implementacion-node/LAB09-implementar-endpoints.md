# LAB09 – Implementar endpoints desde el contrato OpenAPI

## Objetivo

Implementar los primeros endpoints reales de la API **Task Manager** basados en la definición creada en OpenAPI. En este laboratorio construiremos:

| Método | Ruta     | Descripción       |
| ------ | -------- | ----------------- |
| `GET`  | `/tasks` | Listar tareas     |
| `POST` | `/tasks` | Crear nueva tarea |

Se trabajará **sin base de datos todavía**. Usaremos **memoria local** para simular datos hasta integrar MongoDB en LAB10.

---

## Requisitos previos

* Haber completado LAB07 y LAB08
* API corriendo en `http://localhost:3000`
* Rutas documentadas en Swagger UI (`/explorer`)
* Archivo `src/` ya creado con estructura inicial

---

## Instrucciones paso a paso

### ✅ Paso 1 – Crear servicio de tareas

Archivo: `src/services/taskService.js`

```js
let tasks = []; // Simulación de datos en memoria

export const getAllTasks = () => {
  return tasks;
};

export const createTask = (taskData) => {
  const newTask = {
    id: String(tasks.length + 1), // ID simple
    ...taskData,
  };
  tasks.push(newTask);
  return newTask;
};
```

---

### ✅ Paso 2 – Crear controlador de tareas

Archivo: `src/controllers/taskController.js`

```js
import { getAllTasks, createTask } from "../services/taskService.js";

export const getTasks = (req, res) => {
  const tasks = getAllTasks();
  res.json(tasks);
};

export const addTask = (req, res) => {
  const { title, status } = req.body;

  if (!title || !status) {
    return res.status(400).json({ error: "title and status are required" });
  }

  const newTask = createTask({ title, status });
  res.status(201).json(newTask);
};
```

---

### ✅ Paso 3 – Crear rutas

Archivo: `src/routes/taskRoutes.js`

```js
import { Router } from "express";
import { getTasks, addTask } from "../controllers/taskController.js";

const router = Router();

router.get("/", getTasks);
router.post("/", addTask);

export default router;
```

---

### ✅ Paso 4 – Conectar rutas en Express

Edita `src/app.js` para conectar las rutas:

```js
import taskRoutes from "./routes/taskRoutes.js";
app.use("/tasks", taskRoutes);
```

---

### ✅ Paso 5 – Probar endpoints

**Listar tareas**

```
GET http://localhost:3000/tasks
```

**Crear nueva tarea**

```
POST http://localhost:3000/tasks
Content-Type: application/json

{
  "title": "Aprender OpenAPI",
  "status": "todo"
}
```

Respuesta:

```json
{
  "id": "1",
  "title": "Aprender OpenAPI",
  "status": "todo"
}
```

---

### ✅ Paso 6 – Probar también desde Swagger UI

1. Ir a: `http://localhost:3000/explorer`
2. Probar `GET /tasks`
3. Probar `POST /tasks` usando **"Try it out"**

---

## Resultado esperado

✔ API funcionando con endpoints reales
✔ Datos persistentes en memoria temporal
✔ Implementación alineada con OpenAPI
✔ Arquitectura modular

---

## Reto del alumno 💡

Añade una propiedad opcional `priority` a cada tarea, con valores posibles:

```
low, medium, high
```

Debe aceptarse en el `POST /tasks` y devolverse en el JSON de respuesta si se envía.

---

## ✅ Solución del reto

En `addTask` del controlador cambia:

```js
const { title, status, priority } = req.body;
const newTask = createTask({ title, status, priority });
```

---

## Entrega (commit sugerido)

```bash
git add src/
git commit -m "lab09: implementar endpoints GET y POST /tasks con arquitectura modular"
```

---

## Tiempo estimado

30–45 minutos
