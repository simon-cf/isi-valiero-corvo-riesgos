# 6. PLAN DE MITIGACIÓN

## 6.1 Objetivo

El objetivo del presente plan es reducir los riesgos identificados durante el análisis STRIDE y priorizados mediante DREAD, implementando controles técnicos, administrativos y operativos que permitan mejorar la confidencialidad, integridad y disponibilidad de la plataforma LMS.

Las medidas propuestas se basan en buenas prácticas de OWASP, controles de NIST SP 800-53 y principios generales de seguridad para aplicaciones web.

---

## 6.2 Estrategia General de Mitigación

La estrategia de mitigación se centra en cuatro pilares fundamentales:

### Protección de la Identidad

Se busca garantizar que únicamente usuarios legítimos accedan a la plataforma y que cada usuario opere dentro de los privilegios asignados.

Controles principales:

* Autenticación multifactor (MFA).
* Políticas robustas de contraseñas.
* Gestión segura de sesiones.
* Bloqueo de cuentas ante intentos fallidos repetidos.

### Protección de Datos Académicos

Los exámenes, calificaciones y certificados representan los activos más sensibles del sistema.

Controles principales:

* Cifrado de información sensible.
* Control de acceso basado en roles.
* Auditoría de accesos.
* Protección de bases de datos.

### Protección de la Integridad Académica

La plataforma debe garantizar que los resultados obtenidos por los estudiantes sean auténticos y verificables.

Controles principales:

* Verificación de identidad.
* Protección del banco de preguntas.
* Trazabilidad de modificaciones.
* Validación de certificados emitidos.

### Protección de Disponibilidad

La plataforma debe mantenerse operativa especialmente durante períodos de evaluación.

Controles principales:

* Rate limiting.
* Balanceo de carga.
* Monitoreo continuo.
* Protección contra ataques de denegación de servicio.

---

## 6.3 Mitigaciones por Amenaza

| ID   | Amenaza                           | Mitigaciones Propuestas                                                                       |
| ---- | --------------------------------- | --------------------------------------------------------------------------------------------- |
| TH05 | Uso de credenciales robadas       | MFA, bloqueo de cuentas, monitoreo de accesos sospechosos, detección de ubicaciones inusuales |
| TH08 | Acceso indebido a exámenes        | RBAC, validación de permisos en backend, auditoría de accesos                                 |
| TH10 | Escalación de privilegios         | Principio de mínimo privilegio, revisión periódica de roles, validaciones de autorización     |
| TH11 | SQL Injection                     | ORM Django, consultas parametrizadas, validación de entradas, WAF                             |
| TH15 | Alteración de certificados        | Firmado digital, hash de integridad, validación de certificados                               |
| TH20 | Filtración del banco de preguntas | Cifrado, acceso restringido, monitoreo de consultas y auditoría                               |
| TH23 | Descarga masiva de contenido      | Rate limiting, URLs firmadas, detección de bots                                               |
| TH24 | Suplantación durante exámenes     | MFA, controles de sesión, verificación adicional de identidad                                 |

---

## 6.4 Controles NIST SP 800-53 Relacionados

| Control | Descripción                       | Aplicación                           |
| ------- | --------------------------------- | ------------------------------------ |
| AC-2    | Account Management                | Gestión de usuarios y roles          |
| AC-3    | Access Enforcement                | Restricción de acceso a recursos     |
| AC-6    | Least Privilege                   | Minimización de privilegios          |
| IA-2    | Identification and Authentication | MFA y autenticación segura           |
| AU-2    | Audit Events                      | Registro de eventos críticos         |
| AU-6    | Audit Review                      | Revisión de logs                     |
| SC-28   | Protection of Information at Rest | Protección de datos almacenados      |
| SI-10   | Information Input Validation      | Prevención de inyección              |
| SI-4    | System Monitoring                 | Monitoreo de actividades sospechosas |

---

## 6.5 Mitigaciones por Componente

### Frontend Vue.js

Se recomienda implementar Content Security Policy (CSP), validación de entradas y protección frente a ataques XSS y CSRF.

Adicionalmente, las sesiones deberán almacenarse de forma segura evitando la exposición de tokens en el navegador.

### Backend Django

El backend constituye el componente más crítico de la plataforma.

Se recomienda:

* Implementar RBAC estricto.
* Validar autorizaciones en cada endpoint.
* Utilizar el ORM de Django.
* Registrar eventos de seguridad.
* Implementar MFA para usuarios privilegiados.

### PostgreSQL

La base de datos almacena la información académica y personal de los usuarios.

Se recomienda:

* Cifrado de respaldos.
* Gestión segura de credenciales.
* Segmentación de accesos.
* Backups periódicos.
* Monitoreo de consultas anómalas.

### API de Certificados

Los certificados emitidos deben garantizar autenticidad e integridad.

Se recomienda:

* Firmado digital.
* Validación criptográfica.
* Registro de emisiones.
* Trazabilidad completa de modificaciones.

### CDN de Contenido

Para proteger la propiedad intelectual de los cursos:

* URLs temporales firmadas.
* Protección contra scraping.
* Monitoreo de descargas masivas.
* Restricción geográfica opcional.

---

## 6.6 Plan de Implementación

### Prioridad Alta

* MFA para docentes y administradores.
* Protección contra SQL Injection.
* RBAC estricto.
* Protección del banco de preguntas.
* Auditoría de eventos críticos.

### Prioridad Media

* Firmado digital de certificados.
* Protección avanzada del contenido educativo.
* Monitoreo de comportamiento anómalo.

### Prioridad Baja

* Automatización de auditorías.
* Integración con SIEM.
* Detección avanzada de fraude académico.

---

## 6.7 Riesgo Esperado Post-Mitigación

La implementación de los controles propuestos reducirá significativamente la probabilidad de explotación de las amenazas críticas identificadas.

Sin embargo, persistirán riesgos residuales asociados a:

* Error humano.
* Compromiso de credenciales legítimas.
* Vulnerabilidades desconocidas (Zero-Day).
* Uso indebido de herramientas de inteligencia artificial durante evaluaciones.

Estos riesgos serán analizados en el documento de riesgos residuales.
