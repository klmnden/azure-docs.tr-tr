---
title: Azure işlevleri için Host.JSON başvurusu 1.x
description: Azure işlevleri host.json dosyasıyla v1 çalışma zamanı için başvuru belgeleri.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: glenga
ms.openlocfilehash: 44bc5a245d1bcbc8ff53991af4193ef86f7cd704
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436328"
---
# <a name="hostjson-reference-for-azure-functions-1x"></a>Azure işlevleri için Host.JSON başvurusu 1.x

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Sürüm 1](functions-host-json-v1.md)
> * [Sürüm 2](functions-host-json.md)

*Host.json* meta veri dosyası, tüm işlevler bir işlev uygulaması için etkileyen genel yapılandırma seçenekleri içerir. Bu makalede, v1 çalışma zamanı için kullanılabilen ayarları listelenir. JSON şemasını altındadır http://json.schemastore.org/host.

> [!NOTE]
> Bu makalede, Azure işlevleri için olan 1.x.  İşlevlerde host.json başvurusu için 2.x bkz [Azure işlevleri için host.json başvurusu 2.x](functions-host-json.md).

Başka bir işlev uygulaması yapılandırma seçenekleri yönetilir, [uygulama ayarları](functions-app-settings.md).

Bazı host.json ayarları yalnızca yerel olarak çalıştırırken kullanılan [local.settings.json](functions-run-local.md#local-settings-file) dosya.

## <a name="sample-hostjson-file"></a>Örnek host.json dosyası

Aşağıdaki örnek *host.json* dosyaları belirtilen tüm olası seçeneklerin sahiptir.


```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

Bu makalenin aşağıdaki bölümlerde, her bir üst düzey özellik açıklanmaktadır. Tümü aksi belirtilmediği sürece isteğe bağlıdır.

## <a name="aggregator"></a>Toplayıcı

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>Applicationınsights

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="durabletask"></a>durableTask

[!INCLUDE [durabletask](../../includes/functions-host-json-durabletask.md)]

## <a name="eventhub"></a>eventHub

İçin yapılandırma ayarlarını [olay hub'ı Tetikleyicileri ve bağlamaları](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>işlevler

Proje ana çalışan işlevlerin listesi. Boş bir dizi tüm işlevleri çalıştırma anlamına gelir. Kullanılmak üzere tasarlanmış yalnızca [yerel olarak çalışan](functions-run-local.md). Azure işlev uygulamaları bunun yerine adımları izlemelidir [işlevleri Azure işlevleri'nde devre dışı bırakma](disable-function.md) belirli işlevleri yerine bu ayarı devre dışı bırakmak için.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Tüm İşlevler için zaman aşımı süresini gösterir. Sunucusuz bir tüketim planı geçerli aralık 1 saniye için 10 dakika olan ve varsayılan değer 5 dakikadır. App Service planı, genel bir sınır yoktur ve varsayılan çalışma zamanı sürümüne bağlıdır.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>Durum İzleme

İçin yapılandırma ayarlarını [konak sistem durumu İzleyicisi](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|enabled|true|Özelliğin etkin olup olmadığını belirtir. | 
|healthCheckInterval|10 saniye|Düzenli arka plan sistem arasındaki zaman aralığını denetler. | 
|healthCheckWindow|2 dakika|İle birlikte kullanılan kayan zaman penceresini `healthCheckThreshold` ayarı.| 
|healthCheckThreshold|6|En fazla kaç kez, bir konak geri dönüştürme başlatılmadan önce sistem durumu denetimi başarısız olabilir.| 
|counterThreshold|0.80|Eşik, bir performans sayacı sağlıksız olarak kabul edilir.| 

## <a name="http"></a>http

İçin yapılandırma ayarlarını [http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

*Sürümü yalnızca 1.x.*

Bir proje konak için benzersiz kimliği. Tireler ile küçük harf GUID kaldırılabilir. Yerel olarak çalışan gereklidir. Azure'da çalışan bir kimlik değeri ayarlanmadı öneririz. Azure'da bir kimliği otomatik olarak oluşturulan olduğunda `id` atlanır. 

Birden fazla işlev uygulaması arasında bir depolama hesabını paylaşmak, her işlev uygulaması farklı olduğundan emin olun `id`. Atlayabilirsiniz `id` özelliği veya her işlevi uygulamanın el ile ayarlamanız `id` için farklı bir değer. Zamanlayıcı tetikleyicisi depolama kilidi olacağına dair yalnızca bir zamanlayıcı örneği birden çok örneği için bir işlev uygulaması kullanıma ölçeklendirildiğinde emin olmak için kullanır. İki işlev uygulamaları aynı paylaşıyorsa `id` ve her bir zamanlayıcı tetikleyicisi kullanan, tek bir zamanlayıcı çalışır.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Günlükçü

Tarafından yazılan günlükler için filtreleme denetimlerini bir [ILogger nesne](functions-monitoring.md#write-logs-in-c-functions) ya da [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|categoryFilter|yok|Kategoriye göre filtreleme belirtir| 
|defaultLevel|Bilgi|Belirtilen değil tüm kategorileri için `categoryLevels` dizi, bu düzeyde ve yukarıdaki günlükleri Application Insights'a Gönder.| 
|categoryLevels|yok|Her kategori için Application Insights'a gönderme için en düşük günlük düzeyi belirtir kategoriler dizisi. Burada belirtilen kategori aynı değeri ile başlayan tüm kategorileri denetler ve daha uzun değerleri önceliklidir. Yukarıdaki örnekte *host.json* dosyası günlüğüne "Host.Aggregator" ile başlayan tüm kategorileri `Information` düzeyi. "Ana" gibi "Host.Executor" ile başlayan diğer tüm kategorileri oturum `Error` düzeyi.| 

## <a name="queues"></a>Kuyruklar

İçin yapılandırma ayarlarını [depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md).

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxPollingInterval|60000|Kuyruk yoklamaları arasındaki milisaniye cinsinden en büyük aralık.| 
|visibilityTimeout|0|Bir ileti işlenirken yeniden denemeler arasındaki zaman aralığını başarısız olur.| 
|batchSize|16|İşlevler çalışma zamanı aynı anda alır ve paralel olarak işler sıra iletilerinin sayısı. İşlenmekte olan sayı ne zaman alır aşağı `newBatchThreshold`, çalışma zamanı başka bir toplu iş alır ve bu iletileri işlemeye başlıyor. Eş zamanlı iletileri işlev işlenen sayısı, bu nedenle `batchSize` artı `newBatchThreshold`. Bu sınır, ayrı ayrı her kuyruk ile tetiklenen bir işlev için de geçerlidir. <br><br>Paralel yürütme bir kuyruğa alınan iletileri önlemek istiyorsanız, ayarlayabileceğiniz `batchSize` 1. Ancak, bu ayar yalnızca tek bir sanal makineye (VM) işlev uygulamanızın çalıştırdığı sürece eşzamanlılık ortadan kaldırır. İşlev uygulaması için birden çok VM Ölçeklendirmesi eşitlenene, her VM her kuyruk ile tetiklenen bir işlev bir örneğini çalıştırabilirsiniz.<br><br>En fazla `batchSize` 32'dir. | 
|maxDequeueCount|5|Kaç defa zehirli kuyruğuna taşınmadan önce bir iletiyi işlemeyi deneyin.| 
|newBatchThreshold|batchSize/2|Bu sayıya aynı anda işlenmekte olan ileti sayısını alır olduğunda, başka bir toplu iş çalışma zamanı alır.| 

## <a name="servicebus"></a>serviceBus

Bir yapılandırma ayarı için [Service Bus Tetikleyicileri ve bağlamaları](functions-bindings-service-bus.md).

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxConcurrentCalls|16|İleti pompası başlatmalıdır geri çağırma eş zamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok ileti işler. Bir kerede yalnızca tek bir kuyruk veya konuda ileti işleme için çalışma zamanının ayarlayın `maxConcurrentCalls` 1. | 
|prefetchCount|yok|Varsayılan temel alınan MessageReceiver tarafından kullanılacak PrefetchCount.| 
|autoRenewTimeout|00:05:00|En uzun süre içinde otomatik olarak ileti kilidi yenilenir.| 

## <a name="singleton"></a>singleton

Singleton kilit davranışı için yapılandırma ayarları. Daha fazla bilgi için [GitHub sorunu tekil desteği hakkında daha fazla](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|lockPeriod|00:00:15|İşlev düzeyi kilit için geçen süre. Kilitler otomatik olarak yenilenecek.| 
|listenerLockPeriod|00:01:00|Dinleyici kilit alınması için dönem.| 
|listenerLockRecoveryPollingInterval|00:01:00|Başlangıçta bir dinleyici kilidi aldıysanız uygulanamadı dinleyici kilit kurtarma için kullanılan zaman aralığı.| 
|lockAcquisitionTimeout|00:01:00|En uzun süreyi çalışma zamanı kilit dener.| 
|lockAcquisitionPollingInterval|yok|Kilidi alma girişimi arasındaki zaman aralığı.| 

## <a name="tracing"></a>İzleme

*Sürüm 1.x*

Yapılandırma ayarlarını kullanarak oluşturduğunuz günlükleri için bir `TraceWriter` nesne. Bkz: [C# günlüğü](functions-reference-csharp.md#logging) ve [Node.js günlük](functions-reference-node.md#writing-trace-output-to-the-console).

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|consoleLevel|bilgi|Konsol günlüğü için izleme düzeyi. Seçenekler şunlardır: `off`, `error`, `warning`, `info`, ve `verbose`.|
|fileLoggingMode|debugOnly|Dosya günlüğü için izleme düzeyi. Seçenekler `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Bir dizi [kod dizinlerini paylaşılan](functions-reference-csharp.md#watched-directories) , izlenen değişiklikleri.  Bu dizinler kod değiştirildiğinde, değişiklikleri işlevleriniz tarafından seçilir, sağlar.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Host.json dosya güncelleştirme hakkında bilgi edinin](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ortam değişkenleri içindeki genel ayarlara bakın](functions-app-settings.md)
