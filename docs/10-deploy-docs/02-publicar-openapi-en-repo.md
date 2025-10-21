# Publicar el contrato OpenAPI y la documentación de la API

En este documento aprenderás a publicar la documentación de la API para que sea accesible desde Internet, directamente desde el repositorio. Esto es clave para trabajar de forma profesional: permite compartir la API con equipos, QA, clientes y generar transparencia técnica.

---

## Objetivos

✔ Publicar la documentación HTML generada
✔ Publicar el contrato OpenAPI en YAML y JSON
✔ Activar GitHub Pages para documentación pública
✔ Dejar disponible documentación accesible desde una URL
✔ Preparar el repositorio para entrega pública / demo

---

## Requisitos previos

Antes de comenzar, asegúrate de haber generado la documentación en el LAB anterior (punto 10.1):

✅ Existe `docs/public/api-docs.html`
✅ Existe `openapi/taskmanager.json`
✅ Existe `openapi/taskmanager.yaml`

---

## 1. Estructura recomendada en el repositorio

Debes contar con esta estructura:

```
/
├── openapi/
│   ├── taskmanager.yaml        ✅ Contrato oficial
│   └── taskmanager.json        ✅ Export JSON
├── docs/
│   └── public/
│       └── api-docs.html       ✅ Documentación HTML
```

---

## 2. Publicar documentación con GitHub Pages

GitHub Pages permite publicar archivos HTML directamente desde el repositorio.

### Activación

1. Ve a **Settings** del repositorio en GitHub
2. Menú lateral → **Pages**
3. En **Source**, selecciona:

   ```
   Deploy from branch
   Branch: main
   Folder: /docs
   ```
4. Guarda cambios ✅
5. GitHub generará una URL como:

   ```
   https://<usuario>.github.io/<repositorio>/public/api-docs.html
   ```

---

## 3. Publicar enlaces al contrato OpenAPI

Publica todos estos enlaces en el README del proyecto (punto 10.3):

| Recurso                    | Ruta sugerida                                  |
| -------------------------- | ---------------------------------------------- |
| Swagger UI                 | `/explorer`                                    |
| OpenAPI (YAML)             | `/openapi/taskmanager.yaml`                    |
| OpenAPI (JSON)             | `/openapi/taskmanager.json`                    |
| Documentación HTML (Redoc) | `/docs/public/api-docs.html`                   |
| Colección Postman          | `/postman/TaskManager.postman_collection.json` |

---

## 4. Probar despliegue

Cuando GitHub Pages termine (tarda unos 30–60s), entra en:

✅ Documentación HTML publicada:

```
https://<usuario>.github.io/<repositorio>/public/api-docs.html
```

✅ Contrato OpenAPI disponible en GitHub:

```
https://raw.githubusercontent.com/<usuario>/<repo>/main/openapi/taskmanager.yaml
```

---

## 5. Añadir "badges" al README (opcional)

Mejora mucho la presentación:

```md
[![API Docs](https://img.shields.io/badge/docs-online-green)](https://<usuario>.github.io/<repo>/public/api-docs.html)
[![OpenAPI](https://img.shields.io/badge/OpenAPI-v3-orange)](openapi/taskmanager.yaml)
[![Postman Collection](https://img.shields.io/badge/Postman-ready-blue)](postman/TaskManager.postman_collection.json)
```

---

## 6. Publicación profesional

Para entregar un proyecto real:
✅ Publica contrato OpenAPI
✅ Publica documentación
✅ Expón UI Swagger
✅ Incluye colección Postman
✅ Todo accesible desde el README

---

## Entrega (commit sugerido)

```bash
git add .
git commit -m "docs: publicar documentación OpenAPI y activar GitHub Pages"
```