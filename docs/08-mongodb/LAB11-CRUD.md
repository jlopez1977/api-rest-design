# LAB11 – CRUD completo para Task Manager API

## Objetivo

Completar el CRUD de la API añadiendo los endpoints que faltan:

| Método | Ruta         | Descripción             |
| ------ | ------------ | ----------------------- |
| GET    | `/tasks/:id` | Obtener tarea por ID    |
| PUT    | `/tasks/:id` | Reemplazar una tarea    |
| PATCH  | `/tasks/:id` | Actualizar parcialmente |
| DELETE | `/tasks/:id` | Eliminar tarea          |

Se mantiene la arquitectura modular con **controllers**, **services** y **routes**.

---

## Requisitos

* API en funcionamiento con MongoDB (LAB10)
* Modelo `Task` ya creado
* Swagger funcionando en `/explorer`

---

## 1. Ampliar servicio: `taskService.js`

```js
import Task from "../models/taskModel.js";
import mongoose from "mongoose";

export const getTaskById = async (id) => {
  if (!mongoose.Types.ObjectId.isValid(id)) throw new Error("Invalid ID");
  const task = await Task.findById(id).lean();
  if (!task) throw new Error("Task not found");
  return task;
};

export const updateTask = async (id, data) => {
  if (!mongoose.Types.ObjectId.isValid(id)) throw new Error("Invalid ID");
  const updated = await Task.findByIdAndUpdate(id, data, { new: true }).lean();
  if (!updated) throw new Error("Task not found");
  return updated;
};

export const patchTask = async (id, data) => {
  if (!mongoose.Types.ObjectId.isValid(id)) throw new Error("Invalid ID");
  const updated = await Task.findByIdAndUpdate(id, data, { new: true }).lean();
  if (!updated) throw new Error("Task not found");
  return updated;
};

export const deleteTask = async (id) => {
  if (!mongoose.Types.ObjectId.isValid(id)) throw new Error("Invalid ID");
  const result = await Task.findByIdAndDelete(id);
  if (!result) throw new Error("Task not found");
  return true;
};
```

---

## 2. Ampliar controlador: `taskController.js`

```js
import {
  getAllTasks,
  createTask,
  getTaskById,
  updateTask,
  patchTask,
  deleteTask
} from "../services/taskService.js";

export const getTask = async (req, res, next) => {
  try {
    const task = await getTaskById(req.params.id);
    res.json(task);
  } catch (err) { next(err); }
};

export const replaceTask = async (req, res, next) => {
  try {
    const updated = await updateTask(req.params.id, req.body);
    res.json(updated);
  } catch (err) { next(err); }
};

export const partialUpdateTask = async (req, res, next) => {
  try {
    const updated = await patchTask(req.params.id, req.body);
    res.json(updated);
  } catch (err) { next(err); }
};

export const removeTask = async (req, res, next) => {
  try {
    await deleteTask(req.params.id);
    res.status(204).send();
  } catch (err) { next(err); }
};
```

---

## 3. Actualizar rutas: `taskRoutes.js`

```js
import { Router } from "express";
import {
  getTasks,
  addTask,
  getTask,
  replaceTask,
  partialUpdateTask,
  removeTask
} from "../controllers/taskController.js";

const router = Router();

router.get("/", getTasks);
router.post("/", addTask);
router.get("/:id", getTask);
router.put("/:id", replaceTask);
router.patch("/:id", partialUpdateTask);
router.delete("/:id", removeTask);

export default router;
```

---

## 4. Probar CRUD desde Swagger UI

1. Abre: `http://localhost:3000/explorer`
2. Crea tarea con `POST /tasks`
3. Copia su `id`
4. Prueba:

   * `GET /tasks/{id}`
   * `PUT /tasks/{id}`
   * `PATCH /tasks/{id}`
   * `DELETE /tasks/{id}`

---

## 5. Manejo profesional de errores

Verifica en `errorHandler` (src/middlewares/basic.js) que se manejen errores validation/not found:

```js
export const errorHandler = (err, req, res, next) => {
  const message = err.message || "Internal server error";
  if (message === "Task not found") return res.status(404).json({ error: message });
  if (message === "Invalid ID") return res.status(400).json({ error: message });
  res.status(500).json({ error: message });
};
```

---

## ✅ Resultado

CRUD completo funcionando con MongoDB:
✔ Listado
✔ Consulta por ID
✔ Creación
✔ Actualización total
✔ Actualización parcial
✔ Eliminación real

---

## 💡 Reto del alumno

Añade validación para que **PUT requiera obligatoriamente `title` y `status`**, pero **PATCH no**. Si faltan, devolver `400 Bad Request`.

---

## ✅ Solución del reto

En el controlador:

```js
export const replaceTask = async (req, res, next) => {
  if (!req.body.title || !req.body.status) {
    return res.status(400).json({ error: "title and status are required for PUT" });
  }
  next();
};
```

---

## ✅ Commit sugerido

```bash
git add src/
git commit -m "lab11: CRUD completo para Task Manager API"
```


