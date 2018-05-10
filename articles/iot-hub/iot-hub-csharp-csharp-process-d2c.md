---
title: İleti yönlendirme ile Azure IOT hub'ı (.Net) | Microsoft Docs
description: Diğer arka uç hizmetlerine iletilerinin gönderilmesi için yönlendirme kurallarını ve özel uç noktaları kullanarak Azure IOT Hub cihaz bulut iletilerini işlemek nasıl.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 84a74a59417d3d1b9ebe0e2ede6c105b6fb3a576
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="routing-messages-with-iot-hub-net"></a>IOT hub'ı (.NET) ile ileti yönlendirme

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

Ayrıca okumayı öneririz [Azure Storage] ve [Azure Service Bus].

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
            if (rand.NextDouble() > 0.5)
            {
                messageString = "This is a critical message";
                levelValue = "critical";
            }
            else 
            {
                messageString = "This is a storage message";
                levelValue = "storage";
            }
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

Bu yöntem rastgele özelliği ekler `"level": "critical"` ve `"level": "storage"` aygıt tarafından gönderilen iletiler için hangi benzetimini yapar, uygulama arka uç ya da kalıcı olarak depolanması gereken bir Acil eylem gerektiren bir ileti. Uygulama ileti gövdesinde temelinde yönlendirme iletileri destekler.

> [!NOTE]
> Burada gösterilen hot yolu örnek yanı sıra soğuk yolu işleme dahil olmak üzere çeşitli senaryoları için ileti özellikleri iletileri yönlendirmek için kullanabilirsiniz.

> [!NOTE]
> MSDN makalesinde önerilen üstel geri alma gibi bir yeniden deneme ilkesi uygulamak önerilir [geçici hata işleme].

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

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(İsteğe bağlı) Depolama kapsayıcısı IOT hub ve rota iletileri ona ekleyin

Bu bölümde, depolama hesabı oluşturma, IOT hub'ınıza bağlanın ve iletiyi bir özellik varlığına dayalı hesabına iletileri göndermek için IOT hub'ınızı yapılandırma. Depolama yönetme hakkında daha fazla bilgi için bkz: [Azure Storage ile çalışmaya başlama][Azure Storage].

 > [!NOTE]
   > Birle sınırlı değilse **Endpoint**, Kurulum **StorageContainer** ek olarak **CriticalQueue** ve her iki simulatneously çalıştırın.

1. [Azure depolama belgelerinde] [lnk-depolama] açıklandığı gibi bir depolama hesabı oluşturun. Hesap adını not edin.

2. Azure portalında, IOT hub'ınızı açın ve **uç noktaları**.

3. İçinde **uç noktaları** dikey penceresinde, select **CriticalQueue** uç noktası ve tıklatın **silmek**. Tıklatın **Evet**ve ardından **Ekle**. Uç nokta adı **StorageContainer** ve seçmek için açılır listeleri kullanın **Azure depolama kapsayıcısının**ve oluşturma bir **depolama hesabı** ve **depolama kapsayıcı**.  Adlarını not edin.  İşiniz bittiğinde tıklatın **Tamam** altındaki. 

 > [!NOTE]
   > Birle sınırlı değilse **Endpoint**, silme gerekmez **CriticalQueue**.

4. Tıklatın **yollar** IOT hub'ınızdaki. Tıklatın **Ekle** iletileri kuyruğa yönlendiren bir yönlendirme kuralı oluşturmak için dikey pencerenin üstündeki eklediğiniz. Seçin **cihaz iletilerini** veri kaynağı olarak. Girin `level="storage"` koşul olarak seçin **StorageContainer** yönlendirme kuralı uç noktası olarak özel bir uç noktası olarak. Tıklatın **kaydetmek** altındaki.  

    Emin olun geri dönüş rota ayarlanmış **ON**. Bu ayar, bir IOT hub'ın varsayılan yapılandırmadır.

1. Önceki uygulamalarınızı hala çalıştığından emin olun. 

1. Azure Portalı'nda altında depolama hesabınıza gidin **Blob hizmeti**, tıklatın **BLOB'lar Gözat...** .  Kapsayıcıyı seçin, gidin ve JSON dosyasına tıklayın ve tıklayın **karşıdan** verileri görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, IOT Hub'ının ileti yönlendirme işlevini kullanarak cihaz bulut iletilerini güvenilir bir şekilde gönderme öğrendiniz.

[IOT Hub ile bulut-cihaz iletilerini göndermek nasıl] [ lnk-c2d] aygıtlarınıza çözüm arka ucu iletileri göndermek nasıl gösterir.

IOT hub'ı kullanan tam uçtan uca çözümler örnekleri görmek için bkz: [Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı][lnk-suite].

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
[lnk-c2d]: iot-hub-csharp-csharp-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
