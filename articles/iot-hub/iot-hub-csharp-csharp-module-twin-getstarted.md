---
title: Azure IoT Hub modül kimliğini ve modül ikizini kullanmaya başlama (.NET) | Microsoft Docs
description: .NET için IoT SDK’larını kullanarak modül kimliği oluşturmayı ve modülü güncelleştirmeyi öğrenin.
author: chrissie926
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: 8d5d4ab85d8441998fd384e01f85d1d427d68cc2
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42057725"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-net-back-end-and-net-device"></a>.NET arka ucu ve cihaz .NET kullanarak IOT hub'ı modül kimlik ve modül ikizi ile çalışmaya başlama

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub cihaz kimliği ve cihaz ikizi, arka uç uygulamasının bir cihaz yapılandırmasına imkan tanıyıp cihazın koşullarına yönelik görünürlük sağlarken, modül kimliği ve modül ikizi de bir cihazın tek tek bileşenleri için bu özellikleri sağlar. İşletim sistemi tabanlı cihazlar veya üretici yazılımı cihazları gibi, birden fazla bileşen içeren ve bu özelliklere sahip cihazlarda her bir bileşen için yalıtılmış yapılandırma ve koşullara olanak sağlar.

Bu öğreticinin sonunda iki .NET konsol uygulamanız olacak:

* Cihaz ve modül istemcilerinizi bağlamak için bir cihaz kimliği, bir modül kimliği ve ilişkili güvenlik anahtarı oluşturan **CreateIdentities**.
* Güncelleştirilmiş modül ikizi tarafından raporlanan özelliklerini IoT Hub’ınıza gönderen **UpdateModuleTwinReportedProperties**.

> [!NOTE]
> Hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK'ları hakkında bilgi için bkz. [Azure IoT SDK'ları][lnk-hub-sdks].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

IoT hub'ınızı oluşturdunuz ve bu öğreticinin geri kalanını tamamlamak için gereken ana bilgisayar adı ve IoT Hub bağlantı dizesine sahipsiniz.

[!INCLUDE [iot-hub-get-started-create-module-identity-csharp](../../includes/iot-hub-get-started-create-module-identity-csharp.md)]


## <a name="update-the-module-twin-using-net-device-sdk"></a>.NET cihaz SDK’sını kullanarak modül ikizini güncelleştirme

Bu bölümde, modül ikizi tarafından raporlanan özelliklerini güncelleştiren simülasyon cihazınızda bir .NET konsol uygulaması oluşturursunuz.

1. **Visual Studio projesi oluşturma** - Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak mevcut çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.6.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **UpdateModuleTwinReportedProperties** adını verin.

    ![Visual studio projesi oluşturma][13]

2. **En son Azure IOT hub'ı .NET cihaz SDK'sını yükleme** -modül kimlik ve modül ikizi şu genel Önizleme aşamasındadır. Yalnızca IoT Hub ön sürüm cihaz SDK’larında kullanılabilir. Visual Studio’da araçlar > Nuget paket yöneticisi > çözüm için Nuget paketlerini yönet seçeneğini açın. Microsoft.Azure.Devices.Client öğesini arayın. Ön sürümü dahil et onay kutusunu işaretlediğinizden emin olun. En son sürümü seçin ve yükleyin. Şimdi tüm modül özelliklerine erişiminiz vardır. 

    ![Azure IoT Hub .NET hizmet SDK’sı V1.16.0-preview-005’i yükleme][14]

3. **Modül bağlantı dizenizi alma** -- [Azure portalında][lnk-portal] oturum açarsanız bunu yapabilirsiniz. IoT Hub’ınıza gidin ve IoT Cihazları’na tıklayın. myFirstDevice öğesini bulup açın, böylece myFirstModule öğesinin başarıyla oluşturulduğunu görürsünüz. Modül bağlantı dizesini kopyalayın. Sonraki adımda gerekecektir.

    ![Azure portalı modül ayrıntısı][15]

4. **UpdateModuleTwinReportedProperties konsol uygulaması oluşturma** **Program.cs** dosyasının üst kısmına şu `using` deyimlerini ekleyin:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    ```

    **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, modül bağlantı dizesiyle değiştirin.

    ```csharp
    private const string ModuleConnectionString = "<Your module connection string>";
    private static ModuleClient Client = null;
    static void ConnectionStatusChangeHandler(ConnectionStatus status, ConnectionStatusChangeReason reason)
    {
        Console.WriteLine("Connection Status Changed to {0}; the reason is {1}", status, reason);
    }
    ```

    Aşağıdaki **OnDesiredPropertyChanged** yöntemini **Program** sınıfına ekleyin:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            Console.WriteLine("desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
            Console.WriteLine("Sending current time as reported property");
            TwinCollection reportedProperties = new TwinCollection
            {
                ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
            };

            await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
        }
    ```

    Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    static void Main(string[] args)
    {
        Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;

        try
        {
            Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
            Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
            Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();

            Console.WriteLine("Retrieving twin");
            var twinTask = Client.GetTwinAsync();
            twinTask.Wait();
            var twin = twinTask.Result;
            Console.WriteLine(JsonConvert.SerializeObject(twin.Properties)); 

            Console.WriteLine("Sending app start time as reported property");
            TwinCollection reportedProperties = new TwinCollection();
            reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;

            Client.UpdateReportedPropertiesAsync(reportedProperties);
        }
        catch (AggregateException ex)
        {
            Console.WriteLine("Error in sample: {0}", ex);
        }

        Console.WriteLine("Waiting for Events.  Press enter to exit...");
        Console.ReadLine();
        Client.CloseAsync().Wait();
    }
    ```

    Bu kod örneği, AMQP protokolüyle raporlanan özellikleri güncelleştirme ve modül ikizini alma işlemini nasıl yapacağınızı gösterir. Genel önizleme aşamasında, modül ikizi işlemleri için yalnızca AMQP’yi destekleriz.

5. Yukarıdakilerin yanı sıra **ana** yöntemi ekleyebilirsiniz, modülünden IOT Hub'ına olay göndermek için kod bloğu aşağıda:
    ```csharp
    Byte[] bytes = new Byte[2];
    bytes[0] = 0;
    bytes[1] = 1;
    var sendEventsTask = Client.SendEventAsync(new Message(bytes));
    sendEventsTask.Wait();
    Console.WriteLine("Event sent to IoT Hub.");
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız. Visual Studio'daki Çözüm Gezgini'nde çözümünüze sağ tıklayın ve ardından **Başlangıç projelerini ayarla**'ya tıklayın. **Birden fazla başlangıç projesi** seçin ve sonra konsol uygulaması için eylem olarak **Başlat**’ı seçin. Sonra F5 tuşuna basarak uygulamayı başlatın. 

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimini kullanmaya başlama][lnk-device-management]
* [IoT Edge ile çalışmaya başlama][lnk-iot-edge]


<!-- Images. -->
[13]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.JPG
[14]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png
[15]: ./media\iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
