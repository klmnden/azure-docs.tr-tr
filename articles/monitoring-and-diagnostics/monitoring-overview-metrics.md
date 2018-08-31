---
title: Microsoft azure'da ölçümlere genel bakış
description: Ölçümler ve bunların kullanılması Microsoft azure'da genel bakış
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d61ac48aa7c51bc4b215a7d56b1bbedfdc613f9f
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287565"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Microsoft azure'da ölçümlere genel bakış
Bu makalede, Microsoft Azure'da ölçümler nelerdir anlatılmaktadır faydaları ve bunları kullanmaya başlamak nasıl.  

## <a name="what-are-metrics"></a>Ölçümler nelerdir?
Azure İzleyici, performans ve sistem durumunu azure'da iş yüklerinizi görünürlük elde etmek için telemetri kullanmasına olanak tanır. Azure telemetri verilerinin en önemli ölçümleri (performans sayaçları olarak da bilinir) çoğu Azure kaynakları tarafından gösterilen türüdür. Azure İzleyicisi'ni yapılandırma ve izleme ve sorun giderme için bu ölçümleri kullanabilmeniz için birçok yol sağlar.

## <a name="what-are-the-characteristics-of-metrics"></a>Ölçüm özellikleri nelerdir?
Ölçümler şu özelliklere sahiptir:

