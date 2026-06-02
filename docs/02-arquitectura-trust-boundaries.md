# 2. ARQUITECTURA DEL SISTEMA

## 2.1 Diagrama de Arquitectura

```text
                ┌─────────────────┐
                │   Estudiante    │
                └────────┬────────┘
                         │
                ┌────────▼────────┐
                │    Docente      │
                └────────┬────────┘
                         │
                ┌────────▼────────┐
                │ Administrador   │
                └────────┬────────┘

═══════════════════════════════════════
 TRUST BOUNDARY TB-01
 Usuarios ↔ Aplicación
═══════════════════════════════════════

                         │ HTTPS
                         ▼

                ┌─────────────────┐
                │ Frontend Vue.js │
                └────────┬────────┘

═══════════════════════════════════════
 TRUST BOUNDARY TB-02
 Frontend ↔ Backend
═══════════════════════════════════════

                         │ REST API
                         ▼

                ┌─────────────────┐
                │ Django Backend  │
                │                 │
                │ - Auth Service  │
                │ - Courses       │
                │ - Exams         │
                │ - Forums        │
                │ - Certificates  │
                └───┬─────┬───────┘
                    │     │
                    │     │
                    │     ▼

═══════════════════════════════════════
 TRUST BOUNDARY TB-03
 Servicios Externos
═══════════════════════════════════════

              ┌───────────────┐
              │ API Certificados │
              └───────────────┘

              ┌───────────────┐
              │ CDN Videos    │
              └───────────────┘

═══════════════════════════════════════
 TRUST BOUNDARY TB-04
 Datos Persistentes
═══════════════════════════════════════

                     ▼

              ┌───────────────┐
              │ PostgreSQL    │
              └───────────────┘
```

## 2.2 Flujo de Datos

### Realización de un examen

Estudiante → Frontend → Backend → PostgreSQL

### Visualización de contenido

Estudiante → Frontend → CDN

### Emisión de certificado

Docente → Backend → API Certificados → Estudiante

## 2.3 Actores del Sistema

| Actor            | Descripción                           | Privilegios |
| ---------------- | ------------------------------------- | ----------- |
| Estudiante       | Consume cursos y realiza evaluaciones | Limitados   |
| Docente          | Gestiona cursos y exámenes            | Elevados    |
| Administrador    | Administra la plataforma              | Completos   |
| Atacante Externo | Actor malicioso                       | Ninguno     |


## 2.4 Trust Boundaries

### TB-01 Usuarios ↔ Aplicación

Representa el ingreso de tráfico desde Internet. Los usuarios no son considerados confiables.

### TB-02 Frontend ↔ Backend

Representa la comunicación entre cliente y servidor. Deben aplicarse controles de autenticación y autorización.

### TB-03 Backend ↔ Servicios Externos

Corresponde a la interacción con sistemas fuera del control directo de la organización.

### TB-04 Backend ↔ Base de Datos

Contiene los activos más sensibles de la plataforma y requiere controles estrictos de acceso.

## 2.5 Supuestos funcionales
* La plataforma ofrece cursos online.
* Los usuarios pueden registrarse y autenticarse.
* Existen roles de Estudiante, Docente y Administrador.
* Se emiten certificados de finalización.
* Los cursos contienen videos y documentos.
* Existen foros de discusión.
