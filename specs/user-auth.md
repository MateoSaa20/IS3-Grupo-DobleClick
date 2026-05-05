# Inscripción y Autenticación de Usuarios
## 1. Objetivo y Contexto
Gestiona registro, login JWT y flujo de inscripción autónoma a eventos.

## 2. Historias de Usuario
### Historia 1
Como usuario no registrado, quiero registrarme, para inscribirme a eventos.
- Criterios: Email único, contraseña segura (8+ chars, mayús/minús/números), verificación de email

### Historia 2
Como usuario registrado, quiero login, para acceder a mi panel.
- Criterios: JWT acceso (15min) y refresh (7d), cuenta verificada requerida

### Historia 3
Como usuario autenticado, quiero inscribirme a evento, para asegurar cupo.
- Criterios: No duplicados, cupo disponible = `confirmed`, agotado = `waiting_list`

## 3. Requisitos Funcionales
1. Contraseñas hasheadas con bcrypt
2. Verificación de email con token 24h
3. Roles: `attendee` (default), `organizer`, `admin`
4. Lista de espera FIFO
5. Máximo 5 inscripciones activas por usuario

## 4. Restricciones Técnicas
- JWT con algoritmo RS256
- Transacciones ACID para gestión de cupos
- Contraseñas nunca incluidas en respuestas JSON

## 5. Modelo de Datos
### `usuarios`
| Columna | Tipo | Restricciones |
|---------|------|---------------|
| id | UUID v4 | clave primaria (PK) |
| email | cadena de caracteres (VARCHAR(255)) | único (UNIQUE) no nulo (NOT NULL) |
| password_hash | cadena de caracteres (VARCHAR(255)) | no nulo (NOT NULL) |
| role | enumeración (ENUM) | valor por defecto (DEFAULT) `asistente` |

### `inscripciones_eventos`
| Columna | Tipo | Restricciones |
|---------|------|---------------|
| id | UUID v4 | clave primaria (PK) |
| user_id | UUID v4 | clave foránea (FK) a `usuarios.id` |
| event_id | UUID v4 | clave foránea (FK) a `eventos.id` |
| status | enumeración (ENUM) | valor por defecto (DEFAULT) `confirmado` |
| único (user_id, event_id) | | |

## 6. Plan de Tareas (WBS)
1. Migraciones `users` y `event_registrations`
2. Puntos finales de autenticación: registro/inicio-sesión/actualizar/verificar-email
3. Intermediario JWT
4. Puntos finales de inscripción: registrar/cancelar/listar
5. Pruebas

## 7. Estrategia de Verificación
- Unitarias: Hash contraseñas, JWT, lógica cupos
- Integración: Flujo registro-login-inscripción, duplicados 409
