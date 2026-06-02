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

Las categorías evaluadas fueron:

* Spoofing (Suplantación de identidad)
* Tampering (Manipulación de datos)
* Repudiation (Negación de acciones)
* Information Disclosure (Divulgación de información)
* Denial of Service (Denegación de servicio)
* Elevation of Privilege (Elevación de privilegios)


---

### 3.2 Matriz de Amenazas por Componente

| ID   | Componente         | Categoría STRIDE | Amenaza                                              | CWE     |
|------|--------------------|------------------|------------------------------------------------------------------------|-------------|
| TH01 | Frontend | Spoofing         | Robo de sesión mediante secuestro de cookies             | CWE-384     |
| TH02 | Frontend        | Tampering         | Manipulación de parámetros enviados al backend           | CWE-472     |
| TH03 | Frontend    | Information Disclosure        | Exposición de información sensible en código JavaScript         | CWE-200     |
| TH04 | Frontend   | DoS        | Automatización masiva de solicitudes desde bots    | CWE-400    |
| TH05 | Backend   | Spoofing        | Uso de credenciales robadas para acceder como docente | CWE-287     |
| TH06 | Backend      | Tampering      | Modificación no autorizada de calificaciones        | CWE-284     |
| TH07 | Backend  | Repudiation      | Negación de cambios realizados por docentes      | CWE-778     |
| TH08 | Backend       | Information Disclosure  | Acceso indebido a exámenes antes de su publicación   | CWE-200    |
| TH09 | Backend    | DoS  | Saturación de API durante evaluaciones masivas  | CWE-770     |
| TH10 | Backend   | Elevation of Privilege | Escalación de privilegios de estudiante a docente  | CWE-269     |
| TH11 | PostgreSQL     | Information Disclosure | Extracción de datos personales mediante SQL Injection | CWE-89     |
| TH12 | PostgreSQL | Tampering | Alteración de registros académicos | CWE-89 |
| TH13 | PostgreSQL     | DoS | Consultas masivas que degradan el rendimiento  | CWE-400 |
| TH14 | PostgreSQL | Elevation of Privilege | Obtención de permisos administrativos sobre la base de datos | CWE-266 |
| TH15 | API Certificados | Tampering | Alteración de certificados emitidos   | CWE-345     |
| TH16 | API Certificados  | Spoofing | Emisión fraudulenta de certificados | CWE-290 |
| TH17 | API Certificados  | Information Disclosure | Acceso a certificados de terceros | CWE-639 |
| TH18 | Foro  | Tampering | Publicación de contenido malicioso | CWE-79 |
| TH19 | Foro  | Information Disclosure | Robo de sesiones mediante XSS almacenado | CWE-79 |
| TH20 | Exámenes  | Information Disclosure | Filtración del banco de preguntas antes de una evaluación | CWE-200 |
| TH21 | Exámenes  | Repudiation | Estudiante niega haber realizado un examen | CWE-778 |
| TH22 | Certificación  | Tampering | Modificación de notas para obtener certificados | CWE-284 |
| TH23 | Contenido Educativo  | Information Disclosure | Descarga masiva de videos protegidos | CWE-200 |
| TH24 | Evaluaciones  | Spoofing | Suplantación de identidad durante exámenes | CWE-287 |
| TH25 | Evaluaciones  | Tampering | Uso de herramientas automatizadas o IA para manipular evaluaciones | N/A |


## 3.3 Análisis Detallado de Amenazas Prioritarias

### TH05 – Uso de credenciales robadas para acceder como docente

**Categoría STRIDE:** Spoofing

**Componente afectado:** Backend  / Sistema de autenticación

**Descripción:**
Un atacante obtiene las credenciales de un docente mediante phishing, reutilización de contraseñas filtradas o malware, logrando acceso legítimo a la plataforma.

**Activos afectados:**

