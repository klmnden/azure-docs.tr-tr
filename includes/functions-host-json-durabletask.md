---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 92eb13165326f44432f09322ea97f3cee5ccec2b
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251252"
---
İçin yapılandırma ayarlarını [dayanıklı işlevler](../articles/azure-functions/durable-functions-overview.md).

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

Görev hub adları bir harf ile başlamalı ve yalnızca harf ve sayı oluşur. Belirtilmezse, varsayılan görev hub adı bir işlev uygulaması için olan **DurableFunctionsHub**. Daha fazla bilgi için [görev hubs](../articles/azure-functions/durable-functions-task-hubs.md).

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternatif [görev hub](../articles/azure-functions/durable-functions-task-hubs.md) adları, birden çok dayanıklı işlevler uygulamaları birbirinden ayırmak için kullanılabilir olsa bile aynı depolama arka ucu kullanarak theyre.|
|ControlQueueBatchSize|32|Aynı anda Denetim sıradan çıkarmak için ileti sayısı.|
|PartitionCount |4|Denetim sıranın bölüm sayısı. 1 ile 16 arasında pozitif bir tamsayı olabilir.|
|ControlQueueVisibilityTimeout |5 dakika|Sıradan çıkarılan denetim iletileri görünebilirlik zaman aşımı.|
|WorkItemQueueVisibilityTimeout |5 dakika|Sıradan çıkarılan iş öğesi kuyruk iletileri görünebilirlik zaman aşımı.|
|MaxConcurrentActivityFunctions |Geçerli makinede işlemci sayısını x 10|Tek bir ana bilgisayar örneği üzerinde eşzamanlı olarak işlenebilecek etkinlik işlevlerini maksimum sayısı.|
|MaxConcurrentOrchestratorFunctions |Geçerli makinede işlemci sayısını x 10|Tek bir ana bilgisayar örneği üzerinde eşzamanlı olarak işlenebilecek orchestrator işlevleri maksimum sayısı.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|Temel alınan Azure depolama kaynaklarını yönetmek için kullanılan Azure depolama bağlantı dizesini içeren uygulama ayarının adı.|
|TraceInputsAndOutputs |false|İzleme girişleri ve çıkışları işlev çağrılarının gösteren bir değer. İşlev yürütme olayları izleme kullanırken varsayılan davranış, sıralanmış girişler ve çıkışlar işlev çağrıları için bayt sayısı dahil sağlamaktır. Bu, giriş ve çıkış günlükleri fazla büyümesini veya yanlışlıkla günlükleri için hassas bilgileri gösterme olmadan nasıl göründüğünü hakkında en düşük miktarda bilgiyi sağlar. Bu özellik tüm içeriğini işlev giriş ve çıkışları oturum için varsayılan işlevi günlüğe kaydetme ayarlanması true neden olur.|
|LogReplayEvents|false|Düzenleme yeniden yürütme olayların Application Insights'a yazılıp yazılmayacağını belirten bir değer.|
|EventGridTopicEndpoint ||Bir Azure Event Grid özel konusu uç nokta URL'si. Bu özelliği ayarlandığında düzenleme yaşam döngüsü bildirim olayları Bu uç noktaya yayınlanır. Bu özellik, uygulama ayarları çözümleme destekler.|
|EventGridKeySettingName ||Azure Event Grid özel konusu ile kimlik doğrulaması için kullanılan anahtarı içeren uygulama ayarının adı `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|Olay Kılavuzu konu başlığında yayımlamaya yeniden denemek için kaç kez başarısız olur.|
|EventGridPublishRetryInterval|5 dakika|Event Grid, yeniden deneme aralığı yayımlar *SS* biçimi.|

Bunların çoğu, performansı iyileştirmek için olan. Daha fazla bilgi için [performansı ve ölçeği](../articles/azure-functions/durable-functions-perf-and-scale.md).