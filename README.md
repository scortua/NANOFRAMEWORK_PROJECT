# ![Escuela](./Imagenes/Logotipo-Escuela-Colombiana-de-Ingeniería-Julio-Garavito-5.png)PROYECTO FRAMEWORK .NET

[![Visual Studio Community 2022](https://img.shields.io/badge/Visual%20Studio%20Community%202022-5C2D91?style=for-the-badge&logo=visual-studio&logoColor=white)](https://visualstudio.microsoft.com/es/vs/community/) [![C# .NET](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)](https://docs.microsoft.com/en-us/dotnet/csharp/) [![.NET nanoframework](https://img.shields.io/badge/.NET%20nanoframework-512BD4?style=for-the-badge&logo=.net&logoColor=white)](https://github.com/nanoframework)[![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/en/products/socs/esp32)

# Indice

* [nanoframework](#net-nanoframework)
* [Proyecto](#Desarrollo)

## .NET nanoframework

Todo lo que sigue viene de **[.NET nanoframework](https://github.com/nanoframework)**.

<details>
<summary> Firmware </summary>

Para descargar e implementar nanoframework en el sistema embebido que deseamos, se debe primero instalar [vscode](https://code.visualstudio.com/) o [vscode community](https://visualstudio.microsoft.com/es/vs/community/).

Se requiere descargar estos IDE y en sus extensiones descargar .NET nanoframework.

Ahora, se requiere nanoff y se puede seguir el paso a paso en [nanoff](https://github.com/nanoframework/nanoFirmwareFlasher). Ahora en el terminal como admin.
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

Con ello se verifica que __ESP__ es y ahora actualizar el firmware de nuestra tarjeta.

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

#### Entregables
<details>
<summary> Entregable 1 </summary>

</details>

<details>
<summary> Entregable 2 </summary>

</details>

<details>
<summary> Entregable 3 </summary>

</details>

# DESARROLLO