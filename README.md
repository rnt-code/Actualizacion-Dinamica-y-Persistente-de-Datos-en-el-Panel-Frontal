# Dynamic Update Front Panel Data

# Prueba de concepto: Persistencia de datos de configuración en LabVIEW

## 📄 Descripción del proyecto

Esta aplicación fue desarrollada como una **prueba de concepto** en **LabVIEW**, con el objetivo de abordar una necesidad frecuente en aplicaciones reales: la **persistencia de datos de configuración**.

En muchos desarrollos, es común que ciertos valores deban ser modificados por el usuario durante la ejecución (parámetros de funcionamiento, ajustes, configuraciones, datos personalizados, etc.). Sin embargo, es fundamental que estos valores **no se pierdan al cerrar la aplicación**, sino que se conserven y estén disponibles la próxima vez que se ejecute.

Esta prueba de concepto implementa una solución sencilla y eficaz para este propósito: los datos de configuración se almacenan automáticamente en un archivo **`.ini`**, permitiendo que la aplicación los recupere al iniciarse.

De esta manera, cualquier modificación realizada por el usuario queda registrada de forma permanente, incluso después de cerrar y volver a abrir la aplicación.

## 💡 Objetivo

Demostrar cómo se puede implementar un mecanismo básico de **guardado y recuperación automática de configuraciones** en una aplicación LabVIEW, utilizando archivos `.ini` como medio de almacenamiento persistente.

Este ejemplo puede servir como base para desarrollos más complejos que requieran conservar configuraciones entre sesiones.

## 📂 Estructura del archivo `.ini`

El archivo `.ini` generado contiene los valores de configuración organizados en secciones y claves, con el formato típico de este tipo de archivos:

```ini
[ConfiguracionGeneral]
Parametro1=Valor1
Parametro2=Valor2
...


![imagen](https://github.com/user-attachments/assets/c978b7b1-ad67-47b7-b424-9d211e61dbab)
