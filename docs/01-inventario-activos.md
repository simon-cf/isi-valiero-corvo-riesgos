# 1. IDENTIFICACIÓN DE ACTIVOS

## 1.1 Activos de Información

| ID  | Activo                            | Tipo de Dato                    | Clasificación | C   | I   | D   | Propietario            |
| --- | --------------------------------- | ------------------------------- | ------------- | --- | --- | --- | ---------------------- |
| A01 | Datos de usuarios                 | PII (datos personales)          | Alta          | Alta | Alta | Media | Plataforma LMS     |
| A02 | Credenciales de acceso            | Autenticación                   | Alta          | Alta | Alta | Alta  | Plataforma LMS     |
| A03 | Banco de preguntas y respuestas   | Académico                       | Alta          | Alta | Alta | Alta  | Docentes           |
| A04 | Resultados de evaluaciones        | Académico                       | Alta          | Alta | Alta | Media | Plataforma LMS     |
| A05 | Historial académico               | Académico                       | Alta          | Alta | Alta | Media | Plataforma LMS     |
| A06 | Certificados emitidos             | Académico/Legal                 | Alta          | Alta | Alta | Alta  | Plataforma LMS     |
| A07 | Material educativo (videos, PDFs) | Propiedad intelectual           | **Alta**      | Alta | Media | Alta | Docentes / Institución |
| A08 | Publicaciones en foros            | Contenido generado por usuarios | Media         | Media | Media | Media | Usuarios        |
| A09 | Configuración de roles y permisos | Seguridad                       | Alta          | Alta | Alta | Alta  | Administradores    |
| A10 | Logs de auditoría                 | Seguridad                       | Alta          | Media | Alta | Alta  | Plataforma LMS     |

> **C** = Confidencialidad · **I** = Integridad · **D** = Disponibilidad  
> Valoración según impacto en caso de compromiso (NIST SP 800-30).  

---

## 1.2 Activos Tecnológicos

| ID  | Activo                        | Tipo             | Criticidad | Ubicación   | Depende de         | Notas                                         |
| --- | ----------------------------- | ---------------- | ---------- | ----------- | ------------------ | --------------------------------------------- |
| T01 | Frontend Vue.js               | Software         | Alta       | Cloud       | T02, T04           | Punto de acceso principal para usuarios       |
| T02 | Backend Django                | Software         | Alta       | Cloud       | T03, T04           | Implementa la lógica de negocio               |
| T03 | PostgreSQL                    | Base de datos    | Alta       | Cloud       | T09                | Almacena toda la información crítica          |
| T04 | Sistema de autenticación      | Servicio         | Alta       | Cloud       | T02, T03           | Controla acceso y sesiones (JWT)              |
| T05 | Foro de discusión             | Servicio         | **Alta**   | Cloud       | T02, T03           | Superficie de ataque XSS; interacción pública |
| T06 | CDN de contenido multimedia   | Servicio externo | **Alta**   | Externo     | T02                | Distribuye A07; URL pre-firmadas por T02      |
| T07 | API de certificados digitales | Servicio externo | Alta       | Externo     | T02, T03           | Firma digital de A06; integración crítica     |
| T08 | Sistema de correo electrónico | Servicio externo | **Alta**   | Externo     | T02                | Canal de recuperación de cuentas y entrega de A06 |
| T09 | Infraestructura cloud         | Infraestructura  | Alta       | Cloud       | —                  | Soporte de operación de toda la plataforma    |


---

## 1.3 Activos Intangibles

| Activo Intangible                             | Impacto si se compromete                                  |
| --------------------------------------------- | --------------------------------------------------------- |
| Reputación de la institución educativa        | Pérdida de alumnos, cobertura mediática negativa          |
| Confianza de estudiantes y docentes           | Abandono de la plataforma, litigios                       |
| Integridad académica de los cursos            | Invalidación de certificados emitidos, escándalo público  |
| Propiedad intelectual del contenido educativo | Pérdida de ingresos, acciones legales contra distribuidores|
| Cumplimiento normativo y contractual          | Multas regulatorias (COPPA, GDPR), pérdida de contratos   |
