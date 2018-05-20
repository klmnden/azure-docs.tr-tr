---
title: Azure IOT Hub'ınızı sağlığını izlemek | Microsoft Docs
description: IOT Hub'ınızı izlemek ve sorunları hızla tanılamak için Azure İzleyici ve Azure kaynak durumu kullanın
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: kgremban
ms.openlocfilehash: bf6202b002aaf6d89a30c7c653fdcee00cb50290
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Azure IOT Hub durumunu izlemenize ve sorunları hızla tanılamak

Azure IOT Hub uygulamak işletmeler kendi kaynaklardan güvenilir performans bekler. İşlemlerinizi Kapat watch'unuzda sürdürmenize yardımcı olmak için IOT hub'ı tam olarak tümleşiktir [Azure İzleyici] [ lnk-AM] ve [Azure kaynak durumu] [ lnk-ARH]. Bu iki hizmet IOT çözümlerinizi ve iyi durumda çalışan tutmak için gereken verileri sağlamak için kademeli çalışın. 

Azure İzleyicisi ve izleme için tüm Azure hizmetlerinde oturum tek bir kaynaktır. Özel işleme için günlük analizi, olay hub'ları veya Azure Storage Azure İzleyici oluşturur günlüklerini gönderebilirsiniz. Azure monitörün ölçümleri ve tanılama ayarları kaynaklarınızı performansını gerçek zamanlı görünürlük sağlar. Bilgi edinmek için bu makaleyi okuduktan devam nasıl [kullanım Azure İzleyici](#use-azure-monitor) IOT hub'ınızı ile. 

Azure kaynak durumu tanılamanıza ve Azure sorunları etkiler, kaynaklarınızın destek alma yardımcı olur. Kişiselleştirilmiş bir pano, IOT hub'larınız için geçerli ve geçmiş sistem durumunu sağlar. Bilgi edinmek için bu makaleyi okuduktan devam nasıl [kullanım Azure kaynak durumu](#use-azure-resource-health) IOT hub'ınızı ile. 

Bu iki Hizmetleri ile tümleştirme ek olarak, IOT Hub ayrıca IOT kaynaklarınızın durumunu anlamak için kullanabileceğiniz kendi ölçümleri sağlar. Daha fazla bilgi için bkz: [IOT Hub'ı anlamak ölçümleri][lnk-metrics].

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure İzleyicisi, IOT hub'ınızı içinde gerçekleşmesi işlemleri izleyebilirsiniz anlamına gelir kaynak düzeyi tanılama bilgileri sağlar. 

Azure monitörün tanılama ayarlarını değiştirir IOT hub'ı işlemlerini izleyebilirsiniz. İzleme işlemleri şu anda kullanırsanız, iş akışlarınızı geçirmeniz gerekir. Daha fazla bilgi için bkz: [Operations Tanılama izleme ayarlarını geçirme][lnk-migrate].

Belirli ölçümleri ve Azure İzleyici izleyen olaylar hakkında daha fazla bilgi için bkz: [desteklenen Azure İzleyicisi ile ölçümleri] [ lnk-AM-metrics] ve [için Azure Hizmetleri, şemalar ve kategorileri desteklenen Tanılama günlüklerini][lnk-AM-schemas].

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Günlükleri anlama

Azure İzleyici IOT hub'da ortaya farklı işlemlerini izler. Her kategori bu kategorideki olayları nasıl bildirilir tanımlayan bir şema sahiptir. 

#### <a name="connections"></a>Bağlantılar

Bağlantıları kategorisi cihazlar bağlanmak veya IOT hub'ından bağlantısını oluşan hataları izler. Bu kategori izleme, yetkisiz bağlantı denemeleri tanımlamak için ve zamanlar zayıf bağlantıya alanlarında cihazlar için bir bağlantı kesildiğinde izlemek için yararlıdır.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Information",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}", 
    "location": "Resource location"
}
```

#### <a name="cloud-to-device-commands"></a>Bulut cihaz komutları

Bulut-cihaz komutlarını kategori IOT hub'ına oluşur ve bulut-cihaz ileti ardışık düzene ilgili hatalar izler. Bu kategori (örneğin, yetkisiz gönderen) bulut cihaza ileti gönderme, (örneğin, teslimat sayısı aşıldı) bulut-cihaz iletilerini alma ve bulut-cihaz ileti geri bildirim (görüş süresi gibi) alırken oluşan hataları içerir. Bu kategori, bulut cihaz iletisi başarılı bir şekilde teslim, yanlış bir bulut cihaz iletiyi işleyen bir aygıtı hatalarından yakalamaz.

```json
{
    "time": " UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "messageExpired",
    "category": "C2DCommands",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddresss\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
    "location": "Resource location"
}
```

#### <a name="device-identity-operations"></a>Aygıt Kimliği işlemleri

Cihaz kimliği işlemleri kategorisi oluşturmak, güncelleştirmek ya da bir giriş, IOT hub'ın kimlik kayıt defterinde silme girişimi sırasında oluşan hataları izler. Bu kategori izleme senaryoları sağlamak için kullanışlıdır.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "get",
    "category": "DeviceIdentityOperations",
    "level": "Error",    
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
    "location": "Resource location"
}
```

