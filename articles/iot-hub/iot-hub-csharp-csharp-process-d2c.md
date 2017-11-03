---
title: "Azure IOT Hub cihaz-bulut iletileri yolları (.Net) kullanma | Microsoft Docs"
description: "Diğer arka uç hizmetlerine iletilerinin gönderilmesi için yönlendirme kurallarını ve özel uç noktaları kullanarak IOT Hub cihaz bulut iletilerini işlemek nasıl."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 1d2b52ea005ab520bf294efa603512c00a92ee63
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>Yollar (.NET) kullanılarak IOT Hub cihaz bulut iletilerini işleme

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Bu öğretici derlemeler [IOT Hub ile çalışmaya başlama] Öğreticisi. Öğretici:

* Cihaz bulut iletilerini kolay, yapılandırma tabanlı bir şekilde gönderilmesi için yönlendirme kurallarını kullanmayı gösterir.
* Daha ayrıntılı işleme için çözüm arka uçtan Acil eylem gerekli etkileşimli iletilerin yalıtmak nasıl gösterilmektedir. Örneğin, bir aygıt bir bilet bir CRM sistemine ekleme tetikleyen bir uyarı iletisi gönderebilir. Buna karşılık, sıcaklık telemetri gibi veri noktası iletileri analytics motoruna akış.

Bu öğreticinin sonunda üç .NET konsol uygulamaları çalıştırın:

* **SimulatedDevice**, oluşturduğunuz uygulama değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama] öğretici saniyede veri noktası cihaz bulut iletilerini gönderir ve etkileşimli cihaz bulut iletilerini her 10 saniye.
* **ReadDeviceToCloudMessages** cihaz uygulamanız tarafından gönderilen kritik olmayan telemetriyi görüntüler.
* **ReadCriticalQueue** Service Bus kuyruğundaki iletileri cihaz uygulamanız tarafından gönderilen kritik iletileri çıkarır. Bu sıra IOT hub'ına bağlı.

> [!NOTE]
> IOT hub'ı birçok cihaz platformları ve C, Java ve JavaScript gibi diller için SDK desteğe sahiptir. Sanal cihaz Bu öğreticide bir fiziksel aygıt ile değiştirmek öğrenmek için bkz: [Azure IOT Geliştirme Merkezi].

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017.
* Etkin bir Azure hesabı. <br/>Bir hesabınız yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.

Bazı temel bilgiye sahip [Azure Storage] ve [Azure Service Bus].

## <a name="send-interactive-messages"></a>Etkileşimli iletileri gönder

Oluşturduğunuz cihaz uygulamayı değiştirmek [IOT Hub ile çalışmaya başlama] bazen etkileşimli iletileri göndermek için öğretici.

Visual Studio içinde **SimulatedDevice** proje, yerine `SendDeviceToCloudMessagesAsync` aşağıdaki kod ile yöntemi:

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

Bu yöntem rastgele özelliği ekler `"level": "critical"` aygıt tarafından gönderilen iletiler için hangi benzetim çözüm arka ucu tarafından Acil eylem gerektiren bir ileti. Bu IOT hub'ın uygun mesajı hedefe ileti yönlendirmek için cihaz uygulaması bu bilgileri ileti özelliklerinde yerine ileti gövdesinde geçirir.

> [!NOTE]
> Burada gösterilen hot yolu örnek yanı sıra soğuk yolu işleme dahil olmak üzere çeşitli senaryoları için ileti özellikleri iletileri yönlendirmek için kullanabilirsiniz.

> [!NOTE]
> Basitleştirmek amacıyla, Bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda MSDN makalesinde önerilen üstel geri alma gibi bir yeniden deneme ilkesi uygulamalıdır [geçici hata işleme].

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a>IOT hub'ınızdaki kuyruğuna iletileri yönlendirmek

Bu bölümde şunları yapacaksınız:

* Service Bus kuyruğuna oluşturun.
* IOT hub'ınıza bağlanın.
* İleti bir özellik varlığına dayalı kuyruğa ileti göndermek için IOT hub'ınızı yapılandırma.

