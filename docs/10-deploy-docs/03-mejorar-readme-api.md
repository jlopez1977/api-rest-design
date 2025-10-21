# Mejorar README de la API (Entrega profesional del proyecto)

El README es la **puerta de entrada al proyecto**. Resume qué hace, cómo se usa y cómo empezar. En el contexto de una API profesional, debe incluir enlaces a la documentación, instrucciones de ejecución y ejemplos de uso.

Este documento explica cómo **redactar y estructurar el README final de la Task Manager API** para dejar una entrega limpia, clara y reutilizable.

---

## Objetivos

✔ Crear un README técnico y profesional
✔ Incluir enlaces a documentación y colección Postman
✔ Documentar instalación y ejecución rápida
✔ Describir rutas principales de la API
✔ Preparar el repositorio para publicación pública o portfolios

---

## Estructura recomendada del README final

A continuación se muestra la plantilla recomendada:

---

### Plantilla README.md (lista para aplicar)

````md
# Task Manager API

API REST desarrollada con **Node.js + Express + MongoDB** siguiendo enfoque **API First** con **OpenAPI 3**. Proyecto educativo orientado a buenas prácticas de diseño, documentación y entrega profesional.

---

## 🚀 Documentación

| Recurso | Enlace |
|----------|--------|
| Swagger UI | http://localhost:3000/explorer |
| OpenAPI YAML | ./openapi/taskmanager.yaml |
| OpenAPI JSON | ./openapi/taskmanager.json |
| Documentación HTML (Redoc) | ./docs/public/api-docs.html |
| Colección Postman | ./postman/TaskManager.postman_collection.json |

---

## ✅ Características
- Enfoque **API First**
- Diseño con **OpenAPI 3**
- Mock con Swagger/Prism
- Backend en **Node.js + Express**
- Persistencia con **MongoDB + Mongoose**
- Testing con **Postman Web**
- Documentación viva

---

## 📦 Requisitos
- Node.js 18+
- Docker + Docker Compose
- Git

---

## ⚙️ Instalación

```bash
git clone <url-del-repo>
cd api-rest-design
npm install
````

---

## 🗄️ Levantar MongoDB

```bash
docker compose up -d
```

---

## ▶️ Ejecutar API

Modo desarrollo:

```bash
npm run dev
```

Modo producción:

```bash
npm start
```

---

## 🔧 Variables de entorno

Crea un archivo `.env` en la raíz:

```
PORT=3000
MONGODB_URI=mongodb://root:example@localhost:27017/taskmanager?authSource=admin
```

---

## 📚 Endpoints principales

| Método | Ruta          | Descripción             |
| ------ | ------------- | ----------------------- |
| GET    | `/tasks`      | Listar tareas           |
| GET    | `/tasks/{id}` | Obtener una tarea       |
| POST   | `/tasks`      | Crear tarea             |
| PUT    | `/tasks/{id}` | Reemplazar tarea        |
| PATCH  | `/tasks/{id}` | Actualizar parcialmente |
| DELETE | `/tasks/{id}` | Eliminar tarea          |

---

## 🛠️ Scripts útiles

| Comando                   | Descripción                 |
| ------------------------- | --------------------------- |
| `npm run dev`             | Inicia servidor con nodemon |
| `npm run start`           | Inicia en modo producción   |
| `docker compose up -d`    | Levanta MongoDB             |
| `npm run lint` (opcional) | Linter del proyecto         |
| `npm run docs` (opcional) | Regenerar documentación     |

---

## 🧪 Testing

* Tests manuales: **Postman Web**
* Colección exportada en `/postman`

---

## 📄 Licencia

MIT © 2025

---

````

---

## Buenas prácticas del README

✔ Empieza siempre con una descripción clara del proyecto  
✔ Añade **tablas** para rutas y recurso  
✔ Añade **comandos de ejecución**  
✔ Añade **documentación enlazada**  
✔ Usa **lenguaje profesional y directo**  
✔ Nunca subas `.env` al repo → usa `.env.example`

---

## Entrega (commit sugerido)

```bash
git add README.md
git commit -m "docs: README profesional para API de entrega final"
````