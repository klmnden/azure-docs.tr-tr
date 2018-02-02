---
title: "Azure işlevleri için Host.JSON başvurusu"
description: "Azure işlevleri host.json dosyası için başvuru belgeleri."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/09/2017
ms.author: tdykstra
ms.openlocfilehash: 58fc58049e346d60c0882a91bd04485746a15cbd
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="hostjson-reference-for-azure-functions"></a>Azure işlevleri için Host.JSON başvurusu

*Host.json* meta veri dosyası, bir işlev uygulaması için tüm işlevleri etkileyen genel yapılandırma seçenekleri içerir. Bu makalede, kullanılabilir ayarlar listelenmiştir. JSON şema http://json.schemastore.org/host ' dir.

Diğer genel yapılandırma seçenekleri vardır [uygulama ayarları](functions-app-settings.md) ve [local.settings.json](functions-run-local.md#local-settings-file) dosya.

## <a name="sample-hostjson-file"></a>Örnek host.json dosyası

Aşağıdaki örnek *host.json* dosya belirtilen tüm olası seçeneği vardır.

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

Aşağıdaki bölümlerde, bu makalenin en üst düzey her bir özellik açıklanmaktadır. Tüm aksi belirtilmedikçe isteğe bağlıdır.

## <a name="aggregator"></a>toplayıcısı

Kaç tane işlev çağrılarını belirtir ne zaman bir araya getirilir [için Application Insights ölçümleri hesaplama](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|batchSize|1000|En fazla istek sayısını toplanacak.| 
|flushTimeout|00:00:30|En uzun süre toplam süre.| 

İşlev çağrılarını toplanan ilk iki sınırları ulaşıldı.

## <a name="applicationinsights"></a>applicationInsights

Denetimleri [Application Insights örnekleme özelliği](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|isEnabled|yanlış|Etkinleştirir veya örnekleme devre dışı bırakır.| 
|maxTelemetryItemsPerSecond|5|Hangi örnekleme eşiğine başlar.| 

## <a name="eventhub"></a>eventHub

Yapılandırma ayarları [olay hub'ı Tetikleyicileri ve bağlamaları](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>işlevler

İş konak çalışacak işlevlerin listesi.  Boş bir dizi tüm işlevleri çalıştırmak anlamına gelir.  Kullanılmaya yalnızca [yerel olarak çalışan](functions-run-local.md). İşlev uygulamalarında kullanma *function.json* `disabled` bu özellikte yerine özelliği *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Tüm İşlevler için zaman aşımı süresini gösterir. Tüketim planları, geçerli aralık 1 saniye için 10 dakika arasıdır. ve 5 dakika varsayılan değerdir. Uygulama hizmeti planları, bir sınır yoktur ve hiçbir zaman aşımı belirten varsayılan değeri null.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>Durum İzleme

Yapılandırma ayarları [ana bilgisayar sistem durumu İzleyicisi](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

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
|enabled|doğru|Özellik etkinleştirilip etkinleştirilmediği. | 
|healthCheckInterval|10 saniye|Düzenli arka plan sistem arasındaki zaman aralığını denetler. | 
|healthCheckWindow|2 dakika|İle birlikte kullanılan kayan bir zaman penceresi `healthCheckThreshold` ayarı.| 
|healthCheckThreshold|6|Bir ana bilgisayar geri dönüşüm başlatılmadan önce en fazla kaç kez sistem durumu denetimi başarısız olabilir.| 
|counterThreshold|0.80|Eşik değer, bir performans sayacı sağlıksız olarak kabul edilir.| 

## <a name="http"></a>http

Yapılandırma ayarları [http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

Bir iş ana bilgisayar için benzersiz kimlik. Küçük harf GUID çizgilerle kaldırılabilir. Yerel olarak çalıştırırken gereklidir. Azure işlevleri çalışırken bir kimlik takdirde otomatik olarak oluşturulur `id` atlanır.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Günlükçü

Tarafından yazılan günlükleri için filtreleme denetimleri bir [ILogger nesne](functions-monitoring.md#write-logs-in-c-functions) veya [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

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
|defaultLevel|Bilgi|Belirtilen olmayan herhangi bir kategori için `categoryLevels` dizisindeki, bu düzeyde ve yukarıdaki günlükleri Application Insights'a gönderme.| 
|categoryLevels|yok|Her kategori için Application Insights'a gönderme için en küçük günlük düzeyini belirten bir dizi kategoriler. Burada belirtilen kategori ile aynı değer ile başlayan tüm kategorileri denetler ve uzun değerleri daha önceliklidir. Yukarıdaki örnekteki *host.json* dosya, "Host.Aggregator" günlüğü ile başlayan tüm kategorileri `Information` düzeyi. "Ana" gibi "Host.Executor" ile başlayan diğer tüm kategorileri oturum `Error` düzeyi.| 

## <a name="queues"></a>Kuyruklar

Yapılandırma ayarları [depolama queue Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md).

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
|maxPollingInterval|60000|En büyük zaman aralığını milisaniye cinsinden sıra arasında yoklar.| 
|visibilityTimeout|0|Zaman aralığı bir ileti işlenirken denemeler başarısız olur.| 
|batchSize|16|Almak ve paralel olarak işlemek için sıra iletilerinin sayısı. Maksimum değer 32'dir.| 
|maxDequeueCount|5|Kaç kez zararlı kuyruğuna taşınmadan önce bir ileti işlenirken deneyin.| 
|newBatchThreshold|batchSize/2|En yeni toplu iletiler getirilen eşiği.| 

## <a name="servicebus"></a>serviceBus

Yapılandırma ayarı [Service Bus Tetikleyicileri ve bağlamaları](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

Singleton kilitleme davranışı için yapılandırma ayarları. Daha fazla bilgi için bkz: [GitHub sorunu singleton desteği hakkında](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
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
|lockPeriod|00:00:15|İşlev düzeyi kilitleri gerçekleştirilecek için süre. Kilitler Otomatik yenilemeyi.| 
|listenerLockPeriod|00:01:00|Dinleyici kilitleri gerçekleştirilecek için süre.| 
|listenerLockRecoveryPollingInterval|00:01:00|Bir dinleyici kilidi başlangıçta alınan uygulanamadı ise dinleyicisi kilit kurtarma için kullanılan zaman aralığı.| 
|lockAcquisitionTimeout|00:01:00|En uzun süreyi çalışma zamanı bir kilit dener.| 
|lockAcquisitionPollingInterval|yok|Kilit alma denemesi arasındaki zaman aralığı.| 

## <a name="tracing"></a>İzleme

Yapılandırma ayarları kullanarak oluşturduğunuz günlükleri için bir `TraceWriter` nesnesi. Bkz: [C# günlük](functions-reference-csharp.md#logging) ve [Node.js günlük](functions-reference-node.md#writing-trace-output-to-the-console). 

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
|consoleLevel|bilgileri|Konsol günlüğe kaydetme için izleme düzeyi. Seçenekler şunlardır: `off`, `error`, `warning`, `info`, ve `verbose`.|
|fileLoggingMode|debugOnly|Dosya günlük için izleme düzeyi. Seçenekler `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Bir dizi [kod dizinleri paylaşılan](functions-reference-csharp.md#watched-directories) , izlenen değişiklikleri.  Bu dizinleri kodda değiştirildiğinde, değişiklikleri işlevlerinizi tarafından toplanmış olduğunu sağlar.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="durabletask"></a>durableTask

[Görev hub](durable-functions-task-hubs.md) adı [dayanıklı işlevleri](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Görev hub adları bir harfle başlamalı ve yalnızca harf ve sayılardan oluşur. Belirtilmezse, bir işlev uygulaması için varsayılan görev hub adı. **DurableFunctionsHub**. Daha fazla bilgi için bkz: [görev hub](durable-functions-task-hubs.md).


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Host.json dosyasını güncelleştirme öğrenin](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Bkz. ortam değişkenleri içindeki genel ayarları](functions-app-settings.md)
