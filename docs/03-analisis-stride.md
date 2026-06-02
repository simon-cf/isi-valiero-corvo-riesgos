# 3. ANÁLISIS DE AMENAZAS - METODOLOGÍA STRIDE

## 3.1 Aplicación de STRIDE

La metodología STRIDE fue utilizada para identificar amenazas potenciales sobre los componentes principales de la arquitectura de la plataforma LMS.

Las amenazas fueron analizadas sobre los siguientes componentes:

* Frontend Vue.js
* Backend Django
* PostgreSQL
* Sistema de autenticación
* API de certificados
* CDN de contenido multimedia
* Foro de discusión
* Módulo de exámenes

Las categorías evaluadas fueron:

* **S** — Spoofing (Suplantación de identidad)
* **T** — Tampering (Manipulación de datos)
* **R** — Repudiation (Negación de acciones)
* **I** — Information Disclosure (Divulgación de información)
* **D** — Denial of Service (Denegación de servicio)
* **E** — Elevation of Privilege (Elevación de privilegios)

---

## 3.2 Matriz de Amenazas por Componente

| ID   | Componente           | STRIDE | Amenaza                                                                | CWE      | Contramedida principal                                          |
|------|----------------------|--------|------------------------------------------------------------------------|----------|-----------------------------------------------------------------|
| TH01 | Frontend             | S      | Robo de sesión mediante secuestro de cookies                           | CWE-384  | httpOnly + Secure cookies; SameSite=Strict                      |
| TH02 | Frontend             | T      | Manipulación de parámetros enviados al backend                         | CWE-472  | Validación y firma de parámetros server-side; no confiar en cliente |
| TH03 | Frontend             | I      | Exposición de información sensible en código JavaScript                | CWE-200  | No incluir secretos en JS; variables de entorno en build       |
| TH04 | Frontend             | D      | Automatización masiva de solicitudes desde bots                        | CWE-400  | Rate limiting, CAPTCHA, detección de bots                       |
| TH05 | Backend              | S      | Uso de credenciales robadas para acceder como docente                  | CWE-287  | MFA obligatorio para roles privilegiados; detección de anomalías |
| TH06 | Backend              | T      | Modificación no autorizada de calificaciones                           | CWE-284  | RBAC estricto; validación de ownership; audit log               |
| TH07 | Backend              | R      | Negación de cambios realizados por docentes                            | CWE-778  | Audit log inmutable con hash chaining; timestamps firmados      |
| TH08 | Backend              | I      | Acceso indebido a exámenes antes de su publicación                     | CWE-200  | RBAC; validación de estado del examen en cada endpoint          |
| TH09 | Backend              | D      | Saturación de API durante evaluaciones masivas                         | CWE-770  | Rate limiting por usuario; queue con prioridad; circuit breaker |
| TH10 | Backend              | E      | Escalación de privilegios de estudiante a docente                      | CWE-269  | Mínimo privilegio; revisión periódica de roles; validación en cada operación |
| TH11 | PostgreSQL           | I      | Extracción de datos personales mediante SQL Injection                  | CWE-89   | ORM Django con consultas parametrizadas; WAF; mínimo privilegio DB |
| TH12 | PostgreSQL           | T      | Alteración de registros académicos                                     | CWE-89   | Consultas parametrizadas; integridad referencial; audit log     |
| TH13 | PostgreSQL           | D      | Consultas masivas que degradan el rendimiento                          | CWE-400  | Connection pooling; query timeout; monitoreo de consultas anómalas |
| TH14 | PostgreSQL           | E      | Obtención de permisos administrativos sobre la base de datos           | CWE-266  | Usuarios DB con mínimo privilegio; deshabilitar superuser en producción |
| TH15 | API Certificados     | T      | Alteración de certificados emitidos                                    | CWE-345  | Firma digital (RSA/ECDSA); hash de integridad; verificación criptográfica |
| TH16 | API Certificados     | S      | Emisión fraudulenta de certificados                                    | CWE-290  | Validación server-side de prerrequisitos; audit log de emisiones |
| TH17 | API Certificados     | I      | Acceso a certificados de terceros (IDOR)                               | CWE-639  | UUIDs opacos; validación de ownership en cada request           |
| TH18 | Foro                 | T      | Publicación de contenido malicioso (Stored XSS)                       | CWE-116  | DOMPurify en frontend; bleach en Django; whitelist de tags HTML |
| TH19 | Foro                 | I      | Robo de sesiones mediante XSS almacenado                               | CWE-79   | CSP estricta; httpOnly cookies; sanitización de contenido       |
| TH20 | Exámenes             | I      | Filtración del banco de preguntas antes de una evaluación              | CWE-200  | Acceso restringido por rol; cifrado de banco; monitoreo de consultas |
| TH21 | Exámenes             | R      | Estudiante niega haber realizado un examen                             | CWE-778  | Audit log con timestamps firmados; IP y user-agent registrados  |
| TH22 | Certificación        | T      | Modificación de notas para obtener certificados                        | CWE-284  | Hashing de datos críticos en DB; validación server-side antes de emitir |
| TH23 | Contenido Educativo  | I      | Descarga masiva de videos protegidos                                   | CWE-200  | Pre-signed URLs con TTL corto; rate limiting en CDN; watermark  |
| TH24 | Evaluaciones         | S      | Suplantación de identidad durante exámenes                             | CWE-287  | MFA; controles de sesión; verificación adicional para cursos de alto valor |
| TH25 | Evaluaciones         | T      | Uso de herramientas automatizadas o IA para manipular evaluaciones     | CWE-306* | Evaluación server-side; randomización; detección de anomalías de comportamiento |
| TH26 | Backend              | T      | CSRF en endpoints de examen y certificación                            | CWE-352  | CSRF tokens; validación de Origin header; SameSite=Strict cookies |
| TH27 | Backend              | E      | Mass assignment en modelos Django para elevar rol o marcar curso completado | CWE-915 | read_only_fields en serializers; campos explícitos; nunca usar **kwargs de request |

