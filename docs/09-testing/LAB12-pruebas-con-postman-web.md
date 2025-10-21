Aquí tienes el contenido completo para **docs/09-testing/LAB11-pruebas-con-postman-web.md**, con nivel **profesional (C)** y exportación en **/postman**.

---

# LAB12 – Pruebas con Postman Web (colección final del curso)

## Objetivo

Crear una **colección Postman Web profesional** para la API Task Manager que:

* Use **variables** y **entornos** (baseUrl).
* Encadene peticiones con **variables dinámicas** (taskId).
* Incluya **tests automáticos** (status, estructura y valores).
* Se **exporte** al repositorio en `/postman/TaskManager.postman_collection.json`.

---

## Requisitos previos

* API corriendo en `http://localhost:3000` (LAB07–LAB11).
* Documentación disponible: `/explorer` y `/taskmanager-api.json`.
* MongoDB activo con Docker Compose.

---

## 1) Crear Workspace y Colección en Postman Web

1. Entra en [https://www.postman.com/](https://www.postman.com/) (versión Web).
2. Crea/elige un **Workspace** para el curso.
3. Crea una **Colección** llamada: `Task Manager API (Curso API First)`.

---

## 2) Crear un Environment con variables

1. Crea un **Environment** llamado: `Local Dev`.
2. Añade la variable:

   * `baseUrl` → `http://localhost:3000`
3. (Opcional) Añade `version` → `v1` si usas prefijo.
4. Selecciona el environment en la esquina superior derecha.

> En las peticiones usaremos URLs como: `{{baseUrl}}/tasks`

---

## 3) Añadir peticiones a la colección

### 3.1 GET /tasks – listar

* Método: `GET`
* URL: `{{baseUrl}}/tasks?page=1&limit=5`
* Tests (tab “Tests”):

```js
pm.test("Status 200", () => pm.response.to.have.status(200));
pm.test("Respuesta JSON", () => pm.response.to.be.json);

const body = pm.response.json();
pm.test("Estructura de paginación presente", () => {
  pm.expect(body).to.have.keys(["items", "page", "limit", "totalItems", "totalPages"]);
});
pm.test("Items es un array", () => pm.expect(body.items).to.be.an("array"));
pm.test("Paginación coherente", () => {
  pm.expect(body.page).to.be.a("number");
  pm.expect(body.limit).to.be.a("number");
  pm.expect(body.totalItems).to.be.a("number");
});
```

---

### 3.2 POST /tasks – crear

* Método: `POST`
* URL: `{{baseUrl}}/tasks`
* Headers: `Content-Type: application/json`
* Body (raw JSON):

```json
{
  "title": "Tarea de prueba (Postman)",
  "status": "todo",
  "priority": "high"
}
```

* Tests:

```js
pm.test("Status 201 (creado)", () => pm.response.to.have.status(201));
pm.test("Respuesta JSON", () => pm.response.to.be.json);

const t = pm.response.json();
pm.test("Devuelve id, title y status", () => {
  pm.expect(t).to.have.property("id");
  pm.expect(t).to.have.property("title");
  pm.expect(t).to.have.property("status");
});
pm.test("Valores coherentes", () => {
  pm.expect(t.title).to.be.a("string");
  pm.expect(["todo","in_progress","done"]).to.include(t.status);
});

// Guardar id para el resto de pruebas
pm.collectionVariables.set("taskId", t.id);
```

---

### 3.3 GET /tasks/{{taskId}} – obtener por ID

> Nota: Este endpoint se implementó en el LAB11 CRUD.

* Método: `GET`
* URL: `{{baseUrl}}/tasks/{{taskId}}`
* Tests:

```js
pm.test("Status 200", () => pm.response.to.have.status(200));
pm.test("JSON con la tarea solicitada", () => pm.response.to.be.json);

const t = pm.response.json();
pm.test("Es la tarea correcta", () => {
  pm.expect(t).to.have.property("_id");
  // Aceptamos id o _id según implementación
  const anyId = t.id || t._id;
  pm.expect(anyId).to.exist;
});
```

---

### 3.4 PUT /tasks/{{taskId}} – reemplazar

* Método: `PUT`
* URL: `{{baseUrl}}/tasks/{{taskId}}`
* Headers: `Content-Type: application/json`
* Body:

```json
{
  "title": "Tarea reemplazada (PUT)",
  "status": "in_progress",
  "priority": "medium"
}
```

* Tests:

```js
pm.test("Status 200 en PUT", () => pm.response.to.have.status(200));
const t = pm.response.json();
pm.test("Título y estado actualizados", () => {
  pm.expect(t.title).to.eql("Tarea reemplazada (PUT)");
  pm.expect(["todo","in_progress","done"]).to.include(t.status);
});
```

---

### 3.5 PATCH /tasks/{{taskId}} – actualización parcial

* Método: `PATCH`
* URL: `{{baseUrl}}/tasks/{{taskId}}`
* Headers: `Content-Type: application/json`
* Body:

```json
{
  "priority": "low"
}
```

* Tests:

```js
pm.test("Status 200 en PATCH", () => pm.response.to.have.status(200));
const t = pm.response.json();
pm.test("Priority actualizada", () => pm.expect(t.priority).to.eql("low"));
```

---

### 3.6 DELETE /tasks/{{taskId}} – eliminar

* Método: `DELETE`
* URL: `{{baseUrl}}/tasks/{{taskId}}`
* Tests:

```js
pm.test("Status 204 sin contenido", () => pm.response.to.have.status(204));
```

---

### 3.7 (opcional) GET /tasks/{{taskId}} – comprobar 404 tras borrar

* Método: `GET`
* URL: `{{baseUrl}}/tasks/{{taskId}}`
* Tests:

```js
pm.test("Tras DELETE debería dar 404", () => {
  pm.expect(pm.response.code).to.be.oneOf([404, 400]); // según manejo de errores
});
```

---

## 4) Orden de ejecución (Runner)

Para encadenar correctamente:

1. `GET /tasks` (opcional)
2. `POST /tasks` (guarda `taskId`)
3. `GET /tasks/{{taskId}}`
4. `PUT /tasks/{{taskId}}`
5. `PATCH /tasks/{{taskId}}`
6. `DELETE /tasks/{{taskId}}`
7. `GET /tasks/{{taskId}}` (opcional: esperar 404)

Puedes usar el **Collection Runner** en Postman Web para ejecutar en lote.

---

## 5) Guardar ejemplos (Examples)

En cada petición de éxito, guarda un **Example**:

* En la pestaña **Save Response > Save as example**.
* Útil para comparar regresiones y para documentación viva.

---

## 6) Exportar la colección al repositorio

1. En el panel de la colección → **Export** → formato v2.1 recomendado.
2. Guarda el archivo como:

```
/postman/TaskManager.postman_collection.json
```

(En el repo del curso, crea la carpeta `postman` si no existe.)

---

## Validación / Comprobación

* La colección corre de principio a fin.
* `taskId` se guarda tras el POST y se usa en el resto.
* Los tests validan **status + estructura + valores**.
* Cada petición tiene **al menos 3 tests**.
* La colección está exportada en `/postman`.

---

## Reto del alumno 💡

Añade tests negativos:

1. `POST /tasks` sin `title` → esperar **400**.
2. `GET /tasks/{{taskId}}` con un **ID inválido** (no ObjectId) → esperar **400**.

Pista (test 1, en Tests):

```js
pm.test("400 cuando falta title", () => pm.response.to.have.status(400));
```

---

## ✅ Solución del reto

1. Crear petición `POST /tasks` con body `{ "status": "todo" }` sin `title`.

   * Test: status 400 y JSON con `{ error: "..." }`.

2. Crear petición `GET /tasks/123` (ID inválido).

   * Test: status 400 y JSON con `{ error: "Invalid ID" }` (según errorHandler del curso).

---

## Entrega (commit sugerido)

```bash
git add postman/TaskManager.postman_collection.json
git commit -m "lab12: colección Postman Web con tests profesionales y variables"
```

---

## Tiempo estimado

30–45 minutos