Hizmet veri yolu sıralardan işlem iletileri hakkında daha fazla bilgi için bkz: [kuyruklarla çalışmaya başlama][Service Bus queue].

1. Service Bus kuyruğuna açıklandığı gibi oluşturmak [kuyruklarla çalışmaya başlama][Service Bus queue]. Sıranın IOT hub'ınızı aynı abonelikte ve bölgede olmalıdır. Ad alanı ve sıra adını not edin.

    > [!NOTE]
    > Service Bus kuyrukları ve konularından IOT Hub uç noktaları değil olarak kullanılan **oturumları** veya **yinelenen saptama** etkin. Bu seçeneklerden birini etkinleştirilmişse, uç nokta olarak görünür **ulaşılamıyor** Azure portalında.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.
    
    ![IOT hub uç noktaları][30]

3. İçinde **uç noktaları** dikey penceresinde tıklatın **Ekle** sıranız IOT hub'ınıza eklemek için üst. Uç nokta adı **CriticalQueue** ve seçmek için açılır listeleri kullanın **Service Bus kuyruğuna**, sıranız bulunduğu hizmet veri yolu ad alanı ve sıranız adı. İşiniz bittiğinde tıklatın **kaydetmek** altındaki.
    
    ![Bir uç nokta ekleme][31]
    
4. Şimdi **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **DeviceTelemetry** veri kaynağı olarak. Girin `level="critical"` koşulu olarak ve yeni eklediğiniz yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak sıra'yı seçin. İşiniz bittiğinde tıklatın **kaydetmek** altındaki.
    
    ![Bir yol ekleme][32]
    
    Emin olun geri dönüş rota ayarlanmış **ON**. Bu değer, bir IOT hub'ın varsayılan yapılandırmadır.
    
    ![Geri dönüş yolu][33]

## <a name="read-from-the-queue-endpoint"></a>Sıra uç noktasından okumak

Bu bölümde, sıra uç noktasından iletilerini okuyun.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak geçerli çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. Proje adı **ReadCriticalQueue**.

2. Çözüm Gezgini'nde sağ **ReadCriticalQueue** proje ve ardından **NuGet paketlerini Yönet**. Bu işlem görüntüler **NuGet Paket Yöneticisi** penceresi.

3. Arama **WindowsAzure.ServiceBus**, tıklatın **yükleme**ve kullanım koşullarını kabul edin. Bu işlem indirir, yükler ve Azure hizmet veri yolu, tüm bağımlılıklarıyla birlikte bir başvuru eklenir.

4. Aşağıdakileri ekleyin **kullanarak** deyimleri en üstündeki **Program.cs** dosyası:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Son olarak aşağıdaki satırları ekleyin **ana** yöntemi. Bağlantı dizesi ile değiştirin **dinleme** sıra için izinleri:
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio'da, Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve seçin **başlangıç projelerini Ayarla**. Seçin **birden fazla başlangıç projesi**seçeneğini belirleyip **Başlat** için eylem olarak **ReadDeviceToCloudMessages**, **SimulatedDevice**, ve **ReadCriticalQueue** projeleri.
2. Tuşuna **F5** üç konsol uygulamaları başlatın. **ReadDeviceToCloudMessages** uygulama tarafından gönderilen yalnızca kritik olmayan iletileri sahip **SimulatedDevice** uygulama ve **ReadCriticalQueue** uygulamanın yalnızca kritik iletileri yok.
   
   ![Üç konsol uygulamaları][50]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, IOT Hub'ının ileti yönlendirme işlevini kullanarak cihaz bulut iletilerini güvenilir bir şekilde gönderme öğrendiniz.

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl] [ lnk-c2d] aygıtlarınıza çözüm arka ucu iletileri göndermek nasıl gösterir.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT paketi][lnk-suite].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz: [IOT Hub Geliştirici Kılavuzu].

IOT Hub içinde ileti yönlendirme hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[IOT Hub ile çalışmaya başlama]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IOT Geliştirme Merkezi]: https://azure.microsoft.com/develop/iot
[geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
