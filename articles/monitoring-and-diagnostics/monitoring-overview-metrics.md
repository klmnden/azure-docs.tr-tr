---
title: Microsoft Azure ölçümlerini genel bakış
description: Ölçümleri ve bunların kullanılması Microsoft Azure genel bakış
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 3501c8d35968ecf8e32c806dfb05ccfebc7f4386
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264230"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Microsoft Azure ölçümlerini genel bakış
Microsoft Azure'da ölçümleri nelerdir bu makalede faydaları ve bunları kullanmaya başlamak nasıl.  

## <a name="what-are-metrics"></a>Ölçümleri nelerdir?
Azure İzleyicisi, performans ve sistem durumu, iş yüklerinin Azure üzerinde görünürlük elde etmek için telemetri kullanmasına olanak sağlar. En önemli Azure telemetri verileri çoğu Azure kaynaklar tarafından gösterilen (performans sayaçlarını olarak da bilinir) ölçümleri türüdür. Azure İzleyicisi'ni yapılandırma ve izleme ve sorun giderme için bu ölçümleri kullanmak için çeşitli yöntemler sağlar.

## <a name="what-are-the-characteristics-of-metrics"></a>Ölçümleri özelliklerini nelerdir?
Ölçümleri aşağıdaki özelliklere sahiptir:

