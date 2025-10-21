## README - Curso Diseño de APIs REST con OpenAPI y Node.js

Título del curso:
Diseño de APIs REST con Enfoque API First y OpenAPI 3 usando Node.js + Express

Descripción general:
Este repositorio contiene todo el material del curso oficial de Diseño de APIs REST con enfoque API First. El programa combina teoría, demostraciones en directo y laboratorios prácticos, construyendo progresivamente una API real basada en especificación OpenAPI (Swagger) y desarrollada en Node.js sobre Express, con MongoDB como base de datos.

El contenido está organizado para trabajar íntegramente en entorno cloud usando GitHub Codespaces, de modo que no es necesario instalar nada en local. Todo el código, laboratorios y documentación se ejecutan directamente desde el navegador.

---

## Objetivos del curso

* Comprender los principios de diseño REST y buenas prácticas de arquitectura de APIs.
* Adoptar el enfoque API First para definir APIs antes de implementarlas.
* Dominar la especificación OpenAPI 3 para diseñar contratos API.
* Validar, documentar y mockear APIs con herramientas de especificación.
* Implementar APIs reales en Node.js + Express siguiendo buenas prácticas básicas.
* Conectar la API a MongoDB usando Docker.
* Probar APIs con herramientas 100% cloud (Postman Web, Swagger UI).
* Documentar APIs de forma profesional y reutilizable.

---

## Dinámica del curso

* 70% práctica + 30% teoría
* Trabajo por bloques temáticos
* Cada unidad combina:

  1. Concepto teórico
  2. Demostración guiada
  3. Laboratorio aplicado
* Proyecto progresivo: Task Manager API
* Trabajaremos siempre con enfoque API First
* Cada estudiante trabajará con su propio fork del repositorio
* Todo el desarrollo se hará en GitHub Codespaces
* Uso profesional de ramas y commits
* Ejercicios con entregables reales

---

## Planificación del curso (3 sesiones de 4h)

Día 1 - Fundamentos REST + API First + OpenAPI

* Introducción a REST y diseño basado en recursos
* Enfoque API First
* Introducción a OpenAPI 3
* Diseño del contrato inicial en Swagger Editor
* Laboratorios LAB01 - LAB04

Día 2 - Buenas prácticas + Mock + Versionado

* Contratos API y documentación viva
* Mock servers y testing de contratos
* Buenas prácticas REST
* Versionado de APIs
* Laboratorios LAB05 - LAB06

Día 3 - Implementación + MongoDB + Test

* Implementación con Node.js y Express
* Conexión a MongoDB (Docker)
* Pruebas con Postman Web
* Documentación y despliegue básico
* Laboratorios LAB07 - LAB11

---

## Laboratorios principales

LAB01  Crear la definición inicial de la Task Manager API
LAB02  Definir endpoints básicos y modelos de datos
LAB03  Especificar esquemas y validación en OpenAPI
LAB04  Validar la documentación con herramientas OpenAPI
LAB05  Mockear la API sin backend usando Swagger/Prism
LAB06  Aplicar buenas prácticas REST al contrato API
LAB07  Crear el proyecto base en Node.js + Express
LAB08  Integrar Swagger UI en Express
LAB09  Implementar endpoints a partir del contrato
LAB10  Integrar MongoDB en la API (Docker)
LAB11  Pruebas de API con Postman Web

---

## Foco del curso

* Organización del diseño centrado en el contrato
* Diseño de APIs robustas, escalables y fáciles de mantener
* Pensamiento de arquitectura API orientado al negocio
* Profesionalización del desarrollo backend
* Documentación útil para equipos y clientes
* Preparación para madurar APIs hacia niveles avanzados
  (Validación automática, tests de contrato, CI/CD, versionado, OpenAPI Generator)

---

## Repositorio

Este repositorio contiene:

* Carpeta docs/ con teorías, demos y laboratorios
* Carpeta src/ para el código backend de la API
* Carpeta openapi/ con la especificación Swagger
* Configuración de entorno Codespaces
* docker-compose con MongoDB
* Scripts y utilidades del curso

---

## Requisitos para alumnos

* Conocimientos básicos de JavaScript
* Entender conceptos básicos de APIs REST
* Cuenta en GitHub para trabajar con Codespaces
* Navegador (Chrome/Firefox/Edge). Nada más.

---

## Cómo empezar

1. Haz fork del repositorio
2. Abre el fork en GitHub Codespaces
3. Sigue los laboratorios en la carpeta docs
4. Usa git para versionar tu progreso
5. Consulta dudas en los checkpoints del curso
