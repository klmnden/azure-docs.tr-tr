---
title: Azure IOT Hub'ınızın sağlığını izleyin | Microsoft Docs
description: Azure İzleyici ve Azure kaynak durumu, IOT Hub'ınıza izlemek ve sorunları hızla giderip tanılamak için kullanın
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: kgremban
ms.openlocfilehash: b22b2f5ce9e08b3ee345d6e614c82b7dc8330453
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53438702"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Azure IOT Hub durumunu izleyin ve sorunları hızla tanılayın

Azure IOT hub'ı uygulayan işletmelerin kaynaklarını güvenilir performans bekler. İşlemlerinizi Kapat bir saatin sürdürmenize yardımcı olmak için IOT hub'ı tam olarak tümleşiktir [Azure İzleyici] [ lnk-AM] ve [Azure kaynak durumu] [ lnk-ARH]. Bu iki hizmet ile IOT çözümlerinizi ve sağlıklı bir durumda çalışır durumda tutmak için ihtiyacınız olan verileri sağlamak üzere planlanmıştır çalışır. 

Azure İzleyici, izleme ve günlüğe kaydetme için tüm Azure Hizmetleri tek bir kaynaktır. Özel işleme için Log Analytics, Event Hubs veya Azure depolama için Azure İzleyici oluşturan tanılama günlüklerini gönderebilirsiniz. Azure İzleyicisi'nin ölçümleri ve tanılama ayarları, kaynaklarınızın performansını görünürlük sağlar. Bilgi edinmek için bu makaleyi okumaya devam edin nasıl [kullanımı Azure İzleyici](#use-azure-monitor) IOT hub'ınızla. 

> [!IMPORTANT]
> Azure İzleyici tanılama günlüklerini kullanarak IOT Hub hizmeti tarafından oluşturulan olayları güvenilir ya da sıralı olmasını garanti edilmez. Bazı olayları kaybolabilir veya düzensiz teslim. Tanılama günlükleri de gerçek zamanlı olacak şekilde tasarlanmış değildir ve hedef ettiğiniz için günlüğe kaydedilecek olayları için birkaç dakika sürebilir.

Azure kaynak durumu, tanılamanıza ve bir Azure sorunu kaynaklarınızı etkilediğinde destek almanıza yardımcı olur. Kişiselleştirilmiş bir panoya, IOT hub'ları için mevcut ve eski sistem durumunu sağlar. Bilgi edinmek için bu makaleyi okumaya devam edin nasıl [kullanımı Azure kaynak durumu](#use-azure-resource-health) IOT hub'ınızla. 

IOT Hub, IOT kaynaklarınızın durumunu anlamak için kullanabileceğiniz, kendi ölçümleri de sağlar. Daha fazla bilgi için bkz. [IOT Hub'ı anlama ölçümleri][lnk-metrics].

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure İzleyici, Azure kaynakları için IOT hub'ınıza içinde gerçekleşmesi işlemleri izleyebilirsiniz anlamına gelir. tanılama bilgileri sağlar. 

Azure İzleyicisi'nin tanılama ayarları değiştirir, IOT Hub işlemlerini izleyin. İşlem izleme şu anda kullanıyorsanız, iş akışlarınızı geçirmeniz gerekir. Daha fazla bilgi için [işlem için tanılama ayarları İzleme'ten geçiş][lnk-migrate].

Azure İzleyici izleyen olaylarını ve belirli ölçümleri hakkında daha fazla bilgi için bkz: [Azure İzleyici ile desteklenen ölçümler] [ lnk-AM-metrics] ve [için Azure Hizmetleri, şemalar ve kategoriler desteklenir Tanılama günlükleri][lnk-AM-schemas].

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Günlükleri anlama

Azure İzleyici, IOT Hub'ında gerçekleşen farklı işlem izler. Her kategorinin, bu kategoriye giren olayları nasıl bildirildiğini tanımlayan bir şema vardır. 

#### <a name="connections"></a>Bağlantılar

Bağlantı kategorisi parçaları cihaz bağlayın ve hataları yanı sıra IOT hub'ı, bağlantıyı kesme olayları. Bu kategorideki cihazlar bağlantısı kesildiğinde uyarı ve yetkisiz bağlantı girişimleri tanımlama için kullanışlıdır.

> [!NOTE]
> Cihaz güvenilir bir bağlantı durumunu denetleme [cihaz sinyal][lnk-devguide-heartbeat].


