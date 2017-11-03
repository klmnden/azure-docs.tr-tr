---
title: "Azure IOT hub'ı işlemleri izleme | Microsoft Docs"
description: "Azure IOT Hub işlemleri gerçek zamanlı IOT hub'ınızı işlemlerinin durumunu izlemek için izleme kullanma"
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: nberdy
ms.openlocfilehash: 3eafa32907c8f68cfc44cb2771d625349ff42003
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-operations-monitoring"></a>IOT hub'ı işlemlerini izleme

IOT hub'ı işlemlerini izleme işlemleri gerçek zamanlı IOT hub'ınızı durumunu izlemenize olanak sağlar. IOT hub'ı operations birkaç kategoriler arasında olayları izler. İşleme için IOT hub'ınızın bir uç nokta için bir veya daha fazla kategorilerden olayları göndermeyi seçebilirsiniz. Hatalar için verileri izlemek veya veri düzenlerini esas alarak daha karmaşık işleme ayarlayın.

>[!NOTE]
>IOT hub'ı operations izleme kullanım dışıdır ve IOT Hub'ından gelecekte kaldırılacaktır. IOT Hub durumunu ve işlemlerini izlemek için bkz: [Azure IOT Hub durumunu izlemenize ve sorunları hızla tanılamak][lnk-monitor]. Kullanımdan kaldırma zaman çizelgesi hakkında daha fazla bilgi için bkz: [Azure İzleyici ve Azure kaynak durumu ile Azure IOT çözümlerinizi izlemek][lnk-blog-announcement].

IOT hub'ı olayların altı kategoriye izler:

* Aygıt Kimliği işlemleri
* Cihaz telemetrisi
* Bulut-cihaz iletilerini
* Bağlantılar
* Dosya yüklemeleri
* İleti yönlendirme

> [!IMPORTANT]
> IOT hub'ı operations izleme olayları güvenilir ya da sıralı teslimini garanti etmez. IOT hub'ı temel altyapınıza bağlı olarak, bazı olaylar kaybolabilir veya bozuk teslim. Başarısız bağlantı denemesi veya belirli cihazlar için yüksek sıklıkta bağlantısının kesilmesi gibi hata sinyalleri dayanan uyarıları oluşturmak için izleme işlemleri kullanın. Cihaz durumu için tutarlı bir deposu oluşturmak için olay izleme işlemleri dayanarak doğrulamamalısınız, örneğin izleme deposu bağlı veya aygıtın durumunu bağlantısı kesildi. 

## <a name="how-to-enable-operations-monitoring"></a>İzleme işlemlerini etkinleştirme

1. IOT hub'ı oluşturun. Bir IOT hub oluşturma hakkında yönergeler bulabilirsiniz [Get Started] [ lnk-get-started] Kılavuzu.

1. IOT hub'ınızı dikey penceresini açın. Buradan, tıklatın **izleme işlemleri**.

    ![Yapılandırma portalında izleme erişim işlemleri][1]

