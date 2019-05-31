---
title: Azure Application Insights günlük tabanlı ve önceden toplanmış ölçümlerde | Microsoft Docs
description: Neden oturum tabanlı önceden toplanan ölçümler Azure Application ınsights'ı kullanın.
services: application-insights
keywords: ''
author: vgorbenko
ms.author: vitalyg
ms.reviewer: mbullwin
ms.date: 09/18/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9520cbb9973071bf1c52266d7718837607c1d10f
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66256123"
---
# <a name="log-based-and-pre-aggregated-metrics-in-application-insights"></a>Günlük tabanlı ve önceden toplanan ölçümler Application ınsights

Bu makalede günlüklerine göre "Geleneksel" Application Insights ölçüm ve şu anda genel Önizleme aşamasındadır önceden toplanan ölçümler arasındaki farkı açıklar. Her iki tür ölçüm Application Insights kullanıcıları için kullanılabilir ve her uygulama sistem durumu, tanılama ve analiz izleme benzersiz bir değer getirir. İşaretleme uygulamaları geliştiriciler ölçüm türü en iyi seçenek olduğuna karar verebilirsiniz uygulama boyutuna bağlı olarak belirli bir senaryo için telemetri ve iş gereksinimleri, beklenen birim ölçümleri duyarlık ve uyarı amacıyla uygun.

## <a name="log-based-metrics"></a>Günlük tabanlı ölçümleri

