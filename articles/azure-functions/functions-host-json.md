---
title: Azure işlevleri için Host.JSON başvurusu
description: Azure işlevleri host.json dosyası için başvuru belgeleri.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/12/2018
ms.author: tdykstra
ms.openlocfilehash: d89170f796355b734facc5e08ad1815a2b865d49
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342101"
---
# <a name="hostjson-reference-for-azure-functions"></a>Azure işlevleri için Host.JSON başvurusu

*Host.json* meta veri dosyası, tüm işlevler bir işlev uygulaması için etkileyen genel yapılandırma seçenekleri içerir. Bu makalede, kullanılabilir ayarlar listelenmiştir. JSON şemasını altındadır http://json.schemastore.org/host.

Diğer genel yapılandırma seçeneği yoktur [uygulama ayarları](functions-app-settings.md) ve [local.settings.json](functions-run-local.md#local-settings-file) dosya.

## <a name="sample-hostjson-file"></a>Örnek host.json dosyası

Aşağıdaki örnek *host.json* dosyası belirtilen tüm olası seçeneklerin sahiptir.

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

Kaç tane işlev çağrılarını belirtir ne zaman toplanan [ölçümler için Application Insights hesaplama](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Özellik |Varsayılan  | Açıklama |
|---------|---------|---------| 
|batchSize|1000|En fazla istek sayısını toplanacak.| 
|flushTimeout|00:00:30|En uzun süreyi toplanacak dönem.| 

İşlev çağrılarını sınırlar ilk iki zaman toplanan ulaşıldı.

## <a name="applicationinsights"></a>Applicationınsights

Denetimleri [Application ınsights'ta örnekleme özelliği](functions-monitoring.md#configure-sampling).

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
|IsEnabled|true|Etkinleştirir veya örnekleme devre dışı bırakır.| 
|maxTelemetryItemsPerSecond|5|Hangi örnekleme eşiğine başlar.| 

## <a name="durabletask"></a>durableTask

İçin yapılandırma ayarlarını [dayanıklı işlevler](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Görev hub adları bir harf ile başlamalı ve yalnızca harf ve sayı oluşur. Belirtilmezse, varsayılan görev hub adı bir işlev uygulaması için olan **DurableFunctionsHub**. Daha fazla bilgi için [görev hubs](durable-functions-task-hubs.md).

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternatif [görev hub](durable-functions-task-hubs.md) adları kullanılabilir birden çok dayanıklı işlevler uygulamaları birbirinden ayırmak için aynı depolama arka ucu kullansalar bile.|
|ControlQueueBatchSize|32|Aynı anda Denetim sıradan çıkarmak için ileti sayısı.|
|PartitionCount |4|Denetim sıranın bölüm sayısı. 1 ile 16 arasında pozitif bir tamsayı olabilir.|
|ControlQueueVisibilityTimeout |5 dakika|Sıradan çıkarılan denetim iletileri görünebilirlik zaman aşımı.|
|WorkItemQueueVisibilityTimeout |5 dakika|Sıradan çıkarılan iş öğesi kuyruk iletileri görünebilirlik zaman aşımı.|
|MaxConcurrentActivityFunctions |Geçerli makinede işlemci sayısını x 10|Tek bir ana bilgisayar örneği üzerinde eşzamanlı olarak işlenebilecek etkinlik işlevlerini maksimum sayısı.|
|MaxConcurrentOrchestratorFunctions |Geçerli makinede işlemci sayısını x 10|Tek bir ana bilgisayar örneği üzerinde eşzamanlı olarak işlenebilecek etkinlik işlevlerini maksimum sayısı.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|Temel alınan Azure depolama kaynaklarını yönetmek için kullanılan Azure depolama bağlantı dizesini içeren uygulama ayarının adı.|
|TraceInputsAndOutputs |false|İzleme girişleri ve çıkışları işlev çağrılarının gösteren bir değer. İşlev yürütme olayları izleme kullanırken varsayılan davranış, sıralanmış girişler ve çıkışlar işlev çağrıları için bayt sayısı dahil sağlamaktır. Bu, giriş ve çıkış günlükleri fazla büyümesini veya yanlışlıkla günlükleri için hassas bilgileri gösterme olmadan nasıl göründüğünü hakkında en düşük miktarda bilgiyi sağlar. Bu özellik tüm içeriğini işlev giriş ve çıkışları oturum için varsayılan işlevi günlüğe kaydetme ayarlanması true neden olur.|
|LogReplayEvents|false|Düzenleme yeniden yürütme olayların Application Insights'a yazılıp yazılmayacağını belirten bir değer.|
|EventGridTopicEndpoint ||Bir Azure Event Grid özel konusu uç nokta URL'si. Bu özelliği ayarlandığında düzenleme yaşam döngüsü bildirim olayları Bu uç noktaya yayınlanır. Bu özellik, uygulama ayarları çözümleme destekler.|
|EventGridKeySettingName ||Azure Event Grid özel konusu ile kimlik doğrulaması için kullanılan anahtarı içeren uygulama ayarının adı `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|Olay Kılavuzu konu başlığında yayımlamaya yeniden denemek için kaç kez başarısız olur.|
|EventGridPublishRetryInterval|5 dakika|Yeniden deneme aralığı içinde Event Grid yayımlama *SS* biçimi.|

Bunların çoğu, performansı iyileştirmek için olan. Daha fazla bilgi için [performansı ve ölçeği](durable-functions-perf-and-scale.md).

## <a name="eventhub"></a>eventHub

İçin yapılandırma ayarlarını [olay hub'ı Tetikleyicileri ve bağlamaları](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>işlevler

Proje ana bilgisayar çalıştırılan işlevlerin listesi. Boş bir dizi tüm işlevleri çalıştırma anlamına gelir. Kullanılmak üzere tasarlanmış yalnızca [yerel olarak çalışan](functions-run-local.md). İşlev uygulamaları'nda *function.json* `disabled` bu özellikte yerine özellik *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Tüm İşlevler için zaman aşımı süresini gösterir. Tüketim planları, geçerli aralık 1 saniye için 10 dakika olan ve varsayılan değer 5 dakikadır. App Service planları, sınırı yoktur ve hiçbir zaman aşımı belirten varsayılan değeri null.

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
|enabled|true|Özellik etkinleştirilip etkinleştirilmediği. | 
|healthCheckInterval|10 saniye|Düzenli arka plan sistem arasındaki zaman aralığını denetler. | 
|healthCheckWindow|2 dakika|İle birlikte kullanılan kayan zaman penceresini `healthCheckThreshold` ayarı.| 
|healthCheckThreshold|6|En fazla kaç kez, bir konak geri dönüştürme başlatılmadan önce sistem durumu denetimi başarısız olabilir.| 
|counterThreshold|0,80|Eşik, bir performans sayacı sağlıksız olarak kabul edilir.| 

## <a name="http"></a>http

İçin yapılandırma ayarlarını [http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

Bir proje konak için benzersiz kimliği. Tireler ile küçük harf GUID kaldırılabilir. Yerel olarak çalışan gereklidir. Azure işlevleri'nde çalışırken, bir kimliği otomatik olarak, oluşturulan `id` atlanır.

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

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>serviceBus

Bir yapılandırma ayarı için [Service Bus Tetikleyicileri ve bağlamaları](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

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
