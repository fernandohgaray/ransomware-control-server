
# 📄 Documentación Técnica: `Encoder.py`

## 📌 Descripción del código original

- **Fuente:** Proyecto [RansomwareSim](https://denizhalil.com/ransomwaresim-ransomware-simulator/) desarrollado por Halil Deniz como una herramienta de simulación de ransomware con fines educativos.
- **Propósito:** Este archivo simula el comportamiento de un malware tipo ransomware que cifra archivos en el sistema de la víctima, recolecta información del sistema, cambia el fondo de pantalla y envía datos al servidor de control.

---

## ⚙️ Funciones principales

### 🔐 Cifrado
- Se utiliza la librería `cryptography.fernet` para cifrar archivos con extensiones específicas (`.txt`, `.docx`, `.jpg`).
- El archivo original es eliminado y reemplazado por una versión cifrada con la extensión `.denizhalil`.

### 📡 Comunicación con el servidor
- Se recolectan datos como hostname, usuarios activos, MAC y la clave de cifrado generada.
- Estos datos se envían a un servidor de control usando sockets TCP.

### 🎨 Cambio de fondo de pantalla
- En Windows, cambia el fondo de pantalla del sistema por una imagen específica.

### 📝 Mensaje de advertencia
- Crea un archivo `Readme.txt` en el escritorio con un mensaje informativo sobre el cifrado.

### ♻️ Limpieza de memoria
- Ejecuta una recolección de basura para liberar memoria usada.

---

## 📌 Fragmentos clave del código Python comentados

```python
self.key = Fernet.generate_key()
```
> Se genera una clave simétrica que será utilizada para cifrar los archivos.

```python
def encrypt_file(self, file_path):
    ...
    os.remove(file_path)
```
> Cifra el archivo y elimina el original, simulando el comportamiento destructivo de un ransomware.

```python
def find_and_encrypt_files(self):
    ...
    if any(file.endswith(ext) for ext in self.file_extensions):
```
> Busca archivos con extensiones específicas en la carpeta objetivo y aplica cifrado.

```python
def send_data_to_server(self):
    ...
    self.send_to_server(json.dumps(data))
```
> Empaqueta la información en formato JSON y la envía al servidor de control.

```python
def change_wallpaper(self, image_path):
    ...
    ctypes.windll.user32.SystemParametersInfoW(20, 0, image_path , 0)
```
> Cambia el fondo de pantalla usando una llamada nativa de Windows.

```python
def create_readme(self):
    ...
    file.write("This is a simulation program, your files are encrypted.")
```
> Deja un mensaje en el escritorio simulando un aviso típico de ransomware.

---

## 🚨 Patrones de comportamiento malicioso identificados en Python

| Comportamiento                       | Descripción                                                               |
|-------------------------------------|---------------------------------------------------------------------------|
| **Cifrado de archivos**             | Reemplaza archivos originales por versiones cifradas.                     |
| **Eliminación de archivos**         | Borra los archivos originales tras el cifrado.                            |
| **Recolección de información**      | Obtiene hostname, usuarios y MAC address.                                 |
| **Exfiltración de datos**           | Envía datos sensibles al servidor de control.                             |
| **Modificación del sistema**        | Cambia el fondo de pantalla del usuario.                                  |
| **Notificación a la víctima**       | Deja un archivo de texto con un mensaje de advertencia.                   |
| **Persistencia de clave en RAM**    | Guarda la clave generada solo en memoria hasta que se envía.              |

---

## 🧠 Observaciones para la investigación

- La lógica de cifrado es simple y replicable en otros lenguajes como C.
- La biblioteca `Fernet` garantiza confidencialidad, pero el esquema de distribución de la clave es un punto débil (clave enviada en texto plano).
- El cambio de wallpaper es un marcador clásico de ransomware real, aunque limitado a Windows.
- La estructura modular del código facilita el análisis de cada componente por separado.
- La observación del código compilado y descompilado permitirá identificar patrones como:
  - Operaciones de E/S (lectura, escritura, eliminación de archivos).
  - Uso de sockets (`connect`, `send`).
  - Llamadas a funciones nativas del sistema operativo (en Windows).

---
