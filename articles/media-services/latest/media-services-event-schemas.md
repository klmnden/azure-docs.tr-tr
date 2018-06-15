---
title: Media Services olayları için Azure olay kılavuz şemaları
description: Media Services olayları Azure olay kılavuzu için sağlanan özellikler açıklar
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: c443ea755c9e7e88b4685682d5e43f675d42efa4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788133"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Media Services olayları için Azure olay kılavuz şemaları

Bu makale, Media Services olaylarını şemaları ve özellikleri sağlar.

## <a name="media-services-job-state-change-event"></a>Media Services işi durum değişiklik olayı

Bu bölüm, özellikleri ve Media Services iş durumu için şema olay değiştirme sağlar.  

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir Media Services **iş** aşağıdaki olay türlerini yayar:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| İş değişiklikleri durumunu tetiklenir. |

### <a name="example-event"></a>Örnek olayı

Aşağıdaki örnek, tamamlanmış bir iş durumu olayı şeması gösterir: 

```json
[
  {
    "topic": "/subscriptions/{subscription id}/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
    "subject": "transforms/VideoAnalyzerTransform/jobs/{job id}",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "<id>",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

### <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | string | Bu değer olay kılavuz sağlar. |
| Konu | string | Media Services hesabı kaynak altında kaynak yolu. |
| Olay türü | string | Bu olay kaynağı için kayıtlı olay türünden biri. Örneğin, "Microsoft.Media.JobStateChange". |
| EventTime | string | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | string | Olay için benzersiz tanımlayıcı. |
| veriler | object | Media Services olay verileri. |
| dataVersion | string | Veri nesnesi şema sürümü. Yayımcı şema sürümü tanımlar. |
| metadataVersion | string | Olay meta veri şema sürümü. Olay kılavuz, şemanın en üst düzey özellikleri tanımlar. Bu değer olay kılavuz sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| previousState | string | Olay önce iş durumu. |
| durum | string | Bu durumda bildirilmesini iş yeni durumu. Örneğin, "sıraya alınan: iş kaynakları bekliyor" veya "zamanlanmış: iş başlatmaya hazır".|

Burada iş durumu aşağıdakilerden biri olabilir değerleri: *sıraya alınan*, *zamanlanmış*, *işleme*, *tamamlandı*, *hata*, *İptal*, *iptal etme*

## <a name="next-steps"></a>Sonraki adımlar

[İş durumu değişikliği olayları için kaydolun](job-state-events-cli-how-to.md)