```json
{
    "records": 
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "deviceConnect",
            "category": "Connections",
            "level": "Information",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-commands"></a>Buluttan cihaza komutlar

Bulut-cihaz komutlarını kategorisi, IOT hub ve bulut-cihaz ileti işlem hattına ilgili hataları izler. Bu kategori, öğesinden oluşan hataları içerir:

* (Örneğin, yetkisiz bir gönderenden hatalar), bulut buluttan cihaza iletileri gönderme
* Bulut-cihaz iletilerini (gibi teslim sayısı aşıldı hataları), alma ve
* Bulut-cihaz ileti geri bildirim alan (geri bildirim gibi hataları süresi doldu). 

Bu kategori, bulut buluttan cihaza iletinin başarılı bir şekilde teslim ancak yanlış aygıt tarafından işlenen hataları yakalamaz.

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="device-identity-operations"></a>Cihaz kimlik işlemleri

Cihaz kimlik işlem kategorisi oluşturmak, güncelleştirmek veya IOT hub'ınızın kimlik kayıt defterinde bir girdiyi silmek açmaya çalıştığında oluşan hatalar izler. Bu kategori izleme senaryoları sağlamak için kullanışlıdır.

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="routes"></a>Yollar

İleti yönlendirme kategorisi ileti yönlendirme değerlendirme ve IOT Hub tarafından algılanan uç nokta sistem durumu sırasında oluşan hataları izler. Bu kategori, olayları gibi içerir:

* Bir kural "için tanımlanmamış", değerlendirir.
* IOT hub'ı bir uç nokta ölü işaretler veya
* Bir uç noktasından alınan tüm hatalar. 

Bu kategori, "cihaz telemetrisi" kategorisi altında bildirilen iletilerini kendileri (gibi cihaz) azaltma hataları, ilgili belirli hataları içermez.

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "endpointUnhealthy",
            "category": "Routes",
            "level": "Error",
            "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-telemetry"></a>Cihaz telemetrisi

Cihaz telemetrisi kategorisi, IOT hub ve telemetri ardışık düzene ilgili hataları izler. Bu kategori (azaltma gibi) telemetri olayları gönderirken oluşan hataları içerir ve telemetri olaylar (örneğin, yetkisiz okuyucusu) alma. Bu kategori, cihaz üzerinde çalışan kod tarafından neden olduğu hata yakalayamaz.

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="file-upload-operations"></a>Dosya karşıya yükleme işlemleri

Dosya karşıya yükleme kategorisi, IOT hub ve dosya karşıya yükleme işlevselliği ile ilgili hataları izler. Bu kategori içerir:

* Ne zaman süresi dolmadan önce tamamlanan bir karşıya yükleme hub'a bir cihaz bildirir gibi SAS URI'si ile oluşan hatalar.
* Cihaz tarafından bildirilen karşıya yükleme başarısız oldu.
* Bir dosya depolama alanında, IOT hub'ı bildirim iletisi oluşturulurken bulunmadığında, oluşan hataları.

Bu kategori, cihazın depolama için bir dosya yüklenirken doğrudan ortaya çıkan hataları yakalayamaz.

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="cloud-to-device-twin-operations"></a>Bulut-cihaz ikizi işlemleri

Bulut-cihaz ikizi işlem kategorisi üzerinde cihaz ikizlerini hizmet tarafından başlatılan olayları izler. Bu işlemler get ikizi dahil edebilir, güncelleştirme veya etiketleri, değiştirmek ve güncelleştirme veya istenen özellikleri değiştirin. 

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="device-to-cloud-twin-operations"></a>CİHAZDAN buluta ikizi işlemleri

Buluta cihaz ikizi işlem kategorisi üzerinde cihaz çiftlerini cihaz tarafından başlatılan olayları izler. Bu işlemler get ikizi dahil, bildirilen özellikleri güncelleştirmek ve istenen özellikler abone olun.

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="twin-queries"></a>Çifti sorguları

İkizi sorgularını kategorisi sorgu istekleri için bulutta başlatılan cihaz ikizlerini bildirir. 

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="jobs-operations"></a>İş işlemleri

Cihaz ikizlerini güncelleştirin veya birden fazla cihazda doğrudan metotları çağırma iş isteklerini işler işlem kategorisi raporlar. Bu istekler bulutta başlatılır. 

```json
{
    "records": 
    [
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
    ]
}
```

#### <a name="direct-methods"></a>Doğrudan yöntemler

Doğrudan yöntemler kategoriyi ayrı ayrı cihazlara gönderilen istek-yanıt etkileşimleri izler. Bu istekler bulutta başlatılır. 

```json
{
    "records": 
    [
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
    ]
}
```

### <a name="read-logs-from-azure-event-hubs"></a>Azure Event hubs'tan günlükleri okuma

Olay günlüğü tanılama ayarları ayarladıktan sonra günlüklerini okumak ve böylece bunları içindeki bilgileri temel alan bir eylem sürebilir uygulamalar oluşturabilirsiniz. Bu örnek kod, bir olay hub'ından günlükleri alır:

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

Azure kaynak durumu, IOT hub'ınıza hazır ve çalışır durumda olup olmadığını izlemek için kullanın. Ayrıca, IOT hub'ınızın sağlığını etkileyen bölgesel bir kesinti olup olmadığını öğrenebilirsiniz. Azure IOT Hub'ınıza sistem durumu hakkında belirli diğer ayrıntıları öğrenmek olmasını öneririz, [kullanımı Azure İzleyici](#use-azure-monitor). 

Azure IOT Hub durumu bölge düzeyinde gösterir. Bölgesel bir kesinti IOT hub'ınıza etkiliyorsa olarak sistem durumunu gösteren **bilinmeyen**. Daha fazla bilgi için bkz. [kaynak türleri ve sistem durumu denetimleri bulunan Azure kaynak durumu][lnk-ARH-checks].

IOT hub'ları durumunu denetlemek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Gidin **hizmet durumu** > **kaynak durumu**.
1. Aşağı açılan kutularından aboneliğinizi seçin ve **IOT hub'ı**.

Sistem Durumu verileri nasıl yorumlayacağınız hakkında daha fazla bilgi için bkz: [Azure kaynak durumu genel bakış][lnk-ARH]

## <a name="next-steps"></a>Sonraki adımlar

- [IOT hub'ı ölçüleri anlama][lnk-metrics]
- [IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanan Azure Logic Apps ile bildirimleri][lnk-monitoring-notifications]


[lnk-AM]: ../azure-monitor/index.yml
[lnk-ARH]: ../service-health/resource-health-overview.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-migrate]: iot-hub-migrate-to-diagnostics-settings.md
[lnk-AM-metrics]: ../azure-monitor/platform/metrics-supported.md
[lnk-AM-schemas]: ../azure-monitor/platform/tutorial-dashboards.md
[lnk-ARH-checks]: ../service-health/resource-health-checks-resource-types.md
[lnk-monitoring-notifications]: iot-hub-monitoring-notifications-with-azure-logic-apps.md
[lnk-devguide-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
