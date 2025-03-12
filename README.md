# ![Escuela](./Imagenes/Logotipo-Escuela-Colombiana-de-Ingeniería-Julio-Garavito-5.png)PROYECTO FRAMEWORK .NET

[![Visual Studio Community 2022](https://img.shields.io/badge/Visual%20Studio%20Community%202022-5C2D91?style=for-the-badge&logo=visual-studio&logoColor=white)](https://visualstudio.microsoft.com/es/vs/community/) [![C# .NET](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)](https://docs.microsoft.com/en-us/dotnet/csharp/) [![.NET nanoframework](https://img.shields.io/badge/.NET%20nanoframework-512BD4?style=for-the-badge&logo=.net&logoColor=white)](https://github.com/nanoframework)[![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/en/products/socs/esp32)

# Indice
- [Firmware](#firmware)
- [Conceptos](#conceptos)
    - [Su Velocidad](#su-velocidad)
- [Instancia Funciones Nanoff](#instancia-funciones-nanoff)
    - [Debug](#debug)
        - [WriteLine](#writeline)
    - [Thread](#thread)
        - [Sleep](#sleep)
    - [Try](#try)
    - [Hilos](#hilos)
- [GPio](#gpio)
    - [IO Ports](#io-ports)
        - [Digital](#digital)
        - [Análogo](#análogo)
        - [I2C](#i2c)
        - [SPI](#spi)
        - [UART](#uart)
- [LVGL](#lvgl)
- [Entregable](#entregable)
- [Desarrollo](#desarrollo)
    - [Placa](#placa)
    - [Código](#código)
        - [Conexión](#conexión)
        - [Datos](#datos)

## .NET nanoframework
Todo lo que sigue viene de **[.NET nanoframework](https://github.com/nanoframework)**.
Ejemplos de código para uso estan en [samples](https://github.com/nanoframework/Samples).

### Firmware
Para descargar e implementar nanoframework en el sistema embebido que deseamos, se debe primero instalar [vscode](https://code.visualstudio.com/) o [vscode community](https://visualstudio.microsoft.com/es/vs/community/).
Se requiere descargar estos IDE y en sus extensiones descargar .NET nanoframework.
<details>
<summary>step to step</summary>
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

### Conceptos

#### SU VELOCIDAD
Para el sistema de lenguaje interpretado la velocidad de respuesta es algo clave, pues siendo un sistema tan pesado, al ejecutarse el sistema se demora muchísimo en responder a sistemas o funciones que en programación serian muy efectivas y rápidas, pero acá pueden demorar muchísimo para lo que es mejor usar librerías del mismo nanoframework.
<details>
<summary>ejemplo</summary>
El siguiente código es una representación de un bucle anidado. El sistema se demoro cerca de 200 ms.

```csharp
using System;
using System.Threading;
using System.Diagnostics;
using System.Collections;

namespace NFApp1
{
   public class Program
   {
       static byte[][] Datos;
       public static void Main()
       {
           Debug.WriteLine("Hello from nanoFramework!");
           Datos = new byte[100][];
           for (int i = 0; i < Datos.Length; i++)
           {
               Datos[i] = new byte[100];
           }
           DateTime Start = DateTime.UtcNow;
           for (int i = 0; i < Datos.Length; i++)
           {
               for (int j = 0; j < Datos[i].Length; j++)
               {
                   Datos[i][j] = 0;
               }
           }
           TimeSpan Dif = DateTime.UtcNow - Start;
           Debug.WriteLine("Execute time: " + Dif.TotalMilliseconds + " ms");
           Thread.Sleep(Timeout.Infinite);
       }
   }
}
```
El siguiente código es una representación de una función de nanoff. El sistema se demoro cerca de 6.2 ms.
```csharp
using System;
using System.Threading;
using System.Diagnostics;
using System.Collections;

namespace NFApp1
{
    public class Program
    {
        static byte[][] Datos;
        public static void Main()
        {
            Debug.WriteLine("Hello from nanoFramework!");
            Datos = new byte[100][];
            for (int i = 0; i < Datos.Length; i++)
            {
                Datos[i] = new byte[100];
            }
            DateTime Start = DateTime.UtcNow;
            for (int i = 0; i < Datos.Length; i++)
            {
                Array.Clear(Datos[i], 0, 100);
            }
            TimeSpan Dif = DateTime.UtcNow - Start;
            Debug.WriteLine("Execute time: " + Dif.TotalMilliseconds + " ms");
            Thread.Sleep(Timeout.Infinite);
        }
    }
}

```
</details>

### INSTANCIA FUNCIONES NANOFF
Para el nanoff es importante aclarar como se instancia o se generan variables y unidades de memoria como atributos y funciones de la misma.

#### Debug
Función que permite usar la consola del mismo dispositivo embebido que tiene diversos metodos que permite usar diferentes funcionalidades del sistema.

##### WriteLine
Permite escribir lo que se necesita en la salida del depuración del sistema.
```csharp
Debug.WriteLine("Mensaje de depuración");
```

#### Thread
Thread es una clase que proporciona un conjunto de métodos y propiedades para trabajar con subprocesos. Permite la creación, control y administración de subprocesos en una aplicación.
Crear hilos independientes como las tareas.

##### Sleep
El método `Sleep` detiene la ejecución del subproceso actual durante un período de tiempo especificado.
Ejemplo de uso:
```csharp
Thread.Sleep(1000); // Detiene el subproceso durante 1000 milisegundos (1 segundo)
```
Este método es útil para pausar la ejecución de un subproceso sin bloquear otros subprocesos en la aplicación.

#### try
Forma de evitar y controlar los errores, pero con la funcionalidad de evitarlos y continuar con el proceso.
```csharp
try
{
    int x = 0;
    int y = 5;
    int z = y / x;
}
catch (IndexOutOfRangeException e)
{
    Debug.WriteLine("Error por índice fuera de rango, " + e.ToString());
}
catch (Exception e)
{
    Debug.WriteLine("Error, Información = " + e.ToString());
}
// se pueden seguir anidando catch
finally
{
    Debug.WriteLine("Código de Error Finalizado");
}
```
El bloque `try` permite ejecutar un código que puede generar una excepción. Si ocurre una excepción, el control se transfiere al bloque `catch` correspondiente. El bloque `finally` se ejecuta siempre, independientemente de si se lanzó una excepción o no, y se utiliza para liberar recursos o realizar tareas de limpieza.

#### Hilos
Los hilos (threads) permiten la ejecución concurrente de múltiples tareas dentro de una aplicación. En .NET nanoframework, la clase `Thread` proporciona métodos y propiedades para crear y manejar hilos.
Ejemplo de uso de un hilo en .NET nanoframework:
```csharp
using System;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        public static void Main()
        {
            Thread hilo = new Thread(new ThreadStart(EjecutarHilo));
            hilo.Start();
            Debug.WriteLine("Hilo principal continúa ejecutándose.");
        }

        public static void EjecutarHilo()
        {
            for (int i = 0; i < 5; i++)
            {
                Debug.WriteLine("Hilo secundario ejecutándose: " + i);
                Thread.Sleep(1000); // Pausa el hilo por 1 segundo
            }
        }
    }
}
```
En este ejemplo, se crea un hilo secundario que ejecuta el método `EjecutarHilo`. El hilo principal continúa ejecutándose mientras el hilo secundario realiza su tarea de forma concurrente.

### GPio
#### IO PORTS
Los puertos de entrada/salida (IO Ports) son interfaces que permiten la comunicación entre el microcontrolador y otros dispositivos periféricos. Estos puertos pueden configurarse como entradas o salidas para leer datos de sensores o enviar señales a actuadores.
Ejemplo de uso de un puerto IO en .NET nanoframework:
```csharp
using System;
using System.Device.Gpio;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        static GpioPin led;
        public static void Main()
        {
            
            GpioController gpioController = new GpioController();
            led = gpioControler.OpenPin(2, PinMode.Output); // Pin 2
            while (true)
            {
                gpio.Write(led, PinValue.High); // Enciende el pin
                Thread.Sleep(1000); // Espera 1 segundo
                gpio.Write(led, PinValue.Low); // Apaga el pin
                Thread.Sleep(1000); // Espera 1 segundo
            }
        }
    }
}
```

<details>
<summary>Digital</summary>

### Puertos Digitales
Los puertos digitales permiten la comunicación entre el microcontrolador y otros dispositivos electrónicos mediante señales digitales. En .NET nanoframework, se pueden configurar los pines como entradas o salidas para leer o escribir valores digitales.

#### Escritura Digital
Para escribir un valor digital en un pin, se configura el pin como salida y se utiliza el método `Write` para establecer el valor del pin (alto o bajo).
Ejemplo de escritura digital:
```csharp
using System;
using System.Device.Gpio;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        static GpioPin led;
        public static void Main()
        {
            GpioController gpioController = new GpioController();
            led = gpioController.OpenPin(2, PinMode.Output); // Configura el pin 2 como salida
            while (true)
            {
                gpioController.Write(led, PinValue.High); // Enciende el pin
                Thread.Sleep(1000); // Espera 1 segundo
                gpioController.Write(led, PinValue.Low); // Apaga el pin
                Thread.Sleep(1000); // Espera 1 segundo
            }
        }
    }
}
```
#### Lectura Digital
Para leer un valor digital de un pin, se configura el pin como entrada y se utiliza el método `Read` para obtener el valor del pin (alto o bajo).
Ejemplo de lectura digital:
```csharp
using System;
using System.Device.Gpio;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        static GpioPin button;
        public static void Main()
        {
            GpioController gpioController = new GpioController();
            button = gpioController.OpenPin(3, PinMode.Input); // Configura el pin 3 como entrada
            while (true)
            {
                PinValue value = gpioController.Read(button); // Lee el valor del pin
                if (value == PinValue.High)
                {
                    Debug.WriteLine("Botón presionado");
                }
                else
                {
                    Debug.WriteLine("Botón no presionado");
                }
                Thread.Sleep(500); // Espera 0.5 segundos
            }
        }
    }
}
```
</details>

<details>
<summary>Análogo</summary>

### Puertos Analógicos
Los puertos analógicos permiten la lectura de señales analógicas, que son señales continuas que pueden tener un rango de valores. En .NET nanoframework, se pueden utilizar los puertos analógicos para leer valores de sensores analógicos, como sensores de temperatura, luz, etc.

#### Lectura Analógica
Para leer un valor analógico de un pin, se configura el pin como entrada analógica y se utiliza el método `Read` para obtener el valor del pin.
Ejemplo de lectura analógica:
```csharp
using System;
using System.Device.Adc;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        static AdcChannel sensor;
        public static void Main()
        {
            AdcController adcController = new AdcController();
            sensor = adcController.OpenChannel(0); // Configura el canal 0 como entrada analógica
            while (true)
            {
                int value = sensor.ReadValue(); // Lee el valor analógico del pin
                Debug.WriteLine("Valor del sensor: " + value);
                Thread.Sleep(1000); // Espera 1 segundo
            }
        }
    }
}
```

En este ejemplo, se configura un canal ADC (Analog-to-Digital Converter) para leer valores analógicos de un sensor conectado al pin correspondiente. El valor leído se muestra en la consola de depuración.
</details>

<details>
<summary>i2c</summary>

### I2C
El protocolo I2C (Inter-Integrated Circuit) es un protocolo de comunicación en serie que permite la comunicación entre múltiples dispositivos conectados a un mismo bus. En .NET nanoframework, se puede utilizar la clase `I2cDevice` para interactuar con dispositivos I2C.

#### Configuración y Uso de I2C
Para utilizar I2C en .NET nanoframework, primero se debe configurar el dispositivo I2C especificando la dirección del dispositivo y otros parámetros de configuración.
Ejemplo de uso de I2C:
```csharp
using System;
using System.Device.I2c;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        public static void Main()
        {
            // Configuración del dispositivo I2C
            var settings = new I2cConnectionSettings(1, 0x40); // Bus I2C 1, dirección del dispositivo 0x40
            var device = I2cDevice.Create(settings);

            // Escribir datos al dispositivo I2C
            byte[] writeBuffer = new byte[] { 0x00, 0x01 };
            device.Write(writeBuffer);

            // Leer datos del dispositivo I2C
            byte[] readBuffer = new byte[2];
            device.Read(readBuffer);

            // Mostrar los datos leídos
            Debug.WriteLine("Datos leídos: " + BitConverter.ToString(readBuffer));

            Thread.Sleep(Timeout.Infinite);
        }
    }
}
```
En este ejemplo, se configura un dispositivo I2C con una dirección específica y se realizan operaciones de escritura y lectura. Los datos leídos se muestran en la consola de depuración.
</details>

<details>
<summary>spi</summary>

### SPI
El protocolo SPI (Serial Peripheral Interface) es un protocolo de comunicación en serie que permite la comunicación rápida entre un microcontrolador y uno o más dispositivos periféricos. En .NET nanoframework, se puede utilizar la clase `SpiDevice` para interactuar con dispositivos SPI.

#### Configuración y Uso de SPI
Para utilizar SPI en .NET nanoframework, primero se debe configurar el dispositivo SPI especificando los parámetros de configuración como el bus SPI, la frecuencia de reloj, el modo SPI y otros.
Ejemplo de uso de SPI:
```csharp
using System;
using System.Device.Spi;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        public static void Main()
        {
            // Configuración del dispositivo SPI
            var settings = new SpiConnectionSettings(1, 0) // Bus SPI 1, Chip Select 0
            {
                ClockFrequency = 500000, // Frecuencia de reloj de 500 kHz
                Mode = SpiMode.Mode0 // Modo SPI 0
            };
            var device = SpiDevice.Create(settings);

            // Escribir datos al dispositivo SPI
            byte[] writeBuffer = new byte[] { 0x01, 0x02, 0x03 };
            device.Write(writeBuffer);

            // Leer datos del dispositivo SPI
            byte[] readBuffer = new byte[3];
            device.Read(readBuffer);

            // Mostrar los datos leídos
            Debug.WriteLine("Datos leídos: " + BitConverter.ToString(readBuffer));

            Thread.Sleep(Timeout.Infinite);
        }
    }
}
```
En este ejemplo, se configura un dispositivo SPI con una frecuencia de reloj específica y un modo SPI. Se realizan operaciones de escritura y lectura, y los datos leídos se muestran en la consola de depuración.
</details>

<details>
<summary>uart</summary>

### UART
El protocolo UART (Universal Asynchronous Receiver-Transmitter) es un protocolo de comunicación en serie que permite la transmisión y recepción de datos entre dispositivos. En .NET nanoframework, se puede utilizar la clase `SerialPort` para interactuar con dispositivos UART.

#### Configuración y Uso de UART
Para utilizar UART en .NET nanoframework, primero se debe configurar el puerto serie especificando los parámetros de configuración como la velocidad en baudios, los bits de datos, los bits de parada y la paridad.
Ejemplo de uso de UART:
```csharp
using System;
using System.IO.Ports;
using System.Text;
using System.Threading;

namespace NFApp1
{
    public class Program
    {
        public static void Main()
        {
            // Configuración del puerto serie
            SerialPort serialPort = new SerialPort("COM1", 9600, Parity.None, 8, StopBits.One);
            serialPort.Open();

            // Enviar datos al dispositivo UART
            string message = "Hola desde nanoFramework!";
            byte[] buffer = Encoding.UTF8.GetBytes(message);
            serialPort.Write(buffer, 0, buffer.Length);

            // Leer datos del dispositivo UART
            byte[] readBuffer = new byte[serialPort.BytesToRead];
            serialPort.Read(readBuffer, 0, readBuffer.Length);
            string receivedMessage = Encoding.UTF8.GetString(readBuffer);
            Debug.WriteLine("Mensaje recibido: " + receivedMessage);

            serialPort.Close();
            Thread.Sleep(Timeout.Infinite);
        }
    }
}
```
En este ejemplo, se configura un puerto serie con una velocidad en baudios de 9600 y se realizan operaciones de escritura y lectura. Los datos leídos se muestran en la consola de depuración.
</details>

<details>
<summary>Wifi</summary>

### Configuración y Uso de Wifi
El ESP32 tiene capacidades integradas de Wifi que permiten la conexión a redes inalámbricas. En .NET nanoframework, se puede utilizar la clase `WiFiNetworkHelper` para conectarse a una red Wifi.

#### Conexión a una Red Wifi
Para conectarse a una red Wifi, se debe especificar el SSID y la contraseña de la red.
Ejemplo de conexión a una red Wifi:
```csharp
using System;
using System.Threading;
using nanoFramework.Networking;

namespace NFApp1
{
    public class Program
    {
        public static void Main()
        {
            // Configuración de la red Wifi
            string ssid = "TuSSID";
            string password = "TuContraseña";

            // Conectar a la red Wifi
            var success = WiFiNetworkHelper.ConnectDhcp(ssid, password, requiresDateTime: true, token: new CancellationTokenSource(60000).Token);

            if (success)
            {
                Debug.WriteLine("Conectado a la red Wifi.");
            }
            else
            {
                Debug.WriteLine("Error al conectar a la red Wifi.");
            }

            Thread.Sleep(Timeout.Infinite);
        }
    }
}
```
En este ejemplo, se configura la red Wifi con el SSID y la contraseña, y se intenta conectar a la red. Si la conexión es exitosa, se muestra un mensaje en la consola de depuración.
</details>

## lvgl
[LVGL](https://lvgl.io/) es una herramienta muy utilizada para mostrar widgets en pantallas de sistemas embebidos.
Para generar ya los archivos de los widgets existe [Project-lvgl](https://lvgl.io/tools/project-creator), [SquareLine Studio](https://squareline.io/)
El uso de lvgl, existe para diferentes tipos de lenguajes o frameworks que se usan en los sistemas embebidos

#### Entregable

# DESARROLLO

## Placa

Se usa una placa ESP32-S3-lcd-1.69" como se ve en la imagen.
<p align="center">
    <img src="./Imagenes/ESP32-S3-lcd-1.69.jpg" alt="ESP32-S3-lcd-1.69" width="500" height="300">
</p>

## Código

### Conexión 

### Datos

