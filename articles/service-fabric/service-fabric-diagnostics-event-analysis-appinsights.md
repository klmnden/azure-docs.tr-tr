---
title: Application Insights ile Azure Service Fabric olay analizi | Microsoft Docs
description: İzleme ve Tanılama, Azure Service Fabric kümeleri için Application Insights ile olayları çözümleme ve görselleştirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: f4c620bbb0e17abfacb504866230786a971ff409
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393206"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Olay analizi ve Application Insights ile Görselleştirme

Azure İzleyici, Application Insights parçası, uygulama izleme ve tanılama için genişletilebilir bir platformdur. Uyarı seçenekleri dahil olmak üzere daha fazla otomatik ve güçlü analiz ve aracı, özelleştirilebilir bir Pano ve görsel öğeler sorgulanırken içerir. Application Insights'ın Service Fabric ile tümleştirmesi araç deneyimlerinden Service Fabric belirli ölçümleri yanı sıra, Visual Studio ve Azure portalı için kapsamlı kullanıma hazır günlüğe kaydetme deneyimi sağlama çalışmaları içerir. Birçok günlükleri otomatik olarak oluşturulur ve sizin için Application Insights ile toplanan rağmen özel günlük daha fazla daha zengin bir tanılama deneyimi oluşturmak için uygulamalarınıza eklemenizi öneririz.

Bu makalede, aşağıdaki yaygın soruları yardımcı olur:

* Nasıl uygulama ve Hizmetleri ve toplama telemetrimi içinde neler olduğunu biliyor musunuz?
* Uygulamamın, özellikle birbiriyle iletişim hizmetleri nasıl giderebilirim?
* Ölçümleri nasıl Hizmetlerim, örneğin gerçekleştiriyorsanız, sayfa yükleme süresi, HTTP istekleri hakkında nasıl alabilirim?

