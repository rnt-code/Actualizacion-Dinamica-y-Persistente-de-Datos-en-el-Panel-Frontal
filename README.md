# Prueba de concepto: Actualización Dinámica y Persistente de Datos en el Panel Frontal en LabVIEW

## 📄 Descripción del proyecto

Esta aplicación fue desarrollada como una **prueba de concepto** en **LabVIEW**, con el objetivo de abordar una necesidad frecuente en aplicaciones reales: la **persistencia de datos de configuración**.

En muchos desarrollos, es común que ciertos valores deban ser modificados por el usuario durante la ejecución (parámetros de funcionamiento, ajustes, configuraciones, datos personalizados, etc.). Sin embargo, es fundamental que estos valores **no se pierdan al cerrar la aplicación**, sino que se conserven y estén disponibles la próxima vez que se ejecute.

Esta prueba de concepto implementa una solución sencilla y eficaz para este propósito: los datos de configuración se almacenan automáticamente en un archivo **`.ini`**, permitiendo que la aplicación los recupere al iniciarse.

De esta manera, cualquier modificación realizada por el usuario queda registrada de forma permanente, incluso después de cerrar y volver a abrir la aplicación.

Además, el desarrollo está estructurado en base al patrón de diseño **QMH (Queued Message Handler)**, ampliamente utilizado en aplicaciones escalables y modulares en LabVIEW.

## 💡 Objetivo

Demostrar cómo se puede implementar un mecanismo básico de **guardado y recuperación automática de configuraciones** en una aplicación LabVIEW, utilizando archivos `.ini` como medio de almacenamiento persistente.

Este ejemplo puede servir como base para desarrollos más complejos que requieran conservar configuraciones entre sesiones.

## 🖥️ Características adicionales

El **Front Panel (FP)** de la aplicación incluye, además, una característica útil para ilustrar el funcionamiento del sistema de configuración:

- Se muestran los **nombres de los sectores** (por ejemplo: `[SECTOR1]`) y los **nombres de las llaves** (`key_name=value`) tal como están definidos en el archivo `.ini`.
- Esta información no tendría utilidad directa en una aplicación real, pero en este caso cumple un fin didáctico, ya que permite **visualizar de forma clara la relación entre el archivo `.ini` y la interfaz gráfica**.

Adicionalmente, se ha incorporado un botón denominado **`Update`**, cuya función es **refrescar los valores mostrados en el FP en tiempo de ejecución**. Esto permite que, si el usuario edita manualmente el archivo `.ini` durante la ejecución de la aplicación (por ejemplo, cambiando el nombre de un sector o de una clave, y guardando esos cambios), al presionar el botón **`Update`**, la aplicación **vuelva a leer el archivo actualizado** y **refleje los nuevos valores en pantalla**.

## 📂 Estructura del archivo `.ini`

El archivo `.ini` generado contiene los valores de configuración organizados en secciones y claves, con el formato típico de este tipo de archivos:

```ini
[SEQUENCE]
data = "first"
ID = "TYU45"
capacity = "6789"

[CHASSIS]
name = "midea"
number = "FG56"

[TYPE]
name = "outdoor"
ID = "GH6789"
```

## ▶️ Comportamiento de la aplicación

- **Al iniciar la aplicación:**
  - Se carga el archivo `.ini` (si existe) y se actualizan los valores de configuración.
  - Si el archivo no existe, se emitirá el error correspondiente.

- **Durante la ejecución:**
  - El usuario puede modificar los valores de las llaves desde el Front Panel (FP).
  - También puede editar directamente el archivo `.ini` (manteniendo el número de sectores y llaves), y actualizar los cambios en el FP utilizando el botón **`Update`**.

- **Al cerrar la aplicación:**
  - Los valores actuales se guardan automáticamente en el archivo `.ini` para ser reutilizados en la siguiente ejecución.

---

## 📌 Nota

Este proyecto tiene únicamente fines demostrativos y no está orientado al uso productivo. Su finalidad es ilustrar el concepto de **persistencia de configuraciones** en aplicaciones LabVIEW, y cómo estas configuraciones pueden mantenerse y actualizarse durante el ciclo de vida de la aplicación.

Además, sirve como ejemplo básico del uso del patrón de diseño **QMH (Queued Message Handler)** en el desarrollo de aplicaciones LabVIEW estructuradas, modulares y mantenibles.

Nota: Desarrollada en LV2024. En la carpeta LV2015 hay una versión para LabVIEW 2015.

![imagen](https://github.com/user-attachments/assets/c978b7b1-ad67-47b7-b424-9d211e61dbab)
