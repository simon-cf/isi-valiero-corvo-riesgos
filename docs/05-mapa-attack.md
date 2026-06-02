# 5. MAPA MITRE ATT&CK

## 5.1 Objetivo

Las amenazas identificadas mediante STRIDE fueron mapeadas contra la matriz MITRE ATT&CK Enterprise con el objetivo de relacionarlas con tácticas y técnicas observadas en ataques reales.

Este análisis permite comprender mejor el comportamiento de potenciales atacantes y facilita la selección de controles de seguridad adecuados para cada fase del ataque.

---

## 5.2 Mapeo de Amenazas

| Amenaza                                        | Técnica ATT&CK                          | ID ATT&CK   | Subtécnica                  | Táctica              | Mitigación ATT&CK                       |
| ---------------------------------------------- | --------------------------------------- | ----------- | --------------------------- | -------------------- | --------------------------------------- |
| TH05 – Uso de credenciales robadas             | Valid Accounts                          | T1078       | T1078.003 (Local Accounts)  | Initial Access       | M1032 – Multi-factor Authentication     |
| TH08 – Acceso indebido a exámenes              | Exploit Public-Facing Application       | T1190       | —                           | Initial Access       | M1050 – Exploit Protection; M1051 Update|
| TH10 – Escalación de privilegios               | Abuse Elevation Control Mechanism       | T1548       | —                           | Privilege Escalation | M1026 – Privileged Account Management  |
| TH11 – SQL Injection                           | Exploit Public-Facing Application       | T1190       | —                           | Initial Access       | M1050 – Exploit Protection; WAF         |
| TH15 – Alteración de certificados             | Data Manipulation                       | T1565       | T1565.001 (Stored Data)     | Impact               | M1041 – Encrypt Sensitive Information   |
| TH16 – Emisión fraudulenta de certificados    | Valid Accounts                          | T1078       | T1078.003                   | Initial Access       | M1032 – MFA; M1026 – Least Privilege   |
| TH17 – IDOR en certificados                   | Exploit Public-Facing Application       | T1190       | —                           | Initial Access       | M1018 – User Account Management        |
| TH18 – Stored XSS contenido malicioso        | Drive-by Compromise                     | T1189       | —                           | Initial Access       | M1021 – Restrict Web Content; CSP      |
| TH19 – Stored XSS robo de sesión             | Steal Web Session Cookie                | T1539       | —                           | Credential Access    | M1054 – Software Configuration (CSP); httpOnly |
| TH20 – Filtración banco de preguntas          | Data from Information Repositories     | T1213       | —                           | Collection           | M1018 – User Account Management        |
| TH22 – Modificación de notas                  | Data Manipulation                       | T1565       | T1565.001 (Stored Data)     | Impact               | M1041 – Encrypt; Audit Logs            |
| TH23 – Descarga masiva de contenido           | Automated Collection                    | T1119       | —                           | Collection           | M1018 – Rate Limiting; Signed URLs      |
| TH24 – Suplantación en exámenes               | Valid Accounts                          | T1078       | T1078.003                   | Initial Access       | M1032 – Multi-factor Authentication     |
| TH25 – Uso de IA en evaluaciones             | Valid Accounts + Proxy                  | T1078       | —                           | Defense Evasion      | Controles de proceso; detección de comportamiento |
| TH26 – CSRF en endpoints                      | Exploitation for Client Execution       | T1203       | —                           | Execution            | M1021 – CSRF tokens; SameSite cookies  |
| TH27 – Mass assignment                         | Exploit Public-Facing Application       | T1190       | —                           | Privilege Escalation | M1026 – Least Privilege; Input Validation |


---

## 5.3 Análisis por Táctica

### Tácticas dominantes identificadas

| Táctica              | Amenazas relacionadas              | Relevancia para el LMS                                              |
| -------------------- | ---------------------------------- | ------------------------------------------------------------------- |
| **Initial Access**   | TH05, TH08, TH11, TH16, TH17, TH24 | La plataforma expone servicios web públicos; alto volumen de cuentas de usuarios |
| **Collection**       | TH20, TH23                         | Activos académicos y de contenido con alto valor; banco de preguntas y videos |
| **Credential Access**| TH19                               | Sesiones activas de múltiples usuarios simultáneos durante exámenes |
| **Impact**           | TH15, TH22                         | Modificación de certificados y notas afecta activos de valor legal  |
| **Privilege Escalation** | TH10, TH27                     | Múltiples roles con diferentes niveles de privilegio en la misma plataforma |

### Técnicas más relevantes para el LMS

**T1078 – Valid Accounts** es la técnica de mayor riesgo para la plataforma. La existencia de múltiples roles con distintos privilegios (estudiante, docente, administrador) hace que el compromiso de credenciales sea un vector de ataque primario. El uso de MFA es la mitigación más impactante.

**T1190 – Exploit Public-Facing Application** cubre múltiples amenazas (SQLi, IDOR, mass assignment, acceso indebido a exámenes). La superficie de ataque es amplia dado que la API REST es accesible desde Internet.

**T1213 / T1119 – Collection** es especialmente crítica dado que la plataforma almacena activos académicos de alto valor (banco de preguntas, material educativo propietario) que tienen valor tanto para la competencia como para estudiantes que buscan ventajas.

---

