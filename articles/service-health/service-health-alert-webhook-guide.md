---
title: Bir Web kancası kullanarak var olan sorunu yönetim sistemleri için sistem durumu bildirimleri yapılandırma | Microsoft Docs
description: Varolan sorun yönetimi sisteminiz için hizmet sistem durumu olayları hakkında Kişiselleştirilmiş bildirimler alın.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/27/2018
ms.author: shtabriz
ms.openlocfilehash: 8535caf482b10912e6f7bc6df445756094d7603f
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Bir Web kancası kullanarak var olan sorunu yönetim sistemleri için sistem durumu bildirimleri yapılandırma

Bu makalede, mevcut bir bildirim sisteminizin arasında Kancalarını veri göndermek için hizmet durumu uyarılarının nasıl yapılandırılacağı gösterilmektedir.

Bugün, böylece bir Azure hizmeti olay, etkilediğinde, kısa mesaj veya e-posta bilgi edinin, hizmet durumu uyarıları yapılandırabilirsiniz.
Ancak, kullanmak istediğiniz yerde zaten var olan harici bildirim sistemi sahip olabilir.
Bu belge Web kancası yükü ve hizmet sorunları, etkilerse bildirim almak için özel uyarılar nasıl oluşturabileceğiniz en önemli parçaları gösterilmektedir.

Önceden yapılandırılmış bir tümleştirme kullanmak istiyorsanız, bkz: nasıl yapılır:
* [ServiceNow uyarılarını yapılandırma](service-health-alert-webhook-servicenow.md)
* [Uyarıları PagerDuty ile yapılandırma](service-health-alert-webhook-pagerduty.md)
* [Uyarıları OpsGenie ile yapılandırma](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Hizmet durumu Web kancası yükü kullanarak özel bir bildirim yapılandırma
Kendi özel Web kancası tümleştirmeyi ayarlamak istiyorsanız, hizmet durumu bildirimlerine sırasında gönderilen JSON yükü ayrıştırılamıyor gerekir.

Ara [bir örnek görmek için buraya](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md) ne `ServiceHealth` Web kancası yükü gibi görünüyor.

Bu olan bir hizmet Sistem Durumu Uyarısı bakarak algılayabilir `context.eventSource == "ServiceHealth"`. Buradan, alma en uygun olan özellikleri şunlardır:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Bir olay hizmet sağlığı panosunu doğrudan bağlantı oluşturma
Özel bir URL oluşturarak, hizmet durumu panonuza masaüstünde veya mobil doğrudan bir bağlantı oluşturabilirsiniz. Kullanım `trackingId`, ilk ve son üç rakamı yanı sıra, `subscriptionId`, oluşturmak için:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Örneğin, varsa, `subscriptionId` olan `bba14129-e895-429b-8809-278e836ecdb3` ve `trackingId` olan `0DET-URB`, hizmet durumu URL'niz ise:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Sorunun önem derecesini algılamak için düzeyi kullanma
En yüksek önem derecesi için en düşük önem gelen `level` yükü özelliğinde herhangi biri olabilir `Informational`, `Warning`, `Error`, ve `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Olay tam kapsamı anlamak için etkilenen hizmetler ayrıştırma
Hizmet durumu uyarıları birden çok bölgeleri ve hizmetleri arasında sorunlar hakkında bilgilendirebilir. Tam bilgi almak için değeri ayrıştırılamadı gerekir `impactedServices`.
İçinde içerik bir [kaçışlı JSON](http://json.org/) atlanmayan zaman dize düzenli olarak ayrıştırılabilir başka bir JSON nesnesi içerir.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Olur:

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

Bu, "App Service'te" Avustralya Güneydoğu sorunları yanı sıra, Avustralya Doğu ve Güneydoğu "Uyarıları & ölçümleri" sorun olduğunu gösterir.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Bir HTTP POST isteği üzerinden, Web kancası tümleştirme sınaması
1. Göndermek istediğiniz hizmet sistem durumu yükü oluşturun. Bir örnek hizmet sistem durumu Web kancası yükü sırasında bulduğunuz [Azure etkinlik için Web kancası oturum uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Bir HTTP POST isteği gibi oluşturun:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. Alması gereken bir `2XX - Successful` yanıt.

4. Git [PagerDuty](https://www.pagerduty.com/) tümleştirmenize başarıyla ayarlanmıştır onaylamak için.

## <a name="next-steps"></a>Sonraki adımlar
- Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Hakkında bilgi edinin [hizmet durumu bildirimlerine](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Daha fazla bilgi edinmek [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md).