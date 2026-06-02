# 7. RIESGOS RESIDUALES

## 7.1 Introducción

La implementación de los controles definidos en el plan de mitigación reduce significativamente la probabilidad e impacto de las amenazas identificadas. Sin embargo, ningún sistema puede eliminar completamente todos los riesgos.

Los riesgos residuales representan aquellas amenazas que continúan existiendo luego de aplicar las mitigaciones propuestas. Para cada riesgo se documenta: el nivel inherente (antes de mitigar), el nivel residual (después de mitigar) y el compensating control que permite aceptar el riesgo remanente.

---

## 7.2 Riesgos Residuales Identificados

| ID   | Riesgo Residual                                  | Riesgo Inherente | Mitigaciones Aplicadas                         | Riesgo Residual | Compensating Control                                              |
| ---- | ------------------------------------------------ | ---------------- | ---------------------------------------------- | --------------- | ----------------------------------------------------------------- |
| RR01 | Compromiso de credenciales legítimas vía phishing avanzado (bypass de MFA) | Crítico | MFA, rate limiting, detección de anomalías | Medio | Monitoreo de sesiones activas; notificación inmediata ante nuevo login; capacitación en phishing |
| RR02 | Vulnerabilidades Zero-Day en Django, PostgreSQL o dependencias | Alto | Actualizaciones periódicas, WAF, monitoreo | Medio | Proceso de gestión de parches ≤72 h para críticos; suscripción a CVE feeds; SAST en CI/CD |
| RR03 | Uso indebido de IA en evaluaciones               | Alto             | Randomización, evaluación server-side, detección de anomalías | **Alto** | Diseño de preguntas orientadas a aplicación práctica (no memorización); revisión manual de resultados atípicos; política explícita de uso de IA |
| RR04 | Filtración interna del banco de preguntas (insider threat) | Alto | RBAC, audit log, monitoreo de consultas | Medio | Segregación de acceso: ningún usuario ve el banco completo; alertas por descarga masiva; revisión de logs semanal |
| RR05 | Compartición de cuentas entre usuarios           | Medio            | MFA, detección de sesiones concurrentes        | Medio           | Detección de logins simultáneos desde IPs distintas; invalidar sesiones duplicadas; política de uso aceptable |
| RR06 | Grabación de pantalla y redistribución de contenido | Alto         | Watermark dinámico, pre-signed URLs            | Medio           | Watermark con user_id para rastrear la fuente; proceso DMCA definido para takedown; monitoreo en plataformas de piratería |
| RR07 | Errores de configuración que introducen vulnerabilidades | Medio    | Hardening, revisión de configuración, IaC      | Bajo            | Infrastructure as Code (IaC) para reproducibilidad; checklist de seguridad en deploys; revisión de configuración semestral |
| RR08 | Ataques DDoS durante períodos de evaluación      | **Alto**         | Rate limiting, CDN, balanceo de carga          | Medio           | Protección DDoS en el CDN (Cloudflare/AWS Shield); plan de contingencia para exámenes offline; comunicación proactiva a estudiantes |
| RR09 | Grabación de pantalla de videos del curso        | Alto             | Pre-signed URLs, HLS cifrado                   | Alto            | Watermark visible e invisible; imposible de prevenir completamente; aceptado como limitación técnica del sector |


---

## 7.3 Análisis de Riesgos Residuales Críticos

### RR03 – Uso indebido de Inteligencia Artificial durante evaluaciones

**Riesgo inherente:** Alto | **Riesgo residual:** Alto

Aunque la plataforma implemente randomización de preguntas, evaluación server-side y detección de anomalías de comportamiento, no es posible impedir técnicamente que un estudiante use herramientas externas de IA durante el examen.

Este es uno de los principales desafíos actuales del sector educativo online a nivel global.

**Por qué se acepta este riesgo:**
La naturaleza del problema es fundamentalmente pedagógica, no técnica. Los controles técnicos disponibles (proctoring, detección de tabs, etc.) son bypasseables o generan fricción desproporcionada para estudiantes legítimos.

