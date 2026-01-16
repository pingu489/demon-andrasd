# 👹 Andrasd - Demonio de Integridad de Archivos Críticos

**Andrasd** es un pequeño demonio de **integridad de archivos** diseñado para sistemas Linux.  
Su objetivo es **vigilar archivos críticos del sistema** y notificar cualquier cambio sospechoso, ayudando a detectar posibles intrusiones o modificaciones no autorizadas.

---

## 📝 Archivos que vigila

Actualmente, andrasd monitoriza:

- `/etc/passwd` 🧑‍💻 - Información de usuarios del sistema  
- `/etc/shadow` 🔒 - Hashes de contraseñas  
- `/etc/sudoers` ⚡ - Permisos de usuarios con privilegios

> ⚠️ Si cualquiera de estos archivos cambia, se considera un evento crítico.

---

## ⚙️ Funcionamiento

1. **Baseline de hashes** 📊  
   - Al arrancar, andrasd calcula el **hash SHA256** de cada archivo crítico y los guarda en un archivo seguro (`/var/tmp/andrasd_hashes.txt`).

2. **Vigilancia constante** 👀  
   - Cada cierto intervalo (configurable, por defecto 10 segundos) vuelve a calcular los hashes y los compara con los originales.

3. **Alertas** 🚨  
   - Si detecta un cambio, genera un **log de alerta** (`/var/log/andrasd.log`) indicando el archivo modificado y la fecha.
   - Puede ampliarse para enviar alertas por **email** o **Telegram**.

4. **Protección** 🛡️  
   - Los hashes se actualizan automáticamente después de detectar cambios legítimos, permitiendo que la vigilancia continúe sin interrupciones.

---

## 💻 Requisitos

- Linux  
- Bash ≥ 4.0  
- Permisos de lectura en los archivos críticos  

Opcional:

- `mailutils` para alertas por email  
- `curl` para alertas por Telegram

---

## 🚀 Uso

```bash
# Dar permisos de ejecución
chmod +x andrasd.sh

# Ejecutar el demonio
./andrasd.sh

# Matar el demonio
pkill -f andrasd

