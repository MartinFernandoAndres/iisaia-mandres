# TP2 - IISAIA

**Materia:** Inteligencia Artificial y Sistemas de IA Asistida (IISAIA)  
**Trabajo Práctico:** N° 2

---

## Descripción

Este repositorio contiene el Trabajo Práctico N° 2 de la materia IISAIA.

El proyecto consiste en un contrato de API REST (`api.yaml`) escrito en OpenAPI 3.0 para una aplicación de gestión de proyectos y tareas. El entregable es únicamente el archivo de contrato — sin implementación, base de datos, ni auth.

Se iteró en vivo agregando un endpoint (`PATCH .../complete`) al final del bloque de paths sin reescribir la definición existente, demostrando cómo extender un contrato OpenAPI de forma incremental.

## Archivos

- `api.yaml` — contrato OpenAPI 3.0 (entregable principal)
- `prompt.md` — prompt original y decisiones de diseño
- `README.md` — este archivo

## Cómo visualizarlo en Swagger UI

**Opción A — Swagger Editor online (sin instalar nada)**

1. Ir a [https://editor.swagger.io](https://editor.swagger.io)
2. Menú `File > Import file` → seleccionar `api.yaml`
3. El panel derecho renderiza la UI interactiva.

**Opción B — Docker local**

```bash
docker run -p 8080:8080 \
  -e SWAGGER_JSON=/api/api.yaml \
  -v "$(pwd):/api" \
  swaggerapi/swagger-ui
```

Luego abrir `http://localhost:8080`.

**Opción C — npx (sin Docker)**

```bash
npx @redocly/cli preview-docs api.yaml
```

Abre automáticamente el navegador con la documentación.
