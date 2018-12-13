---
title: Birleştirilmiş uyarılar ve Azure İzleyici'de izleme Klasik uyarı ve izleme değiştirir.
description: Klasik izleme hizmetleri ve işlevleri, Azure portalında uyarılar (Klasik) altında daha önce gösterilen emeklilik genel bakış. Uyarı ve izleme Klasik Klasik ölçüm uyarıları içerir Klasik özel ölçüm Klasik ölçüm uyarıları Application ınsights, Klasik Web testi uyarıları Application ınsights, Azure kaynakları için Application ınsights'ı ve klasik uyarılar tabanlı Application Insights SmartDetection v1 için uyarılar
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 04dbc5c60e802e7861b9e2a98c51446281b7ae3f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53320616"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Birleştirilmiş uyarılar ve Azure İzleyici'de izleme Klasik uyarı ve izleme değiştirir.

Azure İzleyici 'Bir ölçüm' ve 'Bir Uyarılar' kaynakları genelinde artık destekleyen hizmet izleme birleşik tam bir yığın artık olur; Daha fazla bilgi için müşterilerimize [yeni Azure İzleyici Web günlüğü gönderisini](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). Yeni Azure izleme ve uyarı platformları daha hızlı, daha verimli olmasını yerleşik ve giderek büyüyen expanse bulut bilgi işlem ve satır içi Microsoft Akıllı bulut felsefesi ile Genişletilebilir – tutma hızını. 

