# LAB10 – Integrar MongoDB con Node.js (Mongoose + persistencia real)

## Objetivo

Conectar la API a **MongoDB** usando **Mongoose**, reemplazando la lista en memoria por **persistencia real**.
Incluye:

* Conexión con **retry automático**
* Variables de entorno con `.env`
* **Modelo Task con validación** (enums y `required`)
* Refactor de servicios para usar MongoDB
* Pruebas desde Swagger UI (`/explorer`)

---

## Requisitos previos

* Docker Compose con MongoDB + mongo-express corriendo (módulo 08.02)
* Proyecto Node funcionando (LAB07 + LAB08)
* OpenAPI servido en `/explorer`
* Node.js en ESM

---

## 1) Instalar dependencias

```bash
npm install mongoose dotenv
```

---

## 2) Configurar variables de entorno

Crea un archivo `.env` en la raíz del repo:

```
MONGODB_URI=mongodb://root:example@localhost:27017/taskmanager?authSource=admin
PORT=3000
```

> Nota: Con Codespaces/Docker locales, `localhost:27017` es correcto.
> Si usas redes distintas, ajusta el host en la URI (p. ej. `mongodb`).

---

## 3) Conexión a MongoDB con retry (src/config/db.js)

Crea `src/config/db.js`:

```js
import mongoose from "mongoose";

const MAX_RETRIES = 10;
const RETRY_DELAY_MS = 2000; // 2s

export const connectDB = async (uri = process.env.MONGODB_URI) => {
  let attempt = 0;

  const connectOnce = async () => {
    attempt++;
    try {
      await mongoose.connect(uri, {
        // opciones recomendadas
        // useNewUrlParser/useUnifiedTopology ya no son necesarios en Mongoose 6+
      });
      console.log("✅ Conectado a MongoDB");
    } catch (err) {
      console.error(`❌ Error conectando a MongoDB (intento ${attempt}):`, err.message);
      if (attempt < MAX_RETRIES) {
        console.log(`Reintentando en ${RETRY_DELAY_MS / 1000}s...`);
        await new Promise((r) => setTimeout(r, RETRY_DELAY_MS));
        return connectOnce();
      } else {
        console.error("⛔ No fue posible conectar a MongoDB tras varios intentos");
        throw err;
      }
    }
  };

  // Manejo elegante del cierre
  mongoose.connection.on("disconnected", () => {
    console.warn("⚠️ Conexión a MongoDB perdida");
  });

  mongoose.connection.on("error", (e) => {
    console.error("⚠️ Error de conexión MongoDB:", e.message);
  });

  await connectOnce();
};
```

---

## 4) Modelo Task con validación (src/models/taskModel.js)

Crea `src/models/taskModel.js`:

```js
import mongoose from "mongoose";

const TaskSchema = new mongoose.Schema(
  {
    title: {
      type: String,
      required: true,
      trim: true,
      minlength: 1,
    },
    status: {
      type: String,
      required: true,
      enum: ["todo", "in_progress", "done"],
      default: "todo",
    },
    priority: {
      type: String,
      enum: ["low", "medium", "high"],
      default: "medium",
    },
    description: {
      type: String,
      trim: true,
    },
    dueDate: {
      type: Date,
    },
  },
  {
    timestamps: true, // createdAt, updatedAt automáticos
    versionKey: false,
  }
);

export default mongoose.model("Task", TaskSchema);
```

---

## 5) Refactor de servicios para usar MongoDB (src/services/taskService.js)

Sustituye el contenido del servicio por operaciones con Mongoose:

```js
// src/services/taskService.js
import Task from "../models/taskModel.js";

/**
 * Listar tareas (opcional: filtros básicos)
 * filters: { status, priority, page, limit, search }
 */
export const getAllTasks = async (filters = {}) => {
  const {
    status,
    priority,
    page = 1,
    limit = 50,
    search,
  } = filters;

  const query = {};
  if (status) query.status = status;
  if (priority) query.priority = priority;
  if (search) query.title = { $regex: search, $options: "i" };

  const skip = (Number(page) - 1) * Number(limit);

  const [items, totalItems] = await Promise.all([
    Task.find(query).skip(skip).limit(Number(limit)).sort({ createdAt: -1 }).lean(),
    Task.countDocuments(query),
  ]);

  const totalPages = Math.ceil(totalItems / Number(limit) || 1);
  return { items, page: Number(page), limit: Number(limit), totalItems, totalPages };
};

export const createTask = async (taskData) => {
  const created = await Task.create(taskData);
  return created.toObject();
};
```

> Nota: ya dejamos preparado **paginación + filtros** al hilo del LAB06.

---

## 6) Actualizar controladores a async/await (src/controllers/taskController.js)

