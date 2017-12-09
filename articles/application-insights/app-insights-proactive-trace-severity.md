---
title: "Algılama - Azure Application ınsights'ta izleme önem derecesi oranı düşüşü akıllı | Microsoft Docs"
description: "Azure Application Insights ile izleme telemetri olağan dışı düzenleri uygulama izlemelerini izleyin."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 29ed195dadb7230df425d6d981a0a482e09ee79f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>İzleme önem derecesi oranı (Önizleme) düşüşü

Arka planda neler olay anlatın Yardım gibi izlemeleri uygulamalarda, yaygın olarak kullanılır. Şeyler ters gittiğinde izlemeleri istenmeyen durumuna önde gelen olayların sırası önemli görünürlük sağlar. İzlemeler genellikle yapılandırılmamış olsa da, bunları – önem düzeylerini concretely öğrenilebilecek bir şey yoktur. Bir uygulamanın kararlı durumda biz "iyi" izlemeleri arasındaki oran beklediğiniz (*bilgisi* ve *ayrıntılı*) ve "bozuk" izlemeleri (*uyarı*, *hata* ve *kritik*) tutarlı kalması için. "Bozuk" izlemeleri düzenli olarak belirli bir ölçüde birkaç nedenden dolayı nedeniyle oluşabilir varsayılır (örneğin geçici ağ sorunları). Ancak gerçek bir problem büyüyen başladığında, bu genellikle "bozuk" izlemeleri vs "iyi" izlemeleri göreli oranını artış olarak bildirimleri. Uygulama Öngörüler akıllı algılama otomatik olarak, uygulamanız tarafından günlüğe kaydedilen izlemeleri analiz eder ve izleme telemetrinizi önemini olağan dışı düzenleri hakkında uyarır.

Bu özellik, uygulamanız için izleme günlüğü yapılandırma dışında hiçbir özel kurulum gerektirir (bir izleme günlüğü dinleyicisi için yapılandırma konusunda bkz [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) veya [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Uygulamanızı yeterli özel durum telemetrisi oluşturduğunda etkin olur.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Bu tür akıllı algılama bildirim ulaştıklarında?
Böyle bir bildirim geri alabilirsiniz "iyi" izlemeleri arasındaki oran (bir düzeyi ile oturum izlemeleri *bilgisi* veya *ayrıntılı*) ve "bozuk" izlemeleri (bir düzeyi ile oturum izlemeleri *uyarı*, * Hata, veya *önemli*) önceki yedi gün içinde hesaplanan bir taban çizgisine göre belirli bir gün içinde kötüleştirme.

## <a name="does-my-app-definitely-have-a-problem"></a>Uygulamam kesinlikle bir sorun var mı?
Hayır, bir bildirim uygulamanızı kesinlikle bir sorun olduğu anlamına gelmez. "İyi" ve "bozuk" izlemeleri arasındaki oran düşmesine uygulama sorununu gösterebilir karşın, bu değişikliği oranı, zararsız olabilir. Örneğin, artırma varolan Akışları'den daha fazla "bozuk" izleri yayma uygulamasında yeni bir akış nedeniyle olabilir).

## <a name="how-do-i-fix-it"></a>Bunu nasıl düzeltirim?
Bildirimleri tanılama işlemi desteklemek için tanılama bilgileri içerir:
1. **Önceliklendirme.** Bildirim kaç işlemleri etkilenir gösterir. Bu sorun için bir öncelik atamanıza yardımcı olabilir.
2. **Kapsamı.** Sorun, tüm trafik ya da yalnızca başka bir işlem söz konusu? Bu bilgiler bildirimden alınabilir.
3. **Tanılayın.** Daha fazla yardımcı olacak bilgiler, destek bağlama raporları sorunu tanılamak ve ilgili öğeler kullanabilirsiniz.