> *TH25 no tiene CWE técnico preciso — se documenta CWE-306 (Missing Authentication for Critical Function) como aproximación, con nota de que es una amenaza de proceso sin vector técnico único.

---

## 3.3 Análisis Detallado de Amenazas Prioritarias

### TH05 – Uso de credenciales robadas para acceder como docente

**Categoría STRIDE:** Spoofing  
**Componente afectado:** Backend / Sistema de autenticación  

**Descripción:**
Un atacante obtiene las credenciales de un docente mediante phishing, reutilización de contraseñas filtradas o malware, logrando acceso legítimo a la plataforma.

**Activos afectados:** A02, A03, A04, A07

**Impacto:** El atacante podría modificar cursos, acceder a evaluaciones, alterar contenidos o emitir certificados de forma fraudulenta.

**Probabilidad:** Alta

**Contramedidas:**
- MFA obligatorio para docentes y administradores (TOTP o hardware key)
- Bloqueo de cuenta tras N intentos fallidos (AC-7 NIST)
- Monitoreo de accesos desde ubicaciones o dispositivos inusuales
- Notificación al usuario ante nuevo inicio de sesión

---

### TH08 – Acceso indebido a exámenes antes de su publicación

**Categoría STRIDE:** Information Disclosure  
**Componente afectado:** Backend  

**Descripción:**
Un usuario explota fallas de autorización en las APIs para acceder a exámenes aún no publicados o a las respuestas correctas antes de completar el examen.

**Activos afectados:** A03, A04

**Impacto:** Compromete la integridad académica de la evaluación y genera ventajas injustas.

**Probabilidad:** Media-Alta

**Contramedidas:**
- RBAC: solo docente propietario del curso puede ver exámenes en estado "borrador"
- Las respuestas correctas nunca se envían al cliente; evaluación 100% server-side
- Validación de estado del examen en cada endpoint (no solo en la vista)

---

### TH10 – Escalación de privilegios de estudiante a docente

**Categoría STRIDE:** Elevation of Privilege  
**Componente afectado:** Backend  

**Descripción:**
Un estudiante aprovecha una vulnerabilidad de autorización o mass assignment para obtener permisos reservados a docentes o administradores.

**Activos afectados:** A09, A04, A05

**Impacto:** Permitiría modificar calificaciones, crear cursos falsos o acceder a información de otros usuarios.

**Probabilidad:** Media

**Contramedidas:**
- Principio de mínimo privilegio; roles asignados explícitamente por administrador
- Serializers Django con `read_only_fields = ['role', 'is_staff']`
- Tests de autorización automatizados para cada endpoint sensible

---

### TH11 – SQL Injection sobre la base de datos

**Categoría STRIDE:** Information Disclosure / Tampering  
**Componente afectado:** PostgreSQL  

**Descripción:**
Un atacante envía entradas maliciosas para ejecutar consultas SQL no autorizadas.

**Activos afectados:** A01, A03, A04, A05

**Impacto:** Robo, modificación o eliminación masiva de información crítica.

**Probabilidad:** Media (Django ORM reduce superficie; riesgo persiste en raw queries)