* A02 – Credenciales de acceso
* A03 – Banco de preguntas
* A04 – Resultados de evaluaciones
* A07 – Material educativo

**Impacto:**
El atacante podría modificar cursos, acceder a evaluaciones, alterar contenidos o emitir certificados de forma fraudulenta.

**Probabilidad:** Alta

---

### TH08 – Acceso indebido a exámenes antes de su publicación

**Categoría STRIDE:** Information Disclosure

**Componente afectado:** Backend 

**Descripción:**
Un usuario explota fallas de autorización o errores en las APIs para acceder a exámenes aún no publicados.

**Activos afectados:**

* A03 – Banco de preguntas
* A04 – Resultados de evaluaciones

**Impacto:**
Compromete la integridad académica de la evaluación y genera ventajas injustas para determinados estudiantes.

**Probabilidad:** Media-Alta

---

### TH10 – Escalación de privilegios de estudiante a docente

**Categoría STRIDE:** Elevation of Privilege

**Componente afectado:** Backend 

**Descripción:**
Un estudiante aprovecha una vulnerabilidad de autorización para obtener permisos reservados a docentes o administradores.

**Activos afectados:**

* A09 – Configuración de roles y permisos
* A04 – Resultados de evaluaciones
* A05 – Historial académico

**Impacto:**
Permitiría modificar calificaciones, crear cursos falsos o acceder a información de otros usuarios.

**Probabilidad:** Media

---

### TH11 – SQL Injection sobre la base de datos

**Categoría STRIDE:** Information Disclosure / Tampering

**Componente afectado:** PostgreSQL

**Descripción:**
Un atacante envía entradas maliciosas a la aplicación con el objetivo de ejecutar consultas SQL no autorizadas.

**Activos afectados:**

* A01 – Datos personales
* A03 – Banco de preguntas
* A04 – Resultados de evaluaciones
* A05 – Historial académico

**Impacto:**
Podría provocar robo, modificación o eliminación masiva de información crítica.

**Probabilidad:** Media

---

### TH15 – Alteración de certificados emitidos

**Categoría STRIDE:** Tampering

**Componente afectado:** API de certificados

**Descripción:**
Un atacante modifica la información contenida en un certificado o genera versiones falsas aparentando legitimidad.

**Activos afectados:**

* A06 – Certificados emitidos

**Impacto:**
Afecta la credibilidad de la plataforma y la validez de los certificados otorgados.

**Probabilidad:** Media

---

### TH20 – Filtración del banco de preguntas

**Categoría STRIDE:** Information Disclosure

**Componente afectado:** Módulo de exámenes

**Descripción:**
Las preguntas y respuestas son obtenidas antes de una evaluación debido a errores de configuración, accesos indebidos o fugas internas.

**Activos afectados:**

* A03 – Banco de preguntas
* A04 – Resultados de evaluaciones

**Impacto:**
Compromete la integridad de los procesos de evaluación y la confianza en la plataforma.

**Probabilidad:** Alta

---

### TH23 – Descarga masiva de contenido educativo

**Categoría STRIDE:** Information Disclosure

**Componente afectado:** CDN de contenido

**Descripción:**
Usuarios o bots automatizados descargan grandes volúmenes de videos, materiales y recursos educativos.

**Activos afectados:**

* A07 – Material educativo

**Impacto:**
Puede generar pérdida de propiedad intelectual y redistribución no autorizada del contenido.

**Probabilidad:** Alta

---

### TH24 – Suplantación de identidad durante exámenes

**Categoría STRIDE:** Spoofing

**Componente afectado:** Sistema de evaluaciones

**Descripción:**
Una persona distinta al estudiante legítimo realiza la evaluación utilizando sus credenciales.

**Activos afectados:**

* A04 – Resultados de evaluaciones
* A05 – Historial académico

**Impacto:**
Compromete la autenticidad de los resultados académicos y la confianza en el sistema.

**Probabilidad:** Media-Alta


---