* Tüm ölçümleri sahip **bir dakikalık sıklığı** (Aksi halde bir ölçüm 's tanımında belirtilen sürece). Ölçüm değeri dakikada, kaynaktan neredeyse gerçek zamanlı görünürlük durumu kaynağınızın vererek alırsınız.
* Ölçümleri **kullanılabilir hemen**. Kabul et veya ek tanılama ayarlama ayarlayın gerekmez.
* Erişebildiğiniz **geçmişi 93 gün** her ölçüm için. Kaynağınızın sistem durumu ve performans son ve aylık eğilimleri hızlıca göz atabilirsiniz.
* Bazı ölçümler adlı ad-değer çifti öznitelikleri olabilen **boyutları**. Bu segmentlere ayırın ve bir ölçüm daha anlamlı bir şekilde araştırmak için etkinleştirin.

## <a name="what-can-you-do-with-metrics"></a>Ölçümler ile neler?
Ölçümler, aşağıdaki görevleri gerçekleştirmek etkinleştir:


- Bir ölçüm yapılandırma **bildirim gönderen veya alan kural eylemi otomatik uyarı** ne zaman ölçümü belirlediğiniz eşiği aştığında. Eylemleri aracılığıyla denetlenir [Eylem grupları](monitoring-action-groups.md). E-posta, telefon ve SMS bildirimleri ve bir runbook başlatma, bir Web kancasına çağrı örnek Eylemler içerir. **Otomatik ölçeklendirme** ölçeklemenize olanak tanıyan özel bir otomatik işlem maliyetleri düşük olduğunda yük altında tutulması henüz yukarı ve aşağı yükü işlemek için bir kaynak olmasıdır. Bir Eşiği aşan bir ölçüme göre ölçeğini daraltma veya genişletme için bir otomatik ölçeklendirme ayarı kural yapılandırabilirsiniz.
- **Rota** için tüm ölçümleri *Application Insights* veya *Log Analytics* anında analiz, search ve ölçüm verilerini kaynaklarınızı özel uyarı etkinleştirmek için. Ayrıca ölçümleri akışını bir *olay hub'ı*, böylece sonra Azure Stream Analytics veya özel uygulamalar neredeyse gerçek zamanlı analiz için yol. Olay hub'ı, tanılama ayarları kullanarak akış ayarlayın.
- **Arşiv** kaynağınızın denetim ya da çevrimdışı raporlamaya uyumluluk, performans veya sistem durumu geçmişi.  Kaynak tanılama ayarlarını yapılandırdığınızda Azure Blob depolama alanına ölçümlerinizi yönlendirebilirsiniz.
- Kullanım **Azure portalında** bulmak için erişim ve bir kaynak seçin ve ölçümleri bir grafik çizim tüm ölçümleri görüntüleyin. Bu grafiği panonuza sabitleyerek kaynağınızın (örneğin, bir sanal makine, Web sitesi veya mantıksal uygulama) performansını izleyebilirsiniz.  
- **Gelişmiş analiz gerçekleştirmesine** veya kaynağınızın performansını ya da kullanım eğilimlerini raporlama.
- **Sorgu** PowerShell cmdlet'lerini veya platformlar arası REST API kullanarak ölçümleri.
- **Tüketen** yeni Azure İzleyici REST API'leri aracılığıyla ölçümleri.

  ![Azure İzleyici ölçüm yönlendirme](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Portal aracılığıyla erişim ölçümleri
Azure portalını kullanarak bir ölçüm grafiği oluşturmak hakkında hızlı kılavuz aşağıda verilmiştir.

### <a name="to-view-metrics-after-creating-a-resource"></a>Bir kaynak oluşturduktan sonra ölçümleri görüntülemek için
1. Azure portalı açın.
2. Bir Azure App Service Web sitesi oluşturun.
3. Bir Web sitesini oluşturduktan sonra Git **genel bakış** Web sitesinin dikey penceresi.
4. Yeni ölçümler olarak görüntüleyebileceğiniz bir **izleme** Döşe. Kutucukları düzenleme ve daha fazla ölçüm seçin.

   ![Azure İzleyici kaynak ölçümleri](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>Tüm ölçümleri tek bir yerde erişmek için
1. Azure portalı açın.
2. Yeni Git **İzleyici** sekmesini seçin ve sonra **ölçümleri** altındaki seçeneği.
3. Aşağı açılan listeden, aboneliğiniz, kaynak grubu ve kaynağın adını seçin.
4. Kullanılabilir ölçümler listesini görüntüleyin. Sonra ilgilendiğiniz ve tasarlayın ölçümü seçin.
5. Bu PIN sağ üst köşedeki tıklayarak panoya sabitleyebilirsiniz.

   ![Tüm ölçümleri tek bir yerde Azure İzleyici'de erişim](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Konak düzeyinde ölçümler, sanal makineler (Azure Resource Manager tabanlı) erişebilir ve herhangi bir ek tanılama Kurulumu sanal makine ölçek kümeleri. Bu yeni konak düzeyinde ölçümler, Windows ve Linux örnekleri için kullanılabilir. Bu ölçümler, Azure tanılama Vm'lerinizdeki veya sanal makine ölçek kümeleri açmanız erişiminiz olan konuk işletim sistemi düzeyinde ölçümler ile karıştırılmamalıdır üzeresiniz. Tanılama yapılandırma hakkında daha fazla bilgi edinmek için [Microsoft Azure tanılama nedir](../azure-diagnostics.md).
>
>

Azure İzleyici ayrıca grafik deneyimi Önizleme sürümünde yeni bir ölçüm vardır. Bu deneyim, kullanıcıların bir grafik üzerindeki birden çok kaynaklardan ölçümleri kaplama olanak tanır. Kullanıcılar ayrıca, segment, çizim ve bu yeni ölçüm deneyimi Grafiği'ni kullanarak çok boyutlu ölçümleri filtreleyin. Daha fazla bilgi için [buraya tıklayın](https://aka.ms/azuremonitor/new-metrics-charts)

## <a name="access-metrics-via-the-rest-api"></a>REST API aracılığıyla erişim ölçümleri
Azure ölçümleri, Azure İzleyici API'leri erişilebilir. Yardımcı olan iki API bulma ve ölçümleri erişim vardır:

* Kullanım [Azure İzleyici ölçüm tanımlarını REST API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) ölçümleri ve bir hizmet için kullanılabilir olan tüm boyutlarının listesini erişmek için.
* Kullanım [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics) segmentlere filtrelemek ve gerçek ölçümleri verilerine erişmek için.

> [!NOTE]
> Ölçümleri aracılığıyla bu makalede ele alınmaktadır [ölçümler için yeni API](https://docs.microsoft.com/rest/api/monitor/) Azure kaynakları için. Yeni ölçüm tanımlarını ve ölçümleri API'leri için API sürümü 2018-01-01 ' dir. Eski ölçüm tanımlarını ve ölçümler API sürümü 2014-04-01 ile erişilebilir.
>
>

Azure İzleyici REST API'lerini kullanarak daha ayrıntılı bilgi için bkz: [Azure İzleyici REST API Kılavuzu](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Ölçümleri dışarı aktarma
Gidebilirsiniz **tanılama ayarları** altındaki dikey penceresinde **İzleyici** sekme ve ölçümler için dışarı aktarma seçeneklerini görüntüleyin. Blob depolama alanına yönlendirilmesini ölçümleri (ve tanılama günlükleri) Azure Event Hubs veya Log Analytics için bu makalede daha önce bahsedilen kullanım örnekleri için seçebilirsiniz.

 ![Azure İzleyicisi'nde ölçümler için dışarı aktarma seçenekleri](./media/monitoring-overview-metrics/MetricsOverview3.png)

Bu Resource Manager şablonları, yapılandırabileceğiniz [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), veya [REST API'leri](https://msdn.microsoft.com/library/dn931943.aspx).

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="take-action-on-metrics"></a>Ölçümler üzerinde eylem
Ölçüm verilerini otomatik eylemler gerçekleştirin ya da bildirimleri almak için uyarı kuralları veya otomatik ölçeklendirme ayarlarını yapılandırabilirsiniz.

### <a name="configure-alert-rules"></a>Uyarı kurallarını yapılandırma
Ölçümler üzerinde uyarı kuralları yapılandırabilirsiniz. Bu uyarı kuralları, bir ölçüm belirli bir eşiği aşıldığında, kontrol edebilirsiniz. Azure İzleyici tarafından sunulan iki ölçüm uyarı verme özellikleri vardır.

Ölçüm uyarıları: Bunlar daha sonra e-posta ile bildir veya özel bir betik çalıştırmak için kullanılan bir Web kancası yangın. Web kancası, üçüncü taraf ürün tümleştirmeleri yapılandırmak için de kullanabilirsiniz.

 ![Ölçümler ve Azure İzleyici uyarı kuralları](./media/monitoring-overview-metrics/MetricsOverview4.png)

Yeni ölçüm uyarılarının aracılığıyla size bildirir ve birden çok ölçüm ve eşikleri, bir kaynak için izleme olanağı sahip bir [eylem grubu](monitoring-action-groups.md). Daha fazla bilgi edinin [yeni uyarıları burada](https://aka.ms/azuremonitor/near-real-time-alerts).


### <a name="autoscale-your-azure-resources"></a>Otomatik ölçeklendirme, Azure kaynakları
Bazı Azure kaynaklarını veya ölçeklendirme iş yüklerinizi işlemek için birden çok örneğini destekler. Otomatik ölçeklendirme, App Service (Web uygulamaları), sanal makine ölçek kümeleri ve klasik Azure bulut Hizmetleri için geçerlidir. İş yükünüz etkiler belirli bir ölçüm, belirttiğiniz bir eşiği geçtiğinde veya ölçeklendirmek için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Daha fazla bilgi için [otomatik ölçeklendirme bakış](monitoring-overview-autoscale.md).

 ![Ölçümler ve Azure İzleyici otomatik ölçeklendirme](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Desteklenen hizmetler ve ölçümleri hakkında bilgi edinin
Desteklenen tüm hizmetleri ve bunların ölçümlere ayrıntılı bir listesini görüntüleyebileceğiniz [Azure İzleyici ölçümleri--desteklenen ölçümler kaynak türü başına](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalenin tamamında bağlantılara bakın. Ayrıca, hakkında bilgi edinin:  

* [Otomatik ölçeklendirme için genel ölçümler](insights-autoscale-common-metrics.md)
* [Uyarı kuralları oluşturma](insights-alerts-portal.md)
* [Log Analytics ile Azure depolama biriminden günlüklerini çözümleme](../log-analytics/log-analytics-azure-storage.md)
