# 1. IDENTIFICACIÓN DE ACTIVOS

## 1.1 Activos de Información

| ID  | Activo                            | Tipo de Dato                    | Clasificación | Propietario            |
| --- | --------------------------------- | ------------------------------- | ------------- | ---------------------- |
| A01 | Datos de usuarios                 | PII (datos personales)          | Alta          | Plataforma LMS         |
| A02 | Credenciales de acceso            | Autenticación                   | Alta          | Plataforma LMS         |
| A03 | Banco de preguntas y respuestas   | Académico                       | Alta          | Docentes               |
| A04 | Resultados de evaluaciones        | Académico                       | Alta          | Plataforma LMS         |
| A05 | Historial académico               | Académico                       | Alta          | Plataforma LMS         |
| A06 | Certificados emitidos             | Académico/Legal                 | Alta          | Plataforma LMS         |
| A07 | Material educativo (videos, PDFs) | Propiedad intelectual           | Media         | Docentes / Institución |
| A08 | Publicaciones en foros            | Contenido generado por usuarios | Media         | Usuarios               |
| A09 | Configuración de roles y permisos | Seguridad                       | Alta          | Administradores        |
| A10 | Logs de auditoría                 | Seguridad                       | Alta          | Plataforma LMS         |

## 1.2 Activos Tecnológicos

| ID  | Activo                        | Tipo             | Criticidad | Notas                                    |
| --- | ----------------------------- | ---------------- | ---------- | ---------------------------------------- |
| T01 | Frontend Vue.js               | Software         | Alta       | Punto de acceso principal para usuarios  |
| T02 | Backend Django                | Software         | Alta       | Implementa la lógica de negocio          |
| T03 | PostgreSQL                    | Base de datos    | Alta       | Almacena toda la información crítica     |
| T04 | Sistema de autenticación      | Servicio         | Alta       | Controla acceso y sesiones               |
| T05 | Foro de discusión             | Servicio         | Media      | Interacción entre usuarios               |
| T06 | CDN de contenido multimedia   | Servicio externo | Media      | Distribución de videos y material        |
| T07 | API de certificados digitales | Servicio externo | Alta       | Generación de certificados               |
| T08 | Sistema de correo electrónico | Servicio externo | Media      | Notificaciones y recuperación de cuentas |
| T09 | Infraestructura cloud         | Infraestructura  | Alta       | Soporte de operación de la plataforma    |

## 1.3 Activos Intangibles

* Reputación de la institución educativa.
* Confianza de estudiantes y docentes.
* Integridad académica de los cursos.
* Propiedad intelectual del contenido educativo.
* Cumplimiento normativo y contractual.