```js
import { getAllTasks, createTask } from "../services/taskService.js";

export const getTasks = async (req, res, next) => {
  try {
    const { status, priority, page, limit, search } = req.query;
    const result = await getAllTasks({ status, priority, page, limit, search });
    res.json(result);
  } catch (err) {
    next(err); // lo manejará el errorHandler global
  }
};

export const addTask = async (req, res, next) => {
  try {
    const { title, status, priority, description, dueDate } = req.body;

    // Validación mínima a nivel controlador (lo fuerte lo hace Mongoose)
    if (!title || !status) {
      return res.status(400).json({ error: "title and status are required" });
    }

    const newTask = await createTask({ title, status, priority, description, dueDate });
    res.status(201).json(newTask);
  } catch (err) {
    next(err);
  }
};
```

---

## 7) Conectar a la base de datos antes de arrancar (src/server.js)

Modifica `src/server.js` para conectar a Mongo **antes** de `listen`:

```js
// src/server.js
import "dotenv/config.js";              // carga .env
import app from "./app.js";
import { connectDB } from "./config/db.js";

const PORT = process.env.PORT || 3000;

const start = async () => {
  try {
    await connectDB(); // espera a Mongo con retry
    app.listen(PORT, () => {
      console.log(`Servidor escuchando en http://localhost:${PORT}`);
      console.log(`→ Swagger UI:           http://localhost:${PORT}/explorer`);
      console.log(`→ OpenAPI (JSON):       http://localhost:${PORT}/taskmanager-api.json`);
    });
  } catch (err) {
    console.error("Fallo crítico al iniciar la app:", err.message);
    process.exit(1);
  }
};

start();
```

> Importante: añadimos `import "dotenv/config.js";` para leer `.env` sin código adicional.

---

## 8) Probar la API (Swagger UI o curl)

1. Asegúrate de que Docker está levantado:

```bash
docker compose up -d
```

2. Inicia el servidor:

```bash
npm run dev
```

3. Prueba desde Swagger UI:

```
http://localhost:3000/explorer
```

* `GET /tasks` → debería devolver `{ items, page, limit, totalItems, totalPages }`
* `POST /tasks` (body):

```json
{
  "title": "Persistencia real con MongoDB",
  "status": "in_progress",
  "priority": "high"
}
```

---

## 9) Manejo profesional de errores

Ya usamos `next(err)` en controladores. Revisa que en `src/middlewares/basic.js` exista:

```js
export const errorHandler = (err, req, res, next) => {
  console.error("Error:", err.message);
  // Si viene de Mongoose con validación, 400; si no, 500 genérico
  const isValidation = err?.name === "ValidationError" || err?.name === "CastError";
  const status = isValidation ? 400 : 500;
  res.status(status).json({ error: isValidation ? "Invalid data" : "Internal server error", details: err.message });
};
```

Y que en `src/app.js` esté registrado **al final**:

```js
import { logger, notFound, errorHandler } from "./middlewares/basic.js";
app.use(logger);
// ... tus rutas
app.use(notFound);
app.use(errorHandler);
```

---

## Resultado esperado

✔ API conectada a MongoDB (Docker)
✔ `Task` persistente con validación en Mongoose
✔ `GET /tasks` y `POST /tasks` operando contra BD
✔ Paginación y filtros básicos funcionando
✔ Errores centralizados por middleware

---

## Reto del alumno 💡

Añade un **filtro de rango de fechas** en `GET /tasks` usando `from` y `to` (ISO strings) aplicados sobre `createdAt`.

Ejemplo:

```
GET /tasks?from=2025-01-01T00:00:00.000Z&to=2025-01-31T23:59:59.999Z
```

### Pista

En `taskService`, si vienen `from`/`to`, crea `query.createdAt = { $gte: new Date(from), $lte: new Date(to) }` (según parámetros presentes).

---

## ✅ Solución del reto

En `getAllTasks`:

```js
export const getAllTasks = async (filters = {}) => {
  const { status, priority, page = 1, limit = 50, search, from, to } = filters;

  const query = {};
  if (status) query.status = status;
  if (priority) query.priority = priority;
  if (search) query.title = { $regex: search, $options: "i" };

  if (from || to) {
    query.createdAt = {};
    if (from) query.createdAt.$gte = new Date(from);
    if (to) query.createdAt.$lte = new Date(to);
  }

  const skip = (Number(page) - 1) * Number(limit);

  const [items, totalItems] = await Promise.all([
    Task.find(query).skip(skip).limit(Number(limit)).sort({ createdAt: -1 }).lean(),
    Task.countDocuments(query),
  ]);

  const totalPages = Math.ceil(totalItems / Number(limit) || 1);
  return { items, page: Number(page), limit: Number(limit), totalItems, totalPages };
};
```

Y en el controlador:

```js
export const getTasks = async (req, res, next) => {
  try {
    const { status, priority, page, limit, search, from, to } = req.query;
    const result = await getAllTasks({ status, priority, page, limit, search, from, to });
    res.json(result);
  } catch (err) {
    next(err);
  }
};
```

---

## Entrega (commit sugerido)

```bash
git add .env src/config/db.js src/models/taskModel.js src/services/taskService.js src/controllers/taskController.js src/server.js
git commit -m "lab10: integrar MongoDB con Mongoose (validación, retry y persistencia real)"
```

---

## Tiempo estimado

45–60 minutos