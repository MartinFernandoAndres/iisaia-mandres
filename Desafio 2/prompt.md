# Prompt — Desafio 2

## Prompt original del usuario

> Tengo que armar para un file visible en swagger, la tematica no tiene importancia, puede ser Alumnos, Autos o cualquier tematica simple en base a los requerimientos
> Armar un yaml (open api swagger) guardarlo en una nueva carpeta llamada "Desafio 2"
>
> 1. Tres methods, minimo GET POST DELETE
> 2. Jerarquia de recursos: projects -> tasks path nesting visible
> 3. Una respuesta de error: Al menos un 400 o 404 documentado
> 4. Schemas tipados: type, required, format, donde aplique
> 5. Iteracion en vivo, agregar un endpoint sin reescribir todo
>
> No entra: implementacion, base de datos, auth, deploy: Hoy el contrato como archivo, leido y editable
>
> Tenes que generar un file "prompt.md" con este prompt y tu respuesta
> Tenes que generar un file "README.md" siguiendo la misma estructura del file dentro de Desafio 1/README.md pero bajo el contexto de este desafio
> Por ultimo tenes que mostrarme como levantarlo en swagger, aunque el file entregable es unicamente el file swagger

---

## Respuesta del asistente

Se eligió la temática **gestión de proyectos y tareas** por ser semánticamente natural para mostrar jerarquía de recursos.

### Decisiones de diseño

| Requerimiento | Decisión |
|---|---|
| 3 métodos mínimos | GET, POST, DELETE en `/projects` y `/projects/{id}/tasks` |
| Jerarquía visible | Ruta `/projects/{projectId}/tasks/{taskId}` muestra el nesting explícito |
| Error documentado | 400 en POST (body inválido) y 404 en GET/DELETE (recurso inexistente) |
| Schemas tipados | `Project`, `Task`, `ProjectInput`, `TaskInput`, `Error` con `type`, `required`, `format: uuid / date / date-time` |
| Iteración en vivo | Endpoint `PATCH /projects/{projectId}/tasks/{taskId}/complete` agregado al final del bloque de paths sin modificar los anteriores |

### Estructura del contrato

```
/projects                              GET  POST
/projects/{projectId}                  GET  DELETE
/projects/{projectId}/tasks            GET  POST
/projects/{projectId}/tasks/{taskId}   GET  DELETE
/projects/{projectId}/tasks/{taskId}/complete   PATCH  ← iteración 2
```

### Cómo visualizarlo en Swagger UI

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

---

## Iteración 2 — Validación de requerimientos

### Prompt del usuario

> Hace una validacion de los requerimientos para validar si se cumplen todos, este prompt y tu respuesta tambien agregalos a prompt.md

### Respuesta del asistente

Validación realizada leyendo `api.yaml` línea a línea contra cada requerimiento original.

| # | Requerimiento | Evidencia en api.yaml | Estado |
|---|---|---|---|
| 1 | Tres métodos, mínimo GET POST DELETE | GET en líneas 21, 66, 109, 164 — POST en líneas 35, 129 — DELETE en líneas 87, 182 | CUMPLE |
| 2 | Jerarquía projects → tasks (path nesting visible) | Rutas `/projects/{projectId}/tasks` (l.105) y `/projects/{projectId}/tasks/{taskId}` (l.159) | CUMPLE |
| 3 | Al menos un 400 o 404 documentado | 400 en POST `/projects` (l.52) y POST `/projects/{id}/tasks` (l.146) — 404 en GET y DELETE de ambos recursos (ll.77, 96, 122, 175, 191) | CUMPLE |
| 4 | Schemas tipados con type, required, format | `Project` (l.253): `required: [id, name, createdAt]`, `format: uuid`, `format: date-time` — `Task` (l.283): `required: [id, projectId, title, status, createdAt]`, `format: uuid`, `format: date`, `format: date-time`, `enum` — `Error` (l.329): `required: [code, message]` | CUMPLE |
| 5 | Iteración en vivo: endpoint agregado sin reescribir | `PATCH /projects/{projectId}/tasks/{taskId}/complete` añadido como bloque independiente al final de `paths` (l.201), sin modificar ningún path anterior | CUMPLE |

**Resultado: 5/5 requerimientos cumplidos.**
