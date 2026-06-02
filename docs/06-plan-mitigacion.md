# 6. PLAN DE MITIGACIÓN

## 6.1 Objetivo

El objetivo del presente plan es reducir los riesgos identificados durante el análisis STRIDE y priorizados mediante DREAD, implementando controles técnicos, administrativos y operativos que mejoren la confidencialidad, integridad y disponibilidad de la plataforma LMS.

Las medidas propuestas se basan en OWASP Top 10, controles de NIST SP 800-53 y buenas prácticas para aplicaciones Django/Vue.js.

---

## 6.2 Estrategia General de Mitigación

### Pilar 1 — Protección de la Identidad

Garantizar que únicamente usuarios legítimos accedan y que cada usuario opere dentro de sus privilegios.

- Autenticación multifactor (MFA) — prioridad para roles privilegiados
- Políticas robustas de contraseñas (mín. 12 caracteres, verificación contra HaveIBeenPwned)
- Gestión segura de sesiones (httpOnly cookies, SameSite=Strict)
- Bloqueo de cuentas ante intentos fallidos (AC-7 NIST)

### Pilar 2 — Protección de Datos Académicos

Los exámenes, calificaciones y certificados son los activos más sensibles.

- Cifrado de información sensible en reposo (AES-256)
- Control de acceso basado en roles (RBAC) con validación server-side en cada endpoint
- Auditoría de accesos con logs inmutables
- Hashing de datos críticos para detección de manipulación

### Pilar 3 — Protección de la Integridad Académica

Garantizar que los resultados sean auténticos y verificables.

- Firma digital de certificados (RSA-2048 o ECDSA P-256)
- Randomización del banco de preguntas
- Trazabilidad completa de modificaciones en evaluaciones
- Timestamps firmados server-side para exámenes

### Pilar 4 — Protección de Disponibilidad

Mantener la plataforma operativa especialmente durante períodos de evaluación.

- Rate limiting y throttling en todos los endpoints públicos
- Balanceo de carga y circuit breakers para servicios externos
- Monitoreo continuo con alertas automáticas

---

## 6.3 Mitigaciones por Amenaza

| ID   | Amenaza                           | Mitigaciones Propuestas                                                                                        | Responsable   | SLA      |    |
| ---- | --------------------------------- | -------------------------------------------------------------------------------------------------------------- | ------------- | -------- | --------- |
| TH05 | Uso de credenciales robadas       | MFA con TOTP para docentes/admins; bloqueo tras 5 intentos; monitoreo de IPs inusuales                        | Backend dev   | < 24 h   |  |
| TH08 | Acceso indebido a exámenes        | RBAC en cada endpoint; evaluación 100% server-side; respuestas correctas nunca enviadas al cliente             | Backend dev   | < 24 h   |  |
| TH10 | Escalación de privilegios         | Mínimo privilegio; `read_only_fields` en serializers; revisión semestral de roles                              | Backend dev   | 1 semana |  |
| TH11 | SQL Injection                     | Solo ORM Django; prohibir `raw()` sin revisión; WAF; usuario DB con permisos mínimos                          | Backend + DevOps | < 24 h |  |
| TH15 | Alteración de certificados        | Firma digital con clave privada en KMS/HSM; hash SHA-256 en DB; endpoint público de verificación               | Backend dev   | 1 semana |  |
| TH16 | Emisión fraudulenta de certs.     | Validación server-side de prerrequisitos completados; audit log de cada emisión                                | Backend dev   | < 24 h   |  |
| TH17 | IDOR en certificados              | UUIDs opacos en lugar de IDs enteros; validación de ownership en cada request                                  | Backend dev   | 1 semana |  |
| TH18 | Stored XSS — contenido malicioso  | `bleach.clean()` en Django serializer; `DOMPurify.sanitize()` en Vue.js; whitelist estricta de tags HTML       | Full-stack dev | < 24 h  |  |
| TH19 | Stored XSS — robo de sesión       | CSP estricta con nonces; httpOnly cookies; eliminar uso de `innerHTML` sin sanitizar                           | Full-stack dev | < 24 h  |  |
| TH20 | Filtración del banco de preguntas | Acceso restringido al docente propietario; banco ≥5× tamaño del examen; monitoreo de consultas masivas         | Backend dev   | 1 semana |  |
| TH22 | Modificación de notas             | Hashing de datos de examen al guardar; validación pre-emisión; audit log de cambios en calificaciones          | Backend dev   | 1 semana |  |
| TH23 | Descarga masiva de contenido      | Pre-signed URLs con TTL 20 min; rate limiting en CDN; watermark dinámico con user_id                           | Backend + DevOps | 1 semana |  |
| TH24 | Suplantación en exámenes          | MFA al iniciar examen; registro de IP/user-agent; alerta ante cambio de dispositivo a mitad del examen         | Full-stack dev | 1 semana |  |
| TH26 | CSRF en endpoints críticos        | CSRF tokens en todas las mutaciones; validación de Origin header; `SameSite=Strict` en cookies                 | Backend dev   | < 24 h   |  |
| TH27 | Mass assignment                   | `read_only_fields = ['role','is_staff','score']` en todos los serializers; tests automatizados de autorización | Backend dev   | < 24 h   |  |

