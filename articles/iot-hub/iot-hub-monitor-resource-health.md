---
title: Azure IOT Hub'ınızın sağlığını izleyin | Microsoft Docs
description: Azure İzleyici ve Azure kaynak durumu, IOT Hub'ınıza izlemek ve sorunları hızla giderip tanılamak için kullanın
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: kgremban
ms.openlocfilehash: 6dea1add1e329cfc894068732898a856a69c9b4c
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274051"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Azure IOT Hub durumunu izleyin ve sorunları hızla tanılayın

Azure IOT hub'ı uygulayan işletmelerin kaynaklarını güvenilir performans bekler. İşlemlerinizi Kapat bir saatin sürdürmenize yardımcı olmak için IOT hub'ı tam olarak tümleşiktir [Azure İzleyici](../azure-monitor/index.yml) ve [Azure kaynak durumu](../service-health/resource-health-overview.md). IOT çözümlerinizi ve sağlıklı bir durumda çalışır durumda tutmak için ihtiyacınız olan verileri sağlamak için bu iki hizmet çalışır.

Azure İzleyici, izleme ve günlüğe kaydetme için tüm Azure Hizmetleri tek bir kaynaktır. Özel işleme için Azure İzleyici günlükleri, Event Hubs veya Azure depolama için Azure İzleyici oluşturan tanılama günlüklerini gönderebilirsiniz. Azure İzleyicisi'nin ölçümleri ve tanılama ayarları, kaynaklarınızın performansını görünürlük sağlar. Bilgi edinmek için bu makaleyi okumaya devam edin nasıl [kullanımı Azure İzleyici](#use-azure-monitor) IOT hub'ınızla. 

> [!IMPORTANT]
> Azure İzleyici tanılama günlüklerini kullanarak IOT Hub hizmeti tarafından oluşturulan olayları güvenilir ya da sıralı olmasını garanti edilmez. Bazı olayları kaybolabilir veya düzensiz teslim. Tanılama günlükleri de gerçek zamanlı olacak şekilde tasarlanmış değildir ve hedef ettiğiniz için günlüğe kaydedilecek olayları için birkaç dakika sürebilir.

Azure kaynak durumu, tanılamanıza ve bir Azure sorunu kaynaklarınızı etkilediğinde destek almanıza yardımcı olur. Bir Pano her IOT hub'ları için mevcut ve eski sistem durumunu sağlar. Bilgi edinmek için bu makalenin alt kısmına devam nasıl [kullanımı Azure kaynak durumu](#use-azure-resource-health) IOT hub'ınızla. 

IOT Hub, IOT kaynaklarınızın durumunu anlamak için kullanabileceğiniz, kendi ölçümleri de sağlar. Daha fazla bilgi için bkz. [IOT Hub'ı anlama ölçümleri](iot-hub-metrics.md).

## <a name="use-azure-monitor"></a>Azure İzleyici’yi kullanma

Azure İzleyici, Azure kaynakları için IOT hub'ınıza içinde gerçekleşmesi işlemleri izleyebilirsiniz anlamına gelir. tanılama bilgileri sağlar.

Azure İzleyicisi'nin tanılama ayarları değiştirir, IOT Hub işlemlerini izleyin. İşlem izleme şu anda kullanıyorsanız, iş akışlarınızı geçirmeniz gerekir. Daha fazla bilgi için [işlem için tanılama ayarları İzleme'ten geçiş](iot-hub-migrate-to-diagnostics-settings.md).

Azure İzleyici izleyen olaylarını ve belirli ölçümleri hakkında daha fazla bilgi için bkz: [Azure İzleyici ile desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md) ve [desteklenen hizmetler, şemalar ve kategoriler için Azure tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-schema.md).

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Günlükleri anlama

Azure İzleyici, IOT Hub'ında gerçekleşen farklı işlem izler. Her kategorinin, bu kategoriye giren olayları nasıl bildirildiğini tanımlayan bir şema vardır.

#### <a name="connections"></a>Bağlantılar

Bağlantı kategorisi parçaları cihaz bağlayın ve hataları yanı sıra IOT hub'ı, bağlantıyı kesme olayları. Bu kategorideki cihazlar bağlantısı kesildiğinde uyarı ve yetkisiz bağlantı girişimleri tanımlama için kullanışlıdır.

