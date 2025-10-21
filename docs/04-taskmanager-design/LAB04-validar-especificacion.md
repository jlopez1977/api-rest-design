# LAB04 – Validar la especificación OpenAPI

## Objetivo

Validar la definición de la API utilizando herramientas de verificación para garantizar que cumple con la sintaxis y buenas prácticas de OpenAPI antes de avanzar a la fase de mocking o implementación.

---

## Requisitos previos

* Haber completado LAB01, LAB02 y LAB03.
* Tener el archivo `openapi/taskmanager.yaml` en el repositorio.
* Tener acceso a GitHub Codespaces.

---

## Instrucciones paso a paso

### 1. Validación visual con Swagger Editor

1. Abre `https://editor.swagger.io`
2. Copia y pega el contenido de tu archivo `taskmanager.yaml`.
3. Revisa la barra inferior:

   * ✅ Si dice "No errors", continúa.
   * ⚠️ Si hay advertencias (*warnings*), corrígelas.

---

### 2. Validación profesional con Redocly CLI (openapi-cli)

Vamos a instalar la herramienta oficial **openapi-cli** en el Codespace para validar y mejorar la calidad de la API.

#### 2.1 Instalar openapi-cli

Ejecuta en Codespaces:

```
npm install -g @redocly/cli
```

Verifica la instalación:

```
openapi --version
```

---

### 3. Validar el archivo OpenAPI desde terminal

Ejecuta desde la raíz del repositorio:

```
openapi lint openapi/taskmanager.yaml
```

Resultado esperado:

* Aparecerán avisos de buenas prácticas como falta de descripción en propiedades o ausencia de ejemplos.
* Corrige los errores importantes antes de continuar.

---

### 4. Habilitar configuración mínima de calidad (opcional pero recomendado)

Crea el archivo `.redocly.yaml` en la raíz del proyecto:

```yaml
lint:
  extends:
    - recommended
```

Con esto activamos las reglas recomendadas de OpenAPI.

Ejecuta de nuevo:

```
openapi lint openapi/taskmanager.yaml
```

Ahora verás sugerencias útiles de estilo y consistencia.

---

## Resultado esperado

✔ El archivo se valida sin errores críticos
✔ Swagger Editor no muestra fallos sintácticos
✔ Redocly CLI ofrece una primera revisión de calidad del contrato
✔ Se ha añadido `.redocly.yaml` a la raíz del proyecto

---

## Validación

Antes de continuar asegúrate de:

* No hay errores en el archivo OpenAPI.
* El comando `openapi lint` se ejecuta correctamente.
* `.redocly.yaml` está presente en tu repositorio.

---

## Reto del alumno 💡

Configura `.redocly.yaml` para que **obligue a incluir una descripción** en cada endpoint (`operation`).

Pista: busca en la documentación de Redocly la regla `operation-description`.

---

## Solución del reto ✅

```yaml
lint:
  extends:
    - recommended
  rules:
    operation-description: error
```

Esto hará que cada operación (GET, POST, etc.) deba tener descripción obligatoria.

---

## Entrega (commit sugerido)

```
git add openapi/taskmanager.yaml .redocly.yaml
git commit -m "lab04: validación de especificación OpenAPI con Redocly CLI"
```

---

## Tiempo estimado

20 – 25 minutos