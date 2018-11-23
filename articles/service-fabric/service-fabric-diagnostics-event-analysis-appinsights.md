---
title: Application Insights ile Azure Service Fabric olay analizi | Microsoft Docs
description: İzleme ve Tanılama, Azure Service Fabric kümeleri için Application Insights ile olayları çözümleme ve görselleştirme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: f9c7a70eae4c49173b3e11b7fbfa901f7e5b89d6
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291054"
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

Önceki görüntüde sağ panelde listedeki girişleri iki ana türü vardır: istekleri ve olaylar. Bu durumda uygulamanın API'sine HTTP istekleri aracılığıyla yapılan çağrılar isteklerdir ve kodunuzdaki herhangi bir yere ekleyebilirsiniz telemetri olarak davranacak özel olaylar olaylardır. Uygulamalarınızda ölçümlü izleme daha da keşfedebilirsiniz [özel olaylar ve ölçümler için Application Insights API](../application-insights/app-insights-api-custom-events-metrics.md). Bir isteği tıklayarak daha fazla ayrıntı Application ınsights'ı Service Fabric nuget paketinin toplanan Service fabric'e özgü veriler dahil olmak üzere aşağıdaki görüntüde gösterildiği gibi görüntülenebilir. Bu bilgileri, sorun giderme ve uygulamanızın durumunu ne olduğunu bilmek için kullanışlıdır ve bu bilgilerin tümünü Application Insights içinde aranabilir

![Application Insights İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights gelen tüm verilerde sorgulama için bir atanan görünümü vardır. Application Insights portalına gitmek için genel bakış sayfasının üst kısmındaki "Ölçüm Gezgini" tıklayın. Burada özel olaylar daha önce belirtildiği, istekleri, özel durumlar, performans sayaçları ve diğer ölçümleri Kusto sorgu dilini kullanarak karşı sorgular çalıştırabilirsiniz. Aşağıdaki örnek, son 1 saat içindeki tüm istekleri gösterir.

![Application Insights İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Application Insights portalında yeteneklerini daha iyi keşfedilebilmesi için attıktan [Application Insights portal belgeleri](../application-insights/app-insights-dashboards.md).

### <a name="configuring-application-insights-with-wad"></a>WAD ile Application Insights'ı yapılandırma

>[!NOTE]
>Bu yalnızca şu anda Windows kümeleri için geçerlidir.

WAD içinde ayrıntılı olarak WAD yapılandırma için bir Application Insights havuz ekleyerek gerçekleştirilir Azure Application Insights veri göndermek için birincil iki yolu vardır [bu makalede](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-application-insights-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Azure Portalı'nda bir küme oluştururken, bir Application Insights izleme anahtarı Ekle

![Bir AIKey ekleme](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Tanılama "On" kapalıysa bir küme oluştururken, bir Application Insights izleme anahtarını girmek için isteğe bağlı bir alan gösterilir. Application Insights anahtarınızı buraya yapıştırın, Application Insights havuz otomatik olarak sizin için kümenize dağıtmak için kullanılan Resource Manager şablonunda yapılandırılır.

#### <a name="add-the-application-insights-sink-to-the-resource-manager-template"></a>Application Insights havuz için Resource Manager şablonu Ekle

"WadCfg" Resource Manager şablonu içinde aşağıdaki iki değişiklikler dahil ederek bir "havuz" ekleyin:

1. Doğrudan, bildirme sonra havuz Yapılandırması Ekle `DiagnosticMonitorConfiguration` tamamlanır:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Havuza eklemek `DiagnosticMonitorConfiguration` dosyasında aşağıdaki satırı ekleyerek `DiagnosticMonitorConfiguration` , `WadCfg` (hemen önce `EtwProviders` bildirilir):

    ```json
    "sinks": "applicationInsights"
    ```

Her iki önceki kod parçacıklarında, adı "Applicationınsights" havuz tanımlamak için kullanıldı. Bu bir gereksinim değildir ve havuz adı "havuzlarını içinde" dahil olduğu sürece, herhangi bir dize adı ayarlayabilirsiniz.

Şu anda, küme günlüklerinden görünmesini olarak **izlemeleri** Application Insights günlük Görüntüleyicisi. En çok platformdan gelen izlemelerin düzeyi "Bilgilendirici" olduğundan, ayrıca yalnızca "Kritik" veya "Error" türündeki günlükleri göndermek için havuz yapılandırmasını değiştirme göz önünde bulundurun Bu havuz, "Kanalı" ekleyerek gösterildiği şekilde yapılabilir [bu makalede](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Portalı veya Resource Manager şablonunuzu yanlış bir Application Insights anahtarı kullanırsanız, gerekir anahtarı el ile değiştirmeniz ve kümeyi güncelleştir / yeniden dağıtma.

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

.NET ile geliştirme ve büyük olasılıkla Service Fabric'in programlama modelleri ve bu olay ve günlük verilerini çözümleme ve görselleştirme için Application Insights platformunuz kullanmak istediğiniz bazı kullanıp, ardından Application ınsights'ı Git öneririz İzleme ve tanılama iş akışı olarak SDK yolu. Okuma [bu](../application-insights/app-insights-asp-net-more.md) ve [bu](../application-insights/app-insights-asp-net-trace-logs.md) toplamak ve günlüklerinizi görüntülemek için Application Insights'ı kullanmaya başlamak için.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Application Insights kaynağı Azure portalında gezinme

Application Insights olayları ve günlükleri için bir çıktı olarak yapılandırdıktan sonra bilgi birkaç dakika içinde Application Insights kaynağınıza görünmesini başlamanız gerekir. Application Insights kaynağı panoya sürer Application Insights kaynağına gidin. Tıklayın **arama** görev çubuğunda Application Insights alındığında son izlemelere bakın ve bunları filtrelemek için.

*Ölçüm Gezgini* , uygulamalarınıza, hizmetlerinize ve küme raporlama ölçümlere göre özel panolar oluşturmak için kullanışlı bir araçtır. Bkz: [keşfetmeye ölçümler Application ınsights'da](../application-insights/app-insights-metrics-explorer.md) birkaç grafikleri kendiniz toplama verileri temel alan için ayarlanacak.

Tıklayarak **Analytics** burada olaylarla ve izlemelerle daha fazla kapsam ve isteğe bağlı olma sorgulayabilirsiniz sizi Application Insights Analytics portalına götürür. Şu anda hakkında daha fazla bilgiyi [Application Insights analiz](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Yapay ZEKA uyarıları ayarlama](../application-insights/app-insights-alerts.md) performans ya da kullanım değişiklikler hakkında bildirim almak için
* [Akıllı algılama Application ınsights'ta](../application-insights/app-insights-proactive-diagnostics.md) olası performans sorunları sizi uyarabilmek için Application Insights'a gönderilen telemetri bir öngörülü analiz gerçekleştirir
