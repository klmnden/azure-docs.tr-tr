---
title: Azure Application Insights ile Azure Tanılama'yı tümleştirme kullanarak Azure bulut hizmetlerinde uyar | Microsoft Docs
description: Başlatma hataları, çökme ve rol döngüleri Azure Application Insights ile Azure bulut hizmetlerinde geri dönüşüm gibi sorunlar için İzleyici
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/07/2018
ms.reviewer: harelbr
ms.author: mbullwin
ms.openlocfilehash: 219ba632d7688f1a428378309828b689698d2fe5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60409549"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Azure Application Insights ile Azure Tanılama'yı tümleştirme kullanarak Azure bulut hizmetlerinde olduğunda uyar

Bu makalede, biz nasıl başlatma hataları, çökme ve rol döngüleri Azure bulut hizmetlerinde (web ve çalışan rolleri) geri dönüşüm gibi sorunlar için izleme uyarı kuralları ayarlanacağı açıklanmıştır.

Bu makalede açıklanan yöntemi dayanır [Application Insights ile Azure Tanılama'yı tümleştirme](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/)ve kısa süre önce yayımlanan [günlük uyarıları için Application Insights](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/) yeteneği.

## <a name="define-a-base-query"></a>Temel sorgu tanımlama

Başlamak için Windows Azure kanaldan Windows olay günlüğü olaylarını alır temel bir sorgu, Application Insights izleme kayıtları olarak yakalanan tanımlarız.
Bu kayıtlar, Azure bulut Hizmetleri'nde başlatma hataları, çalışma zamanı hataları gibi çeşitli sorunlar algılamak için kullanılabilir ve döngüler geri dönüşüm.

> [!NOTE]
> Aşağıdaki temel sorguyu 30 dakikalık bir zaman penceresinde sorunları olup olmadığını denetler ve telemetri kayıtları almak, bir 10 dakikalık gecikme varsayar. Bu varsayılan gördüğünüz şekilde yapılandırılabilir.

```
let window = 30m;
let endTime = ago(10m);
let EventLogs = traces
| where timestamp > endTime - window and timestamp < endTime
| extend channel = tostring(customDimensions.Channel), eventId = tostring(customDimensions.EventId)
| where channel == 'Windows Azure' and isnotempty(eventId)
| where tostring(customDimensions.DeploymentName) !contains 'deployment' // discard records captured from local machines
| project timestamp, channel, eventId, message, cloud_RoleInstance, cloud_RoleName, itemCount;
```

## <a name="check-for-specific-event-ids"></a>Özel olay kimlikleri denetle

Windows olay günlüğü olaylarını aldıktan sonra belirli sorunları (aşağıdaki örneklere bakın) kendi ilgili olay kimliği ve ileti özelliklerini denetleyerek algılanabilir.
Yalnızca bir sorgu günlük uyarı kuralı tanımlarken birleştirilmiş sorguların aşağıdaki ve kullanılan temel sorgusunun üstüne birleştirin.

> [!NOTE]
> Üçten fazla olayları analiz edilen zaman penceresi boyunca bulunursa, aşağıdaki örneklerde, bir sorun algıladık. Bu varsayılan uyarı kuralı duyarlılığına değiştirmek için yapılandırılabilir.

```
// Detect failures in the OnStart method
EventLogs
| where eventId == '2001'
| where message contains '.OnStart()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures during runtime
EventLogs
| where eventId == '2001'
| where message contains '.Run()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures when running a startup task
EventLogs
| where eventId == '1000'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect recycle loops
EventLogs
| where eventId == '1006'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

## <a name="create-an-alert"></a>Uyarı oluşturma

Gezinti menüsünde içinde Application Insights kaynağınıza gidin **uyarılar**ve ardından **yeni uyarı kuralı**.

![Kural Oluştur ekran görüntüsü](./media/proactive-cloud-services/001.png)

İçinde **oluşturma kuralı** penceresi altında **uyarı koşulunu tanımlama** bölümünde, tıklayarak **Ölçüt Ekle**ve ardından **özel günlük araması**.

![Uyarının koşulu ölçüt tanımla ekran görüntüsü](./media/proactive-cloud-services/002.png)

İçinde **arama sorgusu** kutusunda, önceki adımda hazırladığınız birleşik sorguyu yapıştırın.

Daha sonra devam **eşiği** kutusuna ve değerini 0 olarak ayarlayın. İsteğe bağlı olarak ince **süresi** ve sıklığı **alanları**.
**Bitti**’ye tıklayın.

![Ekran görüntüsü sinyal mantığını sorgu yapılandırma](./media/proactive-cloud-services/003.png)

Altında **uyarı ayrıntılarını tanımlama** bölümünde, sağlayan bir **adı** ve **açıklama** uyarı kuralı ve küme için kendi **önem derecesi**.
Ayrıca, emin olun **oluşturulduktan sonra kuralı etkinleştir** düğmesi ayarlandığında **Evet**.

![Uyarı ayrıntıları ekran görüntüsü](./media/proactive-cloud-services/004.png)

Altında **tanımla eylem grubu** bölümünde, mevcut bir seçebileceğiniz **eylem grubu** veya yeni bir tane oluşturun.
Çeşitli türlerde birden fazla eylem içeren eylem grubu seçebilirsiniz.

![Eylem grubu ekran görüntüsü](./media/proactive-cloud-services/005.png)

Eylem grubu tanımladınız sonra değişikliklerinizi onaylamak ve tıklayın **uyarı kuralı oluştur**.

## <a name="next-steps"></a>Sonraki Adımlar

Otomatik olarak algılama hakkında daha fazla bilgi edinin:

[Hata anomalileri](../../azure-monitor/app/proactive-failure-diagnostics.md)
[bellek sızıntılarını](../../azure-monitor/app/proactive-potential-memory-leak.md)
[performans anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md)

