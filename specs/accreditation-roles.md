# Acreditación y Roles
## 1. Objetivo y Contexto
Gestiona RBAC (Control de Acceso Basado en Roles) y marcado de asistencia (acreditación) mediante panel de organizador.

## 2. Historias de Usuario
### Historia 1
Como organizador, quiero marcar asistencia, para validar presencia.
- Criterios: Solo inscripciones `confirmed`, cambio a estado `attended`, notificación al usuario

#### Controles de Seguridad (OWASP)
- Validar autenticación del usuario antes de acceder al módulo de acreditación.
- Verificar que solo usuarios con rol `organizador` o `admin` puedan marcar asistencia.
- Registrar en logs todas las acreditaciones realizadas.
- Expirar sesión tras inactividad prolongada.
- Validar tokens de sesión para evitar secuestro de sesión.

#### Riesgos Mitigados

- OWASP A01: Broken Access Control
- OWASP A07: Identification and Authentication Failures


### Historia 2
Como admin, quiero asignar rol organizer, para delegar gestión.
- Criterios: Solo admin modifica roles, notificación al usuario
#### Controles de Seguridad (OWASP)
- Validar permisos RBAC antes de modificar roles.
- Registrar auditoría de cambios de roles.
- Requerir autenticación activa para cambios administrativos.
- Evitar escalamiento de privilegios mediante validación server-side.

#### Riesgos Mitigados
- OWASP A01: Broken Access Control
- OWASP A05: Security Misconfiguration

### Historia 3
Como organizador, quiero listar acreditados, para control en tiempo real.
- Criterios: Filtros por nombre, actualización cada 30s, métricas de asistencia

## 3. Requisitos Funcionales
1. Roles: `asistente`, `organizador`, `administrador`
2. Acreditación solo durante rango de fechas del evento
3. Auditoría en `attendance_logs` y `role_audit_logs`
4. Organizadores solo gestionan sus propios eventos (salvo admin)

## 4. Restricciones Técnicas
- Dependencias FastAPI para verificación de roles
- Acreditación limitada a 10 peticiones/minuto
- Registros inmutables

## 5. Modelo de Datos
### `registros_asistencia`
| Columna | Tipo | Restricciones |
|---------|------|---------------|
| id | UUID v4 | clave primaria (PK) |
| registration_id | UUID v4 | clave foránea (FK) a `inscripciones_eventos.id` |
| accredited_by | UUID v4 | clave foránea (FK) a `usuarios.id` |

## 6. Plan de Tareas (WBS)
1. Migraciones `attendance_logs` y `role_audit_logs`
2. Dependencias de verificación de roles
3. Puntos finales de acreditación y gestión de roles
4. Panel de organizador con métricas
5. Pruebas

## 7. Estrategia de Verificación
- Unitarias: Permisos por rol, validación de fechas
- Integración: Flujo acreditación, cambio de rol