Yakın zamanda kadar uygulama Application Insights telemetri veri modelinde izleme yalnızca olaylar, istekler, özel durumlar, bağımlılık çağrıları, sayfa görüntülemeleri, vb. gibi önceden tanımlanmış tür az sayıda temel. Geliştiriciler ya da el ile (SDK'sı açıkça çağıran kod yazarak) bu olayları yaymak için SDK'sını kullanabilir veya otomatik izleme olaylarını otomatik olarak toplama üzerinde güvenebilirsiniz. Her iki durumda da, Application Insights arka uç tüm toplanan olayları günlükleri olarak depolar ve günlüklerinden olay-tabanlı verileri görselleştirmeye yönelik bir analiz ve Tanılama Aracı Azure portalındaki Application Insights dikey pencereleri görür.

Olayları eksiksiz bir kümesini korumak için günlükleri kullanma, analiz ve tanılama fayda getirebilirsiniz. Örneğin, bir tam sayı istekleri belirli bir URL'ye bu çağrıları yapan benzersiz kullanıcı sayısı ile elde edebilirsiniz. Özel durumlar dahil olmak üzere ayrıntılı tanılama izlemeleri alabilirsiniz ve tüm kullanıcı oturumlarını bağımlılık çağrıları. Bu tür bilgiler sahip uygulama durumunu ve kullanım süresini bir uygulama ile ilgili sorunları tanılamak gerekli Kes izin vererek, önemli ölçüde artırabilir. 

Aynı zamanda, eksiksiz bir olay toplama pratik (veya hatta imkansız) olabilir, çok sayıda telemetri oluşturan uygulamalar için. Olayların hacmine çok yüksek olduğunda durumlar için Application Insights birkaç telemetri birim azaltma teknikleri gibi uygulayan [örnekleme](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) ve [filtreleme](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) sayısını azaltın toplanan ve depolanan olayları. Ne yazık ki, depolanmış olayların sayısı da doğruluğu, arka planda, olay günlüklerinde saklanan sorgu süresini toplamalar gerçekleştirmeniz gerekir ölçümleri düşürür.

> [!NOTE]
> Uygulama anlayışları'nda sorgu süresi toplama olayları ve ölçümleri günlüklerinde saklanan bağlı ölçümlerin günlük tabanlı ölçümler çağrılır. Bu ölçümler genellikle bunları, analiz için üstün sağlar, olay özelliklerinden birçok boyutlara sahip ancak bu ölçümler doğruluğunu örnekleme ve filtreleme olumsuz etkilenir.

## <a name="pre-aggregated-metrics"></a>Önceden toplanan ölçümler

Günlük tabanlı ölçümler ek olarak, Application Insights ekibi zaman serisi için en iyi duruma getirilmiş özel bir depoda depolanan ölçümleri genel önizlemesi 2018'den sonbaharda birlikte gelir. Yeni ölçümler, artık çok sayıda özellikleri tek tek olayları olarak tutulur. Bunun yerine, bunlar önceden toplanmış zaman serisi olarak ve yalnızca anahtar boyutlarla depolanır. Bu yeni ölçümler sorgu zamanında üstün sağlar: veri alma, çok daha hızlı gerçekleşir ve daha az işlem gücü gerektirir. Bu nedenle yeni senaryoları gibi sağlar [neredeyse gerçek zamanlı ölçüm boyutlara uyarı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)ve daha duyarlı [panolar](https://docs.microsoft.com/azure/azure-monitor/app/overview-dashboard)ve daha fazlası.

> [!IMPORTANT]
> Hem günlük tabanlı hem de önceden toplanan ölçümler Application Insights'da arada. Geleneksel ölçümleri olayları "için ölçüler günlük tabanlı" adlandırılırsa sırada önceden toplanan ölçümler "Standart ölçümler (Önizleme)", artık adlandırılır Application ınsights'ta UX iki ayırt etmek için.

Yeni SDK'ları ([Application Insights 2.7](https://www.nuget.org/packages/Microsoft.ApplicationInsights/2.7.2) SDK veya .NET için sonraki bir sürümü) önceden toplu ölçümleri telemetri birim azaltma teknikleri önce toplama sırasında etkisini göstermeye. Başka bir deyişle, yeni ölçümler doğruluğunu örnekleme ve en son Application Insights SDK'ları kullanırken filtreleme etkilenmez.

Önceden toplayarak (diğer bir deyişle, eski sürümleri Application Insights SDK'ları veya tarayıcı araçları) uygulamayıp SDK'ları için Application Insights arka uç hala yeni ölçümler uygulama tarafından alınan olayları toplayarak doldurur Insights olay toplama bitiş noktası. Azaltılmış kablo üzerinden aktarılan veri hacmi yararlı değildir, ancak yine de önceden toplanan ölçümler olarak kullanıp daha iyi performans ve neredeyse gerçek zamanlı boyutlu içermeyen SDK'ları ile uyarılar desteği deneyimi, yani Koleksiyon sırasında önceden toplu ölçümleri.

Toplama bitiş noktası önceden anlamına alım örneklemesi önce olayları toplayan bahseden değer olduğu [alım örneklemesi](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) etkisi SDK sürümünden bağımsız olarak önceden toplanan ölçümler doğruluğunu hiçbir zaman, uygulamanızla birlikte kullanın.  

## <a name="using-pre-aggregation-with-application-insights-custom-metrics"></a>Application Insights özel ölçümler ile önceden toplayarak kullanma

Önceden toplayarak ile özel ölçümleri kullanabilirsiniz. İki ana faydası özelliği yapılandırın ve özel ölçüm ve Application Insights toplama bitiş noktası için SDK'sından gönderilen veri hacmini azaltarak boyut uyarı var.

Vardır [özel ölçümler Application Insights SDK'sından gönderme yolları](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). SDK sürümünüze sunuyorsa [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) yöntemleri budur gönderen özel ölçümler, tercih edilen yol olduğundan bu durumda yalnızca depolanan verilerin hacmini azaltarak, SDK içinde önceden toplayarak olur Azure, aynı zamanda SDK'sından Application Insights'a aktarılan veri hacmi. Aksi takdirde kullanın [trackMetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) veri alımı sırasında önceden ölçüm olayları toplayacak yöntemi.

## <a name="custom-metrics-dimensions-and-pre-aggregation"></a>Özel ölçümler boyutları ve önceden toplayarak

Kullanarak gönderdiğiniz tüm ölçümler [trackMetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) veya [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) API çağrılarıdır otomatik olarak hem günlükleri ve ölçümleri depolarında depolanan. Ancak, özel ölçüm günlük tabanlı sürümü her zaman tüm boyutlar tutarken ölçüm önceden toplanmış sürümünü herhangi bir boyutu ile varsayılan olarak depolanır. Özel ölçümler boyutlarını koleksiyonunda kapatma [kullanım ve tahmini maliyet](https://docs.microsoft.com/azure/application-insights/app-insights-pricing) "Özel ölçüm boyutlara etkin uyarı" denetleyerek sekmesinde: 

![Kullanım ve tahmini maliyet](./media/pre-aggregated-metrics-log-metrics/001-cost.png)

## <a name="why-is-collection-of-custom-metrics-dimensions-turned-off-by-default"></a>Neden özel ölçümler boyutları koleksiyonu varsayılan olarak kapalıdır?

Boyutsuz özel ölçümler depolama (en fazla bir kota) ücretsiz kalır ancak gelecekte boyutları olan özel ölçümler depolama ayrı olarak uygulama anlayışları'ndan Faturalandırılacak çünkü özel ölçümler boyutları koleksiyonu varsayılan olarak devre dışıdır . Bizim resmi yaklaşan fiyatlandırma modeli değişiklikleri hakkında bilgi edinebilirsiniz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/monitor/).

## <a name="creating-charts-and-exploring-log-based-and-standard-pre-aggregated-metrics"></a>Grafik oluşturma ve oturum tabanlı ve standart önceden toplanan ölçümleri keşfederken

Kullanım [Azure İzleyici ölçüm Gezgini'ni](../platform/metrics-getting-started.md) grafikleri önceden toplanmış ve günlük tabanlı ölçümleri ve yazar panolara grafiklerle çizmek için. İstediğiniz Application Insights kaynağını seçtikten sonra standart (Önizleme) ve günlük tabanlı ölçümler arasında geçiş yapmak için ad alanı seçiciyi kullanın veya özel bir ölçüm ad alanı seçin:

![Ölçüm ad alanı](./media/pre-aggregated-metrics-log-metrics/002-metric-namespace.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Neredeyse gerçek zamanlı uyarı verme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)
* [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric)