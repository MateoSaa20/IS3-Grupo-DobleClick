# Gestión de Eventos y Publicación
## 1. Objetivo y Contexto
Módulo central para crear, editar, publicar y filtrar eventos académicos, gestionando cupos y validando reglas de negocio.

## 2. Historias de Usuario
### Historia 1
Como organizador, quiero crear un evento, para publicitarlo.
- Criterios: Campos obligatorios (nombre, tipo, fechas, cupos mínimo/máximo, fecha límite de inscripción) validados, fecha inicio > fecha actual, cupo > 0, estado inicial `borrador`

### Historia 2
Como organizador, quiero filtrar eventos, para gestionarlos rápidamente.
- Criterios: Filtros por fecha/tipo/estado, resultados paginados (10 por página)

### Historia 3
Como organizador, quiero modificar cupo, para ajustar capacidad.
- Criterios: No reducir debajo de inscripciones confirmadas, notificar a usuarios si aplica

## 3. Requisitos Funcionales
1. Solo `admin`/`organizer` gestionan eventos
2. Estados: `borrador`, `publicado`, `cancelado`, `finalizado`, `agotado`
3. No eventos con fecha inicio pasada (>1 hora)
4. Al alcanzar cupo, evento marca `agotado`
5. Eliminación lógica (no física)
6. El cupo mínimo (min_quota) no puede ser superior al cupo máximo (max_quota)
7. No se permiten nuevas inscripciones después de la fecha límite de inscripción (registration_deadline)
8. La fecha límite de inscripción debe ser anterior a la fecha de inicio del evento (start_date)

## 4. Restricciones Técnicas
- Fechas validadas con Pydantic ISO 8601
- `descripción` máximo 5000 caracteres
- Imágenes de portada: formatos JPEG/PNG, máximo 2MB
- Auditoría en `registros_auditoria_eventos`

## 5. Modelo de Datos
### `events`
| Columna | Tipo | Restricciones |
|---------|------|---------------|
| id | UUID v4 | clave primaria (PK) |
| name | cadena de caracteres (VARCHAR(255)) | no nulo (NOT NULL) |
| type | enumeración (ENUM) | no nulo (NOT NULL) (curso/congreso/charla) |
| start_date | marca de tiempo con zona horaria (TIMESTAMPTZ) | no nulo (NOT NULL) |
| end_date | marca de tiempo con zona horaria (TIMESTAMPTZ) | no nulo (NOT NULL) |
| registration_deadline | marca de tiempo con zona horaria (TIMESTAMPTZ) | no nulo (NOT NULL) |
| max_quota | entero (INTEGER) | no nulo (NOT NULL) > 0 |
| min_quota | entero (INTEGER) | no nulo (NOT NULL) > 0, valor por defecto (DEFAULT) 1 |
| current_quota | entero (INTEGER) | valor por defecto (DEFAULT) 0 |
| status | enumeración (ENUM) | valor por defecto (DEFAULT) `borrador` |
| organizer_id | UUID v4 | clave foránea (FK) a `usuarios.id` |

## 6. Plan de Tareas (WBS)
1. Migraciones de tablas `events` y `event_audit_logs`
2. Esquemas Pydantic de validación
3. Puntos finales CRUD (Crear, Leer, Actualizar, Eliminar) + filtros
4. Lógica de estados y cupos
5. Pruebas unitarias/integración

## 7. Estrategia de Verificación
- Unitarias: Validación de fechas, cupos, estados
- Integración: Flujo completo de creación/publicación, filtros