#### <a name="routes"></a>Yollar

İleti yönlendirme kategorisi ileti rota değerlendirme ve IOT Hub tarafından algılanan gibi uç noktası sistem durumu sırasında oluşan hataları izler. Bir kural "tanımsız" değerlendirildiğinde zaman IOT hub'ı bir uç nokta olarak atılacak ve bir uç noktasından alınan hatalarını işaretler gibi bu kategoriyi olaylarını içerir. Bu kategori "cihaz telemetrisi" kategorisi altında bildirilen iletilerini kendileri (örneğin, aygıt) hataları azaltma hakkında belirli hataları içermez.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "endpointUnhealthy",
    "category": "Routes",
    "level": "Error",
    "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
    "location": "Resource location"
}
```

#### <a name="device-telemetry"></a>Cihaz telemetrisi

Cihaz telemetri kategorisini IOT hub'ına oluşur ve telemetri ardışık düzene ilgili hatalar izler. Bu kategori (örneğin, azaltma) telemetri olayları gönderirken oluşan hataları içeren ve telemetri olayları (örneğin, yetkisiz okuyucu) alma. Bu kategori cihazda çalışan kod nedeni hatalarını yakalama olamaz.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
    "location": "Resource location"
}
```

#### <a name="file-upload-operations"></a>Dosya karşıya yükleme işlemleri

Dosya karşıya yükleme kategori IOT hub'ına oluşur ve dosya karşıya yükleme işlevselliği için ilgili hatalar izler. Bu kategori içerir:

* Bir aygıt tamamlanan karşıya yükleme hub'ını bildirir önce onu süresi dolduğunda gibi SAS URI'si ile oluşan hatalar.
* Cihaz tarafından raporlanan yüklemeler başarısız oldu.
* Bir dosya depolama alanına IOT hub'ı bildirim iletisi oluşturma sırasında bulunamadı kullanırken oluşan hatalar.

Bu kategori, cihaz bir dosya depolama alanına yükleme doğrudan oluşan hatalar catch olamaz.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "ingress",
    "category": "FileUploadOperations",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
    "location": "Resource location"
}
```

#### <a name="cloud-to-device-twin-operations"></a>Bulut-cihaz çifti işlemleri

Bulut-cihaz çifti işlemleri kategori cihaz çiftlerini hizmet tarafından başlatılan olayları izler. Bu işlemler get twin dahil, bildirilen özelliklerini güncelleştirmek ve istenen özelliklerine abone

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "read",
    "category": "C2DTwinOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}", 
    "location": "Resource location"
}
```

#### <a name="device-to-cloud-twin-operations"></a>Cihaz bulut çifti işlemleri

Cihaz bulut çifti işlemleri kategori cihaz çiftlerini cihaz tarafından başlatılan olayları izler. Bu işlemler get twin dahil edebilir, güncelleştirmek veya etiketleri, değiştirmek ve güncelleştirme veya istenen özelliklerini değiştirin. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "update",
    "category": "D2CTwinOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}", 
    "location": "Resource location"
}
```

#### <a name="twin-queries"></a>Twin sorguları

Twin sorguları kategori sorgu isteklerinde bulutta başlatılan cihaz çiftlerini için raporlar. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "query",
    "category": "TwinQueries",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}", 
    "location": "Resource location"
}
```

