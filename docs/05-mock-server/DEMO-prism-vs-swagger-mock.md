# DEMO – Mock con Swagger Editor vs Prism

En esta demostración compararemos dos formas de generar **mock servers** a partir de un archivo OpenAPI:

| Herramienta       | Tipo                   | Facilidad | Nivel      |
| ----------------- | ---------------------- | --------- | ---------- |
| Swagger Editor    | Mock automático online | Muy fácil | Básico     |
| Prism (Stoplight) | Mock CLI profesional   | Fácil     | Intermedio |

---

## Objetivo

Mostrar dos formas prácticas de simular una API sin backend real:

1. Mock básico con Swagger Editor
2. Mock local avanzado con Prism CLI

---

## 1. Mock automático con Swagger Editor (básico)

Este método genera respuestas falsas automáticamente basadas en la estructura del contrato.

### Paso a paso

1. Abre Swagger Editor:
   [https://editor.swagger.io](https://editor.swagger.io)
2. Carga este ejemplo mínimo:

```yaml
openapi: 3.0.3
info:
  title: Demo API
  version: 1.0.0
paths:
  /hello:
    get:
      summary: Saludo de prueba
      responses:
        '200':
          description: Respuesta ejemplo
          content:
            application/json:
              example:
                message: "Hola desde Swagger Mock"
```

3. Abre el menú **Generate Client > OpenAPI JSON**
4. Ve a **[https://app.swaggerhub.com](https://app.swaggerhub.com)** → Create Mock API
5. Sube el archivo JSON
6. SwaggerHub generará una URL mock pública automáticamente

✅ Ventajas:

* Fácil y rápido
* No requiere instalación

⚠️ Limitaciones:

* No es configurable
* Respuestas poco realistas
* Depende de conexión a internet

---

## 2. Mock profesional con Prism (Stoplight)

Prism es una herramienta CLI que permite simular APIs a partir de un archivo OpenAPI **en local**.

### Instalación

En Codespaces o terminal local:

```bash
npm install -g @stoplight/prism-cli
```

Verificar:

```bash
prism --version
```

---

### Crear archivo OpenAPI para la demo

Guarda esto como `demo-api.yaml`:

```yaml
openapi: 3.0.3
info:
  title: Demo API Prism
  version: 1.0.0
paths:
  /hello:
    get:
      summary: Saludo de prueba
      responses:
        '200':
          description: Saludo correcto
          content:
            application/json:
              example:
                message: "Hola desde Prism Mock"
```

---

### Ejecutar el mock server

```bash
prism mock demo-api.yaml
```

Salida esperada:

```
[CLI]  ...Mock server started on http://127.0.0.1:4010
```

---

### Probar mock

Desde navegador o curl:

```bash
curl http://127.0.0.1:4010/hello
```

Respuesta esperada:

```json
{
  "message": "Hola desde Prism Mock"
}
```

✅ Ventajas:

* Mock local y controlado
* Respuestas basadas en ejemplos OpenAPI
* Compatible con tests

---

## Comparativa

| Característica       | Swagger Mock | Prism                  |
| -------------------- | ------------ | ---------------------- |
| Complejidad          | Muy fácil    | Fácil                  |
| Requiere instalación | No           | Sí                     |
| Realista             | No mucho     | Bastante               |
| Configurable         | Poco         | Mucho                  |
| Ideal para...        | Demo rápida  | Trabajo real API First |

---

Con esto ya estamos listos para aplicar Prism en nuestra **Task Manager API real** usando su contrato OpenAPI.