* Tüm ölçümleri sahip **bir dakikalık sıklığı** (Aksi halde ölçüm 's tanımında belirtilen sürece). Bir ölçü değeri dakikada, kaynaktan durumu ve kaynağınıza durumunu gerçek zamanlı görünürlük yakın vermiş alırsınız.
* Metrik **kullanılabilir hemen**. Kabul ya da ek tanılama ayarlamak gerekmez.
* Erişebileceğiniz **geçmişi 93 gün** her ölçümü için. Son ve aylık eğilimler performansı veya kaynağınız durumunu hızlı bir şekilde bakabilirsiniz.
* Bazı ölçümleri adlı ad-değer çifti öznitelikleri olabilir **boyutları**. Bu, daha fazla segment ve bir ölçüm daha anlamlı bir şekilde keşfetmek etkinleştirin.

## <a name="what-can-you-do-with-metrics"></a>Ölçümleri ile neler yapabileceğiniz?
Ölçümleri, aşağıdaki görevleri gerçekleştirmeniz olanak sağlar:


- Bir ölçüm yapılandırma **bir bildirim gönderir ya da alan kural eylemi otomatik uyarı** zaman ölçümü kestiği ayarladığınız eşiği. Eylemler aracılığıyla denetlenir [Eylem grupları](monitoring-action-groups.md). Örnek Eylemler e-posta, telefon ve SMS bildirimler, başlangıç ve bir runbook bir Web kancası çağırma içerir. **Otomatik ölçeklendirme** ölçeklendirmek sağlayan özel bir otomatik işlem olduğunuz yukarı ve aşağı yükü işlemek henüz maliyetleri olduğunda yük altında daha düşük tutmak için bir kaynak değil. Bir eşik geçmeden bir ölçüm bağlı içeri veya dışarı ölçeklendirmek için otomatik ölçeklendirme ayarı kural yapılandırabilirsiniz.
- **Rota** tüm ölçümleri *Application Insights* veya *günlük analizi* anlık analytics, arama ve kaynaklarınızı ölçümleri verileri üzerinde özel uyarı sağlamak için. Ölçümleri'akışı bir *olay hub'ı*, sağlayacak şekilde sonra Azure Stream Analytics veya özel uygulamalar neredeyse gerçek zamanlı analiz için rota. Tanılama ayarları kullanarak akış olay hub'ı ayarlayın.
- **Arşiv** , kaynak denetim ya da çevrimdışı raporlama amacıyla uyumluluk için performans veya sistem durumu geçmişini.  Kaynağınız için tanılama ayarlarını yapılandırdığınızda, Azure Blob depolama alanına ölçümlerinizi yönlendirebilirsiniz.
- Kullanım **Azure portal** bulmak için erişim ve bir kaynak seçin ve bir grafik ölçümleri çizim tüm ölçümleri görüntüleyin. Bu grafiği panonuza sabitlemenin kaynağınız (örneğin, bir VM, Web sitesi ya da mantıksal uygulama) performansını izleyebilir.  
- **Gelişmiş analizler gerçekleştirmek** veya kaynağınız performans ya da kullanım eğilimlerini üzerinde raporlama.
- **Sorgu** PowerShell cmdlet'lerini veya platformlar arası REST API kullanarak ölçümleri.
- **Tüketen** yeni Azure İzleyici REST API'leri aracılığıyla ölçümleri.

  ![Azure İzleyicisi'nde ölçümleri yönlendirme](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Portal üzerinden erişim ölçümleri
Aşağıda Azure portalını kullanarak bir ölçüm grafik oluşturmak nasıl hızlı bir kılavuz vardır.

### <a name="to-view-metrics-after-creating-a-resource"></a>Bir kaynak oluşturduktan sonra ölçümleri görüntülemek için
1. Azure portalı açın.
2. Azure App Service Web sitesi oluşturun.
3. Bir Web sitesini oluşturduktan sonra Git **genel bakış** Web sitesinin dikey.
4. Yeni ölçümleri olarak görüntüleyebileceğiniz bir **izleme** döşeme. Ardından, döşemesini düzenleyin ve daha fazla ölçümleri seçin.

   ![Azure İzleyicisi'nde kaynak ölçümleri](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>Tüm ölçümlerini tek bir yerde erişmek için
1. Azure portalı açın.
2. Yeni gidin **İzleyici** sekmesini seçin ve sonra **ölçümleri** altındaki seçeneği.
3. Aboneliğiniz, kaynak grubu ve kaynak adının aşağı açılan listeden seçin.
4. Kullanılabilir ölçümler listesini görüntüleyin. Ardından ilgilendiğiniz ve tasarlayın ölçümü seçin.
5. Bu Pano için sabitleme sağ üst köşesinde üzerinde tıklayarak sabitleyebilirsiniz.

   ![Azure İzleyici tek bir yerde tüm ölçümlerini erişim](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Herhangi bir ek tanılama ayar sanal makine ölçek ayarlar ve VM'lerin (Azure Resource Manager tabanlı) ana bilgisayar düzeyinde ölçümleri erişebilirsiniz. Bu yeni ana bilgisayar düzeyinde ölçümleri, Windows ve Linux örnekleri için kullanılabilir. Bu ölçümler zaman Azure tanılama Vm'lerinizdeki veya sanal makine ölçek kümeleri kapatmanız erişiminiz olan konuk işletim sistemi düzeyinde ölçümleri ile karıştırılmamalıdır üzeresiniz. Tanılama yapılandırma hakkında daha fazla bilgi edinmek için [Microsoft Azure tanılama nedir](../azure-diagnostics.md).
>
>

Azure İzleyici deneyimi önizlemede kullanılabilen grafik yeni bir ölçümleri de vardır. Bu deneyim, kullanıcıların bir grafik birden çok kaynaklardan ölçümleri kaplama olanak tanır. Kullanıcılar ayrıca, segment çizmek ve çok boyutlu ölçümleri deneyimi grafik bu yeni ölçüm kullanarak filtre. Daha fazla bilgi için [burayı tıklatın](https://aka.ms/azuremonitor/new-metrics-charts)

## <a name="access-metrics-via-the-rest-api"></a>REST API üzerinden erişim ölçümleri
Azure ölçümleri Azure İzleyici API'leri erişilebilir. Yardımcı iki API'ları bulmak ve erişim ölçümleri vardır:

* Kullanım [Azure İzleyici ölçüm tanımlarını REST API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) ölçümleri ve bir hizmet için kullanılabilir tüm boyutlar, listesi erişmek için.
* Kullanım [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics) segmentlere ayırmak, filtrelemek ve gerçek ölçümleri verilere erişmek için.

> [!NOTE]
> Bu makale aracılığıyla ölçümleri kapsamaktadır [ölçümleri için yeni API](https://docs.microsoft.com/rest/api/monitor/) Azure kaynakları için. 2018-01-01 yeni ölçüm tanımlarını ve ölçümler API'ları için API sürümüdür. Eski ölçüm tanımlarını ve ölçümler API sürümü 2014-04-01 ile erişilebilir.
>
>

Azure İzleyici REST API'lerini kullanarak daha ayrıntılı bilgi için bkz: [Azure İzleyici REST API izlenecek](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Dışarı aktarma ölçümleri
Gidebilirsiniz **tanılama ayarları** altında dikey **İzleyici** sekmesinde ve ölçümleri için dışarı aktarma seçeneklerini görüntüleyin. Ölçümleri (ve tanılama günlükleri) Blob depolaması'na yönlendirilecek Azure Event Hubs veya günlük analizi için bu makalede daha önce bahsedilen kullanım örnekleri için seçebilirsiniz.

 ![Azure İzleyicisi'nde ölçümleri dışa aktarma seçenekleri](./media/monitoring-overview-metrics/MetricsOverview3.png)

Bu Resource Manager şablonları yapılandırabilirsiniz [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), veya [REST API'leri](https://msdn.microsoft.com/library/dn931943.aspx).

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="take-action-on-metrics"></a>Ölçümleri eylem
Bildirimleri almak veya ölçüm verilerini otomatik eylemleri için uyarı kuralları veya otomatik ölçeklendirme ayarlarını yapılandırabilirsiniz.

### <a name="configure-alert-rules"></a>Uyarı kurallarını yapılandırma
Uyarı kuralları ölçümleri yapılandırabilirsiniz. Bu uyarı kuralları, bir ölçüm belirli bir eşiği aşıldığında, kontrol edebilirsiniz. Azure İzleyici tarafından sunulan iki ölçüm uyarı verme özelliği yoktur.

Ölçüm uyarıları: Bunlar daha sonra e-posta ile bildir veya özel komut dosyaları çalıştırmak için kullanılan bir Web kancası tetiklenecek. Web kancası, üçüncü taraf ürün tümleştirmeler yapılandırmak için de kullanabilirsiniz.

 ![Ölçümleri ve Azure İzleyicisi'nde uyarı kuralları](./media/monitoring-overview-metrics/MetricsOverview4.png)

Yeni ölçüm uyarıları sahip birden çok ölçümleri ve bir kaynak için eşikler izlemek ve size yoluyla bildirim olanağı bir [eylem grubu](/monitoring-action-groups.md). Daha fazla bilgi edinmek [burada yeni uyarılar](https://aka.ms/azuremonitor/near-real-time-alerts).


### <a name="autoscale-your-azure-resources"></a>Otomatik ölçeklendirme Azure kaynakları
Bazı Azure kaynaklarını veya ölçeklendirme, iş yüklerini işlemek üzere birden çok örneğini destekler. Otomatik ölçeklendirme, uygulama hizmeti (Web uygulamaları), sanal makine ölçek kümeleri ve klasik Azure bulut Hizmetleri için geçerlidir. İş yükünüzün etkiler belirli bir ölçüm belirttiğiniz bir eşik kestiği olduğunda veya ölçeklendirmek için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Daha fazla bilgi için bkz: [otomatik ölçeklendirmeyi genel bakış](monitoring-overview-autoscale.md).

 ![Ölçümleri ve Azure İzleyicisi'nde otomatik ölçeklendirme](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Desteklenen hizmetler ve ölçümleri hakkında bilgi edinin
Desteklenen tüm hizmetleri ve bunların ölçümlere ayrıntılı bir listesini görüntüleyebileceğiniz [Azure İzleyici ölçümleri--kaynak türü başına desteklenen ölçümleri](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makale boyunca bağlantılara bakın. Ayrıca, hakkında bilgi edinin:  

* [Otomatik ölçeklendirme için ortak ölçümleri](insights-autoscale-common-metrics.md)
* [Uyarı kuralları oluşturma](insights-alerts-portal.md)
* [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md)
