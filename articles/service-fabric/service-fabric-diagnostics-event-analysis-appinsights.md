---
title: Application Insights ile Azure Service Fabric olay çözümleme | Microsoft Docs
description: Görselleştirme ve izleme ve tanılama Azure Service Fabric kümeleri için Application Insights kullanarak olayları analiz etme hakkında bilgi edinin.
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
ms.date: 04/04/2018
ms.author: srrengar
ms.openlocfilehash: 29adf362fdacdb793af071fa6d7bd59214536374
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207762"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Olay çözümleme ve görselleştirme Application Insights ile

Azure Application Insights, uygulama izleme ve tanılama için genişletilebilir bir platformdur. Uyarı seçenekleri de dahil olmak üzere daha fazla otomatik ve güçlü analytics ve aracı, özelleştirilebilir bir Pano ve görselleştirmeleri sorgulama içerir. Bu izleme için önerilen platform ve Service Fabric uygulamaları ve Hizmetleri için tanılama olur. Bu makalede aşağıdaki ortak soruları yardımcı olur.

* Nasıl my uygulama, hizmetler ve toplama telemetri içinde neler olduğunu biliyor musunuz
* Özellikle birbirleriyle iletişim hizmetleri nasıl Uygulamam, giderme
* Ölçümleri nasıl Hizmetlerim, örneğin gerçekleştiriyorsanız, sayfa yükleme süresi, http isteklerini hakkında nasıl sağlarım

