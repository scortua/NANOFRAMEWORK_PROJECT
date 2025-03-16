# ![Escuela](./Imagenes/Logotipo-Escuela-Colombiana-de-Ingeniería-Julio-Garavito-5.png)PROYECTO FRAMEWORK .NET

[![Visual Studio Community 2022](https://img.shields.io/badge/Visual%20Studio%20Community%202022-5C2D91?style=for-the-badge&logo=visual-studio&logoColor=white)](https://visualstudio.microsoft.com/es/vs/community/) [![C# .NET](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)](https://docs.microsoft.com/en-us/dotnet/csharp/) [![.NET ](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=.net&logoColor=white)](https://github.com/nanoframework)[![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/en/products/socs/esp32)
<div align="center">

<a href="https://github.com/nanoframework" style="background: linear-gradient(to right, #4B0082, #800080); color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px;">Nanoframework GitHub</a> | <a href="https://nanoframework.net/" style="background: linear-gradient(to right, #006400, #228B22); color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px;">Nanoframework Web</a>

</div>

# Indice
- [Firmware](#firmware)
- [.NET nanoframework](#net-nanoframework)
- [Visualización en pantalla](#visualización-en-pantalla)
    - [LVGL](#lvgl)
    - [SIMPLE WPF](#simple-wpf)
    - [UGUI](#ugui)
- [Entregable](#entregable)
- [Desarrollo](#desarrollo)
    - [Develop Board](#develop-board)
    - [Code](#code)
        - [Connection](#connection)

## .NET nanoframework
Todo lo que sigue viene de **[.NET nanoframework](https://github.com/nanoframework)**.
Ejemplos de código para uso estan en [samples](https://github.com/nanoframework/Samples).

### Firmware
Para descargar e implementar nanoframework en el sistema embebido que deseamos, se debe primero instalar [vscode](https://code.visualstudio.com/) o [vscode community](https://visualstudio.microsoft.com/es/vs/community/).
Se requiere descargar estos IDE y en sus extensiones descargar .NET nanoframework.
Si se tiene la versión vscode community se tiene que generar un código sin código seleccionado e ir a __Manage Extension__ y descargar NANOFRAMEWORK.

<details>
<summary>step to step</summary>

Ahora, se requiere nanoff y se puede seguir el paso a paso en [nanoframework](https://github.com/nanoframework/nanoFirmwareFlasher). Ahora en el terminal como admin.
Primero se instala .NET o dotnet y si ya esta instalado verificar con:

> dotnet --version

Ahora, descargamos nanoff.

> dotnet tool install -g nanoff

Si ya lo esta entonces actualize.

> dotnet tool update -g nanoff

Ya instalado se pueden ver sus comandos con:

> nanoff --help

Con ello hecho, sigue el análisis del puerto en que este conectado nuestro sistema embebido. Por ahora ESP32-REV3.

> nanoff --listports

Con este comando se da una dirección COM del sistema.
Con el siguiente comando se verifican los sistemas que hay para actualizar.

> nano --listtargets --platform esp32

Con ello se verifica que **ESP** es y ahora actualizar el firmware de nuestra tarjeta.

Si se sabe que tarjeta es:

> nanoff --update --target 'VERSION' --serialport COMX

Si no se sabe que tarjeta es:

> nanoff --platform esp32 --serialport COMX --update

Con ello ya hecho, se utiliza en esto [vscode_com](https://visualstudio.microsoft.com/es/vs/community/) y se crea un proyecto vacio en nanoframework y se siguen los pasos:
1. Ver > Otras ventanas > Device Explorer
2. Click en la lupa y buscara el dispositivo con la salida del terminal en nanoframework se puede ver el proceso de doble verificación
3. Ahora darle click al dispositivo en el COMX
4. Click Device Capabilities
5. Verificar versión mscorlib con el de nuget de referencias
Por último se cargan los programas y se ven en salida/depurar.
</details>

## Visualización en pantalla

<details>
<summary>LVGL</summary>

### LVGL
[LVGL](https://lvgl.io/) es una herramienta muy utilizada para mostrar widgets en pantallas de sistemas embebidos.
Para generar ya los archivos de los widgets existe [Project-lvgl](https://lvgl.io/tools/project-creator), [SquareLine Studio](https://squareline.io/)
El uso de lvgl, existe para diferentes tipos de lenguajes o frameworks que se usan en los sistemas embebidos
</details>
<details>
<summary>SIMPLE WPF</summary>

### SIMPLE WPF
Windows Presentation Foundation (WPF) es un framework de Microsoft para crear aplicaciones de escritorio con interfaces de usuario enriquecidas. Permite el desarrollo de aplicaciones con gráficos avanzados, animaciones y estilos personalizados. Para más información, puedes visitar la [documentación oficial de WPF](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/). Para nanoframework se tiene [WPF](https://github.com/nanoframework/Samples/tree/main/samples/Graphics/SimpleWpf).
</details>
<details>
<sumary>UGUI</summary>

### UGUI
UGUI es un sistema de interfaz de usuario (UI) para Unity, que permite crear interfaces de usuario complejas y personalizadas para aplicaciones y juegos. Proporciona una variedad de componentes y herramientas para diseñar y gestionar elementos de UI como botones, paneles, textos y más. Para más información, puedes visitar la [documentación oficial de UGUI](https://docs.unity3d.com/Manual/UISystem.html). También se puede ver si código open source [$\mu$ GUI](https://github.com/achimdoebler/UGUI).
</details>

###

## Entregable

# DESARROLLO

## DEVELOP BOARD

Se usa una placa ESP32-S3-lcd-1.69" como se ve en la imagen.
<p align="center">
    <img src="./Imagenes/ESP32-S3-lcd-1.69.jpg" alt="ESP32-S3-lcd-1.69" width="500" height="300">
</p>

## CODE

### CONNECTION

