# 5. MAPA MITRE ATT&CK

## 5.1 Objetivo

Las amenazas identificadas mediante STRIDE fueron mapeadas contra la matriz MITRE ATT&CK con el objetivo de relacionarlas con tácticas y técnicas observadas en ataques reales.

Este análisis permite comprender mejor el comportamiento de potenciales atacantes y facilita la selección de controles de seguridad adecuados.

## 5.2 Mapeo de amenazas

| Amenaza                                      | Técnica ATT&CK                        | ID ATT&CK | Táctica              |
| -------------------------------------------- | ------------------------------------- | --------- | -------------------- |
| TH05 – Uso de credenciales robadas           | Valid Accounts                        | T1078     | Initial Access       |
| TH08 – Acceso indebido a exámenes            | Exploitation for Privilege Escalation | T1068     | Privilege Escalation |
| TH10 – Escalación de privilegios             | Abuse Elevation Control Mechanism     | T1548     | Privilege Escalation |
| TH11 – SQL Injection                         | Exploit Public-Facing Application     | T1190     | Initial Access       |
| TH15 – Alteración de certificados            | Data Manipulation                     | T1565     | Impact               |
| TH20 – Filtración banco de preguntas         | Data from Information Repositories    | T1213     | Collection           |
| TH23 – Descarga masiva de contenido          | Automated Collection                  | T1119     | Collection           |
| TH24 – Suplantación de identidad en exámenes | Valid Accounts                        | T1078     | Defense Evasion      |

## 5.3 Análisis

El mapeo realizado muestra que la mayoría de las amenazas identificadas se concentran en las tácticas de:

* Initial Access
* Privilege Escalation
* Collection
* Impact

Las técnicas relacionadas con el uso de cuentas válidas (T1078) representan un riesgo importante para la plataforma debido a la existencia de múltiples perfiles con distintos niveles de privilegios.

Asimismo, la técnica T1190 (Exploit Public-Facing Application) resulta especialmente relevante dado que la plataforma LMS expone servicios web accesibles desde Internet.

Las técnicas de recolección de información (Collection) son particularmente críticas debido a la existencia de activos académicos de alto valor, como bancos de preguntas, evaluaciones y material educativo.
