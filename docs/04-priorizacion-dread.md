# 4. PRIORIZACIÓN DE RIESGOS - DREAD

## ¿Qué es DREAD?

Hasta ahora identificamos amenazas. Pero no todas son igual de peligrosas.

**DREAD sirve para responder:** ¿Cuáles son las amenazas que debemos atender primero?

| Factor           | Pregunta                                       |
| ---------------- | ---------------------------------------------- |
| Damage Potential | ¿Qué daño causa si se explota?                 |
| Reproducibility  | ¿Qué tan fácil es repetir el ataque?           |
| Exploitability   | ¿Qué tan fácil es explotarlo?                  |
| Affected Users   | ¿A cuántos usuarios afecta?                    |
| Discoverability  | ¿Qué tan fácil es descubrir la vulnerabilidad? |

Cada factor se puntúa de 1 a 10. Promedio: **8–10 → Crítico · 6–7.9 → Alto · 4–5.9 → Medio · <4 → Bajo**

---

# 4.1 Matriz de Priorización

| ID   | Amenaza                                               | D  | R  | E  | A  | D₂ | Promedio | Riesgo   |
| ---- | ----------------------------------------------------- | -- | -- | -- | -- | -- | -------- | -------- |
| TH11 | SQL Injection                                         | 10 | 8  | 7  | 10 | 6  | **8.2**  | Crítico  |
| TH05 | Uso de credenciales robadas para acceder como docente | 9  | 8  | 8  | 8  | 8  | **8.2**  | Crítico  |
| TH27 | Mass assignment en modelos Django                     | 9  | 8  | 7  | 8  | 8  | **8.0**  | Crítico  |
| TH20 | Filtración del banco de preguntas                     | 9  | 8  | 7  | 9  | 8  | **8.2**  | Crítico  |
| TH16 | Emisión fraudulenta de certificados                   | 9  | 7  | 7  | 8  | 7  | **7.6**  | Alto     |
| TH10 | Escalación de privilegios                             | 9  | 7  | 7  | 8  | 7  | **7.6**  | Alto     |
| TH19 | XSS almacenado en foros (robo de sesión)              | 9  | 8  | 7  | 9  | 7  | **8.0**  | Crítico  |
| TH08 | Acceso indebido a exámenes antes de publicación       | 8  | 7  | 7  | 8  | 7  | **7.4**  | Alto     |
| TH15 | Alteración de certificados emitidos                   | 8  | 7  | 6  | 7  | 7  | **7.0**  | Alto     |
| TH26 | CSRF en endpoints de examen y certificación           | 8  | 7  | 6  | 8  | 6  | **7.0**  | Alto     |
| TH23 | Descarga masiva de contenido educativo                | 6  | 8  | 8  | 7  | 8  | **7.4**  | Alto     |
| TH24 | Suplantación de identidad durante exámenes            | 8  | 7  | 7  | 8  | 7  | **7.4**  | Alto     |
| TH22 | Modificación de notas para obtener certificados       | 9  | 6  | 5  | 7  | 6  | **6.6**  | Alto     |
| TH18 | Stored XSS — publicación de contenido malicioso      | 7  | 8  | 7  | 8  | 6  | **7.2**  | Alto     |
| TH06 | Modificación no autorizada de calificaciones          | 8  | 6  | 6  | 7  | 6  | **6.6**  | Alto     |
| TH17 | IDOR — acceso a certificados de terceros              | 7  | 9  | 8  | 6  | 6  | **7.2**  | Alto     |
| TH12 | Alteración de registros en PostgreSQL                 | 9  | 5  | 5  | 8  | 5  | **6.4**  | Alto     |
| TH04 | Automatización masiva (bots)                          | 5  | 8  | 8  | 7  | 7  | **7.0**  | Alto     |
| TH09 | Saturación de API durante evaluaciones                | 7  | 7  | 7  | 8  | 7  | **7.2**  | Alto     |
| TH13 | Consultas masivas que degradan PostgreSQL             | 6  | 7  | 6  | 8  | 6  | **6.6**  | Alto     |
| TH21 | Estudiante niega haber realizado examen               | 5  | 6  | 5  | 6  | 5  | **5.4**  | Medio    |
| TH07 | Negación de cambios realizados por docentes           | 6  | 6  | 5  | 5  | 5  | **5.4**  | Medio    |
| TH03 | Exposición de info sensible en JS                     | 5  | 7  | 6  | 5  | 7  | **6.0**  | Alto     |
| TH14 | Permisos admin sobre PostgreSQL                       | 9  | 4  | 4  | 8  | 4  | **5.8**  | Medio    |
| TH25 | Uso de IA en evaluaciones                             | 7  | 9  | 9  | 8  | 2  | **7.0**  | Alto     |
| TH01 | Robo de sesión por secuestro de cookies               | 8  | 6  | 6  | 7  | 6  | **6.6**  | Alto     |
| TH02 | Manipulación de parámetros enviados al backend        | 7  | 7  | 6  | 6  | 6  | **6.4**  | Alto     |