1. İzleme ve ardından istediğiniz izleme kategorileri seçin **kaydetmek**. Olaylar listelenen Event Hub ile uyumlu uç noktasından okumak için kullanılabilir **izleme ayarlarını**. IOT hub'ı uç adlı `messages/operationsmonitoringevents`.

    ![IOT hub'ınızı üzerinde izleme işlemlerini yapılandırma][2]

> [!NOTE]
> Seçme **ayrıntılı** için izleme **bağlantıları** kategori ek tanılama iletilerini oluşturmak için IOT Hub neden olur. Diğer tüm kategorileri için **ayrıntılı** değişiklikleri IOT hub'ı bilgi miktarını ayarlama her hata iletisi içerir.

## <a name="event-categories-and-how-to-use-them"></a>Olay kategorilerini ve bunların nasıl kullanılacağını

Her kategori parçaları izleme işlemleri farklı türde bir IOT Hub ve her izleme kategorisi etkileşimi bu kategorideki olayları nasıl yapılandırıldığı tanımlayan bir şema sahiptir.

### <a name="device-identity-operations"></a>Aygıt Kimliği işlemleri

Cihaz kimliği işlemleri kategorisi oluşturmak, güncelleştirmek ya da bir giriş, IOT hub'ın kimlik kayıt defterinde silme girişimi sırasında oluşan hataları izler. Bu kategori izleme senaryoları sağlamak için kullanışlıdır.

```json
{
    "time": "UTC timestamp",
    "operationName": "create",
    "category": "DeviceIdentityOperations",
    "level": "Error",
    "statusCode": 4XX,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "durationMs": 1234,
    "userAgent": "userAgent",
    "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Cihaz telemetrisi

Cihaz telemetri kategorisini IOT hub'ına oluşur ve telemetri ardışık düzene ilgili hatalar izler. Bu kategori (örneğin, azaltma) telemetri olayları gönderirken oluşan hataları içeren ve telemetri olayları (örneğin, yetkisiz okuyucu) alma. Bu kategori cihazda çalışan kod nedeni hatalarını yakalama olamaz.

```json
{
    "messageSizeInBytes": 1234,
    "batching": 0,
    "protocol": "Amqp",
    "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Bulut cihaz komutları

Bulut-cihaz komutlarını kategori IOT hub'ına oluşur ve bulut-cihaz ileti ardışık düzene ilgili hatalar izler. Bu kategori (örneğin, yetkisiz gönderen) bulut cihaza ileti gönderme, (örneğin, teslimat sayısı aşıldı) bulut-cihaz iletilerini alma ve bulut-cihaz ileti geri bildirim (görüş süresi gibi) alırken oluşan hataları içerir. Bu kategori, bulut cihaz iletisi başarılı bir şekilde teslim, yanlış bir bulut cihaz iletiyi işleyen bir aygıtı hatalarından yakalamaz.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="connections"></a>Bağlantılar

Bağlantıları kategorisi cihazlar bağlanmak veya IOT hub'ından bağlantısını oluşan hataları izler. Bu kategori izleme, yetkisiz bağlantı denemeleri tanımlamak için ve zamanlar zayıf bağlantıya alanlarında cihazlar için bir bağlantı kesildiğinde izlemek için yararlıdır.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Dosya yüklemeleri

Dosya karşıya yükleme kategori IOT hub'ına oluşur ve dosya karşıya yükleme işlevselliği için ilgili hatalar izler. Bu kategori içerir:

* Bir aygıt tamamlanan karşıya yükleme hub'ını bildirir önce onu süresi dolduğunda gibi SAS URI'si ile oluşan hatalar.
* Cihaz tarafından raporlanan yüklemeler başarısız oldu.
* Bir dosya depolama alanına IOT hub'ı bildirim iletisi oluşturma sırasında bulunamadı kullanırken oluşan hatalar.

Bu kategori, cihaz bir dosya depolama alanına yükleme doğrudan oluşan hatalar catch olamaz.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>İleti yönlendirme

İleti yönlendirme kategorisi ileti rota değerlendirme ve IOT Hub tarafından algılanan gibi uç noktası sistem durumu sırasında oluşan hataları izler. Bir kural "tanımsız" değerlendirildiğinde zaman IOT hub'ı bir uç nokta olarak atılacak ve bir uç noktasından alınan hatalarını işaretler gibi bu kategoriyi olaylarını içerir. Bu kategori "cihaz telemetrisi" kategorisi altında bildirilen iletilerini kendileri (örneğin, aygıt) hataları azaltma hakkında belirli hataları içermez.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a>Etkinlikleri görüntüleme

Kullanabileceğiniz *iothub-explorer* hızla IOT hub'ınızı izleme olayları oluşturmak, sınamak için aracı. Aracı yüklemek için yönergeleri görmek [iothub-explorer] [ lnk-iothub-explorer] GitHub depo.

1. Emin olun **bağlantıları** kategori izleme ayarlanmış **ayrıntılı** Portalı'nda.

1. Bir komut isteminde, izleme uç noktasından okumak için aşağıdaki komutu çalıştırın:

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. Başka bir komut isteminden, cihaz-bulut iletileri gönderen bir cihazın benzetimini yapmak için aşağıdaki komutu çalıştırın:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. Sanal cihaz IOT hub'ına bağlandığında ilk komut istemi izleme olayları gösterir.

## <a name="connect-to-the-monitoring-endpoint"></a>İzleme uç noktasına bağlanın

IOT hub'ınızı izleme uç noktada bir Event Hub ile uyumlu uç noktadır. İzleme iletileri Bu uç noktasından okumak için Event Hubs ile çalışan herhangi bir mekanizma kullanabilirsiniz. Aşağıdaki örnek, bir yüksek işleme dağıtımına uygun olmayan temel bir okuyucu oluşturur. Event Hubs'dan iletilerin nasıl işleneceği hakkında daha fazla bilgi için [Event Hubs ile Çalışmaya Başlama][lnk-eventhubs-tutorial] öğreticisine bakın.

İzleme uç noktasına bağlanmak için bir bağlantı dizesi ve uç nokta adı gerekir. Aşağıdaki adımlar Portalı'nda gerekli değerleri bulmaya nasıl gösterir:

1. Portalda, IOT Hub kaynağı dikey penceresine gidin.

1. Seçin **izleme işlemleri**ve Not **Event Hub ile uyumlu adı** ve **Event Hub ile uyumlu uç nokta** değerler:

    ![Olay Hub ile uyumlu uç nokta değerleri][img-endpoints]

1. Seçin **paylaşılan erişim ilkeleri**, ardından **hizmet**. Not **birincil anahtar** değeri:

    ![Hizmet paylaşılan erişim ilkesi birincil anahtarı][img-service-key]

Aşağıdaki C# kod örneği, Visual Studio'dan alınır **Windows Klasik Masaüstü** C# konsol uygulaması. Proje **WindowsAzure.ServiceBus** NuGet paketi yüklü.

* Bağlantı dizesi yer tutucusunu kullanan bir bağlantı dizesi ile değiştirin **Event Hub ile uyumlu uç nokta** ve hizmet **birincil anahtar** aşağıdaki örnekte gösterildiği gibi daha önce not ettiğiniz değerleri:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* İzleme uç nokta ad yer tutucusu ile Değiştir **Event Hub ile uyumlu adı** daha önce not ettiğiniz değer.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Bir aygıt ile Azure IOT kenar benzetimini yapma][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health
[lnk-monitor]: iot-hub-monitor-resource-health.md
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