Bu makalede amacı Öngörüler elde edin ve gelen App Insights içinde ilgili sorunları giderme göstermektir. Ayarlama ve AI Service Fabric ile yapılandırma konusunda bilgi almak istiyorsanız, bunu denetleyin [Öğreticisi](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-app-insights"></a>Uygulama öngörü izleme

Application Insights Service Fabric kullanırken deneyimini dışında bir zengin sahiptir. Genel bakış sayfasında AI hizmetinizin yanıt süresi ve işlenen istek sayısı gibi hakkında önemli bilgileri sağlar. Üst 'Ara' düğmesini tıklatarak, uygulamanızda son isteklerin listesini görebilirsiniz. Ayrıca, başarısız istekler buraya bakın ve hangi hatalar meydana gelebilir tanılamak gerçekleştirebilir.

![AI genel bakış](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

Önceki görüntüde sağ panelde listesinde girişleri iki ana türü vardır: istekleri ve olaylar. Bu durumda uygulamanın API HTTP istekleri üzerinden yapılan çağrılar isteklerdir, ve olaylar kodunuzda herhangi bir yere ekleyebilirsiniz telemetri gibi davranan özel olaylar. Daha fazla uygulamalarınızda düzenleme keşfedebilirsiniz [özel olayları ve ölçümleri için Application Insights API'si](../application-insights/app-insights-api-custom-events-metrics.md). Bir istekte tıklayarak daha fazla ayrıntı AI Service Fabric nuget paketi toplanan Service Fabric özgü verileri dahil olmak üzere aşağıdaki görüntüde gösterildiği gibi görüntüler. Bu bilgileri, uygulamanızın durumunu nedir bilerek ve sorun giderme için yararlı olur ve bu bilgilerin tümünü içinde Application Insights aranabilir

![AI İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights gelen karşı tüm veri sorgulama için bir atanan görünümü vardır. AI portalına gitmek için genel bakış sayfasında üst kısmında "Ölçüm Gezgini"'i tıklatın. Burada önce bahsedilen özel olaylar, istekleri, özel durumlar, performans sayaçları ve Kusto sorgu dili kullanarak diğer ölçümleri karşı sorguları çalıştırabilirsiniz. Aşağıdaki örnek, son 1 saat içindeki tüm istekleri gösterir.

![AI İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Daha fazla App Insights portalında özelliklerini keşfetmek için üzerinden için head [Application Insights portal belgeleri](../application-insights/app-insights-dashboards.md).

### <a name="configuring-ai-with-wad"></a>AI WAD ile yapılandırma

>[!NOTE]
>Bu yalnızca şu anda Windows kümeleri için geçerlidir.

İçinde ayrıntılı olarak WAD yapılandırma AI havuz ekleyerek elde Azure AI WAD veri göndermek için iki birincil yolla [bu makalede](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Azure portalında bir küme oluştururken, bir AI izleme anahtarını ekleyin

![Bir AIKey ekleme](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

"Açık" tanılama etkinleştirdiyseniz bir küme oluştururken, bir uygulama Insights izleme anahtarı girmek için isteğe bağlı bir alan gösterir. Burada AI anahtarınızı yapıştırırsanız AI havuz otomatik olarak sizin için kümenizi dağıtmak için kullanılan Resource Manager şablonu olarak yapılandırılır.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>AI havuz için Resource Manager şablonu Ekle

"WadCfg" Resource Manager şablonu içinde aşağıdaki iki değişiklikler dahil ederek "Havuz" ekleyin:

1. Havuz yapılandırma, bildirme hemen sonra ekleyin `DiagnosticMonitorConfiguration` tamamlanır:

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

2. Havuz dahil `DiagnosticMonitorConfiguration` dosyasında aşağıdaki satırı ekleyerek `DiagnosticMonitorConfiguration` , `WadCfg` (önceki `EtwProviders` bildirilir):

    ```json
    "sinks": "applicationInsights"
    ```

Her iki önceki kod parçacıkları içinde adı "Applicationınsights" havuz açıklamak için kullanıldı. Bu bir gereklilik değildir ve havuz adı "havuzlarını" dahil olduğu sürece, herhangi bir dize adı ayarlayabilirsiniz.

Şu anda, küme günlüklerinden görünmesini olarak **izlemeleri** AI'ın günlük Görüntüleyicisi'nde. Çoğu platformdan gelen izlemeleri düzeyi "Bilgilendirici" olduğundan, aynı zamanda yalnızca "Kritik" veya "Hata." türünde günlükleri göndermek için havuz yapılandırmasını değiştirme düşünebileceğiniz Bu, havuz için "Kanalları" ekleyerek şekilde yapılabilir [bu makalede](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Yanlış bir AI anahtarı portalında veya Resource Manager şablonunu kullanırsanız, gerekir el ile kayıt anahtarını değiştirin ve kümeyi güncelleştirmek / yeniden dağıtmanız.

### <a name="configuring-ai-with-eventflow"></a>AI EventFlow ile yapılandırma

Birleşik olaylarına EventFlow kullanıyorsanız, içeri aktarmak emin olun `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet paketi. Aşağıdaki kod gereklidir *çıkarır* bölümünü *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Yanı sıra, filtrelerinizi gerekli değişiklikleri yapın (birlikte ilgili kendi NuGet paketlerini) başka bir giriş eklemek emin olun.

## <a name="aisdk"></a>AI. SDK'SI

EventFlow ve WAD toplama çözümler olarak çünkü tanılama daha modüler bir yaklaşım için izin ver ve EventFlow, çıkışları değiştirmek istiyorsanız, yani izleme, hiçbir değişiklik, gerçek araçları gerektirir yalnızca kullanılacak önerilen bir Basit bir değişiklikle config dosyasına. Ancak, Application Insights kullanarak yatırım karar ve farklı bir platform için değişmesi olasılığı olan değil, olay toplama ve AI için göndermek için AI'ın yeni bir SDK kullanarak görünmelidir. Bu, verileriniz için AI göndermek için EventFlow yapılandırmak artık olur, ancak bunun yerine ApplicationInsight'ın Service Fabric NuGet paketini yükleyecek anlamına gelir. Pakette Ayrıntılar bulunabilir [burada](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Application Insights mikro ve kapsayıcıları için desteği](https://azure.microsoft.com/blog/app-insights-microservices/) bazı gösterir (üzerinde hala halen beta) çalışan yeni özelliklerini hangi izin AI ile daha zengin Giden kutusu izleme seçeneğiniz vardır. Bu bağımlılık izleme (bir AppMap tüm hizmetleri ve uygulamaları bir küme ve bunların arasındaki iletişimi oluşturulmasında kullanılan) ve daha iyi bağıntı hizmetlerinizin (daha iyi bir sorun iş akışındaki yerini tam olarak belirtmekte yardımcı olur'ten gelen izlemeleri içerir bir uygulama veya hizmet).

.NET geliştirme ve büyük olasılıkla Service Fabric'ın programlama modelleri ve bu olay ve günlük verilerini çözümleme ve görselleştirme için platform olarak AI kullanmak istiyorum bazıları kullanacaklardır, ardından AI SDK yol izleme ve diagnos olarak aracılığıyla Git öneririz ı eşleştir iş akışı. Okuma [bu](../application-insights/app-insights-asp-net-more.md) ve [bu](../application-insights/app-insights-asp-net-trace-logs.md) AI toplamak ve günlüklerinizi görüntülemek için kullanmaya başlamak için.

## <a name="navigating-the-ai-resource-in-azure-portal"></a>Azure portalında AI kaynak gezinme

Olayları ve günlükleri için bir çıktı olarak AI yapılandırdıktan sonra bilgi AI kaynağınız birkaç dakika içinde gösterilmeye başlamanız gerekir. AI kaynak panoya sürer AI kaynağa gidin. Tıklatın **arama** görev çubuğunda AI alınan son izlemelerini görmek ve aralarında filtreleyebilirsiniz.

*Ölçüm Gezgini* uygulamaları, hizmetleri ve küme raporlama ölçümleri temel özel panolar oluşturmak için yararlı bir araçtır. Bkz: [keşfetme ölçümleri Application ınsights'ta](../application-insights/app-insights-metrics-explorer.md) kendiniz toplama verileri temel alan için birkaç grafikleri ayarlamak için.

Tıklatarak **Analytics** burada olayları ve büyük kapsam ve isteğe bağlı olma izlemeleri sorgulayabilir uygulama Öngörüler Analytics portalına sürer. Şu anda hakkında daha fazla bilgiyi [Application Insights analizleri](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Sonraki adımlar

* [AI uyarıları ayarlama](../application-insights/app-insights-alerts.md) performans veya kullanımı değişiklikler hakkında bildirim almak için
* [Akıllı algılama Application ınsights'ta](../application-insights/app-insights-proactive-diagnostics.md) AI için olası performans sorunları sizi uyaracak şekilde telemetri gönderimini öngörülü bir analizini gerçekleştirir
