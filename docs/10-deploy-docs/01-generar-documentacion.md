# Generar documentación de la API

La **documentación** es una parte esencial de cualquier API profesional. Permite comunicar claramente cómo usarla, qué endpoints están disponibles, qué datos espera y qué respuestas devuelve. En este proyecto seguiremos un enfoque **API First**, por lo que la documentación se genera directamente desde la **especificación OpenAPI**.

Este documento explica cómo **generar documentación estática** a partir del archivo:

```
openapi/taskmanager.yaml
```

---

## Objetivos

✔ Generar documentación navegable para la API
✔ Exportar OpenAPI a JSON para compatibilidad con herramientas externas
✔ Preparar documentación para ser publicada más adelante (punto 2)
✔ Mantener documentación **actualizada automáticamente desde el contrato**

---

## Tipos de documentación que generaremos

| Tipo          | Herramienta   | Salida             | Uso                          |
| ------------- | ------------- | ------------------ | ---------------------------- |
| Interactiva   | Swagger UI    | `/explorer`        | Navegar + Testear API        |
| Estática HTML | Redoc         | `api-docs.html`    | Publicación web              |
| JSON          | OpenAPI JSON  | `taskmanager.json` | Integración con herramientas |
| YAML          | **Principal** | `taskmanager.yaml` | Fuente oficial del contrato  |

---

## 1. Documentación interactiva (Swagger UI)

✅ Ya está integrada en este proyecto en el LAB08:

```
http://localhost:3000/explorer
```

No requiere regeneración, se alimenta directamente del contrato `taskmanager.yaml`.

---

## 2. Generar documentación HTML con Redoc

Instalar Redoc CLI:

```bash
npm install -D redoc-cli
```

Generar archivo HTML estático:

```bash
npx redoc-cli build openapi/taskmanager.yaml -o docs/public/api-docs.html
```

Resultado:

```
docs/public/api-docs.html ✅ Documento navegable
```

✅ Este archivo HTML será útil para **publicarlo en GitHub Pages** o compartirlo fácilmente.

---

## 3. Exportar OpenAPI a JSON

Algunas herramientas trabajan mejor con **OpenAPI en JSON** que en YAML, así que también exportamos la especificación:

Instalar YAML parser:

```bash
npm install -D yamljs
```

Convertir YAML → JSON:

```bash
node -e "const YAML=require('yamljs');const fs=require('fs');fs.writeFileSync('openapi/taskmanager.json',JSON.stringify(YAML.load('openapi/taskmanager.yaml'),null,2))"
```

Resultado:

```
openapi/taskmanager.json ✅ Especificación JSON para integraciones
```

---

## 4. Buenas prácticas de documentación

| Recomendación                                  | Razón                                    |
| ---------------------------------------------- | ---------------------------------------- |
| El YAML es la **fuente de la verdad**          | Evita inconsistencias                    |
| No edites nunca el JSON manualmente            | Se regenera                              |
| Genera documentación en cada cambio importante | Documentación viva                       |
| Publica documentación accesible                | Mejora experiencia del equipo y clientes |

---

## 5. Flujo recomendado (Makefile opcional)

Puedes automatizar esto con un `Makefile`:

```makefile
docs:
	npx redoc-cli build openapi/taskmanager.yaml -o docs/public/api-docs.html
	node -e "const YAML=require('yamljs');const fs=require('fs');fs.writeFileSync('openapi/taskmanager.json',JSON.stringify(YAML.load('openapi/taskmanager.yaml'),null,2))"
```

---

## Entrega (commit sugerido)

```bash
git add docs/public/api-docs.html openapi/taskmanager.json
git commit -m "docs: generar documentación HTML y JSON desde OpenAPI"
```