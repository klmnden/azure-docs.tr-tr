---
title: Azure IoT Hub modül kimliğini ve modül ikizini kullanmaya başlama (portal ve .NET) | Microsoft Docs
description: Portal ve .NET kullanarak modül kimliği oluşturmayı ve modülü güncelleştirmeyi öğrenin.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/26/2018
ms.openlocfilehash: d5ac2291f9be6fc8f8d702ec390acf02d5ad9d0f
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59051657"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>Portal ve .NET cihazını kullanarak IoT Hub modül kimliğini ve modül ikizini kullanmaya başlama

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub cihaz kimliği ve cihaz ikizi, arka uç uygulamasının bir cihaz yapılandırmasına imkan tanıyıp cihazın koşullarına yönelik görünürlük sağlarken, modül kimliği ve modül ikizi de bir cihazın tek tek bileşenleri için bu özellikleri sağlar. İşletim sistemi tabanlı cihazlar veya üretici yazılımı cihazları gibi, birden fazla bileşen içeren ve bu özelliklere sahip cihazlarda her bir bileşen için yalıtılmış yapılandırma ve koşullara olanak sağlar.
>

Bu öğreticide şunları öğreneceksiniz:

1. Portalda bir modül kimliği oluşturma 

2. Modül ikizi cihazınızdan .NET cihaz SDK'sı güncelleştirme kullanma

> [!NOTE]
> Hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK'ları hakkında bilgi için bkz. [Azure IoT SDK'ları][lnk-hub-sdks].
>

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>IOT hub için bağlantı dizesi alma

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir cihaz IOT hub'ı Kaydet

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-module-identity-in-the-portal"></a>Portalda bir modül kimliği oluşturma

Tek bir cihaz kimliği içinde en fazla 20 modül kimliği oluşturabilirsiniz. Üstteki **Modül Kimliği Ekle** düğmesine tıklayarak **myFirstModule** adlı ilk modül kimliğinizi oluşturun. 

  ![Cihaz ayrıntıları][9]

Yeni oluşturulan modül kimliğini kaydedip modüle tıklayın. Modül kimliği ayrıntılarını görebilirsiniz. Bağlantı dizesi - birincil anahtarı kaydedin. Bu, cihazda modülünüzü ayarladığınız bir sonraki bölümde kullanılacaktır.

  ![Cihaz ayrıntıları][12]

## <a name="update-the-module-twin-using-net-device-sdk"></a>.NET cihaz SDK’sını kullanarak modül ikizini güncelleştirme

IoT Hub’ınızda modül kimliğini başarıyla oluşturdunuz. Simülasyon cihazınızdan bulutla iletişim kurmayı deneyelim. Bir modül kimliği oluşturulduktan sonra, IoT Hub’ında örtük olarak bir modül ikizi oluşturulur. Bu bölümde, modül ikizi tarafından raporlanan özelliklerini güncelleştiren simülasyon cihazınızda bir .NET konsol uygulaması oluşturacaksınız.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio'da, kullanarak mevcut çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin **konsol uygulaması (.NET Framework)** proje şablonu. .NET Framework sürümünün 4.6.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **UpdateModuleTwinReportedProperties** adını verin.

  ![Visual studio projesi oluşturma][13]

## <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>En son Azure IOT hub'ı .NET cihaz SDK'sını yükleme

Modül kimliği ve modül ikizi genel Önizleme aşamasındadır. Yalnızca, IOT hub'ı ön cihaz SDK'ları içinde de kullanılabilir. Visual Studio’da araçlar > Nuget paket yöneticisi > çözüm için Nuget paketlerini yönet seçeneğini açın. Microsoft.Azure.Devices.Client öğesini arayın. Ön sürümü dahil et onay kutusunu işaretlediğinizden emin olun. En son sürümü seçin ve yükleyin. Şimdi tüm modül özelliklerine erişiminiz vardır. 

  ![Azure IoT Hub .NET hizmet SDK’sı V1.16.0-preview-005’i yükleme][14]

## <a name="get-your-module-connection-string"></a>Modülü bağlantı dizesini alma

Oturum açma [Azure portalında][lnk-portal]. IoT Hub’ınıza gidin ve IoT Cihazları’na tıklayın. Bul myFirstDevice, açık myFirstModule göreceksiniz başarıyla oluşturuldu. Modül bağlantı dizesini kopyalayın. Sonraki adımda gerekecektir.

  ![Azure portalı modül ayrıntısı][15]

## <a name="create-updatemoduletwinreportedproperties-console-app"></a>UpdateModuleTwinReportedProperties konsol uygulaması oluşturma

Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

```csharp
using Microsoft.Azure.Devices.Client;
using Microsoft.Azure.Devices.Shared;
```

**Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, modül bağlantı dizesiyle değiştirin.

```csharp
private const string ModuleConnectionString = "<Your module connection string>";
private static ModuleClient Client = null;
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
        Console.WriteLine(JsonConvert.SerializeObject(twin));

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
    Console.ReadKey();
    Client.CloseAsync().Wait();
}

private static void ConnectionStatusChangeHandler(ConnectionStatus status, ConnectionStatusChangeReason reason)
{
    Console.WriteLine($"Status {status} changed: {reason}");
}
```

Bu kod örneği, AMQP protokolüyle raporlanan özellikleri güncelleştirme ve modül ikizini alma işlemini nasıl yapacağınızı gösterir. Genel önizleme aşamasında, modül ikizi işlemleri için yalnızca AMQP’yi destekleriz.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız. Visual Studio'daki Çözüm Gezgini'nde çözümünüze sağ tıklayın ve ardından **Başlangıç projelerini ayarla**'ya tıklayın. **Birden fazla başlangıç projesi** seçin ve sonra konsol uygulaması için eylem olarak **Başlat**’ı seçin. ' İ tıklatın ve ardından her iki uygulama çalıştırma başlatmak için F5 tuşuna basın. 

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [.NET yedekleme ve cihaz .NET kullanarak IOT hub'ı modül kimlik ve modül ikizi ile çalışmaya başlama][lnk-csharp-csharp-getstarted]
* [IoT Edge ile çalışmaya başlama][lnk-iot-edge]


<!-- Images. -->
[9]:./media/iot-hub-portal-csharp-module-twin-getstarted/create-module-id.JPG
[12]:./media/iot-hub-portal-csharp-module-twin-getstarted/module-details.JPG
[13]: ./media/iot-hub-csharp-csharp-module-twin-getstarted/update-twins-csharp1.JPG
[14]: ./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png
[15]: ./media/iot-hub-csharp-csharp-module-twin-getstarted/module-detail.JPG
<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-csharp-csharp-getstarted]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
