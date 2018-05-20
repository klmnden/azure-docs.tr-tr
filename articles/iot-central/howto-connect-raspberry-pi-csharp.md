---
title: Bağlantı, Azure IOT merkezi uygulamanıza (C#) Raspberry Pi'yi | Microsoft Docs
description: Bir aygıt geliştiricisi olarak C# kullanarak Azure IOT merkezi uygulamanızı Raspberry Pi'yi bağlanma.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/22/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: d09d3de04f8c846eadc7367ca4d4559eb55f995b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Azure IOT merkezi uygulamanıza (C#) Raspberry Pi'yi Bağlan

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Bu makalede Raspberry Pi'yi C# programlama dilini kullanarak Microsoft Azure IOT merkezi bağlamak için bir aygıt geliştiricisi olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* [.NET core 2](https://www.microsoft.com/net) geliştirme makinenizde yüklü. Uygun kod düzenleyicisini gibi olmalıdır [Visual Studio Code](https://code.visualstudio.com/).
* Oluşturulan Azure IOT Merkezi uygulama **örnek Devkits** uygulama şablonu. Daha fazla bilgi için bkz: [Azure IOT merkezi uygulamanızı oluşturma](howto-create-application.md).
* Raspbian işletim sistemi çalıştıran bir Raspberry Pi'yi aygıt.

Oluşturulan bir uygulamayı **örnek Devkits** uygulama şablonu içeren bir **Raspberry Pi'yi** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamaklar |
| -------------- | ------ | ------- | ------- | -------------- |
| nem oranı       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | MG     | -2000   | 2000    | 0              |
| accelerometerY | MG     | -2000   | 2000    | 0              |
| accelerometerZ | MG     | -2000   | 2000    | 0              |
| gyroscopeX     | MDP'ler   | -2000   | 2000    | 0              |
| gyroscopeY     | MDP'ler   | -2000   | 2000    | 0              |
| gyroscopeZ     | MDP'ler   | -2000   | 2000    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarları

| Görüntüleme adı | Alan adı | Birimler | Ondalık basamaklar | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltaj      | setVoltage | Volt | 0              | 0       | 240     | 0       |
| Geçerli      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan hızı    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Geçiş ayarları

| Görüntüleme adı | Alan adı | Metni | Metin kapalı | İlk |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | AÇIK      | KAPALI      | Kapalı     |

### <a name="properties"></a>Özellikler

| Tür            | Görüntüleme adı | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | numarası    |
| Mesaj gönder            | Konum     | konum   | Yok       |

### <a name="add-a-real-device"></a>Gerçek bir cihaz ekleme

Azure IOT merkezi uygulamanızda gerçek bir aygıttan ekleyin **Raspberry Pi'yi** cihaz şablonu ve cihaz bağlantı dizesini Not. Daha fazla bilgi için bkz: [gerçek bir cihazı Azure IOT merkezi uygulamanıza eklemek](tutorial-add-device.md).

## <a name="create-your-net-application"></a>.NET uygulaması oluşturma

Oluşturun ve cihaz uygulamayı Masaüstü makinenizde test.

Aşağıdaki adımları tamamlamak için Visual Studio Code kullanabilirsiniz. Daha fazla bilgi için bkz: [C# ile çalışma](https://code.visualstudio.com/docs/languages/csharp).

> [!NOTE]
> İsterseniz, farklı bir kod düzenleyicisini kullanarak aşağıdaki adımları tamamlayabilirsiniz.

1. .NET projenizi başlatmak ve gerekli NuGet paketlerini eklemek için aşağıdaki komutları çalıştırın:

  ```cmd/sh
  mkdir pisample
  cd pisample
  dotnet new console
  dotnet add package Microsoft.Azure.Devices.Client
  dotnet restore
  ```

1. Açık `pisample` Visual Studio Code klasöründe. Ardından açın **pisample.csproj** proje dosyası. Ekleme `<RuntimeIdentifiers>` aşağıdaki kod parçacığında gösterildiği etiketi:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.5.2" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > **Microsoft.Azure.Devices.Client** paket sürüm numarası gösterilen olandan daha yüksek olabilir.

1. Kaydet **pisample.csproj**. Visual Studio Code restore komutu yürütmek için isterse seçin **geri**.

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
    > Yer tutucu güncelleştirme `{your device connection string}` sonraki adımda.

## <a name="run-your-net-application"></a>.NET uygulamanızı çalıştırma

Aygıta özgü bağlantı dizenizi Azure IOT Merkezi ile kimlik doğrulaması için cihazın kodunu ekleyin. Azure IOT merkezi uygulamanıza gerçek Cihazınızı eklediğinizde, bu bağlantı dizesini Not yaptığınız.

1. Değiştir `{your device connection string}` içinde **Program.cs** daha önce not ettiğiniz bağlantı dizesiyle dosya.

1. Komut satırı ortamınızda aşağıdaki komutu çalıştırın:

  ```cmd/sh
  dotnet restore
  dotnet publish -r linux-arm
  ```

1. Kopya `pisample\bin\Debug\netcoreapp2.0\linux-arm\publish` Raspberry Pi'yi Cihazınızı klasörüne. Kullanabileceğiniz **scp** komut dosyaları, örneğin kopyalamak için:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Daha fazla bilgi için bkz: [Raspberry Pi'yi uzaktan erişim](https://www.raspberrypi.org/documentation/remote-access/).

1. Raspberry Pi'yi Cihazınızı oturum açın ve bir Kabuğu'nda aşağıdaki komutları çalıştırın:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. Raspberry Pi'yi üzerinde aşağıdaki komutları çalıştırın:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![Program başlar](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. Azure IOT merkezi uygulamanızda Raspberry Pi'yi üzerinde çalışan kodu uygulaması ile nasıl etkileşim kurduğunu görebilirsiniz:

    * Üzerinde **ölçümleri** sayfa gerçek cihazınız için telemetri görebilirsiniz.
    * Üzerinde **özellikleri** sayfasında, bildirilen değeri görebilirsiniz **öldürmüş numarası** özelliği.
    * Üzerinde **ayarları** sayfasında Raspberry Pi'yi voltaj ve fan hızı gibi çeşitli ayarları değiştirebilirsiniz.

    Aşağıdaki ekran görüntüsü ayar değişikliğini alma Raspberry Pi'yi gösterir:

    ![Ayar değişikliğini Böğürtlenli Pi alır](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT merkezi uygulamanıza Raspberry Pi'yi bağlanma öğrendiniz, önerilen sonraki adımlar şunlardır:

* [Bir genel Node.js istemci uygulamaya Azure IOT merkezi bağlama](howto-connect-nodejs.md)