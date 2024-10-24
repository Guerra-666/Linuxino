

# Linuxino - Configuración Automática de Entorno Arduino en Linux

**Linuxino** es un proyecto cuyo objetivo es simplificar y automatizar la configuración del entorno de desarrollo de **Arduino** en varias distribuciones de **Linux**. Este script gestiona la instalación de dependencias, configuración de permisos y reglas **udev** para facilitar la interacción con las placas Arduino de manera eficiente y sin complicaciones.

## Tabla de Contenidos
- [Descripción](#descripción)
- [Finalidad del Proyecto](#finalidad-del-proyecto)
- [Características](#características)
- [Distribuciones Compatibles](#distribuciones-compatibles)
- [Requisitos](#requisitos)
- [Dependencias](#dependencias)
- [Uso](#uso)
  - [Pasos Previos](#pasos-previos)
  - [Ejecución](#ejecución)
- [Cambios que Realiza el Script](#cambios-que-realiza-el-script)
  - [Instalación de Paquetes](#instalación-de-paquetes)
  - [Acceso al Grupo `dialout`](#acceso-al-grupo-dialout)
  - [Creación de Reglas udev](#creación-de-reglas-udev)
- [Comportamiento del Script](#comportamiento-del-script)
- [Solución de Problemas](#solución-de-problemas)
- [Contribuciones](#contribuciones)
- [Licencia](#licencia)

## Descripción

**Linuxino** automatiza las tareas necesarias para configurar un entorno de desarrollo de Arduino en sistemas Linux:
- Instalación de paquetes y dependencias necesarias.
- Configuración de permisos para el acceso a dispositivos serie.
- Creación de reglas **udev** para el reconocimiento automático de las placas Arduino.

El objetivo principal de este script es eliminar la necesidad de configuraciones manuales, haciendo que cualquier usuario de Linux pueda preparar su sistema para trabajar con Arduino sin dificultad.

## Finalidad del Proyecto

El proyecto **Linuxino** fue creado para solventar los problemas más comunes que enfrentan los usuarios de Linux al configurar el entorno de desarrollo para Arduino, tales como:

- **Compatibilidad de dependencias**: Evitar la búsqueda manual e instalación de paquetes necesarios.
- **Permisos de acceso a dispositivos**: Configura automáticamente los permisos para acceder a dispositivos serie sin intervención manual.
- **Creación de reglas udev**: El sistema reconoce y asigna permisos a las placas Arduino automáticamente cuando se conectan, eliminando la necesidad de cambiar permisos manualmente cada vez.
- **Errores en la instalación**: En caso de que alguna parte del proceso falle, se genera un archivo de texto con los paquetes que deben ser instalados manualmente, proporcionando instrucciones claras al usuario.

Este script pretende reducir la curva de aprendizaje y simplificar el uso de Arduino en Linux, asegurando que los desarrolladores puedan centrarse en la creación de proyectos en lugar de la configuración del entorno.

## Características

- **Multi-distribución**: Compatible con varias distribuciones de Linux, incluyendo Debian, Ubuntu, Arch Linux, Fedora, openSUSE, y más.
- **Automatización completa**: Gestiona la instalación de dependencias, configuración de permisos y creación de reglas de manera automática.
- **Control de errores**: Si algo falla, genera un archivo de texto con una lista de paquetes para instalar manualmente.
- **Mensajes informativos**: Proporciona un feedback claro y en tiempo real durante la ejecución.

## Distribuciones Compatibles

El script **Linuxino** soporta las siguientes distribuciones de Linux:
- **Debian/Ubuntu** y derivados: Utiliza `apt-get`.
- **Arch Linux** y derivados como Manjaro: Utiliza `pacman`.
- **Fedora**: Utiliza `dnf`.
- **openSUSE**: Utiliza `zypper`.

Para otras distribuciones, puede ser necesario instalar las dependencias manualmente o adaptar el script.

## Requisitos

- **Acceso root**: Necesario para instalar paquetes, modificar permisos y crear reglas **udev**.
- **Conexión a Internet**: Requerido para descargar los paquetes necesarios.

## Dependencias

El script instala las siguientes dependencias:
- **gcc-avr**
- **avr-libc**
- **avrdude**
- **arduino-core**
- **arduino** (opcional si se usa otro IDE)

## Uso

### Pasos Previos

1. **Clonar el repositorio**:
   ```bash
   git clone https://github.com/tu-usuario/linuxino.git
   cd linuxino
   ```

2. **Ejecutar con permisos de superusuario**:
   Debes ejecutar el script con `sudo` para que pueda realizar todas las modificaciones necesarias en tu sistema.

### Ejecución

Ejecuta el script con el siguiente comando:

```bash
sudo ./configurar_arduino.sh
```

Este comando inicia el proceso automático para configurar el entorno de desarrollo Arduino en tu sistema Linux.

### Salida Esperada

El script mostrará mensajes claros sobre cada tarea que está realizando, como:

- Detectando la distribución de Linux.
- Instalando dependencias.
- Configurando permisos de acceso a dispositivos.
- Creando reglas **udev**.

Si ocurre un error, el script generará un archivo `linuxino-errors.txt` con instrucciones sobre cómo instalar los paquetes manualmente.

## Cambios que Realiza el Script

### Instalación de Paquetes

Dependiendo de la distribución, **Linuxino** utiliza el gestor de paquetes correspondiente para instalar los siguientes paquetes:

- **Debian/Ubuntu**:
  ```bash
  apt-get update && apt-get install -y gcc-avr avr-libc avrdude arduino arduino-core
  ```
- **Arch Linux**:
  ```bash
  pacman -Sy --noconfirm avr-gcc avr-libc avrdude arduino
  ```
- **Fedora**:
  ```bash
  dnf install -y avr-gcc avr-libc avrdude arduino
  ```
- **openSUSE**:
  ```bash
  zypper refresh && zypper install -y gcc-avr avr-libc avrdude arduino
  ```

### Acceso al Grupo `dialout`

El script añade al usuario actual al grupo `dialout`, permitiendo la comunicación con dispositivos serie sin necesidad de ajustes manuales:

```bash
usermod -aG dialout "$SUDO_USER"
```

### Creación de Reglas udev

Crea reglas **udev** para que las placas Arduino sean reconocidas automáticamente al conectarse al puerto USB:

```bash
# Arduino Uno
SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0043", MODE="0666", GROUP="dialout"
```

## Comportamiento del Script

Durante su ejecución, **Linuxino**:
- Detecta la distribución y utiliza el gestor de paquetes adecuado.
- Añade al usuario al grupo `dialout`.
- Crea reglas **udev** para dispositivos Arduino.

## Solución de Problemas

Si el script falla, verifica lo siguiente:
- Revisa el archivo `linuxino-errors.txt` para detalles sobre paquetes que podrían requerir instalación manual.
- Asegúrate de haber cerrado sesión y vuelto a iniciarla para que los cambios en los grupos surtan efecto.

## Contribuciones

Las contribuciones son bienvenidas. Haz un **fork** del proyecto y envía un **pull request** si deseas mejorar el script.

## Licencia

Este proyecto está licenciado bajo la [MIT License](https://opensource.org/licenses/MIT) - consulta el archivo LICENSE para más detalles.

--- 

Este README incluye todos los detalles mencionados y ajusta la información para adaptarse al proyecto **Linuxino**.