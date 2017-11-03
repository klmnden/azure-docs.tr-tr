---
title: "ASP.NET Core için Azure Application Insights | Microsoft Docs"
description: "Kullanılabilirlik, performans ve kullanım için Web uygulamalarını izleyin."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 74f99dd6f31ecff7c838d8f710a7fe4279ce0ea9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-for-aspnet-core"></a>ASP.NET Core için Application Insights
[Application Insights](app-insights-overview.md) , web uygulamanızın kullanılabilirlik, performans ve kullanım izlemenize izin verir. Uygulamanızın gerçek hayattaki performansı ve etkinliğine ilişkin aldığınız geri bildirimlerden yararlanarak her geliştirme yaşam döngüsünde tasarımın yönü konusunda bilinçli kararlar alabilirsiniz.

![Örnek](./media/app-insights-asp-net-core/sample.png)

Bir aboneliğe gerekir [Microsoft Azure](http://azure.com). Windows, XBox Live veya diğer Microsoft bulut hizmetlerinde kullanıyor olabileceğiniz bir Microsoft hesabıyla oturum açın. Takımınızın kurumsal bir Azure aboneliğine sahip olabilir: sahibinden Microsoft hesabınızı kullanarak eklemeli isteyin.

## <a name="getting-started"></a>Başlarken

* Visual Studio Çözüm Gezgini'nde, projenize sağ tıklayın ve seçin **yapılandırma Application Insights**, veya **Ekle > Application Insights**. [Daha fazla bilgi edinin](app-insights-asp-net.md).
* Bu menü komutlarını görmüyorsanız izleyin [alma Başlarken Kılavuzu](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Projeniz Visual Studio sürümü ile 2017 önce oluşturulduysa, bunu yapmanız gerekebilir.

## <a name="using-application-insights"></a>Application Insights’ı Kullanma
Oturum [Microsoft Azure portal](https://portal.azure.com)seçin **tüm kaynakları** veya **Application Insights**ve ardından oluşturduğunuz uygulamanızı izlemek için kaynak seçin.

Ayrı bir tarayıcı penceresinde uygulamanızı biraz kullanın. Application Insights grafikte görüntülenen verileri görürsünüz. (Yenile'yi tıklatın gerekebilir.) Geliştirme yapıyorsanız sırasında yalnızca küçük miktarda veri olacaktır, ancak uygulamanızı yayınlama ve çok sayıda kullanıcı varsa bu grafikler gerçekten Canlı gelir. 

Genel bakış sayfasında anahtar performans grafiklerini gösterir: sunucu yanıt süresi, sayfa yükleme süresi ve başarısız isteklerin sayısı. Daha fazla grafikler ve veri görmek için herhangi bir grafiğe tıklayın.

Portal görünümlerde üç ana kategoriye ayrılır:

* [Ölçüm Gezgini](app-insights-metrics-explorer.md) grafikleri ve ölçümleri ve yanıt sürelerini, başarısızlık oranları veya kendiniz ile oluşturduğunuz ölçümleri gibi sayıları tabloları gösterir [API](app-insights-api-custom-events-metrics.md). Verileri daha iyi uygulamanızı ve kullanıcılarına anlamak almak için özellik değerlerine göre segmentlere ayırmak ve filtreleyebilirsiniz.
* [Arama Gezgini](app-insights-diagnostic-search.md) belirli istekleri, özel durumlar, günlük izlemelerini veya kendiniz ile oluşturulan olaylar gibi olayları tek tek listeler [API](app-insights-api-custom-events-metrics.md). Filtre ve olayları arayın ve sorunları araştırmak için ilgili olaylar arasında gidin.
* [Analytics](app-insights-analytics.md) siz telemetrinize SQL benzeri sorguları çalıştırmak sağlar ve güçlü bir Analitik ve tanı aracıdır.

## <a name="alerts"></a>Uyarılar
* Otomatik olarak Al [öngörülü tanılama uyarıları](app-insights-proactive-diagnostics.md) , belirtilir başarısızlık oranları ve diğer ölçümleri anormal değişiklikler hakkında.
* Ayarlanan [kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) sürekli konumlardan dünya çapında Web sitenizi test ve herhangi bir test başarısız olarak e-postaları almak için.
* Ayarlanan [ölçüm uyarıları](app-insights-monitor-web-app-availability.md) ölçümleri yanıt sürelerini veya özel durum oranları gibi dış kabul edilebilir sınırlar Git olmadığını bilmek.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Açık kaynak
[Okuma ve koda katkıda bulunan](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Sonraki adımlar
* [Web sayfalarınıza telemetri ekleyin](app-insights-javascript.md) sayfa kullanımını izleme ve performans.
* [İzleme bağımlılıkları](app-insights-asp-net-dependencies.md) REST, SQL veya diğer dış kaynaklara, yavaşlamadan olmadığını görmek için.
* [API kullanan](app-insights-api-custom-events-metrics.md) kendi olayları ve ölçümleri, uygulamanızın performansı ve kullanımı daha ayrıntılı bir görünüm için gönderilecek.
* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) uygulamanızdan sürekli dünyanın denetleyin. 

