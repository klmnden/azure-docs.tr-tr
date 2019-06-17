---
title: Akıllı algılama - Azure Application ınsights izleme önem derecesi oranı düşüş | Microsoft Docs
description: Azure Application Insights ile uygulama izlemeleri için izleme telemetrisi olağandışı desenleri izleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 10b909fd5239546047aa4696a1f6a68a703778c0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60306403"
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>İzleme önem derecesi oranı (Önizleme) içinde performans düşüşü

Bunlar perde arkasında neler hikayeyi anlatmalarına yardımcı olmak gibi izlemeleri uygulamalarında yaygın olarak kullanılır. İşler kötüye gittiğinde izlemeleri istenmeyen duruma önde gelen olayların sırası önemli görünürlük sağlar. İzlemeleri genel olarak yapılandırılmamış olsa da, concretely bunları – önem düzeylerini öğrenilmesi bir şey yoktur. Bir uygulamanın durağan biz "good" izlemeleri arasındaki oran beklediğiniz (*bilgisi* ve *ayrıntılı*) ve "bozuk" izlemeleri (*uyarı*, *hata*, ve *kritik*) tutarlı kalmasını. "Kötü" izlemeleri düzenli olarak belirli bir ölçüde birkaç nedenden dolayı ortaya çıkabilir varsayılır (örneğin geçici ağ sorunları). Ancak gerçek bir soruna büyüyen başladığında, bu genellikle bir artış "kötü" izlemeleri vs "good" izlemeleri göreli oranı olarak bildirimleri. Application Insights, akıllı algılama otomatik olarak uygulamanız tarafından günlüğe izlemeleri analiz eder ve olağan dışı önem derecesi, izleme telemetrisi yaklaşımlar hakkında uyarır.

Bu özellik, uygulamanız için izleme günlüğü yapılandırma dışında hiçbir özel kurulum gerektirir (görmek için bir izleme günlüğü dinleyici yapılandırma [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) veya [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Uygulamanızın yeterli özel durum telemetrisi oluşturduğunda etkin değil.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür bir akıllı algılama bildirim ne zaman elde edersiniz?
Bu tür bir bildirim geri alabilirsiniz "good" izlemeleri arasındaki oran (bir düzeyde günlüğe izlemeleri *bilgisi* veya *ayrıntılı*) ve "bozuk" izlemeleri (bir düzeyde günlüğe izlemeleri *uyarı*, *Hata*, veya *önemli*) son yedi gün hesaplanan temel karşılaştırıldığında belirli bir gün içinde düşürmesini.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamamı kesinlikle bir sorun var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. "Good" ve "bozuk" izlemeleri arasındaki oran düşüş bir uygulama sorunu olduğunu belirtebilir olsa da, bu değişikliği oranı, zararsız olabilir. Örneğin, artış için mevcut akışları değerinden daha fazla "kötü" izlemeleri yayma uygulama yeni bir akışta olabilir).

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimler tanılama işleminde desteklemek için tanılama bilgileri içerir:
1. **Değerlendirme.** Bildirim kaç operations etkilediğini gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsam.** Sorun, tüm trafiği veya yalnızca bazı işlem etkileniyor? Bu bilgiler gelen bildirim elde edilebilir.
3. **Tanılayın.** İlgili öğeleri kullanabilirsiniz ve daha fazla yardımcı olacak bilgiler için destek bağlama raporları sorunu tanılayın.


