---
title: Azure Application Insights'a Azure tanılama tümleştirmesi kullanılarak Azure Cloud Services sorunları uyar | Microsoft Docs
description: Başlatma hataları, kilitlenme ve rol Azure Application ınsights'la Azure Cloud Services Döngülerde geri dönüşüm gibi sorunlar için İzleyici
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: harelbr; mbullwin
ms.openlocfilehash: 18817fd84a86a72d379f96973b71658f2cdf4afd
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851357"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Azure Application Insights'a Azure tanılama tümleştirmesi kullanılarak Azure Cloud Services sorunları uyar

Bu makalede, biz başlatma hataları, kilitlenme ve rol Azure bulut Hizmetleri (web ve çalışan rolleri) Döngülerde geri dönüşüm gibi İzleyicisi sorunları için uyarı kuralları ayarlanacağı açıklanmıştır.

Bu makalede açıklanan yöntemi dayanır [Application Insights ile Azure tanılama tümleştirme](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/)ve yakın zamanda piyasaya çıkan [Application Insights için günlük uyarıları](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/) yeteneği.

## <a name="define-a-base-query"></a>Temel sorgu tanımlayın

Başlamak için biz Windows Azure kanaldan Windows olay günlüğü olaylarını alır temel bir sorgu, Application Insights izleme kayıtları olarak içine yakalanan tanımlayacaksınız.
Bu kayıtları Azure Cloud Services ' çalışma zamanı hataları başlatma hataları gibi çok çeşitli sorunlar algılamak için kullanılan ve döngüler geri dönüştürün.

> [!NOTE]
> Aşağıdaki temel sorgu 30 dakikalık bir zaman penceresi sorunları olup olmadığını denetler ve telemetri kayıtları alma, 10 dakikalık gecikme varsayar. Bu varsayılan uygun gördüğünüz şekilde yapılandırılabilir.

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

## <a name="check-for-specific-event-ids"></a>Belirli olay kimlikleri denetle

Windows olay günlüğü olaylarını aldıktan sonra belirli sorunları (aşağıdaki örneklere bakın) kendi ilgili olay kimliği ve ileti özelliklerini denetleyerek algılanabilir.
Yalnızca yukarıda temel sorgu sorgu günlük uyarı kuralı tanımlarken birleştirilmiş sorguları aşağıdaki ve kullanılan biri ile birleştirin.

> [!NOTE]
> Üçten fazla olayları çözümlenen zaman penceresi sırasında bulunursa, aşağıdaki örneklerde, bir sorun algılanır. Bu varsayılan uyarı kuralı duyarlılığını değiştirmek için yapılandırılabilir.

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

Application Insights kaynağınıza içinde gezinti menüsünde Git **uyarıları**ve ardından **yeni uyarı kuralı**.

![Kural, ekran görüntüsü oluştur](./media/app-insights-proactive-cloud-services/001.png)

İçinde **oluşturma kuralı** penceresi altında **Uyarı koşulu tanımla** bölümünde, tıklayın **ölçüt eklemek**ve ardından **özel günlük arama**.

![Uyarının koşulu ölçüt tanımla ekran görüntüsü](./media/app-insights-proactive-cloud-services/002.png)

İçinde **arama sorgusu** kutusunda, önceki adımda hazırladığınız birleşik sorgu yapıştırın.

Daha sonra devam **eşik** kutusuna ve değeri 0 olarak ayarlayın. İsteğe bağlı olarak ince ayar **süresi** ve sıklığı **alanları**.
**Bitti**’ye tıklayın.

![Ekran görüntüsü sinyal mantığı sorgu yapılandırın](./media/app-insights-proactive-cloud-services/003.png)

Altında **uyarı ayrıntılarını tanımlayın** bölümünde, sağlayan bir **adı** ve **açıklama** uyarı kuralı ve küme için kendi **önem**.
Ayrıca, olduğundan emin olun **etkinleştir kural oluşturulduktan sonra** düğmesi ayarlanmış **Evet**.

![Uyarı ayrıntıları ekran görüntüsü](./media/app-insights-proactive-cloud-services/004.png)

Altında **tanımla eylem grubu** bölümüne, var olan seçebilirsiniz **eylem grubu** veya yeni bir tane oluşturun.
Çeşitli türlerdeki birden çok eylem içeren eylem grup seçebilirsiniz.

![Ekran eylem grubu](./media/app-insights-proactive-cloud-services/005.png)

Eylem grubunu tanımladığınız sonra yaptığınız değişiklikleri onaylamak ve tıklatın **oluşturma uyarı kuralı**.

## <a name="next-steps"></a>Sonraki Adımlar

Otomatik olarak algılama hakkında daha fazla bilgi edinin:

[Hata anormallikleri](app-insights-proactive-failure-diagnostics.md)
[bellek sızıntıları](app-insights-proactive-potential-memory-leak.md)
[performans anormalliklerini](app-insights-proactive-performance-diagnostics.md)