---

## 4.2 Resumen de Distribución de Riesgos

| Nivel    | Cantidad | Amenazas                                                  |
| -------- | -------- | --------------------------------------------------------- |
| Crítico  | 5        | TH11, TH05, TH27, TH20, TH19                             |
| Alto     | 18       | TH16, TH10, TH08, TH15, TH26, TH23, TH24, TH22, TH18... |
| Medio    | 3        | TH21, TH07, TH14                                         |
| Bajo     | 0        | —                                                         |

---

## 4.3 Justificación Amenazas Críticas

### TH11 – SQL Injection (8.2)

| Dimensión        | Score | Justificación                                                                 |
| ---------------- | ----- | ----------------------------------------------------------------------------- |
| Damage           | 10    | Puede comprometer la totalidad de la DB: datos de todos los usuarios, exámenes, historial |
| Reproducibility  | 8     | Reproducible con herramientas estándar (sqlmap); requiere encontrar punto vulnerable |
| Exploitability   | 7     | Django ORM reduce la superficie; persiste riesgo en raw queries o inputs no sanitizados |
| Affected Users   | 10    | Afecta a todos los usuarios de la plataforma (DB centralizada)                |
| Discoverability  | 6     | No trivial con ORM; detectable con scanners si hay errores de DB en respuestas |


---

### TH05 – Credenciales robadas (8.2)

| Dimensión        | Score | Justificación                                                               |
| ---------------- | ----- | --------------------------------------------------------------------------- |
| Damage           | 9     | Acceso como docente permite alterar cursos, notas, banco de preguntas       |
| Reproducibility  | 8     | Credential stuffing es trivialmente automatizable con listas de HaveIBeenPwned |
| Exploitability   | 8     | Sin MFA, basta con usuario y contraseña; herramientas públicas disponibles  |
| Affected Users   | 8     | Afecta a todos los estudiantes del docente comprometido                     |
| Discoverability  | 8     | El endpoint de login es público y conocido                                  |

---

### TH20 – Filtración del banco de preguntas (8.2)

| Dimensión        | Score | Justificación                                                               |
| ---------------- | ----- | --------------------------------------------------------------------------- |
| Damage           | 9     | Compromete toda la integridad académica del curso afectado                  |
| Reproducibility  | 8     | Si el endpoint es accesible, la extracción es sistemática y automatizable   |
| Exploitability   | 7     | Requiere cuenta válida y encontrar el endpoint; no requiere habilidades avanzadas |
| Affected Users   | 9     | Afecta a todos los estudiantes que tomaron o tomarán ese examen             |
| Discoverability  | 8     | Los endpoints de examen son explorables con un token válido de estudiante   |

---

### TH19 – XSS almacenado en foros (8.0)

| Dimensión        | Score | Justificación                                                               |
| ---------------- | ----- | --------------------------------------------------------------------------- |
| Damage           | 9     | Robo masivo de sesiones de todos los usuarios que lean el post infectado    |
| Reproducibility  | 8     | Una vez publicado el payload, se ejecuta automáticamente en cada visita     |
| Exploitability   | 7     | Requiere conocimiento de XSS; payloads estándar disponibles públicamente    |
| Affected Users   | 9     | Todos los usuarios que lean el post afectado                                |
| Discoverability  | 7     | Foros con HTML sin sanitizar son detectables con scanners básicos (OWASP ZAP) |

---

### TH27 – Mass assignment (8.0)

| Dimensión        | Score | Justificación                                                               |
| ---------------- | ----- | --------------------------------------------------------------------------- |
| Damage           | 9     | Elevación de privilegios completa con solo modificar un campo JSON          |
| Reproducibility  | 8     | Reproducible en cualquier endpoint PATCH/PUT que no proteja campos sensibles |
| Exploitability   | 7     | Requiere explorar la API; herramientas como Burp Suite facilitan el proceso |
| Affected Users   | 8     | Si un atacante escala a admin, afecta a toda la plataforma                  |
| Discoverability  | 8     | Fácil de detectar explorando la API; la documentación puede revelar los campos |

---

## 4.4 Vinculación con Plan de Mitigación

| ID Amenaza | Score | Nivel   | Ver Mitigación (doc 06)                           |
| ---------- | ----- | ------- | ------------------------------------------------- |
| TH11       | 8.2   | Crítico | Sección 6.3 TH11 — Protección SQL; Sección 6.5 PostgreSQL |
| TH05       | 8.2   | Crítico | Sección 6.3 TH05 — MFA y gestión de identidad    |
| TH20       | 8.2   | Crítico | Sección 6.3 TH20 — Protección banco de preguntas |
| TH19       | 8.0   | Crítico | Sección 6.5 Foro — Sanitización XSS              |
| TH27       | 8.0   | Crítico | Sección 6.5 Backend — Mass assignment             |
