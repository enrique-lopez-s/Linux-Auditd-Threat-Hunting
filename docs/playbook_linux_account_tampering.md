# 📖 SOC Playbook: Modificación de Cuentas en Linux (T1098 / T1078)

## 1. Descripción de la Alerta
* **Nombre:** Modificación o creación no autorizada de usuarios en Linux (`/etc/passwd` / `/etc/shadow`).
* **Severidad:** Alta.
* **Origen de Telemetría:** Auditd (`type=PATH` / `key=identity_changes`).

---

## 2. Flujo de Investigación para el Analista

1. **Identificar el usuario que ejecutó la acción (`auid`):**
   * Verificar en el log de `auditd` el ID de usuario original (`auid`) que realizó la modificación.
2. **Inspeccionar el proceso ejecutante (`exe` / `cmd`):**
   * Analizar si se utilizó `useradd`, `usermod`, `vipw` o si un script malicioso editó directamente el archivo.
3. **Verificar sesión y terminal:**
   * Revisar el origen de la sesión (SSH, consola local, proceso en segundo plano web).

---

## 3. Acciones de Mitigación
* Bloquear inmediatamente la cuenta sospechosa creada o modificada (`usermod -L <usuario>`).
* Revisar y revocar llaves SSH en `/home/<usuario>/.ssh/authorized_keys`.
* Relevar actividad posterior realizada por el ID de usuario afectado.
