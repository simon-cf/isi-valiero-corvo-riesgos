# Plataforma de Educación Online (LMS)

## Integrantes

* Nazarena Valiero
* Simón Corvo

## Descripción del Proyecto

El presente trabajo consiste en el análisis de riesgos y modelado de amenazas de una plataforma de educación online (Learning Management System - LMS).

La plataforma permite a estudiantes acceder a cursos virtuales, visualizar contenido multimedia, participar en foros de discusión, realizar evaluaciones en línea y obtener certificados digitales al completar satisfactoriamente los cursos.

Los docentes pueden crear y administrar cursos, gestionar evaluaciones y realizar seguimiento del desempeño académico de los estudiantes. Los administradores gestionan la configuración general de la plataforma y el control de usuarios.



## Metodologías Utilizadas

* STRIDE
* DREAD
* MITRE ATT&CK
* NIST SP 800-30
* NIST SP 800-53

## Alcance del Análisis
* Componentes incluidos
* Frontend desarrollado en Vue.js
* Backend desarrollado en Python/Django
* Base de datos PostgreSQL
* Sistema de autenticación y autorización
* Foros de discusión
* CDN para distribución de contenido multimedia
* API externa para emisión de certificados digitales

## Supuestos
* Todas las comunicaciones utilizan HTTPS/TLS.
* Los usuarios acceden mediante credenciales personales.
* El backend implementa autenticación basada en sesiones o tokens.
* Los servicios externos son considerados confiables, aunque sus integraciones serán analizadas.
* La plataforma almacena información académica y datos personales de estudiantes y docentes.
* Existen roles diferenciados para estudiantes, docentes y administradores.


## Herramientas de IA Utilizadas

* ChatGPT (GPT-5.5) para asistencia en generación de documentación, identificación de amenazas y elaboración de controles de seguridad.
