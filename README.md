# ![Escuela](./Imagenes/Logotipo-Escuela-Colombiana-de-Ingeniería-Julio-Garavito-5.png)PROYECTO FRAMEWORK .NET

[![Visual Studio Community 2022](https://img.shields.io/badge/Visual%20Studio%20Community%202022-5C2D91?style=for-the-badge&logo=visual-studio&logoColor=white)](https://visualstudio.microsoft.com/es/vs/community/) [![C# .NET](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)](https://docs.microsoft.com/en-us/dotnet/csharp/) [![.NET nanoframework](https://img.shields.io/badge/.NET%20nanoframework-512BD4?style=for-the-badge&logo=.net&logoColor=white)](https://github.com/nanoframework)[![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/en/products/socs/esp32)

# Indice

- [nanoframework](#net-nanoframework)
- [Firmware](#firmware)
    - [Instalación de nanoff](#instalación-de-nanoff)
    - [Actualización del firmware](#actualización-del-firmware)
    - [Configuración en VSCode](#configuración-en-vscode)
- [Conceptos](#conceptos)
    - [Velocidad](#velocidad)
    - [Instancia Funciones Nanoff](#instancia-funciones-nanoff)
        - [int](#int)
        - [float](#float)
        - [double](#double)
        - [char](#char)
        - [string](#string)
        - [bool](#bool)
        - [byte](#byte)
        - [array](#array)
- [Sistemas embebidos](#sistemas-embebidos)
    - [Debug](#debug)
        - [WriteLine](#writeline)
    - [Thread](#thread)
        - [Sleep](#sleep)
    - [IO Ports](#io-ports)
        - [Gpio](#gpio)
        - [ADC](#adc)
        - [DAC](#dac)
        - [PWM](#pwm)
        - [UART](#uart)
        - [I2C](#i2c)
        - [SPI](#spi)
- [Proyecto](#Desarrollo)

## .NET nanoframework

Todo lo que sigue viene de **[.NET nanoframework](https://github.com/nanoframework)**.

<details>
<summary> Firmware </summary>

# Firmware
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

<details>
<summary> Conceptos </summary>

## Conceptos

<details>
<summary> Velocidad </summary>

### SU VELOCIDAD
Para el sistema de lenguaje interpretado la velocidad de respuesta es algo clave, pues siendo un sistema tan pesado, al ejecutarse el sistema se demora muchísimo en responder a sistemas o funciones que en programación serian muy efectivas y rápidas, pero acá pueden demorar muchísimo para lo que es mejor usar librerías del mismo nanoframework.

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

<details>
<summary> Instancia Funciones Nanoff </summary>

### INSTANCIA FUNCIONES NANOFF
Para el nanoff es importante aclarar como se instancia o se generan variables y unidades de memoria como atributos y funciones de la misma.

<details>
<summary> int </summary>

#### int
Utilizado para representar números enteros.
Ejemplo de declaración:

```csharp
int entero = 10;
```
- **Vector de enteros**: Un array unidimensional de enteros.

```csharp
int[] vectorEnteros = new int[10];
```
- **Matriz de enteros**: Un array bidimensional de enteros.

```csharp
int[,] matrizEnteros = new int[3, 3];
```
</details>
<details>
<summary> float </summary>

#### float
Utilizado para representar números de punto flotante de precisión simple.
Ejemplo de declaración:

```csharp
float flotante = 10.5f;
```
- **Vector de floats**: Un array unidimensional de floats.

```csharp
float[] vectorFloats = new float[10];
```
- **Matriz de floats**: Un array bidimensional de floats.

```csharp
float[,] matrizFloats = new float[3, 3];
```
</details>
<details>
<summary> double </summary>

#### double
Utilizado para representar números de punto flotante de doble precisión.
Ejemplo de declaración:

```csharp
double doble = 20.5;
```
- **Vector de doubles**: Un array unidimensional de doubles.

```csharp
double[] vectorDoubles = new double[10];
```
- **Matriz de doubles**: Un array bidimensional de doubles.

```csharp
double[,] matrizDoubles = new double[3, 3];
```
</details>
<details>
<summary> char </summary>

#### char
Utilizado para representar un solo carácter.
Ejemplo de declaración:

```csharp
char caracter = 'A';
```
- **Vector de chars**: Un array unidimensional de chars.

```csharp
char[] vectorChars = new char[10];
```
- **Matriz de chars**: Un array bidimensional de chars.

```csharp
char[,] matrizChars = new char[3, 3];
```
</details>
<details>
<summary> string </summary>

#### string
Utilizado para representar una secuencia de caracteres.
Ejemplo de declaración:

```csharp
string cadena = "Hola, nanoframework";
```
- **Vector de strings**: Un array unidimensional de strings.

```csharp
string[] vectorStrings = new string[10];
```
- **Matriz de strings**: Un array bidimensional de strings.

```csharp
string[,] matrizStrings = new string[3, 3];
```
</details>
<details>
<summary> bool </summary>

#### bool
Utilizado para representar valores booleanos (true o false).
Ejemplo de declaración:

```csharp
bool booleano = true;
```
- **Vector de bools**: Un array unidimensional de bools.

```csharp
bool[] vectorBools = new bool[10];
```
- **Matriz de bools**: Un array bidimensional de bools.

```csharp
bool[,] matrizBools = new bool[3, 3];
```
</details>
<details>
<summary> byte </summary>

#### byte
Utilizado para representar un byte (8 bits).
Ejemplo de declaración:

```csharp
byte byteVariable = 0xFF;
```
- **Vector de bytes**: Un array unidimensional de bytes.

```csharp
byte[] vectorBytes = new byte[10];
```
- **Matriz de bytes**: Un array bidimensional de bytes.

```csharp
byte[,] matrizBytes = new byte[3, 3];
```
</details>
<details>
<summary> array </summary>

#### array
Utilizado para representar una colección de elementos del mismo tipo.

Ejemplo de declaración:

```csharp
int[] arreglo = new int[10];
```
- **Vector de arrays**: Un array unidimensional de arrays.

```csharp
int[][] vectorArrays = new int[10][];
```
- **Matriz de arrays**: Un array bidimensional de arrays.

```csharp
int[,][] matrizArrays = new int[3, 3][];
```
</details>
</details>
</details>


<details>
<summary> Sistemas embebidos </summary>
<details>
<summary> Debug.xxxx </summary>

### Debug
Función que permite usar la consola del mismo dispositivo embebido que tiene diversos metodos que permite usar diferentes funcionalidades del sistema.

<details>
<summary> WriteLine </seummary>

#### WriteLine
Permite escribir lo que se necesita en la salida del depuración del sistema.

```csharp
Debug.WriteLine("Mensaje de depuración");
```
</details>
</details>

<details>
<summary> Thread.xxxx </summary>

### Thread
Thread es una clase que proporciona un conjunto de métodos y propiedades para trabajar con subprocesos. Permite la creación, control y administración de subprocesos en una aplicación.
<details>
<summary> Sleep </summary>

#### Sleep
El método `Sleep` detiene la ejecución del subproceso actual durante un período de tiempo especificado.
Ejemplo de uso:

```csharp
Thread.Sleep(1000); // Detiene el subproceso durante 1000 milisegundos (1 segundo)
```
Este método es útil para pausar la ejecución de un subproceso sin bloquear otros subprocesos en la aplicación.
</details>
</details>

<details>
<summary> IO PORTS </summary>
Para usar los puertos de la tarjeta ESP32 con nanoff, se pueden utilizar las siguientes funciones básicas:

#### Gpio
[GPIO (Introducción/Salida de Propósito General) explicado](https://docs.nanoframework.net/content/getting-started-guides/gpio-explained.html)

```csharp
using System.Device.Gpio;
// connection
GpioController gpio = new GpioController();
gpio.OpenPin(25, PinMode.Output);
gpio.Write(25, PinValue.High);
// blink
GpioController gpio = new GpioController();
GpioPin = led = gpio.OpenPin(25, PinMode.Output);
led.Write(PinValue.High);
GpioController gpio = new GpioController();
GpioPin = led = gpio.OpenPin(25, PinMode.Output);
led.Write(PinValue.Low);

```

#### ADC
[ADC (Analog-to-Digital Converter) explained](https://docs.nanoframework.net/content/getting-started-guides/adc-explained.html)

```csharp
using System.Device.Adc;
// connection
AdcController adc = new AdcController();
AdcChannel channel = adc.OpenChannel(0);
int value = channel.ReadValue();
// measure
AdcController adc = new AdcController();
int max1 = adc.MaxValue;
int min1 = adc.MinValue;
Console.WriteLine("min1=" + min1.ToString() + " max1=" + max1.ToString());
AdcChannel ac0 = adc.OpenChannel(0);
int value = ac0.ReadValue();
double percent = ac0.ReadRatio();
Console.WriteLine("value0=" + value.ToString() + " ratio=" + percent.ToString());
```

#### DAC
[DAC (Digital-to-Analog Converter) explained](https://docs.nanoframework.net/content/getting-started-guides/dac-explained.html)

```csharp
DacController dac = DacController.GetDefault();
// open channel 0
DacChannel dacChannel = dac.OpenChannel(0);
// get DAC resolution
dacResolution = dac.ResolutionInBits;
int timeResolution = 5;
int value = 0;
int upperValue;
int periodCounter = 0;
int halfPeriod;
// get upper value from DAC resolution
upperValue = (int)Math.Pow(2.0, dacResolution);
// figure out an expedite way to get a more or less square wave from the DAC and time resolution
halfPeriod = ( upperValue / (timeResolution * 10) ) / 2;
while(true)
{
    if (periodCounter == halfPeriod)
    {
        // tweak the value so it doesn't overflow the DAC
        value = upperValue - 1;
    }
    else if (periodCounter == halfPeriod * 2)
    {
        value = 0;

        periodCounter = 0;
    }
    channel.WriteValue((ushort)value);
    //Output the current value to console when in debug.
    Debug.WriteLine($"DAC SquareWave output current value: {value}");
    Thread.Sleep(timeResolution);
    periodCounter++;
}
```

#### PWM
[All you've always wanted to know about PWM](https://docs.nanoframework.net/content/getting-started-guides/pwm-explained.html)

```csharp
using System.Device.Pwm;
// connection
PwmChannel pwm = PwmChannel.Create(0, 0, 1000, 0.5);
pwm.Start();
// prove
PwmChannel pwmPin = PwmChannel.CreateFromPin(18, 40000, 0.3);
pwmPin.Start();
// Do something here
// You can even adjust the duty cycle
pwmPin.DutyCycle = 0.5
// And at the end, you can stop it
pwmPin.Stop();
// Define LED PWM channel 1 GPIO 16
Configuration.SetPinFunction(16, DeviceFunction.PWM1);
```

#### UART
[UART (Universal Asynchronous Receiver/Transmitter) or Serial Port Communication](https://docs.nanoframework.net/content/getting-started-guides/uart-explained.html)

```csharp
using System.IO.Ports;
SerialPort uart = new SerialPort("COM1", 9600);
uart.Open();
uart.WriteLine("Hello ESP32");

using System.IO.Ports;
// open COM2
var serialPort = new SerialPort("COM2");
// set parameters
serialPort.BaudRate = 9600;
serialPort.Parity = Parity.None;
serialPort.StopBits = StopBits.One;
serialPort.Handshake = Handshake.None;
serialPort.DataBits = 8;
// Additional properties can be adjusted like the timeout, here 4 seconds = 4000 milliseconds
serialPort.ReadTimeout = 4000;
// Open the Serial Port!
serialPort.Open();
// Write string data
serialPort.WriteLine(DateTime.UtcNow + " hello from nanoFramework!");
// Read some data
byte[] buffer = new byte[5];
var bytesRead = serialPort.Read(buffer, 0, buffer.Length);
// Convert those data as a string
if (bytesRead > 0)
{
    String temp = Encoding.UTF8.GetString(buffer, 0, bytesRead);
    Debug.WriteLine("String: >>" + temp + "<< ");
}
```

#### I2C
[I2C (Inter-Integrated Circuit) communication explained](https://docs.nanoframework.net/content/getting-started-guides/i2c-explained.html)

```csharp
using System.Device.I2c;
I2cConnectionSettings settings = new I2cConnectionSettings(1, 0x40);
I2cDevice device = I2cDevice.Create(settings);
byte[] writeBuffer = new byte[] { 0x00, 0x01 };
device.Write(writeBuffer);

using System.Device.I2c;
I2cDevice i2c = new(new I2cConnectionSettings(1, 0x42));
// Example of writing a byte
var res = i2c.WriteByte(0x07);
// Example of reading 5 bytes
SpanByte span = new byte[5];
res = i2c.Read(span);
// res does contain the status.
Console.Write($"0x42 read status: {res.Status}, number of bytes transferred: {res.BytesTransferred}");
// Success is when res.STatus is equal to I2cTransferStatus.FullTransfer
// Redefine I2C2 data pin (SDA) from GPIO 25 (default) to GPIO 17
Configuration.SetPinFunction(17, DeviceFunction.I2C2_DATA);
```

#### SPI
[SPI (Serial Peripheral Interface) explained](https://docs.nanoframework.net/content/getting-started-guides/spi-explained.html)

```csharp
using System.Device.Spi;
SpiConnectionSettings settings = new SpiConnectionSettings(1, 0);
SpiDevice device = SpiDevice.Create(settings);
byte[] writeBuffer = new byte[] { 0x00, 0x01 };
device.Write(writeBuffer);

using System.Device.Spi;

SpiDevice spiDevice;
SpiConnectionSettings connectionSettings;

// Note: the ChipSelect pin should be adjusted to your device, here 12
connectionSettings = new SpiConnectionSettings(1, 12);
// You can adjust other settings as well in the connection
connectionSettings.ClockFrequency = 1_000_000;
connectionSettings.DataBitLength = 8;
connectionSettings.DataFlow = DataFlow.LsbFirst;
connectionSettings.Mode = SpiMode.Mode2;

// Then you create your SPI device by passing your settings
spiDevice = SpiDevice.Create(connectionSettings);

// You can write a SpanByte
SpanByte writeBufferSpanByte = new byte[2] { 42, 84 };
spiDevice.Write(writeBufferSpanByte);

// The read operations are similar
SpanByte readBufferSpanByte = new byte[2];
// This will read 2 bytes
spiDevice.Read(readBufferSpanByte);
// Define MOSI pin for SPI2 as GPIO 15
Configuration.SetPinFunction(15, DeviceFunction.SPI2_MOSI);
```
</details>

</details>

#### Entregables


# DESARROLLO
