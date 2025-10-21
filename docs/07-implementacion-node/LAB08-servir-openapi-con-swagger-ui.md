# LAB08 – Servir OpenAPI con Swagger UI en Express

## Objetivo

Publicar la documentación interactiva de la API a partir del archivo **openapi/taskmanager.yaml** usando **Swagger UI** y exponer también la especificación en JSON descargable.

Rutas objetivo:

* Documentación UI: `/explorer`
* Especificación JSON: `/taskmanager-api.json`

---

## Requisitos previos

* Haber completado LAB07 (proyecto Node + Express en marcha)
* Archivo `openapi/taskmanager.yaml` existente y válido
* Uso de **ES Modules** en el proyecto (`"type": "module"` en package.json)

---

## Instrucciones paso a paso

### 1) Instalar dependencias necesarias

En la raíz del repo (Codespaces):

```bash
npm install swagger-ui-express yamljs
```

* `swagger-ui-express`: integra Swagger UI con Express
* `yamljs`: carga el YAML y lo convierte a objeto JS

---

### 2) Cargar la especificación OpenAPI en `app.js`

Edita `src/app.js` y añade lo siguiente (debajo de tus imports actuales):

```js
import swaggerUi from "swagger-ui-express";
import YAML from "yamljs";
import path from "path";
import { fileURLToPath } from "url";

// __dirname en ESM
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// Ruta absoluta al YAML (app.js está en src/, el YAML en openapi/)
const openapiPath = path.resolve(__dirname, "../openapi/taskmanager.yaml");

// Cargar el documento OpenAPI
const openapiDocument = YAML.load(openapiPath);

// Montar Swagger UI en /explorer
app.use("/explorer", swaggerUi.serve, swaggerUi.setup(openapiDocument));

// Exponer también la especificación en JSON con nombre de la API
app.get("/taskmanager-api.json", (req, res) => {
  res.type("application/json").send(JSON.stringify(openapiDocument, null, 2));
});
```

Asegúrate de que arriba en el archivo ya tienes:

```js
import express from "express";
const app = express();
app.use(express.json());
```

---

### 3) Ajustar (si hace falta) la sección `servers` en el YAML

Para que el botón “Try it out” de Swagger UI apunte al backend local, revisa `openapi/taskmanager.yaml`:

```yaml
servers:
  - url: http://localhost:3000/v1
    description: Servidor local de desarrollo
```

Si tu API no usa prefijo `/v1` en Express aún, puedes dejarlo como:

```yaml
servers:
  - url: http://localhost:3000
    description: Desarrollo local (sin prefijo)
```

> Recomendado mantener `/v1` porque trabajamos con versionado.

---

### 4) Ejecutar el servidor y comprobar

Arranca el proyecto en modo desarrollo:

```bash
npm run dev
```

Abre en el navegador del Codespace:

```
http://localhost:3000/explorer
```

Deberías ver la documentación interactiva de Swagger UI.

Prueba también la especificación en JSON:

```
http://localhost:3000/taskmanager-api.json
```

---

### 5) (Opcional) Mejorar el log de arranque en `server.js`

Para que aparezcan las rutas útiles al arrancar:

```js
// src/server.js
import app from "./app.js";

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Servidor escuchando en http://localhost:${PORT}`);
  console.log(`→ Swagger UI:           http://localhost:${PORT}/explorer`);
  console.log(`→ OpenAPI (JSON):       http://localhost:${PORT}/taskmanager-api.json`);
});
```

---

## Resultado esperado

* Swagger UI sirviendo la documentación en `/explorer`
* La especificación disponible como JSON en `/taskmanager-api.json`
* La página de Swagger muestra tus endpoints y permite “Try it out”

---

## Validación / comprobación

* La ruta `/explorer` carga sin errores
* La ruta `/taskmanager-api.json` devuelve el JSON formateado del contrato
* Los **servers** del YAML apuntan a `http://localhost:3000` (o `.../v1`) según tu configuración

---

## Reto del alumno 💡

Cambia el **título** y la **descripción** en `openapi/taskmanager.yaml` y verifica que el cambio se refleja en Swagger UI **sin reiniciar** el servidor (si estás usando nodemon).

* Nuevo título sugerido: `Task Manager API – Curso API First`
* Nueva descripción breve: `API de referencia para el curso (definición OpenAPI + implementación Express).`

---

## ✅ Solución del reto

En `openapi/taskmanager.yaml` ajusta:

```yaml
info:
  title: Task Manager API – Curso API First
  description: API de referencia para el curso (definición OpenAPI + implementación Express).
  version: 1.0.0
```

Guarda el archivo. Con **nodemon** activo, recarga `/explorer` y verás el cambio.

---

## Entrega (commit sugerido)

```bash
git add src/app.js src/server.js openapi/taskmanager.yaml
git commit -m "lab08: servir OpenAPI con Swagger UI en /explorer y export JSON"
```

---

## Tiempo estimado

20–30 minutos