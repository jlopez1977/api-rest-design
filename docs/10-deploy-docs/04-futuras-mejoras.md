# Futuras mejoras del proyecto (Roadmap técnico)

Este documento describe posibles mejoras para evolucionar la Task Manager API hacia un entorno de producción profesional. Marca el camino para convertir esta API educativa en un proyecto **real**, con estándares de **seguridad, calidad y despliegue continuo**.

---

## ✅ Situación actual

Estado del proyecto al finalizar el curso:

| Elemento                         | Estado |
| -------------------------------- | ------ |
| API REST con CRUD completo       | ✅      |
| Diseño API First (OpenAPI 3)     | ✅      |
| Documentación Swagger UI         | ✅      |
| Mock Server                      | ✅      |
| Buenas prácticas básicas         | ✅      |
| Persistencia MongoDB             | ✅      |
| Testing manual con Postman       | ✅      |
| Documentación HTML (Redoc)       | ✅      |
| Publicación del contrato OpenAPI | ✅      |

---

## 🚀 Roadmap sugerido

### 1. Validación avanzada

| Mejora                    | Descripción                  |
| ------------------------- | ---------------------------- |
| Joi o Zod                 | Validación de payloads       |
| Middlewares de validación | Validar parámetros y cuerpos |
| Respuestas RFC 7807       | Errores estándar             |

---

### 2. Seguridad

| Aspecto       | Mejora recomendada     |
| ------------- | ---------------------- |
| Autenticación | JWT + Refresh Tokens   |
| Autorización  | Roles y permisos       |
| CORS          | Configuración segura   |
| Rate Limiting | Evitar abuso de API    |
| Helmet        | Cabeceras seguras HTTP |
| Sanitización  | Evitar inyección NoSQL |

---

### 3. Testing automatizado

| Tipo                     | Herramienta          |
| ------------------------ | -------------------- |
| Unit testing             | Jest                 |
| Supertest para endpoints | ✅                    |
| API contract testing     | Dredd o Schemathesis |
| Mock DB                  | MongoMemoryServer    |

---

### 4. Calidad de código

| Mejora                  | Herramienta          |
| ----------------------- | -------------------- |
| Linter                  | ESLint               |
| Formateo                | Prettier             |
| Git hooks               | Husky + lint-staged  |
| Convenciones de commits | Conventional Commits |

---

### 5. Observabilidad

| Área                 | Integración              |
| -------------------- | ------------------------ |
| Logging estructurado | pino o winston           |
| Métricas             | Prometheus + Grafana     |
| Health checks        | `/health` + `/readiness` |
| Monitorización       | Status page opcional     |

---

### 6. CI/CD

| Nivel              | Trabajo                                 |
| ------------------ | --------------------------------------- |
| CI básico          | Test + lint en cada push                |
| Build Docker       | Multi-stage build                       |
| Publicación imagen | GitHub Container Registry               |
| Despliegue CD      | Render, Railway, Fly.io o Docker remoto |

---

### 7. Contenedorización completa

| Entorno             | Estado    |
| ------------------- | --------- |
| MongoDB             | ✅ Ya está |
| API en Docker       | Pendiente |
| Docker Compose Prod | Pendiente |

---

### 8. Versionado y compatibilidad

| Tema              | Recomendación           |
| ----------------- | ----------------------- |
| Versionado SemVer | v1 → v2 sin crear deuda |
| Soporte versiones | `/v1`, `/v2`            |
| Breaking changes  | Deprecar ordenadamente  |

---

### 9. API pública

| Mejora                    | Objetivo                     |
| ------------------------- | ---------------------------- |
| Portal de desarrolladores | Quickstart + ejemplos        |
| Uso de SDK                | openapi-generator para JS/TS |
| Rate limits y keys        | Preparar comercialización    |

---

## 🔚 Conclusión

El proyecto ha alcanzado una base sólida:
✅ Diseño profesional
✅ Contrato API First
✅ Backend funcional
✅ Persistencia real
✅ Documentación pública

Este roadmap es la **siguiente etapa natural** si deseas convertir esta API en un proyecto real y desplegable a producción.

---

## ✅ Próximo paso sugerido

| Si quieres continuar hacia… | Empieza por…                  |
| --------------------------- | ----------------------------- |
| Calidad y mantenimiento     | ESLint, Prettier, scripts npm |
| Seguridad                   | JWT + middlewares             |
| Escalado                    | Docker + despliegue           |
| API pública real            | CI/CD + documentación viva    |

