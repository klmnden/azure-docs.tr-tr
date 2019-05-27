---
title: Azure Cosmos DB Tetikleyicisi günlükleri
description: İşlem hattı günlük kaydı, Azure işlevleri için Azure Cosmos DB tetikleyicisi günlükleri kullanıma sunma konusunda bilgi edinin
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: maquaran
ms.openlocfilehash: bf5216dc3b296c98176387c6e2cfff7c31daedab
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241035"
---
# <a name="how-to-configure-and-read-the-azure-cosmos-db-trigger-logs"></a>Azure Cosmos DB tetikleyicisi günlüklerini okumak ve yapılandırma

Bu makalede, Azure Cosmos DB tetikleyicisi günlükleri, yapılandırılmış göndermek için Azure işlevleri ortamınızı nasıl yapılandırabileceğiniz açıklanmaktadır [izleme çözümü](../azure-functions/functions-monitoring.md).

## <a name="included-logs"></a>Dahil edilen günlükleri

Azure Cosmos DB tetikleyicisi kullanan [değişiklik akışı işlemci Kitaplığı](./change-feed-processor.md) dahili olarak, kitaplık için iç işlemleri izlemek için kullanılan sistem günlükleri bir dizi oluşturur [sorun giderme amacıyla](./troubleshoot-changefeed-functions.md).

Sistem günlükleri, Azure Cosmos DB tetikleyicisi işlem çalışırken Yük Dengeleme senaryolarıyla veya başlatma sırasında nasıl davranacağını açıklanmaktadır.

## <a name="enabling-logging"></a>Günlüğe kaydetmeyi etkinleştirme

Azure Cosmos DB tetikleyicisi günlüğe kaydetmeyi etkinleştirmek için bulun `host.json` dosyasında Azure işlevleri projenizi veya Azure işlev uygulaması ve [gerekli günlüğe kaydetme düzeyini yapılandırma](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson). İzlemelerini etkinleştirmek sahip olduğunuz `Host.Triggers.CosmosDB` aşağıdaki örnekte gösterildiği gibi:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Azure işlevi güncelleştirilmiş yapılandırmayla dağıtıldıktan sonra Azure Cosmos DB tetikleyicisi günlüklerini, izlemeleri bir parçası olarak görürsünüz. Günlükleri altında yapılandırılmış günlük Sağlayıcınızdaki görüntüleyebileceğiniz *kategori* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Günlükleri sorgulama

Azure Cosmos DB tetikleyicisi tarafından günlükleri oluşturulan sorgu için sorgu aşağıdaki komutu çalıştırın [Azure Application Insights Analytics](../azure-monitor/app/analytics.md):

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Sonraki adımlar

* [İzlemeyi Etkinleştir](../azure-functions/functions-monitoring.md) uygulamalarınızda Azure işlevleri.
* Bilgi nasıl [Tanıla ve sık karşılaşılan sorunları giderme](./troubleshoot-changefeed-functions.md) Azure Cosmos DB tetikleyicisi Azure işlevleri'nde kullanırken.