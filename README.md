# 🐧 Linux Threat Hunting & Telemetry Lab (`auditd` + Splunk)

[![MITRE ATT&CK](https://img.shields.io/badge/ATT%26CK-Linux-red.svg)](https://attack.mitre.org/matrices/enterprise/linux/)
[![Telemetry](https://img.shields.io/badge/Telemetry-auditd-blue.svg)](https://wiki.archlinux.org/title/Audit_framework)
[![SIEM](https://img.shields.io/badge/SIEM-Splunk_Enterprise-green.svg)](https://www.splunk.com/)
[![Rules](https://img.shields.io/badge/Detections-Sigma-orange.svg)](https://github.com/SigmaHQ/sigma)

Proyecto práctico de **Threat Hunting y Detección de Amenazas en entornos Linux**, centrado en la recolección de telemetría del kernel mediante el marco de auditoría `auditd` y la ingesta/análisis de eventos en **Splunk Enterprise**.

---

## 🎯 Objetivos del Proyecto

1. **Configuración de Telemetría de Kernel:** Implementar e invocar reglas de auditoría personalizadas en Linux Mint (`/etc/audit/rules.d/`) para monitorear modificaciones en archivos críticos de autenticación y privilegios.
2. **Ingesta y Normalización:** Configurar el flujo de logs desde `/var/log/audit/audit.log` hacia el SIEM (Splunk Enterprise).
3. **Desarrollo de Consultas Analíticas (SPL):** Crear búsquedas estructuradas en lenguaje SPL para detectar manipulación de credenciales (`/etc/passwd`, `/etc/shadow`) y cambios en permisos (`/etc/sudoers`).
4. **Reglas de Detección Universales:** Mapear los casos de uso a formato **Sigma (YAML)** para compartir lógica analítica agnóstica a la plataforma.
5. **Procedimientos de Respuesta (Playbooks):** Documentar guías operativas paso a paso para la investigación y mitigación en un Centro de Operaciones de Seguridad (SOC).

---

## 🏗️ Arquitectura de la Solución

```text
   [ Sistema Linux Mint ]
             │
   (Subsistema del Kernel)
             │
         [ auditd ] ────► /var/log/audit/audit.log
                                     │
                                (Ingesta)
                                     │
                             [ Splunk SIEM ]
                                     │
                            [ Búsquedas SPL ]


🔍 Cobertura de Técnicas MITRE ATT&CK®

ID Técnica      Nombre de la Técnica                    Evento Registrado                           Regla Auditd (key)
T1098           Account Manipulation                    Edición de /etc/passwd o /etc/shadow        identity_changes
T1548.001       Abuse Elevation Control: Setuid/Setgid  Modificación de /etc/sudoers o uso de sudo  privilege_escalation
T1053.003       Scheduled Task/Job: Cron                Edición o creación de trabajos en crontab   persistence_cron

📊 Visualización de Resultados en Splunk
1. Detección de Modificación de Credenciales (/etc/passwd)

Búsqueda de eventos asociados al monitoreo de archivos de cuentas de usuario:
Splunk SPL

index=main "/etc/passwd"
| table _time, host, type, name, exe, key

2. Detección de Escalación de Privilegios (/etc/sudoers)

Filtrado de eventos de auditoría vinculados a la edición de privilegios del sistema:
Splunk SPL

index=main "/etc/sudoers"
| table _time, host, type, name, exe, key

📁 Estructura del Repositorio
Plaintext

Linux-Auditd-Threat-Hunting/
├── assets/                          # Capturas de pantalla y diagramas
│   ├── linux_dashboard_passwd.png
│   └── linux_dashboard_sudoers.png
├── auditd_rules/                    # Reglas personalizadas de auditoría
│   └── persistence_credentials.rules
├── docs/                            # Playbooks de respuesta para el SOC
│   └── playbook_linux_account_tampering.md
├── queries/                         # Consultas en Splunk Processing Language (SPL)
│   └── linux_detections.spl
├── sigma/                           # Reglas de detección universales en YAML
│   ├── auditd_account_modification.yml
│   └── auditd_sudoers_modification.yml
└── README.md                        # Documentación principal

🚀 Despliegue e Instalación Rápida

    Instalar y activar auditd en Linux Mint / Debian / Ubuntu:
    Bash

    sudo apt update && sudo apt install auditd -y
    sudo systemctl enable --now auditd

    Cargar reglas de auditoría personalizadas:
    Bash

    sudo cp auditd_rules/persistence_credentials.rules /etc/audit/rules.d/
    sudo augenrules --load

    Verificar estado de las reglas activas:
    Bash

    sudo auditctl -l

✍️ Autor

Enrique López — SOC & Threat Hunting Specialist

GitHub
