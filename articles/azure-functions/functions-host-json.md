---
title: Azure işlevleri için Host.JSON başvurusu 2.x
description: Azure işlevleri host.json dosyasıyla v2 çalışma zamanı için başvuru belgeleri.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: glenga
ms.openlocfilehash: e24c5b2be1df41d84fa4461250f51cb009f77529
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60737218"
---
# <a name="hostjson-reference-for-azure-functions-2x"></a>Azure işlevleri için Host.JSON başvurusu 2.x  

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Sürüm 1](functions-host-json-v1.md)
> * [Sürüm 2](functions-host-json.md)

*Host.json* meta veri dosyası, tüm işlevler bir işlev uygulaması için etkileyen genel yapılandırma seçenekleri içerir. Bu makalede, v2 çalışma zamanı için kullanılabilen ayarları listelenir.  

> [!NOTE]
> Bu makalede, Azure işlevleri için olan 2.x.  İşlevlerde host.json başvurusu için 1.x, bkz: [Azure işlevleri için host.json başvurusu 1.x](functions-host-json-v1.md).

Başka bir işlev uygulaması yapılandırma seçenekleri yönetilir, [uygulama ayarları](functions-app-settings.md).

Bazı host.json ayarları yalnızca yerel olarak çalıştırırken kullanılan [local.settings.json](functions-run-local.md#local-settings-file) dosya.

## <a name="sample-hostjson-file"></a>Örnek host.json dosyası

Aşağıdaki örnek *host.json* dosyaları belirtilen tüm olası seçeneklerin sahiptir.


```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
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
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

Bu makalenin aşağıdaki bölümlerde, her bir üst düzey özellik açıklanmaktadır. Tümü aksi belirtilmediği sürece isteğe bağlıdır.

## <a name="aggregator"></a>Toplayıcı

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

Bu ayar bir alt öğesidir [günlüğü](#logging).

Denetimleri [Application ınsights'ta örnekleme özelliği](./functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "samplingSettings": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

> [!NOTE]
> Günlük örnekleme Application Insights izleme dikey penceresinde görünmesini olmayan bazı yürütmeleri neden olabilir.

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|isEnabled|true|Etkinleştirir veya örnekleme devre dışı bırakır.| 
|maxTelemetryItemsPerSecond|5|Hangi örnekleme eşiğine başlar.| 

## <a name="cosmosdb"></a>cosmosDb

Yapılandırma ayarı olarak bulunabilir [Cosmos DB Tetikleyicileri ve bağlamaları](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

Yapılandırma ayarı olarak bulunabilir [dayanıklı işlevler için bağlamaları](durable/durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>eventHub

Yapılandırma ayarları bulunabilir [olay hub'ı Tetikleyicileri ve bağlamaları](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>Uzantıları

Tüm bağlama özgü ayarları gibi içeren bir nesne döndürür özelliği [http](#http) ve [eventHub](#eventhub).

## <a name="functions"></a>işlevler

Proje ana çalışan işlevlerin listesi. Boş bir dizi tüm işlevleri çalıştırma anlamına gelir. Kullanılmak üzere tasarlanmış yalnızca [yerel olarak çalışan](functions-run-local.md). Azure işlev uygulamaları bunun yerine adımları izlemelidir [işlevleri Azure işlevleri'nde devre dışı bırakma](disable-function.md) belirli işlevleri yerine bu ayarı devre dışı bırakmak için.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Tüm İşlevler için zaman aşımı süresini gösterir. Sunucusuz bir tüketim planı geçerli aralık 1 saniye için 10 dakika olan ve varsayılan değer 5 dakikadır. App Service planı, genel bir sınır yoktur ve varsayılan çalışma zamanı sürümüne bağlıdır. Sürüm 2.x, bir App Service planı, 30 dakika için varsayılan değeri. Sürümünde olduğu 1.x, *null*, hiçbir zaman aşımını gösterir.

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

Yapılandırma ayarları bulunabilir [http Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="logging"></a>Günlüğe kaydetme

Application Insights da dahil olmak üzere, bir işlev uygulaması günlük davranışlarını denetler.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "applicationInsights": {
        ...
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Dosya günlük düzeyini etkin tanımlar.  Seçenekler `never`, `always`, `debugOnly`. |
|logLevel|yok|Uygulama işlevler için filtreleme günlük kategoriyi tanımlayan nesne. Sürüm 2.x günlük kategorisi filtreleme için ASP.NET Core düzenini izler. Bu sayede belirli işlevleri günlüğünü filtreleyin. Daha fazla bilgi için [günlük filtreleme](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) ASP.NET Core belgelerindeki. |
|console|yok| [Konsol](#console) günlüğü ayarı. |
|applicationInsights|yok| [Applicationınsights](#applicationinsights) ayarı. |

## <a name="console"></a>console

Bu ayar bir alt öğesidir [günlüğü](#logging). Bu, hata ayıklama modunda değil, oturum konsol denetler.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|isEnabled|false|Etkinleştirir veya konsol günlüğü devre dışı bırakır.| 

## <a name="queues"></a>Kuyruklar

Yapılandırma ayarları bulunabilir [depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md#host-json).  

## <a name="sendgrid"></a>SendGrid

Yapılandırma ayarı olarak bulunabilir [SendGrid Tetikleyicileri ve bağlamaları](functions-bindings-sendgrid.md#host-json).

## <a name="servicebus"></a>serviceBus

Yapılandırma ayarı olarak bulunabilir [Service Bus Tetikleyicileri ve bağlamaları](functions-bindings-service-bus.md#host-json).

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

## <a name="version"></a>version

Sürüm dizesi `"version": "2.0"` v2 çalışma zamanını hedefleyen bir işlev uygulaması için gereklidir.

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
