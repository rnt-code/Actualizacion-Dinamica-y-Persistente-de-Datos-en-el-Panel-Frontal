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

Nota: Desarrollada en LabVIEW 2024. En la carpeta LV2015 hay una versión para LabVIEW 2015.

![imagen](https://github.com/user-attachments/assets/c978b7b1-ad67-47b7-b424-9d211e61dbab)

![Diagrama de flujo](https://github.com/user-attachments/assets/0ab7f27b-417b-418b-bea2-c7f72bc8565f)

### 📌 Historial de versiones

#### 🟢 Versión 2.1 – Soporte para controles Ring dependientes
En esta versión, se introduce una jerarquía entre los controles Ring. En particular, el control Ring llamado `Type` depende del valor seleccionado en el control `Chassis`.

Esto significa que la lista de opciones disponibles en `Type` se actualiza dinámicamente en tiempo de ejecución, en función del ítem seleccionado en `Chassis`.  
La relación de dependencia se gestiona mediante un nuevo archivo de configuración auxiliar.

### Nuevo archivo: `type by chassis.ini`

Este archivo contiene una sección por cada tipo de chasis definido en `ring options.ini`. Cada sección debe tener como sufijo obligatorio `_TYPE` y enumerar los posibles valores para el control `Type` correspondientes al chasis especificado.

#### Ejemplo:

```ini
[MIDEA_TYPE]
type0 = "AB ROS"
type1 = "INVERTER AG"
type2 = "INVERTER SAMSUNG"

[TCL_TYPE]
type0 = "ON/OFF"
type1 = "INVERTER1"
type2 = "INVERTER2"
```

---

## Nota técnica – Reglas de consistencia entre archivos

Para mantener la integridad del sistema y evitar errores en tiempo de ejecución, es imprescindible respetar las siguientes reglas:

### En el archivo `type by chassis.ini`:

> **⚠️ ATENCIÓN:** Cada vez que se agregue una nueva sección a este archivo:
>
> - El nombre **debe** incluir el sufijo `_TYPE`.
> - Debe haber una entrada correspondiente en la sección `[CHASSIS]` del archivo `ring options.ini`.
> - Por ejemplo: si se crea la sección `[NEW_TYPE]`, entonces debe agregarse en `ring options.ini` la línea `kitN = "NEW"` en `[CHASSIS]`.

### En el archivo `ring options.ini`:

> **⚠️ ADVERTENCIA:** Cada vez que se agregue una nueva entrada en la sección `[CHASSIS]`:
>
> - Debe crearse en `type by chassis.ini` una sección con el nombre correspondiente seguido de `_TYPE`.
> - Por ejemplo: si se agrega `kitN = "NEW"` en `[CHASSIS]`, se debe crear `[NEW_TYPE]` en `type by chassis.ini`.

Ambos archivos deben permanecer sincronizados para asegurar el correcto funcionamiento del control dependiente `Type`.

#### ⚪ Versión 2.0
Esta es la versión 2.0 de la aplicación de persistencia, desarrollada en LabVIEW. Esta versión gestiona el almacenamiento y recuperación de la configuración de controles tipo **Ring** en el Front Panel (FP) de un VI de LabVIEW. La aplicación es compatible con la lectura de archivos `.ini`, los cuales permiten poblar los controles en tiempo de ejecución y almacenar las opciones seleccionadas por el usuario.

## Características principales de la versión 2.0
- **Persistencia de configuraciones:** Los valores seleccionados en los controles **Ring** se almacenan en el archivo `user_ring_data.ini` y se recuperan al reiniciar la aplicación.
- **Verificación de integridad:** Se realiza un control de integridad para verificar que los archivos de configuración (`ring_options.ini` y `user_ring_data.ini`) sean consistentes y estén bien formateados.
- **Interacción en tiempo real:** Cuando el usuario cambia una selección en los controles **Ring**, la aplicación guarda automáticamente el estado del control en el archivo `user_ring_data.ini`.

## Archivos de Configuración

`ring_options.ini`
Este archivo contiene las opciones disponibles para los controles **Ring**. Ejemplo:

```ini
[SEQUENCES]
seq0 = "LOW"
seq1 = "LOW,MID"
seq2 = "LOW,HIGH"
seq3 = "LOW,TURBO"
seq4 = "LOW,MID,HIGH"
seq5 = "LOW,MID,TURBO"
seq6 = "LOW,HIGH,TURBO"
seq7 = "LOW,MID,HIGH,TURBO"

[CHASSIS]
kit0 = "MIDEA"
kit1 = "TCL"

[TYPE]
type0 = "AB ROS"
type1 = "INVERTER AG"
type2 = "INVERTER SAMSUNG"

[CAPACITY]
capacity0 = "9k"
capacity1 = "12k"
capacity2 = "18k"
capacity3 = "24k"
```

`user_ring_data.ini`
Este archivo guarda las selecciones del usuario en los controles Ring. Ejemplo:

```ini
[SEQUENCES]
sequence = "LOW,MID,TURBO"
index = "5"

[CHASSIS]
chassis = "TCL"
index = "1"

[TYPE]
type = "INVERTER SAMSUNG"
index = "2"

[CAPACITY]
capacity = "12k"
index = "1"
```
### Flujo de Funcionamiento

- Lectura de archivos de configuración: Al iniciar la aplicación, se leen los archivos `ring_options.ini` y `user_ring_data.ini` para poblar los controles Ring con las opciones disponibles y las configuraciones previas del usuario.

- Verificación de integridad: Se verifica que los sectores y las etiquetas de ambos archivos coincidan en nombre y cantidad. Si hay discrepancias, se genera un mensaje de error.

- Interacción del usuario: Los controles Ring muestran las opciones al usuario, quien puede cambiar la selección.

- Persistencia de datos: Cuando el usuario cambia un valor en un control Ring, la selección se guarda automáticamente en `user_ring_data.ini` para su futura recuperación.

![imagen](https://github.com/user-attachments/assets/bcfd6256-2a75-48d3-9a55-090f07f966b1)

#### ⚪ Versión 1.1
- Se mejora la implementación del manejo de persistencia de datos en el Front Panel.
- Se reemplazaron los nodos de propiedad (`Property Node – Value`) por los nodos `Set Control Values by Index` y `Get Control Values by Index`.
- Este cambio favorece la escalabilidad, limpieza y mantenibilidad del código, especialmente en aplicaciones con múltiples controles.
- El nuevo enfoque requiere una lógica algo más elaborada para gestionar la lectura y escritura de archivos `.ini`, lo cual demandó mayor dedicación en el diseño del módulo correspondiente. No fue complejo, pero sí requirió cierto grado de astucia.

#### ⚪ Versión 1.0
- Implementación original de esta prueba de concepto.
- Se utilizó la propiedad `Value` mediante nodos de propiedad (`Property Node`) para actualizar los controles del Front Panel.
- El módulo de lectura y escritura del archivo `.ini` fue sencillo de desarrollar, dada la menor complejidad de este enfoque inicial.
- El enfoque actual resulta completamente funcional para aplicaciones pequeñas o con bajo número de controles.