---

## 6.4 Controles NIST SP 800-53 Relacionados

| Control | Descripción                       | Aplicación en la plataforma LMS                         |
| ------- | --------------------------------- | ------------------------------------------------------- |
| AC-2    | Account Management                | Gestión del ciclo de vida de usuarios y roles           |
| AC-3    | Access Enforcement                | RBAC en cada endpoint del backend Django                |
| AC-6    | Least Privilege                   | Roles con mínimos permisos necesarios; serializers      |
| AC-7    | Unsuccessful Login Attempts       | Bloqueo de cuenta tras intentos fallidos                |
| IA-2    | Identification and Authentication | MFA y autenticación segura con tokens JWT               |
| AU-2    | Audit Events                      | Registro de emisión de certificados, acceso a exámenes  |
| AU-6    | Audit Review                      | Revisión periódica de logs; alertas automáticas         |
| AU-9    | Protection of Audit Information   | Logs append-only con hash chaining; acceso restringido  |
| SC-8    | Transmission Confidentiality      | HTTPS/TLS 1.3 en todas las comunicaciones               |
| SC-28   | Protection of Information at Rest | Cifrado AES-256 en DB y backups                         |
| SI-10   | Information Input Validation      | Validación y sanitización de todas las entradas         |
| SI-4    | System Monitoring                 | Monitoreo de accesos sospechosos y anomalías            |

---

## 6.5 Mitigaciones por Componente

### Frontend Vue.js

- Implementar Content Security Policy (CSP) estricta con nonces: `script-src 'nonce-{random}'`
- Usar `DOMPurify.sanitize()` antes de cualquier renderizado de HTML dinámico
- Almacenar JWT en httpOnly cookies (no localStorage)
- Deshabilitar `innerHTML` directo; usar `v-text` o `textContent` cuando sea posible
- Protección CSRF: incluir token en headers de todas las peticiones de mutación

### Backend Django

- RBAC estricto: decoradores `@permission_required` o `@has_object_permission` en cada endpoint
- Validar autorización en el servidor, nunca confiar en el estado del cliente
- Usar exclusivamente el ORM de Django; documentar y revisar cualquier `raw()`
- Registrar eventos de seguridad: login, logout, cambio de rol, emisión de certificado, acceso a examen
- MFA para docentes y administradores (django-otp o integración con TOTP)
- Serializers con `read_only_fields` explícitos para todos los campos privilegiados

### PostgreSQL

- Usuario de aplicación con permisos mínimos (SELECT/INSERT/UPDATE en tablas necesarias; sin DROP/ALTER)
- Cifrado de respaldos (AES-256); backups almacenados en ubicación separada con acceso restringido
- Monitoreo de consultas lentas o masivas (pg_stat_statements)
- Auditoría de accesos directos a la DB (pgaudit)
- Rotación periódica de credenciales de la base de datos