Bu makalenin amacı, Öngörüler ve gelen Application Insights içinde ilgili sorunları giderme göstermektir. Ayarlama ve Application Insights ile Service Fabric yapılandırma konusunda bilgi almak istiyorsanız, bunu kontrol [öğretici](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-application-insights"></a>Application ınsights'ı izleme

Application Insights Service Fabric kullanırken Zengin bir deneyimi vardır. Genel bakış sayfasında, Application Insights, hizmet yanıt süresi ve işlenen istek sayısı gibi hakkında önemli bilgiler sağlar. Üst 'Ara' düğmesine tıklayarak, uygulamanızda son isteklerinin bir listesini görebilirsiniz. Ayrıca, başarısız istekler burada görebilir ve hataları meydana gelebilir tanılama olacaktır.

![Application Insights'a genel bakış](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

Önceki görüntüde sağ panelde listedeki girişleri iki ana türü vardır: istekleri ve olaylar. Bu durumda uygulamanın API'sine HTTP istekleri aracılığıyla yapılan çağrılar isteklerdir ve kodunuzdaki herhangi bir yere ekleyebilirsiniz telemetri olarak davranacak özel olaylar olaylardır. Uygulamalarınızda ölçümlü izleme daha da keşfedebilirsiniz [özel olaylar ve ölçümler için Application Insights API](../azure-monitor/app/api-custom-events-metrics.md). Bir isteği tıklayarak daha fazla ayrıntı Application ınsights'ı Service Fabric nuget paketinin toplanan Service fabric'e özgü veriler dahil olmak üzere aşağıdaki görüntüde gösterildiği gibi görüntülenebilir. Bu bilgileri, sorun giderme ve uygulamanızın durumunu ne olduğunu bilmek için kullanışlıdır ve bu bilgilerin tümünü Application Insights içinde aranabilir

![Application Insights İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights gelen tüm verilerde sorgulama için bir atanan görünümü vardır. Application Insights portalına gitmek için genel bakış sayfasının üst kısmındaki "Ölçüm Gezgini" tıklayın. Burada özel olaylar daha önce belirtildiği, istekleri, özel durumlar, performans sayaçları ve diğer ölçümleri Kusto sorgu dilini kullanarak karşı sorgular çalıştırabilirsiniz. Aşağıdaki örnek, son 1 saat içindeki tüm istekleri gösterir.

![Application Insights İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Application Insights portalında yeteneklerini daha iyi keşfedilebilmesi için attıktan [Application Insights portal belgeleri](../azure-monitor/app/app-insights-dashboards.md).

### <a name="configuring-application-insights-with-eventflow"></a>EventFlow ile Application Insights'ı yapılandırma

Olayları toplama EventFlow kullanıyorsanız, içeri aktarmak emin olun `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet paketi. Aşağıdaki kodu gereklidir *çıkarır* bölümünü *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Yanı sıra gerekli değişiklikleri yaptırın filtrelerinizi dahil tüm diğer girişler (birlikte, ilgili NuGet paketlerinin) emin olun.

## <a name="application-insights-sdk"></a>Application Insights SDK'sı

EventFlow ve WAD toplama çözümler olarak, çıkış EventFlow değiştirmek istiyorsanız, yani izleme, değişiklik, gerçek araçları gerektirir ve tanılama daha modüler bir yaklaşım için izin ver olduğundan yalnızca kullanın. önerilen bir Basit bir değişiklikle yapılandırma dosyanıza. Ancak, Application Insights kullanarak yatırım yapmaya karar verin ve farklı bir platform için değişmesi olasılığı olan değil, olayları toplamak ve Application Insights'a göndererek için yeni Application Insights SDK'sını kullanarak görünmelidir. Başka bir deyişle, artık verilerinizi uygulama anlayışları'na göndermek için EventFlow yapılandırması gerekir, ancak bunun yerine ApplicationInsight'ın Service Fabric NuGet paketi yükler. Paket Ayrıntıları bulunabilir [burada](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights, mikro hizmetler ve kapsayıcılar için destek](https://azure.microsoft.com/blog/app-insights-microservices/) bazıları gösterilmektedir (yine de şu anda beta) çalışan yeni özelliklerinin hangi izin Application Insights ile daha zengin kullanıma hazır izleme seçeneğiniz vardır. Bu bağımlılık izleme (tüm hizmetleri ve uygulamaları bir küme ve bunların arasındaki iletişimi bir AppMap oluşturulmasında kullanılan) ve daha iyi bağıntısı hizmetlerinizi (daha iyi bir sorun iş akışındaki sunulan içinde yardımcı geldiğini izlemeleri içerir bir uygulama veya hizmet).

.NET ile geliştirme ve büyük olasılıkla Service Fabric'in programlama modelleri ve bu olay ve günlük verilerini çözümleme ve görselleştirme için Application Insights platformunuz kullanmak istediğiniz bazı kullanıp, ardından Application ınsights'ı Git öneririz İzleme ve tanılama iş akışı olarak SDK yolu. Okuma [bu](../azure-monitor/app/asp-net-more.md) ve [bu](../azure-monitor/app/asp-net-trace-logs.md) toplamak ve günlüklerinizi görüntülemek için Application Insights'ı kullanmaya başlamak için.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Application Insights kaynağı Azure portalında gezinme

Application Insights olayları ve günlükleri için bir çıktı olarak yapılandırdıktan sonra bilgi birkaç dakika içinde Application Insights kaynağınıza görünmesini başlamanız gerekir. Application Insights kaynağı panoya sürer Application Insights kaynağına gidin. Tıklayın **arama** görev çubuğunda Application Insights alındığında son izlemelere bakın ve bunları filtrelemek için.

*Ölçüm Gezgini* , uygulamalarınıza, hizmetlerinize ve küme raporlama ölçümlere göre özel panolar oluşturmak için kullanışlı bir araçtır. Bkz: [keşfetmeye ölçümler Application ınsights'da](../azure-monitor/app/metrics-explorer.md) birkaç grafikleri kendiniz toplama verileri temel alan için ayarlanacak.

Tıklayarak **Analytics** burada olaylarla ve izlemelerle daha fazla kapsam ve isteğe bağlı olma sorgulayabilirsiniz sizi Application Insights Analytics portalına götürür. Şu anda hakkında daha fazla bilgiyi [Application Insights analiz](../azure-monitor/app/analytics.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Yapay ZEKA uyarıları ayarlama](../azure-monitor/app/alerts.md) performans ya da kullanım değişiklikler hakkında bildirim almak için
* [Akıllı algılama Application ınsights'ta](../azure-monitor/app/proactive-diagnostics.md) olası performans sorunları sizi uyarabilmek için Application Insights'a gönderilen telemetri bir öngörülü analiz gerçekleştirir
