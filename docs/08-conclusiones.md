# 8. CONCLUSIONES Y RECOMENDACIONES

## 8.1 Resumen del Trabajo Realizado

Durante el presente trabajo se realizó un análisis de riesgos y modelado de amenazas para una Plataforma de Educación Online (LMS), utilizando metodologías reconocidas de la industria como STRIDE, DREAD y MITRE ATT&CK.

El análisis abarcó los principales componentes de la arquitectura propuesta:

- Frontend Vue.js
- Backend Python/Django
- Base de datos PostgreSQL
- CDN para distribución de contenido
- API de emisión de certificados

Se identificaron los activos críticos del sistema, los flujos de información y los trust boundaries relevantes, permitiendo modelar amenazas asociadas a la confidencialidad, integridad y disponibilidad de la plataforma.

---

## 8.2 Principales Hallazgos

El análisis STRIDE permitió identificar múltiples amenazas potenciales sobre los distintos componentes del sistema.

Entre las amenazas más relevantes se destacan:

- Uso de credenciales robadas para acceder como docente.
- Escalación de privilegios.
- SQL Injection sobre la base de datos.
- Filtración del banco de preguntas.
- Alteración de certificados emitidos.
- Suplantación de identidad durante evaluaciones.
- Descarga masiva de contenido educativo.

La priorización mediante DREAD determinó que las amenazas con mayor criticidad son:

1. SQL Injection (TH11)
2. Uso de credenciales robadas (TH05)
3. Filtración del banco de preguntas (TH20)

Estas amenazas presentan el mayor potencial de impacto sobre los activos académicos y personales almacenados en la plataforma.

---

## 8.3 Evaluación General del Riesgo

La plataforma presenta un nivel de riesgo moderado-alto debido a la naturaleza sensible de la información que procesa y a la exposición de servicios accesibles desde Internet.

Los activos de mayor criticidad identificados fueron:

- Credenciales de usuarios.
- Banco de preguntas y evaluaciones.
- Resultados académicos.
- Certificados emitidos.
- Datos personales de estudiantes y docentes.

La protección de estos activos debe ser considerada prioritaria durante el diseño e implementación de la solución.

---

## 8.4 Recomendaciones

Se recomienda implementar las siguientes medidas como parte de la estrategia de seguridad de la plataforma:

### Gestión de Identidad y Acceso

- Implementar autenticación multifactor (MFA) para docentes y administradores.
- Aplicar el principio de mínimo privilegio.
- Revisar periódicamente roles y permisos.

### Protección de Datos

- Cifrar información sensible almacenada.
- Proteger respaldos y bases de datos.
- Implementar políticas de retención de información.

### Seguridad de Aplicaciones

- Utilizar consultas parametrizadas y ORM.
- Realizar pruebas periódicas de seguridad.
- Implementar validación de entradas.
- Aplicar controles contra XSS y CSRF.

### Monitoreo y Auditoría

- Registrar eventos de seguridad relevantes.
- Monitorear accesos sospechosos.
- Mantener trazabilidad de cambios en evaluaciones y certificados.

### Integridad Académica

- Implementar controles para reducir el fraude académico.
- Proteger el banco de preguntas.
- Fortalecer los mecanismos de verificación de identidad durante evaluaciones.

---

## 8.5 Riesgos Futuros

Se identifican desafíos emergentes que deberán ser considerados en futuras versiones de la plataforma:

- Uso de herramientas de Inteligencia Artificial durante evaluaciones.
- Amenazas internas por usuarios con acceso legítimo.
- Vulnerabilidades Zero-Day en componentes de terceros.
- Ataques dirigidos contra servicios de autenticación.

Estos riesgos requerirán procesos continuos de monitoreo y mejora.

---

## 8.6 Conclusión Final

La aplicación de metodologías de threat modeling permitió identificar de forma estructurada los principales riesgos de seguridad asociados a una plataforma LMS moderna.

La implementación de los controles propuestos reduce significativamente la superficie de ataque y mejora la protección de los activos críticos del sistema. Sin embargo, la seguridad debe entenderse como un proceso continuo que requiere monitoreo, actualización y revisión permanente.

Se concluye que la plataforma puede operar dentro de niveles aceptables de riesgo siempre que se implementen los controles recomendados y se mantenga una estrategia de mejora continua de la seguridad.