---
title: Azure IOT Central uygulamanızı Raspberry Pi'yi bağlayın (C#) | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central kullanarak uygulama için bir Raspberry Pi bağlanma C#.
author: viv-liu
ms.author: viviali
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 341b4d23664900cdf1f9a209df663ad4e6e96fe4
ms.sourcegitcommit: ef20235daa0eb98a468576899b590c0bc1a38394
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426367"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Raspberry Pi'yi bağlanmak için Azure IOT Central, uygulama (C#)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Bu makalede, Microsoft Azure IOT Central uygulamanıza C# programlama dilini kullanarak Raspberry Pi'yi bağlanmak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için aşağıdaki bileşenleri gerekir:

* Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* Raspbian işletim sistemi çalıştıran bir Raspberry Pi cihaz. Raspberry Pi internet'e bağlanabiliyor olmanız gerekir. Daha fazla bilgi için [Raspberry Pi'yi ayarlama](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulama

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi** cihaz şablonu aşağıdaki özelliklere sahip:

- Telemetri, cihaz toplayacak aşağıdaki ölçüleri içerir:
  - Nem
  - Sıcaklık
  - Basınç
  - Magnetometer (X, Y, Z)
  - İvme ölçer (X, Y, Z)
  - Jiroskop (X, Y, Z)
- Ayarlar
  - Voltaj
  - Geçerli
  - Fan hızı
  - IR Aç/Kapat.
- Özellikler
  - Numara cihaz özelliği öldürmüş
  - Konum bulut özelliği

Cihaz şablon yapılandırmasının tam Ayrıntılar için bkz. [Raspberry Pi cihaz şablon ayrıntılarını](#raspberry-pi-device-template-details).

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **Raspberry Pi** cihaz şablonu. Bağlantı ayrıntıları cihazın not edin (**kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**). Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).

### <a name="create-your-net-application"></a>.NET uygulamanızı oluşturun

Oluşturulur ve Masaüstü makinenizde cihaz uygulamayı test edin.

Aşağıdaki adımları tamamlamak için Visual Studio Code kullanabilirsiniz. Daha fazla bilgi için [C# ile çalışma](https://code.visualstudio.com/docs/languages/csharp).

> [!NOTE]
> İsterseniz, farklı Kod Düzenleyicisi'ni kullanarak aşağıdaki adımları tamamlayabilirsiniz.

1. .NET projenizi başlatın ve gerekli NuGet paketlerini eklemek için aşağıdaki komutları çalıştırın:

   ```cmd/sh
   mkdir pisample
   cd pisample
   dotnet new console
   dotnet add package Microsoft.Azure.Devices.Client
   dotnet restore
   ```

1. Açık `pisample` Visual Studio code'da klasörü. Açılacağını **pisample.csproj** proje dosyası. Ekleme `<RuntimeIdentifiers>` aşağıdaki kod parçacığında gösterilen etiketi:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.19.0" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > **Microsoft.Azure.Devices.Client** paketinin sürüm numarasını gösterilen olandan daha yüksek olabilir.

1. Kaydet **pisample.csproj**. Visual Studio Code geri yükleme komutu çalıştırmak isteyip istemediğinizi sorar kaldığınızda **geri**.

1. Açık **Program.cs** ve içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;

    namespace pisample
    {
      class Program
      {
        static string DeviceConnectionString = "{your device connection string}";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();
        static CancellationTokenSource cts;
        static double baseTemperature = 60;
        static double basePressure = 500;
        static double baseHumidity = 50;
        static void Main(string[] args)
        {
          Console.WriteLine("Raspberry Pi Azure IoT Central example");

          try
          {
            InitClient();
            SendDeviceProperties();

            cts = new CancellationTokenSource();
            SendTelemetryAsync(cts.Token);

            Console.WriteLine("Wait for settings update...");
            Client.SetDesiredPropertyUpdateCallbackAsync(HandleSettingChanged, null).Wait();
            Console.ReadKey();
            cts.Cancel();
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static void InitClient()
        {
          try
          {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static async void SendDeviceProperties()
        {
          try
          {
            Console.WriteLine("Sending device properties:");
            Random random = new Random();
            TwinCollection telemetryConfig = new TwinCollection();
            reportedProperties["dieNumber"] = random.Next(1, 6);
            Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static async void SendTelemetryAsync(CancellationToken token)
        {
          try
          {
            Random rand = new Random();

            while (true)
            {
              double currentTemperature = baseTemperature + rand.NextDouble() * 20;
              double currentPressure = basePressure + rand.NextDouble() * 100;
              double currentHumidity = baseHumidity + rand.NextDouble() * 20;

              var telemetryDataPoint = new
              {
                humidity = currentHumidity,
                pressure = currentPressure,
                temp = currentTemperature
              };
              var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
              var message = new Message(Encoding.ASCII.GetBytes(messageString));

              token.ThrowIfCancellationRequested();
              await Client.SendEventAsync(message);

              Console.WriteLine("{0} > Sending telemetry: {1}", DateTime.Now, messageString);

              await Task.Delay(1000);
            }
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Intentional shutdown: {0}", ex.Message);
          }
        }

        private static async Task HandleSettingChanged(TwinCollection desiredProperties, object userContext)
        {
          try
          {
            Console.WriteLine("Received settings change...");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            string setting = "fanSpeed";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setVoltage";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setCurrent";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "activateIR";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }

          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static void AcknowledgeSettingChange(TwinCollection desiredProperties, string setting)
        {
          reportedProperties[setting] = new
          {
            value = desiredProperties[setting]["value"],
            status = "completed",
            desiredVersion = desiredProperties["$version"],
            message = "Processed"
          };
        }
      }
    }
    ```

    > [!NOTE]
    > Yer tutucuyu güncelleştirdiğinizden `{your device connection string}` sonraki adımda.

## <a name="run-your-net-application"></a>.NET uygulamanızı çalıştırın

Cihaza özgü bağlantı dizenizi Azure IOT Central ile kimlik doğrulaması bir cihaz için kod ekleyin. Bu yönergeleri izleyin [cihaz bağlantı dizesini oluşturmak](howto-generate-connection-string.md) kullanarak **kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar** , yapılan bir daha önce not edin.

1. Değiştirin `{your device connection string}` içinde **Program.cs** oluşturduğunuz bağlantı dizesiyle dosya.

1. Komut satırı ortamınızda aşağıdaki komutu çalıştırın:

   ```cmd/sh
   dotnet restore
   dotnet publish -r linux-arm
   ```

1. Kopyalama `pisample\bin\Debug\netcoreapp2.1\linux-arm\publish` Raspberry Pi cihazınıza klasör. Kullanabileceğiniz **scp** komut dosyaları, örneğin kopyalamak için:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Daha fazla bilgi için [Raspberry Pi uzaktan erişim](https://www.raspberrypi.org/documentation/remote-access/).

1. Raspberry Pi cihazınıza oturum açın ve bir kabuğunda aşağıdaki komutları çalıştırın:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. Raspberry Pi'yi aşağıdaki komutları çalıştırın:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![Program başlangıcı](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. Azure IOT Central uygulamanızda Raspberry Pi üzerinde çalışan kodu uygulaması ile nasıl etkileşim kurduğunu görebilirsiniz:

   * Üzerinde **ölçümleri** sayfa gerçek cihazınız için telemetriyi görebilirsiniz.
   * Üzerinde **özellikleri** sayfasında değeri bildirilen gördüğünüz **öldürmüş numarası** özelliği.
   * Üzerinde **ayarları** sayfasında, Raspberry Pi voltaj ve giriş hızı gibi çeşitli ayarları değiştirebilirsiniz.

     Aşağıdaki ekran görüntüsünde, ayar değişikliğini alma Raspberry Pi gösterir:

     ![Raspberry Pi ayar değişikliği alır](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="raspberry-pi-device-template-details"></a>Raspberry Pi cihaz şablonu ayrıntıları

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birim  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| Temp           | °C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | Yönetim grubu     | -2000   | 2000    | 0              |
| accelerometerY | Yönetim grubu     | -2000   | 2000    | 0              |
| accelerometerZ | Yönetim grubu     | -2000   | 2000    | 0              |
| gyroscopeX     | MDP'ler   | -2000   | 2000    | 0              |
| gyroscopeY     | MDP'ler   | -2000   | 2000    | 0              |
| gyroscopeZ     | MDP'ler   | -2000   | 2000    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarları

| Görünen ad | Alan adı | Birim | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltaj      | setVoltage | Volt | 0              | 0       | 240     | 0       |
| Geçerli      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan hızı    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Geçiş ayarları

| Görünen ad | Alan adı | Metni | Metin kapalı | İlk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AÇIK      | KAPALI      | Kapalı     |

### <a name="properties"></a>Özellikler

| Type            | Görünen ad | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | sayı    |
| Metin            | Konum     | konum   | YOK       |

## <a name="next-steps"></a>Sonraki adımlar

Raspberry Pi'yi, Azure IOT Central uygulamasına bağlanmak öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Azure IOT Central için genel bir Node.js istemci uygulaması bağlama](howto-connect-nodejs.md)
