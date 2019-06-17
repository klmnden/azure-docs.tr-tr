---
title: Azure İzleyicisi'nde oturum | Microsoft Docs
description: İzleme verilerini Gelişmiş analiz için kullanılan Azure İzleyici günlüklerine açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 0203/26/2019
ms.author: bwren
ms.openlocfilehash: 897f2eef0a52838d6190cb85a6a7f4492250935b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244848"
---
# <a name="logs-in-azure-monitor"></a>Azure İzleyici'deki günlükler

> [!NOTE]
> Azure İzleyici tarafından toplanan tüm verileri ölçüm ve günlükleri olmak üzere iki temel türünden birini uyar. Bu makalede, günlükleri açıklanır. Başvurmak [Azure İzleyicisi'nde ölçümler](data-platform-metrics.md) ölçümleri ve çok ayrıntılı bir açıklaması için [İzleme verilerine Azure İzleyicisi tarafından toplanan](data-platform.md) iki bir karşılaştırması.

Azure İzleyici günlüklerine çeşitli kaynaklardan veri üzerinde karmaşık bir analiz gerçekleştirmek için özellikle yararlıdır. Bu makalede, günlükleri nasıl verilerle yapabilecekleriniz Azure İzleyici'de yapılandırılmış ve veri günlüklerine depolayan farklı veri kaynakları tanımlayan açıklanır.

> [!NOTE]
> Azure İzleyici günlüklerine ve azure'da günlük veri kaynakları arasındaki farkı anlamak önemlidir. Örneğin, azure'da abonelik düzeyindeki olayların yazılan bir [etkinlik günlüğü](activity-logs-overview.md) , Azure İzleyici Menüsü'nden görüntüleyebilirsiniz. En fazla kaynak için kullanım bilgileri yazacak bir [tanılama günlüğü](diagnostic-logs-overview.md) , farklı konumlara iletebilir. Azure İzleyici günlüklerine etkinlik günlükleri ve diğer izleme verilerinin tüm kaynak kümesini üzerinde ayrıntılı analiz sağlamak üzere birlikte tanılama günlükleri toplayan bir günlük veri platformudur.

## <a name="what-are-azure-monitor-logs"></a>Azure İzleyici günlüklerine nelerdir?

Azure İzleyici günlüklerine farklı türde kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve verileri içerir. Günlükleri, Azure İzleyici ölçümleri gibi sayısal değerleri içeren ancak genellikle ayrıntılı açıklamalar içeren metin verileri içerir. Kendi yapısında değişir ve düzenli aralıklarla toplanır değildir, bunlar daha fazla ölçüm verileri farklı. Olaylarla ve izlemelerle gibi telemetri depolanan Azure İzleyici günlüklerine ek performans verileri alabilir ve böylece tüm nelze kombinovat analiz için.

Ortak günlük girişi tutularak toplandığı bir olay türüdür. Olayları, uygulama veya hizmet tarafından oluşturulan ve genellikle kendi başlarına tüm bağlam sağlamak için yeterli bilgi içerir. Örneğin, bir olay belirli bir kaynağa oluşturulmuş veya değiştirilmiş, artan trafiğe yanıt olarak başlatılan yeni bir ana bilgisayar veya bir uygulamada bir hata algılandı olduğunu gösterebilir.

 Veri biçimi farklılık gösterebileceğinden, ihtiyaç duydukları yapısını kullanarak özel günlükleri uygulamalar oluşturabilirsiniz. Ölçüm verileri diğer veri analizi ve eğilimleri belirlemek için diğer izleme verilerinin ile birleştirmek için günlükleri bile depolanabilir.


## <a name="what-can-you-do-with-azure-monitor-logs"></a>Azure İzleyici günlüklerine ile ne yapabilirsiniz?
Aşağıdaki tabloda, Azure İzleyici'de günlüklerini kullanabileceğiniz farklı yollarını listeler.


|  |  |
|:---|:---|
| Çözümle | Kullanım [Log Analytics](../log-query/get-started-portal.md) yazmak için Azure portalında [oturum sorguları](../log-query/log-query-overview.md) ve güçlü veri Gezgini analiz altyapısı kullanarak günlük verileri etkileşimli olarak çözümlemek.<br>Kullanım [Application Insights analitik Konsolu](../app/analytics.md) günlük sorguları yazma ve Application ınsights'tan günlük verileri etkileşimli olarak çözümlemek için Azure portalında. |
| Görselleştirin | Sabitleme, tablolar veya grafikler için çizilir sorgu sonuçlarını bir [Azure panosuna](../../azure-portal/azure-portal-dashboards.md).<br>Oluşturma bir [çalışma kitabı](../app/usage-workbooks.md) birden çok etkileşimli bir rapordaki veri kümesi ile birleştirilecek. <br>Sorgu sonuçlarını dışarı aktarma [Power BI](powerbi.md) farklı görselleştirme kullanın ve Azure dışındaki kullanıcılarla paylaşmak için.<br>Sorgu sonuçlarını dışarı aktarma [Grafana](grafana-plugin.md) kendi yönelik Kompozit yararlanın ve diğer veri kaynaklarıyla birleştirmek için.|
| Uyarı | Yapılandırma bir [günlük uyarı kuralı](alerts-log.md) bildirim gönderen veya alan [eylemi otomatik](action-groups.md) zaman sorgunun sonuçlarını eşleşen belirli bir sonuç.<br>Yapılandırma bir [ölçüm uyarısı kuralının](alerts-metric-logs.md) ölçümler olarak ayıklanan belirli günlük veri günlükleri ile ilgili. |
| Alma | Erişim günlüğü sorgu sonuçlarına kullanılarak bir komut satırından [Azure CLI](/cli/azure/ext/log-analytics/monitor/log-analytics).<br>Erişim günlüğü sorgu sonuçlarına kullanılarak bir komut satırından [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/az.operationalinsights).<br>Günlük sorgu sonuçlarını kullanarak bir özel uygulama erişimi [REST API](https://dev.loganalytics.io/). |
| Dışarı Aktarma | Günlük verilerini almak ve kullanarak bir dış konuma kopyalamak için bir iş akışı derleme [Logic Apps](~/articles/logic-apps/index.yml). |


## <a name="how-is-data-in-azure-monitor-logs-structured"></a>Azure İzleyici günlüklerine yapılandırılmış verileri nasıl mi?
Azure İzleyici günlüklerine tarafından toplanan verilerin depolandığı bir [Log Analytics çalışma alanı](../platform/manage-access.md). Yapabilecekleriniz [birden çok çalışma alanı oluşturma](manage-access.md#determine-the-number-of-workspaces-you-need) farklı günlük veri kümelerini yönetmek için aboneliğinizdeki. Her bir çalışma alanı her belirli bir kaynaktan verileri depolamak birden çok tablo içeriyor. Tüm tabloları paylaşma sırada [bazı ortak özellikler](log-standard-properties.md), her bir dizi benzersiz, depoladığı verilerin türüne bağlı olarak özellikleri vardır. Daha fazla tablo farklı izleme çözümleri ile çalışma alanına yazma diğer hizmetler tarafından eklenir ve yeni bir çalışma alanı standart tablo kümesini gerekecektir.

Günlük verilerini Application Insights ile çalışma alanları aynı Log Analytics motorunu kullanır, ancak her izlenen uygulama için ayrı olarak depolanır. Her uygulama, tablolar, uygulama istekleri, özel durumlar ve sayfa görüntülemeleri gibi verileri tutmak için standart bir dizi sahiptir.

Günlük sorguları bir Log Analytics çalışma alanı kullanım verilerini ya da bir Application Insights uygulama olur. Kullanabileceğiniz bir [kaynaklar arası sorgu](../log-query/cross-workspace-query.md) diğer günlük verileriyle birlikte uygulama verilerini analiz etmek için veya birden çok çalışma alanında veya uygulamaları dahil olmak üzere sorgular oluşturabilirsiniz.

![Çalışma Alanları](media/data-platform-logs/workspaces.png)

## <a name="log-queries"></a>Günlük sorguları
Azure İzleyici günlüklerine verileri kullanarak alınır bir [günlük sorgusu](../log-query/log-query-overview.md) ile yazılmış [Kusto sorgu dili](../log-query/get-started-queries.md), hızlı bir şekilde almak, birleştirmek ve toplanan verileri çözümlemek yapmanıza olanak tanıyan. Kullanım [Log Analytics](../log-query/portals.md) yazma ve günlük sorguları Azure Portalı'nda test etmek için. İş sonuçları ile etkileşimli olarak veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin olanak tanır.

![Log Analytics](media/data-platform-logs/log-analytics.png)

Açık [Application ınsights'tan Log Analytics](../app/analytics.md) Application Insights verilerini analiz etmek için.

![Application Insights Analytics](media/data-platform-logs/app-insights-analytics.png)

Kullanarak günlük verilerini alabilirsiniz [Log Analytics API](https://dev.loganalytics.io/documentation/overview) ve [Application Insights REST API](https://dev.applicationinsights.io/documentation/overview).


## <a name="sources-of-azure-monitor-logs"></a>Azure İzleyici günlüklerine kaynakları
Azure İzleyici, çeşitli kaynaklardan hem de azure'daki ve şirket içi kaynaklardan gelen günlük verilerini toplayabilir. Aşağıdaki tablolar kullanılabilir Azure İzleyici günlüklerine veri yazma farklı kaynaklardan farklı veri kaynaklarını listeler. Her bir bağlantı ayrıntıları için gerekli yapılandırmalar üzerinde vardır.

### <a name="azure-tenant-and-subscription"></a>Azure kiracısı ve abonelik

| Veriler | Açıklama |
|:---|:---|
| Azure Active Directory denetim günlükleri | Her dizin için tanılama ayarları aracılığıyla yapılandırılır. Bkz: [günlükleri Azure İzleyici ile Azure AD tümleştirme günlükleri](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).  |
| Etkinlik günlükleri | Varsayılan olarak ayrı olarak depolanır ve gerçek zamanlı uyarılar için kullanılabilir. Etkinlik günlüğü analizi çözümü, Log Analytics çalışma alanına yazılacak yükleyin. Bkz: [toplamak ve Log analytics'te Azure etkinlik günlüklerini çözümleme](activity-log-collect.md). |

### <a name="azure-resources"></a>Azure kaynakları

| Veriler | Açıklama |
|:---|:---|
| Kaynak tanılama | Bir Log Analytics çalışma alanı için ölçümler de dahil olmak üzere tanılama verilerini yazmak için tanılama ayarları yapılandırın. Bkz: [Stream Azure tanılama günlükleri Log analytics'e](diagnostic-logs-stream-log-store.md). |
| İzleme çözümleri | İzleme çözümleri, bunlar toplamak, Log Analytics çalışma alanına veri yazma. Bkz [veri koleksiyonu ayrıntıları Azure yönetim çözümlerine için](../insights/solutions-inventory.md) çözümleri listesi. Bkz: [çözümlerini Azure İzleyici'de izleme](../insights/solutions.md) yükleme ve çözümlerle ilgili ayrıntılar için. |
| Ölçümler | Günlük verilerini korumak için uzun süreler için ve diğer veri türleri kullanarak karmaşık bir analiz gerçekleştirmek için bir Log Analytics çalışma alanı kaynakları Azure izleme için platform ölçümleri göndermek [Kusto sorgu dili](/azure/kusto/query/). Bkz: [Stream Azure tanılama günlükleri Log analytics'e](diagnostic-logs-stream-log-store.md). |
| Azure tablo depolama | Azure depolama alanından Azure bazı kaynaklar burada yazma toplamak izleme verileri. Bkz: [kullanımı Azure blob Depolama olaylarını Log Analytics ile IIS ve Azure tablo depolama için](azure-storage-iis-table.md). |

### <a name="virtual-machines"></a>Virtual Machines

| Veriler | Açıklama |
|:---|:---|
|  Aracı veri kaynakları | Veri kaynakları toplandı [Windows](agent-windows.md) ve [Linux](../learn/quick-collect-linux-computer.md) aracıları, olayları, performans verilerini ve özel günlükler içerir. Bkz: [Azure İzleyici aracı veri kaynaklarında](data-sources.md) listesini veri kaynakları ve yapılandırma hakkında ayrıntılı bilgi için. |
| İzleme çözümleri | İzleme çözümleri, bunlar aracılarından Log Analytics çalışma alanına toplayabilir veri yazma. Bkz [veri koleksiyonu ayrıntıları Azure yönetim çözümlerine için](../insights/solutions-inventory.md) çözümleri listesi. Bkz: [çözümlerini Azure İzleyici'de izleme](../insights/solutions.md) yükleme ve çözümlerle ilgili ayrıntılar için. |
| System Center Operations Manager | Azure İzleyici'nın şirket içi aracılardan oturum açtığı olay ve performans verilerini toplamaya Operations Manager yönetim grubuna bağlanın. Bkz: [Log Analytics için Operations Manager'ı bağlama](om-agents.md) bu yapılandırma hakkında ayrıntılı bilgi için. |


### <a name="applications"></a>Uygulamalar

| Veriler | Açıklama |
|:---|:---|
| İstekler ve özel durumlar | Uygulama istekleri ve özel durumlar hakkında ayrıntılı veri bulunduğunuz _istekleri_, _pageViews_, ve _özel durumları_ tablolar. Çağrılar [dış bileşenler](../app/asp-net-dependencies.md) bulunan _bağımlılıkları_ tablo. |
| Kullanım ve performans | Uygulama için performans kullanılabilir _istekleri_, _browserTimings_ ve _performanceCounters_ tablolar. Verileri [özel ölçümler](../app/api-custom-events-metrics.md#trackevent) bulunduğu _customMetrics_ tablo.|
| İzleme verileri | Oluşur [izleme dağıtılmış](../app/distributed-tracing.md) depolanır _izlemeleri_ tablo. |
| Kullanılabilirlik testleri | Özet verileri [kullanılabilirlik testleri](../app/monitor-web-app-availability.md) depolanan _availabilityResults_ tablo. Bu testler ayrıntılı verilerinden ayrı depolama içindedir ve Azure portalında uygulama anlayışları'ndan erişilebilir. |

### <a name="insights"></a>Insights

| Veriler | Açıklama |
|:---|:---|
| Kapsayıcılar için Azure İzleyici | Tarafından toplanan Envanter ve performans verileri [kapsayıcılar için Azure İzleyici](../insights/container-insights-overview.md). Bkz: [kapsayıcı veri toplama ayrıntıları](../insights/container-insights-log-search.md#container-records) tabloların listesi. |
| VM'ler için Azure İzleyici | Tarafından toplanan Haritası ve performans verileri [VM'ler için Azure İzleyici](../insights/vminsights-overview.md). Bkz: [VM'ler için Azure İzleyici günlüklerinden sorgulama](../insights/vminsights-log-search.md) bu verilerin sorgulanması ilişkin ayrıntılar için. |

### <a name="custom"></a>Özel 

| Veriler | Açıklama |
|:---|:---|
| REST API | Veri herhangi bir REST istemcisinden Log Analytics çalışma alanına yazın. Bkz: [gönderme günlük verilerini Azure İzleyici ile HTTP veri toplayıcı API'sini](data-collector-api.md) Ayrıntılar için.
| Logic App | Bir mantıksal uygulama iş akışı ile bir Log Analytics çalışma alanına herhangi bir veri yazma **Azure Log Analytics Veri Toplayıcı** eylem. |

### <a name="security"></a>Güvenlik

| Veriler | Açıklama |
|:---|:---|
| Azure Güvenlik Merkezi | [Azure Güvenlik Merkezi](/azure/security-center/) burada analiz ile diğer günlük verilerini bir Log Analytics çalışma alanında toplanan verileri depolar. Bkz: [Azure Güvenlik Merkezi'nde veri toplamayı](../../security-center/security-center-enable-data-collection.md) çalışma alanı yapılandırması hakkında ayrıntılı bilgi için. |
| Azure Sentinel | [Azure Sentinel](/azure/sentinel/) bir Log Analytics çalışma alanına veri kaynaklarından alınan verileri depolar. Bkz: [veri kaynağına bağlanın](/azure/sentinel/connect-data-sources).  |


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure İzleyici, veri platformu](data-platform.md).
- Hakkında bilgi edinin [Azure İzleyicisi'nde ölçümler](data-platform-metrics.md).
- Hakkında bilgi edinin [izleme verilerini kullanılabilir](data-sources.md) azure'daki farklı kaynakları.
