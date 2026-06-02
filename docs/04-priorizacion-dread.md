# ¿Qué es DREAD?

Hasta ahora identificamos amenazas.
Pero no todas son igual de peligrosas.

## DREAD sirve para responder:
### ¿Cuáles son las amenazas que debemos atender primero?
---

### DREAD evalúa 5 factores:
| Factor           | Pregunta                                       |
| ---------------- | ---------------------------------------------- |
| Damage Potential | ¿Qué daño causa?                               |
| Reproducibility  | ¿Qué tan fácil es repetir el ataque?           |
| Exploitability   | ¿Qué tan fácil es explotarlo?                  |
| Affected Users   | ¿A cuántos usuarios afecta?                    |
| Discoverability  | ¿Qué tan fácil es descubrir la vulnerabilidad? |

Cada factor se puntúa de 1 a 10.

Promedio:
* 8-10 → Crítico
* 6-7.9 → Alto
* 4-5.9 → Medio
* <4 → Bajo
---

# 4. PRIORIZACIÓN DE RIESGOS - DREAD

| ID   | Amenaza                                               | D  | R | E | A  | D | Promedio | Riesgo  |
| ---- | ----------------------------------------------------- | -- | - | - | -- | - | -------- | ------- |
| TH05 | Uso de credenciales robadas para acceder como docente | 9  | 8 | 8 | 8  | 8 | 8.2      | Crítico |
| TH08 | Acceso indebido a exámenes antes de publicación       | 8  | 7 | 7 | 8  | 7 | 7.4      | Alto    |
| TH10 | Escalación de privilegios                             | 9  | 7 | 7 | 8  | 7 | 7.6      | Alto    |
| TH11 | SQL Injection                                         | 10 | 9 | 8 | 10 | 8 | 9.0      | Crítico |
| TH15 | Alteración de certificados                            | 8  | 7 | 6 | 7  | 7 | 7.0      | Alto    |
| TH20 | Filtración del banco de preguntas                     | 9  | 8 | 7 | 9  | 8 | 8.2      | Crítico |
| TH23 | Descarga masiva de contenido educativo                | 6  | 8 | 8 | 7  | 8 | 7.4      | Alto    |
| TH24 | Suplantación de identidad durante exámenes            | 8  | 7 | 7 | 8  | 7 | 7.4      | Alto    |

## 4.1 Riesgos Prioritarios

Del análisis DREAD surgen tres amenazas con prioridad crítica:

### TH11 – SQL Injection (9.0)

Representa el mayor riesgo para la plataforma debido a que puede comprometer la totalidad de la base de datos, permitiendo acceso, modificación o eliminación de información sensible.

### TH05 – Uso de credenciales robadas (8.2)

Permite acceso legítimo a funcionalidades críticas de docentes y administradores, afectando múltiples activos del sistema.

### TH20 – Filtración del banco de preguntas (8.2)

Compromete directamente la integridad académica de la plataforma y afecta la confianza de estudiantes y docentes.

Estas amenazas serán consideradas prioritarias en el plan de mitigación.
