 🛡️ Servidor de Control y Comunicación - Ransomware

Este repositorio contiene una simulación del componente **servidor de control (C2)** de un ransomware, como parte del proyecto académico [Ransomware](https://github.com/SEICUdeA/Ransomware), del Semillero de Investigación en Ciberseguridad de la Universidad de Antioquia, que a su vez se basa en el proyecto (tambíen con fines académicos): [RansomwareSim](https://denizhalil.com/ransomwaresim-ransomware-simulator/) desarrollado por Halil Deniz. El archivo `ControlServer.py` actúa como un centro de comando desde el cual se reciben datos de los equipos infectados y se puede enviar una clave de descifrado simulada.

> ⚠️ **Advertencia:** Este proyecto es estrictamente educativo. No debe utilizarse con fines maliciosos ni fuera de entornos controlados.

---

## 📄 Descripción del archivo `ControlServer.py`

El servidor escucha conexiones entrantes desde los clientes (simulados como dispositivos infectados) y maneja los siguientes comportamientos:

- Recibe y registra información enviada por las víctimas.
- Puede responder con una clave de descifrado si se solicita.
- Registra los eventos en un archivo de log (`server_log.txt`).
- Soporta múltiples conexiones simultáneas mediante el uso de hilos.

---

## 🧩 Requisitos

- Python 3.6 o superior
- Paquetes: `colorama`

Instalación de dependencias:
```bash
pip install colorama
```

---

## 🚀 Ejecución del servidor

```bash
python ControlServer.py
```

Por defecto, el servidor escucha en el puerto `12345` y en todas las interfaces (`0.0.0.0`).

---

## 📚 Documentación detallada

Puedes encontrar una descripción técnica completa del archivo `ControlServer.py` en el siguiente documento:

👉 [`ControlServer_documentacion.md`](./ControlServer_documentacion.md)

Incluye análisis de:

- Propósito y estructura del código.
- Fragmentos comentados.
- Patrones de comportamiento malicioso.
- Observaciones para su análisis y traducción a C.

---

## ⚖️ Licencia

Este proyecto se distribuye únicamente con fines educativos y de investigación. El uso no autorizado o malicioso está estrictamente prohibido.

