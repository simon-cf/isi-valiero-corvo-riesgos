# Plataforma de Educación Online (LMS) — Análisis de Riesgos

## Integrantes

* Nazarena Valiero
* Simón Corvo

**Fecha de elaboración:** Junio 2026

---

## Descripción del Proyecto

El presente trabajo consiste en el análisis de riesgos y modelado de amenazas de una plataforma de educación online (Learning Management System - LMS).

La plataforma permite a estudiantes acceder a cursos virtuales, visualizar contenido multimedia, participar en foros de discusión, realizar evaluaciones en línea y obtener certificados digitales al completar satisfactoriamente los cursos.

Los docentes pueden crear y administrar cursos, gestionar evaluaciones y realizar seguimiento del desempeño académico. Los administradores gestionan la configuración general y el control de usuarios.

---

## Índice de Documentos

| Doc | Archivo | Descripción |
|-----|---------|-------------|
| 01 | [01-inventario-activos.md](docs/01-inventario-activos.md) | Inventario de activos de información y tecnológicos |
| 02 | [02-arquitectura-trust-boundaries.md](docs/02-arquitectura-trust-boundaries.md) | Diagrama de arquitectura y límites de confianza |
| 03 | [03-analisis-stride.md](docs/03-analisis-stride.md) | Matriz STRIDE con 27 amenazas identificadas |
| 04 | [04-priorizacion-dread.md](docs/04-priorizacion-dread.md) | Priorización DREAD con justificación por dimensión |
| 05 | [05-mapa-attack.md](docs/05-mapa-attack.md) | Mapeo de amenazas a técnicas MITRE ATT&CK |
| 06 | [06-plan-mitigacion.md](docs/06-plan-mitigacion.md) | Plan de mitigación priorizado con responsables y SLA |
| 07 | [07-riesgos-residuales.md](docs/07-riesgos-residuales.md) | Riesgos residuales con compensating controls |
| 08 | [08-conclusiones.md](docs/08-conclusiones.md) | Conclusiones y recomendaciones |

---

## Metodologías Utilizadas

* **STRIDE** — Modelado de amenazas por categoría
* **DREAD** — Priorización cuantitativa de riesgos
* **MITRE ATT&CK Enterprise** — Mapeo a tácticas y técnicas reales
* **NIST SP 800-30** — Marco de evaluación de riesgos
* **NIST SP 800-53** — Controles de seguridad de referencia

---

## Alcance del Análisis

**Componentes incluidos:**
- Frontend desarrollado en Vue.js
- Backend desarrollado en Python/Django
- Base de datos PostgreSQL
- Sistema de autenticación y autorización (JWT)
- Foros de discusión
- CDN para distribución de contenido multimedia
- API externa para emisión de certificados digitales
- Sistema de notificaciones por correo electrónico

**Componentes excluidos:**
- Infraestructura de red interna del proveedor cloud
- Sistemas de terceros fuera del alcance de la integración (internos del proveedor CDN/API)

---

## Supuestos

* Todas las comunicaciones utilizan HTTPS/TLS 1.3.
* Los usuarios acceden mediante credenciales personales con autenticación JWT.
* Existen roles diferenciados: Estudiante, Docente y Administrador.
* Los servicios externos (CDN, API de certificados) se consideran confiables pero sus integraciones son analizadas.
* La plataforma almacena información académica y datos personales, pudiendo incluir datos de menores de edad.

---

## Herramientas de IA Utilizadas

* **ChatGPT (GPT-5.5)** — Asistencia en generación de documentación, identificación de amenazas, elaboración de controles de seguridad.

* **Claude (Anthropic)** — Asistencia en revisión de documentación y organización de información.
