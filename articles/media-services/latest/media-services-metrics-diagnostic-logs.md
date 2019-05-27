---
title: Media Services ölçümleri ve tanılama günlüklerini Azure İzleyici ile izleme | Microsoft Docs
description: Bu makalede, Media Services ölçümleri ve tanılama günlüklerini Azure İzleyici ile izleme hakkında genel bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2019
ms.author: juliako
ms.openlocfilehash: bbf43ecb07947fad8cc1ee064d2038e4a21d4444
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65964757"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Media Services ölçümleri ve tanılama günlüklerini izleyin

[Azure İzleyici](../../azure-monitor/overview.md) ölçümlerini izleme ve yardımcı olacak tanılama günlüklerini anlama, uygulamalarınızın performansını sağlar. Azure İzleyici tarafından toplanan tüm verileri ölçüm ve günlükleri olmak üzere iki temel türünden birini uyar. Media Services tanılama günlüklerini izleyin ve uyarılar ve bildirimler için toplanan bir ölçüm ve günlükleri oluşturun. Görselleştirme ve ölçüm verileri kullanarak Analiz [ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md). Günlükleri size gönderebilir [Azure depolama](https://azure.microsoft.com/services/storage/), kendisine akış [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)ve bunları dışarı aktarma [Log Analytics](https://azure.microsoft.com/services/log-analytics/), veya 3. taraf hizmetleri kullanın.

Ayrıntılı bir bakış için bkz: [Azure İzleyici ölçümleri](../../azure-monitor/platform/data-platform.md) ve [Azure İzleyici tanılama günlükleri](../../azure-monitor/platform/diagnostic-logs-overview.md).

Bu konuda şu anda kullanılabilir anlatılmaktadır [Media Services ölçüm](#media-services-metrics) ve [Media Services tanılama günlükleri](#media-services-diagnostic-logs).

## <a name="media-services-metrics"></a>Medya Hizmetleri ölçümleri

Ölçümler, değeri değiştirir olup olmadığını düzenli aralıklarla toplanır. Bunlar sık örneklenebilir ve bir uyarı ile göreceli olarak basit bir mantıksal hızla harekete çünkü uyarmak için yararlıdır.

Şu anda aşağıdaki Media Services [akış uç noktalarını](https://docs.microsoft.com/rest/api/media/streamingendpoints) ölçümleri Azure tarafından yayılan:

|Ölçüm|Display name|Açıklama|
|---|---|---|
|İstekler|İstekler|Akış uç noktası hizmet isteklerinin toplam sayısı etrafında ayrıntılarını verir.|
|Çıkış|Çıkış|Toplam çıkış bayt sayısı. Örneğin, akış uç noktası tarafından akışa bayt.|
|Başarı E2e|Başarı uçtan uca gecikme süresi| Başarılı isteklerin uçtan uca gecikme süresi hakkında bilgi sağlar.|

Örneğin, "Çıkış" ölçümleri CLI ile almak için aşağıdaki çalıştırırsınız `az monitor metrics` CLI komutunu:

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Ölçüm uyarıları oluşturma hakkında daha fazla bilgi için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak ölçüm Uyarıları yönetme](../../azure-monitor/platform/alerts-metric.md).

## <a name="media-services-diagnostic-logs"></a>Media Services tanılama günlükleri

Şu anda aşağıdaki tanılama günlükleri elde edebilirsiniz:

|Ad|Açıklama|
|---|---|
|Anahtar teslim hizmet isteği|Anahtar dağıtımı hizmetiyle Göster günlükleri bilgi isteyin. Daha fazla ayrıntı için [şemaları](media-services-diagnostic-logs-schema.md).|

Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için aşağıdaki çalıştıracağınız `az monitor diagnostic-settings` CLI komutunu: 

```cli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Örneğin:

```cli
az monitor diagnostic-settings create --name amsv3diagnostic \
    --storage-account storageaccountforamsv3  \
    --resource "/subscriptions/00000000-0000-0000-0000-0000000000/resourceGroups/amsv3ResourceGroup/providers/Microsoft.Media/mediaservices/amsv3account" \
    --resource-group "amsv3ResourceGroup" \
    --logs '[{"category": "KeyDeliveryRequests",  "enabled": true, "retentionPolicy": {"days": 3, "enabled": true }}]'
```

## <a name="next-steps"></a>Sonraki adımlar 

[Azure kaynaklarınızdan günlük verilerini toplamak ve nasıl](../../azure-monitor/platform/diagnostic-logs-overview.md).
