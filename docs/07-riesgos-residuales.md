# 7. RIESGOS RESIDUALES

## 7.1 Introducción

La implementación de controles de seguridad reduce significativamente la probabilidad e impacto de las amenazas identificadas. Sin embargo, ningún sistema puede eliminar completamente todos los riesgos.

Los riesgos residuales representan aquellas amenazas que continúan existiendo luego de aplicar las mitigaciones propuestas y que deben ser monitoreadas continuamente.

---

## 7.2 Riesgos Residuales Identificados

| ID | Riesgo Residual | Justificación | Nivel Residual |
|----|----------------|---------------|---------------|
| RR01 | Compromiso de credenciales legítimas | Un usuario puede aprobar MFA mediante phishing o ingeniería social | Medio |
| RR02 | Vulnerabilidades Zero-Day | Pueden existir fallas desconocidas en Django, PostgreSQL o dependencias | Medio |
| RR03 | Uso indebido de IA en evaluaciones | Los estudiantes pueden utilizar herramientas de IA para responder exámenes | Alto |
| RR04 | Filtración interna de preguntas | Un docente autorizado puede divulgar exámenes fuera de la plataforma | Medio |
| RR05 | Compartición de cuentas | Usuarios pueden compartir credenciales con terceros | Medio |
| RR06 | Descarga y redistribución de contenido | El material educativo puede ser grabado o copiado manualmente | Medio |
| RR07 | Errores de configuración | Configuraciones incorrectas pueden introducir nuevas vulnerabilidades | Medio |
| RR08 | Ataques DDoS a gran escala | Servicios externos pueden verse afectados incluso con mitigaciones implementadas | Bajo |

---

## 7.3 Riesgos Residuales Críticos

### RR03 – Uso indebido de Inteligencia Artificial durante evaluaciones

Aunque la plataforma implemente controles de autenticación y monitoreo, no es posible impedir completamente el uso de herramientas externas de inteligencia artificial durante evaluaciones remotas.

Este riesgo afecta directamente la integridad académica y constituye uno de los principales desafíos actuales para plataformas educativas.

---

### RR04 – Filtración interna del banco de preguntas

Los controles de acceso reducen significativamente el riesgo de acceso no autorizado, pero no eliminan la posibilidad de que un usuario legítimo con permisos adecuados divulgue información sensible.

Este riesgo corresponde a una amenaza interna (Insider Threat).

---

### RR02 – Vulnerabilidades Zero-Day

La existencia de vulnerabilidades desconocidas en frameworks, bibliotecas o servicios de terceros representa un riesgo permanente.

La organización deberá mantener procesos de actualización y monitoreo continuo para reducir la exposición.

---

## 7.4 Estrategia de Monitoreo

Para gestionar los riesgos residuales se recomienda:

- Revisión periódica de logs.
- Auditorías de seguridad semestrales.
- Actualización continua de componentes.
- Capacitación de usuarios y docentes.
- Monitoreo de comportamientos anómalos.
- Revisión periódica de roles y permisos.

---

## 7.5 Aceptación del Riesgo

Considerando las mitigaciones propuestas y el nivel de riesgo residual identificado, se concluye que la plataforma puede operar dentro de niveles aceptables de riesgo siempre que se mantenga un proceso continuo de monitoreo y mejora de los controles implementados.