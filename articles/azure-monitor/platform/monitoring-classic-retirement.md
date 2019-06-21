---
title: Birleştirilmiş uyarılar ve Azure İzleyici'de izleme Klasik uyarı ve izleme değiştirir.
description: Klasik izleme hizmetleri ve işlevleri, Azure portalında uyarılar (Klasik) altında daha önce gösterilen emeklilik genel bakış. Uyarı ve izleme Klasik Klasik ölçüm uyarıları içerir Klasik özel ölçüm Klasik ölçüm uyarıları Application ınsights, Klasik Web testi uyarıları Application ınsights, Azure kaynakları için Application ınsights'ı ve klasik uyarılar tabanlı Application Insights SmartDetection v1 için uyarılar
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 2/7/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: fb7821b07e68459cb3d76812a12e85387b9f0f52
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295095"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Birleştirilmiş uyarılar ve Azure İzleyici'de izleme Klasik uyarı ve izleme değiştirir.

Azure İzleyici 'Bir ölçüm' ve 'Bir Uyarılar' kaynakları genelinde artık destekleyen hizmet izleme birleşik tam bir yığın artık olur; Daha fazla bilgi için müşterilerimize [yeni Azure İzleyici Web günlüğü gönderisini](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). Yeni Azure izleme ve uyarı platformları daha hızlı, daha verimli olmasını yerleşik ve giderek büyüyen expanse bulut bilgi işlem ve satır içi Microsoft Akıllı bulut felsefesi ile Genişletilebilir – tutma hızını. 