**Compensating controls:**
- Diseñar evaluaciones con preguntas de aplicación práctica, análisis de casos y síntesis — resistentes al copy-paste de respuestas de IA
- Combinar evaluaciones escritas con proyectos prácticos verificables
- Política explícita sobre el uso permitido/no permitido de IA, con consecuencias documentadas
- Revisión manual de resultados estadísticamente atípicos (score muy alto con tiempo muy corto)

---

### RR04 – Filtración interna del banco de preguntas (Insider Threat)

**Riesgo inherente:** Alto | **Riesgo residual:** Medio

Los controles de acceso reducen el riesgo de acceso no autorizado, pero no eliminan la posibilidad de que un docente con permisos legítimos divulgue el banco de preguntas intencionalmente.

**Compensating controls:**
- Ningún usuario accede al banco de preguntas completo — la selección de preguntas para un examen es automática (sistema)
- Audit log de cada acceso al banco, con alertas ante descarga de más de N preguntas en una sesión
- Política de conducta del docente firmada; consecuencias legales documentadas
- Renovación del 30% del banco cada trimestre para reducir el valor de datos filtrados

---

### RR02 – Vulnerabilidades Zero-Day

**Riesgo inherente:** Alto | **Riesgo residual:** Medio

La existencia de vulnerabilidades desconocidas en Django, PostgreSQL, Vue.js o dependencias de terceros representa un riesgo permanente.

**Compensating controls:**
- Suscripción a feeds de CVE para Django, PostgreSQL y las librerías principales (dependabot, Snyk)
- Proceso de gestión de parches: críticos en ≤72 h, altos en ≤1 semana
- SAST (Static Application Security Testing) integrado en el pipeline de CI/CD
- WAF como capa adicional de protección ante exploits conocidos
- Pruebas de penetración anuales

---

### RR06 / RR09 – Redistribución de contenido y grabación de pantalla

**Riesgo inherente:** Alto | **Riesgo residual:** Medio (RR06) / Alto (RR09)

La grabación de pantalla con software externo o dispositivo secundario es imposible de prevenir técnicamente. Es el vector de piratería más común en plataformas de contenido digital.

**Compensating controls:**
- Watermark visible con user_id parcial + fecha sobre el player de video
- Watermark invisible (esteganográfico) en el stream para permitir rastrear la fuente
- Proceso DMCA definido: monitoreo de plataformas de distribución (YouTube, Telegram, sitios de cursos piratas); protocolo de takedown en ≤48 h
- Aceptado explícitamente como limitación técnica del sector; la estrategia es disuasión y trazabilidad, no prevención

---

## 7.4 Estrategia de Monitoreo Continuo

| Actividad                              | Frecuencia     | Responsable       |
| -------------------------------------- | -------------- | ----------------- |
| Revisión de logs de auditoría          | Semanal        | Administrador     |
| Auditoría de roles y permisos          | Semestral      | Administrador     |
| Actualización de componentes y parches | Continua (CI)  | DevOps            |
| Prueba de penetración externa          | Anual          | Equipo externo    |
| Revisión del plan de mitigación        | Ante cambios   | Arquitecto        |
| Monitoreo de filtraciones en internet  | Mensual        | Administrador     |
| Capacitación de usuarios en seguridad  | Anual          | RRHH / Plataforma |
| Revisión de CVEs en dependencias       | Semanal (auto) | DevOps (dependabot)|

---

## 7.5 Aceptación del Riesgo

Considerando las mitigaciones propuestas y el nivel de riesgo residual identificado, se concluye que la plataforma puede operar dentro de niveles aceptables de riesgo siempre que:

1. Se implementen los controles de Prioridad Alta del plan de mitigación antes del lanzamiento
2. Se mantenga el proceso continuo de monitoreo definido en la sección 7.4
3. Se realice una revisión completa del modelo de amenazas ante cambios arquitectónicos significativos
4. Los riesgos residuales Alto (RR03 y RR09) sean formalmente aceptados por la dirección de la institución, con firma del documento de aceptación de riesgo
