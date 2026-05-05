# Generación de Certificados e Informes
## 1. Objetivo y Contexto
Valida asistencia mínima, genera certificados en formato PDF y reportes post-evento.

## 2. Historias de Usuario
### Historia 1
Como acreditado, quiero descargar certificado, para validar participación.
- Criterios: Asistencia ≥ 80%, PDF con membrete, código QR de validación

### Historia 2
Como organizador, quiero reporte de asistencia, para evaluar el evento.
- Criterios: Exportable PDF/CSV, solo eventos `finished`

### Historia 3
Como admin, quiero reporte global, para estadísticas de plataforma.
- Criterios: Exportable XLSX/CSV, solo admin

## 3. Requisitos Funcionales
1. Porcentaje mínimo de asistencia configurable (valor por defecto 80%)
3. Generación asíncrona con Celery
4. Almacenamiento local (entorno desarrollo) o S3 (entorno producción)

## 4. Restricciones Técnicas
- PDF con ReportLab
- Validación pública de certificados vía QR
- Reportes con openpyxl (Excel) y csv nativo

## 5. Modelo de Datos
### `certificados`
| Columna | Tipo | Restricciones |
|---------|------|---------------|
| id | UUID v4 | clave primaria (PK) |
| registration_id | UUID v4 | único (UNIQUE) clave foránea (FK) a `inscripciones_eventos.id` |
| certificate_number | cadena de caracteres (VARCHAR(50)) | único (UNIQUE) |

## 6. Plan de Tareas (WBS)
1. Migraciones `certificates` y `event_reports`
2. Plantillas de PDF para certificados
3. Puntos finales de generación de certificados y reportes
4. Integración con ReportLab y almacenamiento
5. Pruebas

## 7. Estrategia de Verificación
- Unitarias: Cálculo de asistencia, generación de PDF
- Integración: Flujo acreditación-certificado, reportes exportados