### API de Certificados

- Firma digital de cada certificado con clave privada almacenada en HSM o AWS KMS
- Registro inmutable de cada emisión (fecha, usuario, curso, score)
- Endpoint de verificación pública que devuelve solo: nombre, curso, fecha y estado (válido/inválido)
- Fallback asíncrono: si la API externa no responde, encolar y reintentar; notificar al estudiante

### CDN de Contenido Multimedia

- Pre-signed URLs con TTL de 15–30 minutos generadas por Django en el momento de la solicitud
- Validación de enrollment activo antes de generar cada URL
- Rate limiting en el CDN: máximo N requests por minuto por token de sesión
- Watermark dinámico visible (user_id + fecha parcial) sobre el player de video
- Bloqueo de hotlinking: validar Referer y Origin en el CDN

### Foro de Discusión 

- Sanitización de HTML: `bleach.clean(content, tags=ALLOWED_TAGS, strip=True)` en el serializer de Django
- En Vue.js: nunca usar `v-html` con contenido de foros sin pasar por `DOMPurify.sanitize()`
- Lista blanca de tags HTML permitidos: `<p>`, `<b>`, `<i>`, `<ul>`, `<li>`, `<a href>` (solo HTTPS)
- Rate limiting: máximo 10 posts por hora por usuario; cooldown de 30 segundos entre posts
- Moderación automática: detección de links externos, adjuntos ejecutables, palabras clave de riesgo
- Para menores de 13: perfil sin nombre real visible; foro en modo solo lectura si no hay consentimiento parental

---

## 6.6 Plan de Implementación

| Prioridad | Controles                                           | Responsable    | Plazo    |
| --------- | --------------------------------------------------- | -------------- | -------- |
| Alta      | MFA para docentes/admins (TH05)                     | Backend dev    | < 24 h   |
| Alta      | httpOnly cookies para JWT (TH01, TH19)              | Full-stack dev | < 24 h   |
| Alta      | Sanitización XSS en foros (TH18, TH19)              | Full-stack dev | < 24 h   |
| Alta      | Mass assignment fix en serializers (TH27)           | Backend dev    | < 24 h   |
| Alta      | Validación server-side de prerrequisitos (TH16)     | Backend dev    | < 24 h   |
| Alta      | RBAC y evaluación server-side (TH08, TH10)          | Backend dev    | 1 semana |
| Alta      | Protección banco de preguntas — RBAC + random (TH20)| Backend dev    | 1 semana |
| Alta      | UUIDs + ownership en endpoints (TH17)               | Backend dev    | 1 semana |
| Media     | Firma digital de certificados (TH15)                | Backend dev    | 2 semanas|
| Media     | Pre-signed URLs CDN con TTL corto (TH23)            | Backend + DevOps| 2 semanas|
| Media     | Watermark dinámico en videos (TH23)                 | Frontend + DevOps| 1 sprint |
| Media     | Audit logs con hash chaining (TH07, TH21)           | Backend dev    | 1 sprint |
| Baja      | Automatización de auditorías de acceso              | DevOps         | 1 mes    |
| Baja      | Integración con SIEM                                | DevOps         | 1 mes    |
| Baja      | Detección avanzada de fraude académico              | Backend dev    | Backlog  |

---

## 6.7 Riesgo Esperado Post-Mitigación

La implementación de los controles propuestos reducirá significativamente la probabilidad de explotación de las amenazas críticas. Se estima que las amenazas críticas (DREAD ≥8.0) descenderán a nivel Alto o Medio una vez implementadas las mitigaciones de Fase 1.

Persistirán riesgos residuales asociados a: error humano, compromiso de credenciales legítimas con MFA (phishing avanzado), vulnerabilidades zero-day, y uso indebido de IA durante evaluaciones. Estos se documentan en el documento 07.
