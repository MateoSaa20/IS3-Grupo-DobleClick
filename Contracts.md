# Contracts.md
## Estándares de API REST
1. Versionado: Todas las rutas bajo `/api/v1/`
2. Métodos HTTP: GET (consulta), POST (creación), PUT (actualización completa), PATCH (parcial), DELETE (eliminación)
3. Puntos finales: Pluralización, notación `kebab-case` para recursos compuestos (ej. `/event-registrations`)
4. Filtros: Parámetros de consulta (ej. `/events?type=congress&page=1`)

## Códigos de Estado
| Código | Significado | Caso de Uso |
|--------|-------------|-------------|
| 200 | OK | Petición exitosa |
| 201 | Creado | Recurso creado |
| 400 | Solicitud Incorrecta | Validación fallida |
| 401 | No Autorizado | Token inválido/expirado |
| 403 | Prohibido | Sin permisos |
| 404 | No Encontrado | Recurso no existe |
| 409 | Conflicto | Inscripción duplicada |
| 500 | Error Interno del Servidor | Error no controlado |

## Formato JSON
Estructura unificada:
```json
{
  "data": {},
  "error": null,
  "message": ""
}
```
- Identificadores: UUID v4
- Fechas: ISO 8601 (ej. `2026-05-04T14:30:00-03:00`)
- Enumeraciones: notación `snake_case` (ej. `event_type: congress`)

## Convenciones de Nomenclatura
- Base de datos: `snake_case` plural (tablas: `events`, columnas: `start_date`)
- Endpoints: `kebab-case` (ej. `/api/v1/event-registrations`)
