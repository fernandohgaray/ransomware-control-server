# 📄 Documentación Técnica: `ControlServer.py`

## 📌 Descripción del código original

- **Fuente:** Proyecto [RansomwareSim](https://denizhalil.com/ransomwaresim-ransomware-simulator/) desarrollado por Halil Deniz como simulador educativo para comprender el comportamiento de un ransomware a alto nivel.
- **Propósito:** Este archivo implementa un servidor de control que recibe datos exfiltrados desde la máquina víctima o solicitudes de clave para descifrado. Simula el rol del *Command & Control (C2)* en un ataque real de ransomware, escuchando conexiones y respondiendo con claves de descifrado si se solicita.

---

## ⚙️ Funciones principales

### 🔐 Cifrado
Aunque este archivo **no realiza cifrado directamente**, **interactúa con la clave de cifrado generada por el malware** (ver `Encoder.py`). El servidor:
- Recibe solicitudes con datos de la víctima (hostname, usuarios activos, MAC, clave de cifrado).
- Puede responder con la clave si se le solicita explícitamente (`request: "key"`).

### 📡 Propagación
El archivo **no incluye mecanismos de propagación**, pero **facilita el control remoto** del ransomware al recibir conexiones múltiples desde sistemas infectados.

### ♻️ Persistencia
No implementa técnicas de persistencia en el sistema. Su función se limita a **mantener una sesión de escucha continua**, lo cual simula la disponibilidad de un servidor C2 real.

---

## 📌 Fragmentos clave del código Python comentados

```python
class ControlServer:
    def __init__(self, host, port, log_file):
        ...
```
> Inicializa el servidor con dirección IP, puerto y archivo de log.  

```python
def handle_client(self, connection, address):
```
> Maneja cada conexión entrante desde una máquina infectada.

```python
message = json.loads(data.decode())
```
> Decodifica el mensaje recibido. Se espera que esté en formato JSON.

```python
if 'request' in message and message['request'] == 'key':
    ...
    key = input("Please enter the encryption key: ")
```
> Si la víctima solicita la clave, el servidor la ingresa manualmente. Esto **simula la respuesta de un atacante** tras un pago.

```python
self.server.listen(5)
```
> El servidor puede aceptar hasta 5 conexiones simultáneas en espera, permitiendo el control de múltiples víctimas.

```python
client_thread = threading.Thread(target=self.handle_client, args=(connection, address))
client_thread.start()
```
> Cada conexión se maneja en un **hilo separado**, simulando la infraestructura de servidores reales de ransomware.

---

## 🚨 Patrones de comportamiento malicioso identificados en Python

| Comportamiento                            | Descripción                                                                 |
|------------------------------------------|-----------------------------------------------------------------------------|
| **Infraestructura de C2**                 | Crea un servidor que actúa como centro de comando y control.               |
| **Recepción de claves de cifrado**       | Recibe claves desde las víctimas, lo que simula exfiltración de datos.     |
| **Atención a múltiples víctimas**        | Usa hilos para manejar múltiples conexiones, como un servidor real.        |
| **Interacción humana para descifrado**   | Solicita manualmente la clave, replicando la práctica de liberar claves tras el pago. |
| **Persistencia del servidor**            | Permanece activo indefinidamente hasta recibir una interrupción (Ctrl+C).  |
| **Registro de actividad**                | Guarda logs que pueden ser utilizados por el atacante para hacer seguimiento. |

---

## 🧠 Observaciones para la investigación

- Aunque no se trata de malware real, el código reproduce fielmente la lógica básica de un C2 en un entorno de ransomware.
- Es útil para observar cómo podrían comportarse las comunicaciones en red, el manejo de claves, y la lógica multicliente.
- En fases posteriores del proyecto, la **traducción a C** y la **decompilación a ensamblador** permitirán observar patrones como:
  - Instrucciones relacionadas con `socket()`, `bind()`, `recv()`, `send()`
  - Llamadas al sistema relacionadas con hilos (`pthread_create`, etc.)
  - Manipulación de cadenas y estructuras de datos para interpretar JSON o datos de clave.

---