Yeni Azure izleme ve uyarı yerinde platformu ile biz "izleme ve uyarı içinde barındırılan platformu - Klasik" kullanımdan kaldırılacak *Klasik uyarıları görüntüleme* Azure uyarıları bölümünü **tarafından kullanım Ağustos 2019 Azure genel Bulutu**. [Azure kamu Bulutu](../../azure-government/documentation-government-welcome.md) ve [Azure Çin 21Vianet](https://docs.azure.cn/) etkilenmez.

> [!NOTE]
> Sunum geçiş aracının gecikme nedeniyle, o tarihten Klasik uyarılar geçiş yapıldı [31 Ağustos 2019'için Genişletilmiş](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) ilk duyurulan tarihinden 30 Haziran 2019.

 ![Azure portalında Klasik uyarı](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Kullanmaya başlayın ve yeni platform uyarılarınızı oluşturmanız önerilir. Çok sayıda uyarı sahip müşteriler, duyuyoruz [aşamaları out içinde çalışırken](alerts-understand-migration.md#rollout-phases), [gönüllü bir geçiş aracı](alerts-using-migration-tool.md) kesintisi veya ek maliyet olmadan yeni uyarılar sistemde mevcut Klasik uyarıları taşımak için.

> [!IMPORTANT]
> Etkinlik günlüğü üzerinde oluşturulan Klasik uyarı kuralları kullanım dışı veya geçirildi. Etkinlik günlüğü üzerinde oluşturulan tüm Klasik uyarı kuralları erişilebilen ve olarak kullanılan-yeni Azure İzleyici'deki - uyarılar olduğundan. Daha fazla bilgi için [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Etkinlik günlüğü Uyarıları yönetme](../../azure-monitor/platform/alerts-activity-log.md). Benzer şekilde, hizmet durumu uyarıları erişilebilen ve olarak kullanılan-yeni hizmet durumu bölümünden olduğu. Ayrıntılar için bkz [hizmet durumu bildirimlerine uyarılarda](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Birleşik ölçümleri ve Uyarıları Application ınsights

Azure İzleyicisi'nin daha yeni ölçüm platformu artık Application Insights izleme gelen güç. Bu taşıma Eylem grupları için Application Insights kanca yalnızca önceki e-posta ve Web kancası çağrıları'den çok daha fazla seçeneklerine izin verme anlamına gelir. Uyarılar, sesli aramalar, Azure işlevleri, Logic Apps, SMS ve ServiceNow ve Otomasyon runbook'ları gibi ITSM araçları artık tetikleyebilirsiniz. İle diğer Azure kaynakları arasında izleme ve Microsoft ürünlerinin izleme taşlarından teknolojiden yararlanarak Application Insights kullanıcılar yeni platformu gerçek zamanlı izleme ve uyarı verme, sağlar.

Yeni birleştirilmiş izleme ve uyarı verme için Application Insights içerir:

- **Application Insights Platform ölçümleri** – Application Insights ürün önceden oluşturulmuş yaygın olarak kullanılan ölçümleri sağlar. Kullanma hakkında daha fazla bilgi için bu makalede bkz [Platform ölçümler için Application ınsights'ı yeni Azure İzleyicisi](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Application Insights kullanılabilirlik ve Web test** -Bu, web uygulamanızı veya sunucu kullanılabilirliğini ve yanıt hızını değerlendirmek olanağı sağlar. Kullanma hakkında daha fazla bilgi için bu makalede bkz [kullanılabilirlik testleri ve Application ınsights'ı yeni Azure İzleyicisi için uyarı](../../azure-monitor/app/monitor-web-app-availability.md).
- **Application Insights özel ölçümleri** – olanak sağlayan tanımlama ve izleme ve Uyarılar için kendi ölçümleri göster. Kullanma hakkında daha fazla bilgi için bu makalede bkz [özel ölçüm için Application ınsights'ı yeni Azure İzleyicisi](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).
- **Application Insights hata Anomalileri (Akıllı algılama parçası)** –, otomatik olarak bildirir, neredeyse gerçek zamanlı olarak web uygulamanızın olağandışı artışı başarısız HTTP isteklerini veya bağımlılık çağrıları oranını karşılaşırsa. Application Insights hata Anomalileri (Akıllı algılama parçası) yeni Azure İzleyici, bir parçası olarak kullanılabilir olan en kısa sürede ve bunu-önümüzdeki aylarda kullanıma gibi bu belgenin sonraki yineleme bağlantıları ile güncelleştireceğiz.

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>Birleşik ölçümleri ve diğer Azure kaynakları için uyarılar

Mart 2018'den itibaren yeni nesil çok boyutlu Azure kaynakları için izleme ve uyarı kullanılabilirlik olmuştur. Şimdi yeni ölçüm platform ve uyarı neredeyse gerçek zamanlı özelliklerle daha hızlı. Daha da önemlisi, yeni platform boyutları, dilim ve filtre belirli değer birleşimi, koşul veya işlem için izin ver seçeneği içerdiğinden daha yeni ölçüm platform uyarılar, daha fazla ayrıntı sağlar. Yeni Azure İzleyici'de tüm uyarılar gibi yeni ölçüm uyarılarının e-posta veya SMS, sesli, Azure işlevi, Otomasyon Runbook'u ve daha fazla Web kancası ötesine genişletmek bildirimleri verme ActionGroups – kullanarak daha genişletilebilir. Daha fazla bilgi için [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak ölçüm Uyarıları yönetme](../../azure-monitor/platform/alerts-metric.md).
Azure kaynakları için yeni ölçümler olarak kullanılabilir:

- **Azure İzleyici standart platform ölçümleri** – çeşitli Azure Hizmetleri ve ürünleriyle popüler önceden doldurulmuş ölçümleri sağlar. Daha fazla bilgi için bu makaleye bakın [desteklenen ölçümler üzerinde Azure İzleyici](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported) ve [üzerinde Azure İzleyici ölçüm uyarıları destekleyen](../../azure-monitor/platform/alerts-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Azure İzleyici özel ölçümleri** – Azure Tanılama Aracı gibi kaynakları kullanan bir kullanıcıdan ölçümleri sağlar. Daha fazla bilgi için bu makaleye bakın [Azure İzleyici'de özel ölçümleri](../../azure-monitor/platform/metrics-custom-overview.md). Özel ölçüm kullanarak tarafından toplanan ölçümleri de yayımlayabilirsiniz [Windows Azure tanılama aracısını](../../azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm.md) ve [InfluxData Telegraf aracı](../../azure-monitor/platform/collect-custom-metrics-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>İzleme ve uyarı platform Klasik devre dışı bırakma

İzleme ve uyarı platform şu anda kullanılabilir gelen Klasik daha önce belirtildiği gibi [uyarılar (Klasik) bölümünde](../../azure-monitor/platform/alerts-classic.overview.md) Azure portalı kullanımdan kaldırılacak ay verilen geliyor, bunlar yeni sistem tarafından değiştirilmiştir.
İzleme ve uyarı eski Klasik üzerinde 31 Ağustos 2019 kullanımdan kaldırılacaktır; Kapanış ilgili API'leri, Azure portal arabirimi ve Hizmetleri içinde dahil. Özellikle, bu özellikler kullanım dışı kalacaktır:

- Eski (Klasik) ölçümleri ve uyarıları olarak şu anda Azure kaynakları için aracılığıyla kullanılabilen [uyarılar (Klasik) bölümünde](../../azure-monitor/platform/alerts-classic.overview.md) Azure portalı; olarak erişilebilir [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) kaynak
- Eski (Klasik) platformu ve özel ölçümler için Application Insights yanı sıra olarak bunlar üzerinde şu anda aracılığıyla kullanılabilen uyarı [uyarılar (Klasik) bölümünde](../../azure-monitor/platform/alerts-classic.overview.md) Azure portal ve olarak erişilebilir [microsoft.insights/ alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) kaynak
- Eski (Klasik) hata Anomalileri uyarı şu anda kullanılabilir olarak [Application Insights içinde akıllı algılama](../../azure-monitor/app/proactive-diagnostics.md) yapılandırılan uyarı; Azure portalında gösterilen [uyarılar (Klasik) bölümünde](../../azure-monitor/platform/alerts-classic.overview.md) Azure Portal

Tüm Klasik izleme ve sistemlerle ilgili uyarı [API](https://msdn.microsoft.com/library/azure/dn931945.aspx), [PowerShell](../../azure-monitor/platform/alerts-classic-portal.md), [CLI](../../azure-monitor/platform/alerts-classic-portal.md), [Azure portal sayfasındaki](../../azure-monitor/platform/alerts-classic-portal.md)ve [ Kaynak şablonu](../../azure-monitor/platform/alerts-enable-template.md) Ağustos 2019 sonuna kadar kullanılabilir kalır. 

Azure İzleyici'de Ağustos 2019 sonunda:

- Klasik izleme ve Uyarılar hizmetinin kullanımdan kaldırma ve yeni uyarı kuralları oluşturulması için artık kullanılabilir olacaktır.
- Uyarılar (Klasik) Ağustos 2019 ötesinde var olmaya devam uyarı kuralları yürütün ve bildirimleri harekete geçin, ancak değişiklik kullanılamaz.
- Eylül 2019, Klasik izleme ve uyarı, geçirilebilir, uyarı kuralları başlatma otomatik olarak Microsoft tarafından yeni platformu Azure İzleyici'taki eşdeğerlerine birkaç hafta kapsayan aşamalarında taşınır. İşlem kapalı kalma süresi sorunsuz ve müşterilerinin kapsamı izleme kaybı olmadan olacaktır.
- Uyarı kuralları geçişi yeni uyarılar platformu izleme kapsamı önceki gibi sağlar ancak yeni yüklerini bildirimi ateşlenir. Herhangi bir e-posta adresi, Web kancası uç noktası veya Klasik bir uyarı kuralı ile ilişkili mantıksal uygulama bağlantısı geçişi olduğunda İleri taşınır, ancak doğru şekilde çalışmayabilir gibi uyarı yükü yeni platformu farklı olacaktır.
- Bazı [otomatik olarak geçirilemez Klasik uyarı kuralları](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated) ve kullanıcıların el ile gerçekleştirilen eylem Haziran 2020'ye kadar çalışmaya devam eder.

> [!IMPORTANT]
> Microsoft Azure İzleyici piyasaya sunuluyor aşamalarında [gönüllü olarak geçirmek için aracı](alerts-using-migration-tool.md) , klasik bir uyarı kuralları yakında yeni platformu açın. Çalıştırıp onu zorla hala mevcut ve Eylül 2019 başlangıç geçirilebilir tüm Klasik uyarı kuralları tarafından. Müşteriler kullanan klasik bir uyarı kuralı yükü yeni yükü işlemek için uyarlanmış Otomasyon sağlamak gerekir [birleşik ölçümleri ve Uyarıları Application ınsights'ta](#unified-metrics-and-alerts-in-application-insights) veya [birleşik ölçümleri ve diğer Azure uyarıları Kaynakları](#unified-metrics-and-alerts-for-other-azure-resources), Klasik uyarı kuralları geçiş sonrası. Daha fazla bilgi için [klasik bir uyarı kuralı geçiş için hazırlama](alerts-prepare-migration.md)

Geçiş Aracı, uyarılardan gönüllü geçirmenizi biz sunmaya [uyarılar (Klasik) bölümünde](../../azure-monitor/platform/alerts-classic.overview.md) yeni Azure uyarıları için Azure portal'ın. Uyarılar (Klasik), yeni Azure İzleyicisi geçişi yapılandırılan tüm kurallar ücretsiz kalır ve ücret alınmaz. Geçirilen Klasik uyarı kuralları için e-posta, Web kancası veya mantıksal uygulama bildirimleri gönderme herhangi bir ücrete da size aittir değil. Ancak, daha yeni bir bildirim ya da eylem türleri (örneğin, SMS, sesli arama, ITSM tümleştirmesi, vb.) kullanımı için geçirilen veya yeni bir uyarı eklenip ücrete tabi olacaktır. Daha fazla bilgi için [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/).

Ayrıca, aşağıdaki ambit altında ücrete tabi olacaktır [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/):

- Yeni Azure İzleyici platformunda ücretsiz birimleri aşan oluşturulan tüm yeni (geçirilmeyen) uyarı kuralı
- Azure İzleyici tarafından dahil edilen ücretsiz birimler dışında tutulur ve alınan tüm veriler
- Application Insights tarafından yürütülen herhangi bir çok test web testi
- Azure İzleyici'de dahil edilen ücretsiz birimleri aşan depolanan tüm özel ölçümler

Bu makalede, bağlantılar ve yeni Azure İzleyici platformu benimsenmesi kullanıcılara yardımcı olmak için izleme ve kullanılabilirlik araçların yanı sıra işlevselliği uyarı yeni Azure ile ilgili ayrıntıları ile sürekli olarak güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [yeni birleşik Azure İzleyici](../../azure-monitor/overview.md).
* Yeni hakkında daha fazla bilgi [Azure uyarıları](../../azure-monitor/platform/alerts-overview.md).