> [!NOTE]
> Cihaz güvenilir bir bağlantı durumunu denetleme [cihaz sinyal](iot-hub-devguide-identity-registry.md#device-heartbeat).

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
            "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddress\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
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
            "properties": "{\"deviceId\":<messageSize>, \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="distributed-tracing-preview"></a>Dağıtılmış izleme (Önizleme)

Dağıtılmış izleme kategorisi izleme bağlamı üstbilgisine iletileri için bağıntı kimlikleri izler. Bu günlükler tam olarak etkinleştirmek için istemci tarafı kod izleyerek güncelleştirilmelidir [analiz ve IOT uygulamaları için uçtan uca IOT hub'ı dağıtılmış izleme (Önizleme) ile tanılama](iot-hub-distributed-tracing.md).

Unutmayın `correlationId` uyan [W3C izleme bağlamı](https://github.com/w3c/trace-context) teklifi, burada içerdiği bir `trace-id` yanı sıra bir `span-id`.

##### <a name="iot-hub-d2c-device-to-cloud-logs"></a>IOT hub'ı D2C (cihaz-bulut) günlükleri

IOT Hub, IOT Hub geçerli izleme özelliklerini içeren bir ileti geldiğinde, bu günlük kaydeder.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubD2C",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-0144d2590aacd909-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Receive message success",
            "durationMs": "",
            "properties": "{\"messageSize\": 1, \"deviceId\":\"<deviceId>\", \"callerLocalTimeUtc\": : \"2017-02-22T03:27:28.633Z\", \"calleeLocalTimeUtc\": \"2017-02-22T03:27:28.687Z\"}",
            "location": "Resource location"
        }
    ]
}
```

Burada, `durationMs` hesaplanmaz IOT Hub'ın saati cihaz saatiyle eşitlenmiş olmayabilir ve bu nedenle süresi hesaplaması yanıltıcı olabilir. Veritabanındaki tarih damgası kullanarak mantığı yazmak öneririz `properties` CİHAZDAN buluta gecikme ani değişiklikleri yakalamak için bölüm.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **messageSize** | Tamsayı | CİHAZDAN buluta ileti bayt cinsinden boyutu |
| **deviceId** | ASCII 7 bit alfasayısal karakter dizesi | Cihaz kimliği |
| **callerLocalTimeUtc** | UTC zaman damgası | Cihaz yerel saat tarafından belirlendiği şekilde iletinin oluşturma zamanı |
| **calleeLocalTimeUtc** | UTC zaman damgası | IOT Hub Hizmet tarafı saat tarafından belirlendiği şekilde, IOT Hub'ının geçidinde ileti geliş saati |

##### <a name="iot-hub-ingress-logs"></a>IOT hub'ı giriş günlükleri

İç veya yerleşik olay hub'ına geçerli izleme özelliklerini içeren bir ileti yazar, IOT hub'ı bu günlüğe kaydeder.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubIngress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-349810a9bbd28730-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Ingress message success",
            "durationMs": "10",
            "properties": "{\"isRoutingEnabled\": \"true\", \"parentSpanId\":\"0144d2590aacd909\"}",
            "location": "Resource location"
        }
    ]
}
```

İçinde `properties` bölümü, bu günlük iletisi giriş hakkında ek bilgiler içerir.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **isRoutingEnabled** | String | True veya false, IOT Hub'ında ileti yönlendirme etkin olup olmadığını gösterir |
| **parentSpanId** | String | [Yayılma kimliği](https://w3c.github.io/trace-context/#parent-id) D2C iletisini izlemeyi bu durumda olabilecek üst iletisi |

##### <a name="iot-hub-egress-logs"></a>IOT hub'ı çıkış günlükleri

IOT hub'ı kayıtları bu oturum zaman [yönlendirme](iot-hub-devguide-messages-d2c.md) etkinleştirilir ve ileti yazılan bir [uç nokta](iot-hub-devguide-endpoints.md). Yönlendirme etkin değilse, IOT hub'ı bu günlüğe kaydetmez.

```json
{
    "records":
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "DiagnosticIoTHubEgress",
            "category": "DistributedTracing",
            "correlationId": "00-8cd869a412459a25f5b4f31311223344-98ac3578922acd26-01",
            "level": "Information",
            "resultType": "Success",
            "resultDescription":"Egress message success",
            "durationMs": "10",
            "properties": "{\"endpointType\": \"EventHub\", \"endpointName\": \"myEventHub\", \"parentSpanId\":\"349810a9bbd28730\"}",
            "location": "Resource location"
        }
    ]
}
```

İçinde `properties` bölümü, bu günlük iletisi giriş hakkında ek bilgiler içerir.

| Özellik | Tür | Açıklama |
|--------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------|
| **Uçnoktaadı** | String | Yönlendirme uç nokta adı |
| **EndpointType** | String | Yönlendirme uç noktası türü |
| **parentSpanId** | String | [Yayılma kimliği](https://w3c.github.io/trace-context/#parent-id) üst iletinin, IOT hub'ı giriş ileti izleme bu durumda olacaktır |

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
        var d2cPartitions = eventHubClient.GetRuntimeInformationAsync().PartitionIds;
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

Azure IOT Hub durumu bölge düzeyinde gösterir. Bölgesel bir kesinti IOT hub'ınıza etkiliyorsa olarak sistem durumunu gösteren **bilinmeyen**. Daha fazla bilgi için bkz. [kaynak türleri ve sistem durumu denetimleri bulunan Azure kaynak durumu](../service-health/resource-health-checks-resource-types.md).

IOT hub'ları durumunu denetlemek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Gidin **hizmet durumu** > **kaynak durumu**.

3. Aşağı açılan kutularından aboneliğinizi seçin, ardından seçin **IOT hub'ı** kaynak türü.

Sistem Durumu verileri nasıl yorumlayacağınız hakkında daha fazla bilgi için bkz: [Azure kaynak durumu genel bakış](../service-health/resource-health-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [IOT hub'ı ölçüleri anlama](iot-hub-metrics.md)
* [IOT Uzaktan izleme ve IOT hub ve posta kutusu bağlanan Azure Logic Apps ile bildirimleri](iot-hub-monitoring-notifications-with-azure-logic-apps.md)