#### <a name="jobs-operations"></a>İş işlemleri

Cihaz çiftlerini güncelleştirmek veya doğrudan yöntemlerini birden çok aygıta çağırmak için iş isteklerinde işleri işlemleri kategorisi bildirir. Bu istekleri bulutta başlatılır. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "jobCompleted",
    "category": "JobsOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}", 
    "location": "Resource location"
}
```

#### <a name="direct-methods"></a>Doğrudan yöntemleri

Doğrudan yöntemleri kategori tek cihazlara gönderilen istek-yanıt etkileşimleri izler. Bu istekleri bulutta başlatılır. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "send",
    "category": "DirectMethods",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\", \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}", 
    "location": "Resource location"
}
```

### <a name="read-logs-from-azure-event-hubs"></a>Azure Event Hubs okuma günlükleri

Tanılama Ayarları'nda oturum olay ayarladıktan sonra günlüğünü okuyun ve böylece bunları içindeki bilgileri temel önlem uygulamalar oluşturabilirsiniz. Bu örnek kod, bir olay hub'ından günlüklerini alır:

```csharp
class Program 
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}"; 
    static string monitoringEndpointName = "{your AMS event hub endpoint name}"; 
    static EventHubClient eventHubClient; 
//This is the Diagnostic Settings schema 
    class AzureMonitorDiagnosticLog 
    { 
        string time { get; set; } 
        string resourceId { get; set; } 
        string operationName { get; set; } 
        string category { get; set; } 
        string level { get; set; } 
        string resultType { get; set; } 
        string resultDescription { get; set; } 
        string durationMs { get; set; } 
        string callerIpAddress { get; set; } 
        string correlationId { get; set; } 
        string identity { get; set; } 
        string location { get; set; } 
        Dictionary<string, string> properties { get; set; } 
    }; 
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
                var deserializer = new JavaScriptSerializer(); 
                //deserialize json data to azure monitor object 
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result); 
 
            } 
        } 
    } 
} 
```

## <a name="use-azure-resource-health"></a>Azure kaynak durumu kullanın

Azure kaynak durumu, IOT hub'ınızı çalışır durumda olup olmadığını izlemek için kullanın. Ayrıca bölgesel bir kesintinin IOT hub'ınızı sağlığını etkileyen olup olmadığını öğrenin. Azure IOT Hub'ınızı sistem durumunu belirli ayrıntılarını anlamak için öneririz, [kullanım Azure İzleyici](#use-azure-monitor). 

Azure IOT Hub, bölgesel düzeyde sistem durumu gösterir. Var. IOT hub'ınızı etkileyen bölgesel bir kesintinin sistem durumu olarak gösteriliyorsa **bilinmeyen**. Azure kaynak durumu yaptığı belirli sistem durumu denetimleri hakkında daha fazla bilgi için bkz: [denetler kaynak türleri ve sistem durumu, Azure kaynak durumu][lnk-ARH-checks].

IOT hub'larınız durumunu denetlemek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Gidin **hizmet durumu** > **kaynak durumu**.
1. Aşağı açılan kutularından aboneliğinizi seçin ve **IOT hub'ı**.

Sistem Durumu verileri yorumlama hakkında daha fazla bilgi için bkz: [Azure kaynak sistem durumu genel bakış][lnk-ARH]

## <a name="next-steps"></a>Sonraki adımlar

- [IOT hub'ı ölçümleri anlama][lnk-metrics]
- [IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanma Azure Logic Apps ile bildirimleri][lnk-monitoring-notifications]


[lnk-AM]: ../monitoring-and-diagnostics/index.yml
[lnk-ARH]: ../service-health/resource-health-overview.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-migrate]: iot-hub-migrate-to-diagnostics-settings.md
[lnk-AM-metrics]: ../monitoring-and-diagnostics/monitoring-supported-metrics.md
[lnk-AM-schemas]: ../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md
[lnk-ARH-checks]: ../service-health/resource-health-checks-resource-types.md
[lnk-monitoring-notifications]: iot-hub-monitoring-notifications-with-azure-logic-apps.md
