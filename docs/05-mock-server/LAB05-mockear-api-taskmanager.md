# LAB05 – Mockear la API Task Manager con Prism CLI

## Objetivo

Simular el comportamiento de la API **Task Manager** usando **Prism Mock Server**, a partir del archivo `openapi/taskmanager.yaml`, sin necesidad de implementar todavía el backend real.

---

## Requisitos previos

* Haber completado LAB01–LAB04.
* Tener definido el archivo OpenAPI: `openapi/taskmanager.yaml`.
* Estar trabajando en GitHub Codespaces.
* Node.js instalado (ya viene con Codespaces).

---

## Instrucciones paso a paso

### 1. Instalar Prism CLI en Codespaces

Instala Prism globalmente:

```bash
npm install -g @stoplight/prism-cli
```

Verifica instalación:

```bash
prism --version
```

---

### 2. Ejecutar el mock server

Lanza Prism usando nuestro contrato OpenAPI con este comando:

```bash
prism mock openapi/taskmanager.yaml --port 3001
```

Salida esperada:

```
[CLI] ... Mock server started on http://127.0.0.1:3001
```

---

### 3. Probar el mock server

Prueba el endpoint `/tasks` desde el navegador del Codespace o con `curl`:

```bash
curl http://localhost:3001/tasks
```

Respuesta esperada (si no hay ejemplos definidos):

```json
{}
```

*Esto es normal: Prism necesita ejemplos en OpenAPI para generar respuestas más realistas.*

---

### 4. Añadir ejemplos para mejorar las respuestas Mock

Abre `openapi/taskmanager.yaml` y añade ejemplos de respuesta en `GET /tasks`:

```yaml
responses:
  '200':
    description: Lista de tareas
    content:
      application/json:
        examples:
          ejemplo:
            value:
              - id: "1"
                title: "Mock desde OpenAPI"
                status: "todo"
```

Guarda y reinicia Prism:

```bash
prism mock openapi/taskmanager.yaml --port 3001
```

Vuelve a probar:

```bash
curl http://localhost:3001/tasks
```

Ahora la respuesta debe incluir el ejemplo configurado.

---

### 5. Probar un POST simulado

Prueba también crear una tarea ficticia:

```bash
curl -X POST http://localhost:3001/tasks \
  -H "Content-Type: application/json" \
  -d '{"title": "Nueva tarea mock", "status": "in_progress"}'
```

---

## Resultado esperado

✅ Prism ejecutándose localmente en `http://localhost:3001`
✅ Mock generado automáticamente a partir del contrato
✅ Respuestas basadas en ejemplos
✅ Independencia total del backend real

---

## Validación

Antes de cerrar el laboratorio:

* El comando `prism mock` funciona sin errores.
* Los endpoints `/tasks` responden correctamente.
* Has añadido al menos **1 ejemplo JSON** en OpenAPI.

---

## Reto del alumno 💡

Añade un ejemplo de respuesta para el endpoint `POST /tasks`, devolviendo una tarea creada con `id`, `title`, `status` y `createdAt`.

---

## Solución del reto ✅

```yaml
responses:
  '201':
    description: Tarea creada correctamente
    content:
      application/json:
        examples:
          ejemplo:
            value:
              id: "2"
              title: "Nueva tarea desde mock"
              status: "todo"
              createdAt: "2025-01-01T10:00:00Z"
```

---

## Entrega (commit sugerido)

```bash
git add openapi/taskmanager.yaml
git commit -m "lab05: mock de Task Manager API con Prism"
```

---

## Tiempo estimado

20 – 30 minutos