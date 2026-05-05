# Project.md
## Visión General
La Plataforma Web de Gestión de Eventos Académicos es un sistema de software diseñado para organizaciones académicas que requieren administrar el ciclo de vida de eventos (cursos, congresos, charlas). Permite a organizadores crear eventos, gestionar inscripciones, validar asistencia y emitir certificados; a usuarios registrarse, inscribirse y descargar certificados.

## Alcance
### Funcionalidades Incluidas
1. CRUD de eventos con filtros por fecha, tipo y estado
2. Registro de usuarios, autenticación JWT, inscripción autónoma
3. Panel de organizador para acreditación y gestión de roles RBAC
4. Generación de certificados PDF y reportes post-evento

### Exclusiones
1. Pasarelas de pago (eventos gratuitos o pago externo)
2. Eventos híbridos (solo presenciales/virtuales mediante enlaces externos)
3. Notificaciones SMS (solo correo electrónico)

## Objetivos del TP
1. Cumplir con estándares SDD de la cátedra de Ingeniería de Software III
2. Implementar arquitectura modular orientada a objetos
3. Generar documentación sin ambigüedades para equipos colaborativos en GitHub
4. Validar metodologías ágiles en planificación de requisitos

## Arquitectura Sugerida
### Backend
- FastAPI (Python 3.11+), SQLAlchemy 2.0, Alembic, JWT (`python-jose`)

### Frontend
- React 18+ TypeScript, Vite, TanStack Query, Zustand, Tailwind CSS

### Base de Datos
- PostgreSQL 15+, UUID v4 como PK, `snake_case` para tablas/columnas

### Comunicación
- API REST versionada `/api/v1/`, JSON, documentación OpenAPI automática