**Contramedidas:**
- Usar ORM de Django; prohibir `raw()` o `extra()` sin revisión de seguridad
- WAF con reglas de detección de SQLi
- Usuario de DB con permisos mínimos (solo SELECT/INSERT/UPDATE en tablas necesarias)
- Validación y sanitización de todas las entradas

---

### TH15 – Alteración de certificados emitidos

**Categoría STRIDE:** Tampering  
**Componente afectado:** API de certificados  

**Descripción:**
Un atacante modifica la información contenida en un certificado o genera versiones falsas.

**Activos afectados:** A06

**Impacto:** Afecta la credibilidad de la plataforma y la validez de los certificados.

**Probabilidad:** Media

**Contramedidas:**
- Firma digital con clave privada (RSA-2048 o ECDSA P-256) almacenada en HSM o KMS
- Hash SHA-256 del certificado almacenado en DB para verificación posterior
- Endpoint público de verificación que valida la firma sin exponer datos innecesarios
- Timestamp de autoridad (TSA) para no repudio temporal

---

### TH20 – Filtración del banco de preguntas

**Categoría STRIDE:** Information Disclosure  
**Componente afectado:** Módulo de exámenes  

**Descripción:**
Las preguntas y respuestas son obtenidas antes de una evaluación por errores de configuración, IDOR o filtraciones internas.

**Activos afectados:** A03, A04

**Impacto:** Compromete la integridad de los procesos de evaluación.

**Probabilidad:** Alta

**Contramedidas:**
- Acceso al banco restringido exclusivamente al docente propietario del curso
- Banco de preguntas ≥5× el tamaño del examen con selección aleatoria por intento
- Monitoreo de consultas masivas a la tabla de preguntas
- Política de renovación del 30% del banco cada trimestre

---

### TH23 – Descarga masiva de contenido educativo

**Categoría STRIDE:** Information Disclosure  
**Componente afectado:** CDN de contenido  

**Descripción:**
Usuarios o bots descargan grandes volúmenes de videos y materiales del curso usando credenciales válidas.

**Activos afectados:** A07

**Impacto:** Pérdida de propiedad intelectual y redistribución no autorizada.

**Probabilidad:** Alta

**Contramedidas:**
- Pre-signed URLs con TTL de 15–30 minutos generadas por Django al solicitar el recurso
- Rate limiting en CDN: máximo N requests de segmento por minuto por sesión
- Watermark dinámico con user_id para rastrear la fuente de filtraciones
- Detección de patrones de descarga anómala (volumen, frecuencia, horario)

---

### TH24 – Suplantación de identidad durante exámenes

**Categoría STRIDE:** Spoofing  
**Componente afectado:** Sistema de evaluaciones  

**Descripción:**
Una persona distinta al estudiante legítimo realiza la evaluación utilizando sus credenciales.

**Activos afectados:** A04, A05

**Impacto:** Compromete la autenticidad de los resultados académicos.

**Probabilidad:** Media-Alta

**Contramedidas:**
- MFA al iniciar el examen (segundo factor específico para la sesión de evaluación)
- Registro de IP, user-agent y geolocalización al iniciar y durante el examen
- Para cursos de alto valor: snapshot de webcam al iniciar (revisión manual o IA)
- Detección de anomalías: cambio de IP o dispositivo a mitad del examen genera alerta

---

### TH26 – CSRF en endpoints de examen y certificación 

**Categoría STRIDE:** Tampering  
**Componente afectado:** Backend  

**Descripción:**
Un atacante engaña al usuario autenticado para que su navegador envíe una solicitud maliciosa a un endpoint privilegiado (ej: submit de examen, emisión de certificado).

**Activos afectados:** A04, A06

**Impacto:** Modificación no autorizada de resultados o emisión de certificados fraudulentos.

**Probabilidad:** Media

**Contramedidas:**
- CSRF tokens en todos los formularios y endpoints de mutación
- Validación de Origin y Referer headers en el backend
- SameSite=Strict en cookies de sesión

---

### TH27 – Mass assignment en modelos Django 

**Categoría STRIDE:** Elevation of Privilege  
**Componente afectado:** Backend  

**Descripción:**
Un atacante envía campos no esperados en el payload JSON de una petición (PATCH/PUT) para modificar atributos protegidos como `role`, `is_staff`, o `exam_score`.

**Activos afectados:** A09, A04, A05

**Impacto:** Elevación de privilegios sin explotar vulnerabilidades técnicas complejas; simplemente enviando JSON con campos adicionales.

**Probabilidad:** Media-Alta

**Contramedidas:**
- `read_only_fields = ['role', 'is_staff', 'is_verified', 'score']` en todos los serializers
- Nunca usar `**request.data` directamente en la creación de objetos
- Tests automatizados que verifiquen que campos protegidos no son modificables via API
