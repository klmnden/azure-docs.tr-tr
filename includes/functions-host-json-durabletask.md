---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d79d1bd5ec244ad4399a02c349e2504516d06ccd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188940"
---
İçin yapılandırma ayarlarını [dayanıklı işlevler](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "controlQueueBatchSize": 32,
    "partitionCount": 4,
    "controlQueueVisibilityTimeout": "00:05:00",
    "workItemQueueVisibilityTimeout": "00:05:00",
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "maxQueuePollingInterval": "00:00:30",
    "azureStorageConnectionStringName": "AzureWebJobsStorage",
    "trackingStoreConnectionStringName": "TrackingStorage",
    "trackingStoreNamePrefix": "DurableTask",
    "traceInputsAndOutputs": false,
    "logReplayEvents": false,
    "eventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName":  "EventGridKey",
    "eventGridPublishRetryCount": 3,
    "eventGridPublishRetryInterval": "00:00:30",
    "eventGridPublishEventTypes": ["Started", "Pending", "Failed", "Terminated"]
  }
}
```

Görev hub adları bir harf ile başlamalı ve yalnızca harf ve sayı oluşur. Belirtilmezse, varsayılan görev hub adı bir işlev uygulaması için olan **DurableFunctionsHub**. Daha fazla bilgi için [görev hubs](../articles/azure-functions/durable-functions-task-hubs.md).

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|hubName|DurableFunctionsHub|Alternatif [görev hub](../articles/azure-functions/durable-functions-task-hubs.md) adları kullanılabilir birden çok dayanıklı işlevler uygulamaları birbirinden ayırmak için aynı depolama arka ucu kullanmıyor olsanız bile.|
|ControlQueueBatchSize|32|Aynı anda Denetim sıradan çıkarmak için ileti sayısı.|
|partitionCount |4|Denetim sıranın bölüm sayısı. 1 ile 16 arasında pozitif bir tamsayı olabilir.|
|ControlQueueVisibilityTimeout |5 dakika|Sıradan çıkarılan denetim iletileri görünebilirlik zaman aşımı.|
|WorkItemQueueVisibilityTimeout |5 dakika|Sıradan çıkarılan iş öğesi kuyruk iletileri görünebilirlik zaman aşımı.|
|MaxConcurrentActivityFunctions |Geçerli makinede işlemci sayısını x 10|Tek bir ana bilgisayar örneği üzerinde eşzamanlı olarak işlenebilecek etkinlik işlevlerini maksimum sayısı.|
|maxConcurrentOrchestratorFunctions |Geçerli makinede işlemci sayısını x 10|Tek bir ana bilgisayar örneği üzerinde eşzamanlı olarak işlenebilecek orchestrator işlevleri maksimum sayısı.|
|maxQueuePollingInterval|30 saniye|İş öğesi kuyruk yoklama aralığı ve en yüksek denetim *SS* biçimi. Yüksek değerler daha yüksek ileti işleme gecikmeleri neden olabilir. Düşük değerler daha yüksek depolama maliyetleri, daha yüksek depolama işlemleri nedeniyle neden olabilir.|
|azureStorageConnectionStringName |AzureWebJobsStorage|Temel alınan Azure depolama kaynaklarını yönetmek için kullanılan Azure depolama bağlantı dizesini içeren uygulama ayarının adı.|
|trackingStoreConnectionStringName||Geçmiş ve örnekleri tablolar için kullanılacak bir bağlantı dizesi adı. Belirtilmezse, `azureStorageConnectionStringName` bağlantı kullanılır.|
|trackingStoreNamePrefix||Geçmiş ve örnekleri için kullanılacak önek tabloları `trackingStoreConnectionStringName` belirtilir. Ayarlanmazsa varsayılan ön ek değerini olup olmayacağını `DurableTask`. Varsa `trackingStoreConnectionStringName` geçmiş ve örnekleri tablolarını kullanır, belirtilmemiş `hubName` kendi önekini ve her ayar için değer `trackingStoreNamePrefix` göz ardı edilir.|
|TraceInputsAndOutputs |false|İzleme girişleri ve çıkışları işlev çağrılarının gösteren bir değer. İşlev yürütme olayları izleme kullanırken varsayılan davranış, sıralanmış girişler ve çıkışlar işlev çağrıları için bayt sayısı dahil sağlamaktır. Bu davranış, girdileri ve çıktıları günlükleri fazla büyümesini veya hassas bilgilerin yanlışlıkla gösterme göründüğünü hakkında en düşük miktarda bilgiyi sağlar. Bu özellik tüm içeriğini işlev giriş ve çıkışları oturum için varsayılan işlevi günlüğe kaydetme ayarlanması true neden olur.|
|LogReplayEvents|false|Düzenleme yeniden yürütme olayların Application Insights'a yazılıp yazılmayacağını belirten bir değer.|
|EventGridTopicEndpoint ||Bir Azure Event Grid özel konusu uç nokta URL'si. Bu özelliği ayarlandığında düzenleme yaşam döngüsü bildirim olayları Bu uç noktaya yayınlanır. Bu özellik, uygulama ayarları çözümleme destekler.|
|eventGridKeySettingName ||Azure Event Grid özel konusu ile kimlik doğrulaması için kullanılan anahtarı içeren uygulama ayarının adı `EventGridTopicEndpoint`.|
|eventGridPublishRetryCount|0|Olay Kılavuzu konu başlığında yayımlamaya yeniden denemek için kaç kez başarısız olur.|
|EventGridPublishRetryInterval|5 dakika|Event Grid, yeniden deneme aralığı yayımlar *SS* biçimi.|
|eventGridPublishEventTypes||Event Grid yayımlama için olay türleri listesi. Belirtilmezse, tüm olay türleri yayımlanır. İzin verilen değerler arasında `Started`, `Completed`, `Failed`, `Terminated`.|

Bu ayarların çoğu, performansı iyileştirmek için olan. Daha fazla bilgi için [performansı ve ölçeği](../articles/azure-functions/durable-functions-perf-and-scale.md).