Yeni Azure izleme ve uyarı yerinde platformu ile biz "izleme ve uyarı içinde barındırılan platformu - Klasik" kullanımdan kaldırılacak *Klasik uyarıları görüntüleme* bölümü tarafından Haziran 2019 Azure uyarıları, kullanım dışı bırakılacaktır.

 ![Azure portalında Klasik uyarı](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Kullanmaya başlayın ve yeni platform uyarılarınızı oluşturmanız önerilir. Çok sayıda uyarı olan müşteriler, mevcut Klasik uyarıları kesintiye uğratmadan yeni uyarılar sisteme taşımak için otomatik bir yol sağlamak üzere çalışma veya maliyetleri eklendi.

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Birleşik ölçümleri ve Uyarıları Application ınsights

Azure İzleyicisi'nin daha yeni ölçüm platformu artık Application Insights izleme gelen güç. Bu taşıma Eylem grupları için Application Insights kanca yalnızca önceki e-posta ve Web kancası çağrıları'den çok daha fazla seçeneklerine izin verme anlamına gelir. Uyarılar, sesli aramalar, Azure işlevleri, Logic Apps, SMS ve ServiceNow ve Otomasyon runbook'ları gibi ITSM araçları artık tetikleyebilirsiniz. İle diğer Azure kaynakları arasında izleme ve Microsoft ürünlerinin izleme taşlarından teknolojiden yararlanarak Application Insights kullanıcılar yeni platformu gerçek zamanlı izleme ve uyarı verme, sağlar.

Yeni birleştirilmiş izleme ve uyarı verme için Application Insights içerir:

- **Application Insights Platform ölçümleri** – Application Insights ürün önceden oluşturulmuş yaygın olarak kullanılan ölçümleri sağlar. Kullanma hakkında daha fazla bilgi için bu makalede bkz [Platform ölçümler için Application ınsights'ı yeni Azure İzleyicisi](../application-insights/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Application Insights kullanılabilirlik ve Web test** -Bu, web uygulamanızı veya sunucu kullanılabilirliğini ve yanıt hızını değerlendirmek olanağı sağlar. Kullanma hakkında daha fazla bilgi için bu makalede bkz [kullanılabilirlik testleri ve Application ınsights'ı yeni Azure İzleyicisi için uyarı](../application-insights/app-insights-monitor-web-app-availability.md).
- **Application Insights özel ölçümleri** – olanak sağlayan tanımlama ve izleme ve Uyarılar için kendi ölçümleri göster. Kullanma hakkında daha fazla bilgi için bu makalede bkz [özel ölçüm için Application ınsights'ı yeni Azure İzleyicisi](../application-insights/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).
- **Application Insights hata Anomalileri (Akıllı algılama parçası)** –, otomatik olarak bildirir, neredeyse gerçek zamanlı olarak web uygulamanızın olağandışı artışı başarısız HTTP isteklerini veya bağımlılık çağrıları oranını karşılaşırsa. Application Insights hata Anomalileri (Akıllı algılama parçası) yeni Azure İzleyici, bir parçası olarak kullanılabilir olan en kısa sürede ve bunu-önümüzdeki aylarda kullanıma gibi bu belgenin sonraki yineleme bağlantıları ile güncelleştireceğiz.

## <a name="unified-metrics--alerts-for-other-azure-resources"></a>Birleşik Ölçümler ve diğer Azure kaynakları için uyarılar

Mart 2018'den itibaren yeni nesil çok boyutlu Azure kaynakları için izleme ve uyarı kullanılabilirlik olmuştur. Şimdi yeni ölçüm platform ve uyarı neredeyse gerçek zamanlı özelliklerle daha hızlı. Daha da önemlisi, yeni platform boyutları, dilim ve filtre belirli değer birleşimi, koşul veya işlem için izin ver seçeneği içerdiğinden daha yeni ölçüm platform uyarılar, daha fazla ayrıntı sağlar. Yeni Azure İzleyici'de tüm uyarılar gibi yeni ölçüm uyarılarının e-posta veya SMS, sesli, Azure işlevi, Otomasyon Runbook'u ve daha fazla Web kancası ötesine genişletmek bildirimleri verme ActionGroups – kullanarak daha genişletilebilir.
Azure kaynakları için yeni ölçümler olarak kullanılabilir:

- **Azure İzleyici standart platform ölçümleri** – çeşitli Azure Hizmetleri ve ürünleriyle popüler önceden doldurulmuş ölçümleri sağlar. Daha fazla bilgi için bu makaleye bakın [desteklenen ölçümler üzerinde Azure İzleyici](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported) ve [üzerinde Azure İzleyici ölçüm uyarıları destekleyen](../azure-monitor/platform/alerts-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Azure İzleyici özel ölçümleri** – Azure Tanılama Aracı gibi kaynakları kullanan bir kullanıcıdan ölçümleri sağlar. Daha fazla bilgi için bu makaleye bakın [Azure İzleyici'de özel ölçümleri](../azure-monitor/platform/metrics-custom-overview.md). Özel ölçüm kullanarak tarafından toplanan ölçümleri de yayımlayabilirsiniz [Windows Azure tanılama aracısını](../azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm.md) ve [InfluxData Telegraf aracı](../azure-monitor/platform/collect-custom-metrics-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>İzleme ve uyarı platform Klasik devre dışı bırakma

İzleme ve uyarı platform şu anda kullanılabilir gelen Klasik daha önce belirtildiği gibi [uyarılar (Klasik) bölümünde](../azure-monitor/platform/alerts-classic.overview.md) Azure portalı kullanımdan kaldırılacak ay verilen geliyor, bunlar yeni sistem tarafından değiştirilmiştir.
İzleme ve uyarı eski Klasik üzerinde 30 Haziran 2019 kullanımdan kaldırılacaktır; Kapanış ilgili API'leri, Azure portal arabirimi ve Hizmetleri içinde dahil. Özellikle, bu özellikler kullanım dışı kalacaktır:

- Eski (Klasik) ölçümleri ve uyarıları olarak şu anda Azure kaynakları için aracılığıyla kullanılabilen [uyarılar (Klasik) bölümünde](../azure-monitor/platform/alerts-classic.overview.md) Azure portalı; olarak erişilebilir [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) kaynak
- Eski (Klasik) platformu ve özel ölçümler için Application Insights yanı sıra olarak bunlar üzerinde şu anda aracılığıyla kullanılabilen uyarı [uyarılar (Klasik) bölümünde](../azure-monitor/platform/alerts-classic.overview.md) Azure portal ve olarak erişilebilir [microsoft.insights/ alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) kaynak
- Eski (Klasik) hata Anomalileri uyarı şu anda kullanılabilir olarak [Application Insights içinde akıllı algılama](../application-insights/app-insights-proactive-diagnostics.md) yapılandırılan uyarı; Azure portalında gösterilen [uyarılar (Klasik) bölümünde](../azure-monitor/platform/alerts-classic.overview.md) Azure Portal

Tüm Klasik izleme ve sistemlerle ilgili uyarı [API](https://msdn.microsoft.com/library/azure/dn931945.aspx), [PowerShell](../azure-monitor/platform/alerts-classic-portal.md), [CLI](../azure-monitor/platform/alerts-classic-portal.md), [Azure portal sayfasındaki](../azure-monitor/platform/alerts-classic-portal.md)ve [ Kaynak şablonu](monitoring-enable-alerts-using-template.md) Haziran 2019 sonuna kadar kullanılabilir kalır. 

Azure İzleyici'de, Haziran 2019 sonunda:

- Klasik izleme ve Uyarılar hizmetinin kullanımdan kaldırılmış ve artık yeni uyarı kuralları oluşturma
- Uyarılar (Klasik) Haziran 2019 ötesinde var olmaya devam uyarı kuralları yürütün ve bildirimleri harekete geçin, ancak değişiklik kullanılamaz.
- Temmuz 2019 başlayarak, hiçbir uyarı kuralları Klasik izleme ve uyarı otomatik olarak Microsoft tarafından eşdeğerine yeni Azure İzleyici platformuna geçirilecektir. İşlem kapalı kalma süresi sorunsuz ve müşterilerinin kapsamı izleme kaybı olmadan olacaktır.

Araçlar, uyarılardan gönüllü geçirmenizi yakında sağlayacağız [uyarılar (Klasik) bölümünde](../azure-monitor/platform/alerts-classic.overview.md) yeni Azure uyarıları için Azure portal'ın. Uyarılar (Klasik), yeni Azure İzleyicisi geçişi yapılandırılan tüm kurallar ücretsiz kalır ve ücret alınmaz. Geçirilen Klasik uyarı kuralları için e-posta, Web kancası veya mantıksal uygulama bildirimleri gönderme herhangi bir ücrete da size aittir değil. Ancak, daha yeni bir bildirim ya da eylem türleri (örneğin, SMS, sesli arama, ITSM tümleştirmesi, vb.) kullanımı için geçirilen veya yeni bir uyarı eklenip ücrete tabi olacaktır. Daha fazla bilgi için [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/).

Ayrıca, aşağıdaki ambit altında ücrete tabi olacaktır [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/):

- Yeni Azure İzleyici platformunda ücretsiz birimleri aşan oluşturulan tüm yeni (geçirilmeyen) uyarı kuralı
- Azure İzleyici tarafından dahil edilen ücretsiz birimler dışında tutulur ve alınan tüm veriler
- Application Insights tarafından yürütülen herhangi bir çok test web testi
- Azure İzleyici'de dahil edilen ücretsiz birimleri aşan depolanan tüm özel ölçümler

Bu makalede, sürekli güncelleştirilen olacak bağlantılar ve yeni Azure izleme ile ilgili & işlevselliği yanı sıra yeni Azure İzleyici platformu benimsenmesi kullanıcılara yardımcı olmak için araçları kullanılabilirliğini uyarı ayrıntıları olacaktır.


## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [yeni birleşik Azure İzleyici](../azure-monitor/overview.md).
* Yeni hakkında daha fazla bilgi [Azure uyarıları](monitoring-overview-alerts.md).
