# Memory Bank — Contexto para IA

Este archivo provee el contexto necesario para que una herramienta de IA (ChatGPT, Claude, Copilot, Cursor, etc.) comprenda el proyecto y pueda asistir en la generación o revisión de documentos sin necesidad de re-explicar el escenario cada vez.

---

## Escenario

**Nombre:** Plataforma de Educación Online (LMS) — Escenario 4  
**Curso:** Introducción a la Seguridad Informática (ISI) — 2026  
**Grupo:** Nazarena Valiero, Simón Corvo  
**Presentación:** Jueves 11 de Junio de 2026

---

## Descripción del Sistema

Plataforma de e-learning que permite:

- Cursos en video y material educativo (PDFs)
- Exámenes y evaluaciones en línea
- Foros de discusión
- Certificados digitales al completar cursos
- Panel de progreso y gamificación
- Roles: Estudiante, Docente y Administrador

**Stack tecnológico:** Frontend Vue.js · Backend Python/Django · Base de datos PostgreSQL · CDN para video · API externa de certificados · Servicio de email para notificaciones · Autenticación JWT

**Consideraciones especiales:** datos de menores (COPPA), integridad académica, propiedad intelectual del contenido, protección de banco de preguntas.

---

## Archivos de Contexto Necesarios

Para comprender el proyecto completo, cargar los siguientes archivos:

| Archivo                                    | Contenido                                                                                                                  |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| `docs/01-inventario-activos.md`            | Activos de información (A01–A10) y tecnológicos (T01–T09): PII, credenciales, banco de preguntas, certificados, CDN, foros |
| `docs/02-arquitectura-trust-boundaries.md` | Arquitectura del sistema y los 4 trust boundaries (TB-01 a TB-04)                                                          |
| `docs/03-analisis-stride.md`               | Análisis STRIDE completo: 27 amenazas identificadas (TH01–TH27)                                                            |
| `docs/04-priorizacion-dread.md`            | Matriz DREAD con scoring; TH05, TH11, TH20, TH27 = CRÍTICO (~8.0–8.2)                                                      |
| `docs/05-mapa-attack.md`                   | Técnicas MITRE ATT&CK mapeadas (T1078, T1190, T1539, T1565, T1213, etc.)                                                   |
| `docs/06-plan-mitigacion.md`               | Plan de mitigación priorizado con controles NIST SP 800-53 y plazos de implementación                                      |
| `docs/07-riesgos-residuales.md`            | 9 riesgos residuales (RR01–RR09) con compensating controls                                                                 |
| `docs/08-conclusiones.md`                  | Resumen ejecutivo, hallazgos principales y recomendaciones                                                                 |
| `diagrams/arquitectura.drawio`             | Diagrama de arquitectura editable (draw.io)                                                                                |
| `diagrams/arquitectura.png`                | Exportación del diagrama para presentación                                                                                 |

---

## Amenazas Identificadas (resumen)

| ID   | Amenaza                                              | STRIDE | Promedio DREAD | Nivel   |
| ---- | ---------------------------------------------------- | ------ | -------------- | ------- |
| TH11 | SQL Injection en PostgreSQL                          | I      | 8.2            | Crítico |
| TH05 | Credenciales robadas — acceso como docente           | S      | 8.2            | Crítico |
| TH20 | Filtración del banco de preguntas                    | I      | 8.2            | Crítico |
| TH27 | Mass assignment en modelos Django                    | E      | 8.0            | Crítico |
| TH16 | Emisión fraudulenta de certificados                  | S      | 7.6            | Alto    |
| TH10 | Escalación de privilegios (estudiante → docente)     | E      | 7.6            | Alto    |
| TH18 | Stored XSS en foros                                  | T      | 7.4            | Alto    |
| TH23 | Descarga masiva de videos protegidos                 | I      | 7.2            | Alto    |
| TH25 | Uso de IA/herramientas automatizadas en evaluaciones | T      | 7.0            | Alto    |

---

## Controles Prioritarios

1. **MFA** para docentes y administradores (mitiga TH05, TH16)
2. **httpOnly cookies + SameSite=Strict** para JWT (mitiga TH01, TH19)
3. **Sanitización XSS** en foros — bleach + DOMPurify (mitiga TH18, TH19)
4. **read_only_fields** en serializers Django (mitiga TH27)
5. **RBAC + evaluación server-side** (mitiga TH08, TH10, TH20)
6. **Firma digital de certificados** vía HSM/KMS (mitiga TH15, TH16)
7. **Pre-signed URLs con TTL corto** en CDN (mitiga TH23)

---

## Trust Boundaries

| ID    | Límite                       | Componentes                           |
| ----- | ---------------------------- | ------------------------------------- |
| TB-01 | Usuarios ↔ Aplicación        | Internet → Frontend Vue.js            |
| TB-02 | Frontend ↔ Backend           | Vue.js → Django REST API (JWT)        |
| TB-03 | Backend ↔ Servicios externos | Django → CDN, API Certificados, Email |
| TB-04 | Backend ↔ Datos persistentes | Django → PostgreSQL                   |

---

## Metodologías Utilizadas

- **STRIDE** — Clasificación de amenazas (Microsoft)
- **DREAD** — Priorización cuantitativa de riesgos
- **MITRE ATT&CK Enterprise** — Mapeo a técnicas de ataque reales
- **NIST SP 800-30** — Marco de evaluación de riesgos
- **NIST SP 800-53 Rev. 5** — Controles de seguridad (AC, AU, SC, SI)

---

## Prompts disponibles en este repositorio

| Archivo                               | Propósito                                           |
| ------------------------------------- | --------------------------------------------------- |
| `prompts/00-memory-bank.md`           | Este archivo — contexto general                     |
| `prompts/01-inventario-activos.txt`   | Inventario de activos de información y tecnológicos |
| `prompts/02-arquitectura.txt`         | Diagrama de arquitectura y trust boundaries         |
| `prompts/03-stride.txt`               | Análisis STRIDE de amenazas                         |
| `prompts/04-dread.txt`                | Priorización DREAD                                  |
| `prompts/05-mapa-attack.txt`       | Mapeo MITRE ATT&CK                                  |
| `prompts/06-plan-mitigacion.txt`           | Plan de mitigación y controles NIST                 |
| `prompts/07-riesgos-residuales.txt` | Riesgos residuales                  |

---

## Instrucciones para la IA

Al asistir en este proyecto:

1. Mantener coherencia con las amenazas TH01–TH27 definidas en `docs/03-analisis-stride.md`.
2. Usar los IDs de activos (A01–A10, T01–T09) definidos en `docs/01-inventario-activos.md`.
3. Referenciar los trust boundaries TB-01 a TB-04 cuando se hable de flujos de datos.
4. El sistema usa Vue.js + Django + PostgreSQL; las recomendaciones técnicas deben ser compatibles.
5. Todos los controles deben mapearse a NIST SP 800-53 cuando sea posible.
6. Considerar el contexto académico: integridad de evaluaciones, certificados, datos de menores (COPPA) y propiedad intelectual del contenido educativo.
