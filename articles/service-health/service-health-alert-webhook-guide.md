---
title: Web kancası kullanarak mevcut sorun yönetim sistemleri için sistem durumu bildirimleri yapılandırma | Microsoft Docs
description: Hizmet durumu olayları için mevcut sorun yönetim sisteminizde hakkında Kişiselleştirilmiş bildirimler alın.
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.workload: Supportability
ms.date: 3/27/2018
ms.openlocfilehash: 69b142cd46c006e562218c949fb450864589a661
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622167"
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Web kancası kullanarak mevcut sorun yönetim sistemleri için sistem durumu bildirimlerini yapılandırma

Bu makalede mevcut bildirim sistemi için Web kancaları aracılığıyla veri göndermek için hizmet sistem durumu uyarılarının nasıl yapılandırılacağı gösterilmektedir.

Bugün, böylece bir Azure hizmeti olay sizi etkiliyorsa, kısa mesaj veya e-posta bildirim alın, hizmet sistem durumu uyarılarını yapılandırabilirsiniz.
Ancak, kullanmak istediğiniz bir yerde zaten var olan harici bildirim sistemi olabilir.
Bu belge, en önemli bölümleri Web kancası yükü ve hizmet sorunlardan etkilendiğiniz durumlar bildirim almak için özel uyarıları nasıl oluşturabileceğinizi gösterir.

Önceden yapılandırılmış bir tümleştirme kullanmak istiyorsanız, bkz: nasıl yapılır:
* [ServiceNow ile uyarıları yapılandırma](service-health-alert-webhook-servicenow.md)
* [PagerDuty ile uyarıları yapılandırma](service-health-alert-webhook-pagerduty.md)
* [OpsGenie ile uyarıları yapılandırma](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Hizmet durumu Web kancası yükü kullanarak özel bir bildirim yapılandırma
Kendi özel Web kancası tümleştirmesini ayarlamak istiyorsanız, hizmet durumu bildirimlerine sırasında gönderilen JSON yükü ayrıştırılamıyor gerekir.

Konum [bir örnek görmek için buraya](../azure-monitor/platform/activity-log-alerts-webhook.md) ne `ServiceHealth` gibi görünen Web kancası yükü.

Bu, hizmet durumu uyarısı bakarak algılayabilir `context.eventSource == "ServiceHealth"`. Burada, alımı en uygun olan özellikleri şunlardır:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Hizmet durumu Panosu bir olay için doğrudan bir bağlantı oluşturma
Özel bir URL üretmek için bir hizmet durumu Panosu Masaüstü veya mobil doğrudan bir bağlantı oluşturabilirsiniz. Kullanım `trackingId`, ilk ve son üç hanesi yanı sıra, `subscriptionId`oluşturmak için:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Örneğin, varsa, `subscriptionId` olduğu `bba14129-e895-429b-8809-278e836ecdb3` ve `trackingId` olduğu `0DET-URB`, sistem durumu hizmeti URL'NİZİN ise:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Sorunun önem derecesine algılamak için düzeyi kullanma
En düşük önem derecesi en yüksek önem derecesi, gelen `level` özelliği yükünde aşağıdakilerden biri olabilir `Informational`, `Warning`, `Error`, ve `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Tam olay anlamak için etkilenen hizmetler ayrıştırma
Hizmet sistem durumu uyarılarını birden çok bölgede ve hizmetlerinizde sorunları hakkında bildirebilirsiniz. Tam bilgi almak için değeri ayrıştırılamıyor ihtiyacınız `impactedServices`.
İçerik içinde bir [JSON kaçan](https://json.org/) dize, atlanmamış, düzenli olarak ayrıştırılabilir başka bir JSON nesnesi içerir.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

olur:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Bu, Avustralya Güneydoğu "App Service" sorun yanı sıra Avustralya Doğu ve Güneydoğu "Uyarılar ve ölçümler" bir sorun olduğunu gösterir.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme testi
1. Göndermek istediğiniz hizmet sistem durumu yükü oluşturun. Bir örnek hizmet sistem durumu Web kancası yükü konumunda bulabilirsiniz [günlük uyarıları Azure etkinlik için Web kancaları](../azure-monitor/platform/activity-log-alerts-webhook.md).

2. Bir HTTP POST isteği şu şekilde oluşturun:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. Alması gereken bir `2XX - Successful` yanıt.

4. Git [PagerDuty](https://www.pagerduty.com/) tümleştirmenizi başarılı bir şekilde ayarlandığını doğrulamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerini](../azure-monitor/platform/service-notifications.md).
- Daha fazla bilgi edinin [Eylem grupları](../azure-monitor/platform/action-groups.md).