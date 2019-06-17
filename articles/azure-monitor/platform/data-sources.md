---
title: Azure İzleyici'de veri kaynaklarını | Microsoft Docs
description: Sistem durumu ve Azure kaynaklarınızı ve bunlar üzerinde çalışan uygulamaların performansını izlemek için kullanılabilir veri açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: bwren
ms.openlocfilehash: b77fb3ab5651147c59b9f0afd22a2d6d0159c90e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66357455"
---
# <a name="sources-of-monitoring-data-for-azure-monitor"></a>Azure İzleyici için veri izleme kaynakları
Azure İzleyicisi'ni temel alan bir [izleme ortak veri platformu](data-platform.md) içeren [günlükleri](data-platform-logs.md) ve [ölçümleri](data-platform-metrics.md). Bu platformda verileri toplama araçları ortak bir dizi Azure İzleyici'de birlikte kullanarak analiz için çeşitli kaynaklardan toplanan veriler sağlar. Veri izleme de diğer konumlara belirli senaryoları desteklemek için gönderilebilir ve günlükleri ya da ölçümler toplanan kullanılabilmesi için öncelikle bazı kaynaklar diğer konumlara yazabilir.

Bu makalede, Azure kaynakları tarafından oluşturulan izleme verilerini yanı sıra Azure İzleyici tarafından toplanan verileri izleme farklı kaynakları açıklanır. Bu veriler farklı konumlara toplamak için gerekli yapılandırma hakkında ayrıntılı bilgi için bağlantılar sağlanır.

## <a name="application-tiers"></a>Uygulama katmanları

Azure uygulamalarından veri izleme kaynakları katmanları, en yüksek katmanları, uygulamanızın olan ve Azure platformu bileşenleri olan alt katmanları düzenlenebilir. Yöntem her katmandan veri erişimi değişir. Uygulama katmanları aşağıdaki tabloda özetlenir ve her katmanında veri izleme kaynakları aşağıdaki bölümlerde sunulur. Bkz: [veri konumlarını azure'da izleme](data-locations.md) her verileri konumu ve verilerini nasıl erişebileceğiniz bir açıklaması.


![Katmanları izleme](../media/overview/overview.png)


### <a name="azure"></a>Azure
Aşağıdaki tabloda, Azure'a özgüdür uygulama katmanları kısaca açıklanmaktadır. Aşağıdaki bölümlerde her bağlantı için daha fazla aşağıdaki ayrıntıları.

| Katman | Açıklama | Toplama yöntemi |
|:---|:---|:---|
| [Azure Kiracısı](#azure-tenant) | Azure Active Directory gibi Azure hizmetlerinin Kiracı düzeyinde çalışması hakkında veriler. | AAD verileri portalda görüntüleyebilir veya Azure İzleyici'de bir kiracı tanılama ayarını kullanarak koleksiyona yapılandırın. |
| [Azure aboneliği](#azure-subscription) | Sistem durumu ve Resource Manager ve hizmet sistem durumu gibi Azure aboneliğinizdeki kaynaklar arası Hizmetleri Yönetimi ile ilgili veriler. | Portalda görüntüleyebilir veya Azure İzleyici'de bir günlük profili kullanarak koleksiyona yapılandırın. |
| [Azure kaynakları](#azure-resources) |  Her Azure kaynağı, performans ve işlem ile ilgili veriler. | Toplanan ölçümler otomatik olarak ölçüm Gezgini'nde görüntüleyin.<br>Azure İzleyici'de günlükleri toplamak için tanılama ayarlarını yapılandırın.<br>İzleme çözümleri ve öngörüleri belirli kaynak türleri için daha ayrıntılı izleme için kullanılabilir. |

### <a name="azure-other-cloud-or-on-premises"></a>Azure, diğer Bulut veya şirket içi 
Aşağıdaki tabloda, Azure, başka bir bulutta veya şirket içinde olabilir uygulama katmanları kısaca açıklanmaktadır. Aşağıdaki bölümlerde her bağlantı için daha fazla aşağıdaki ayrıntıları.

| Katman | Açıklama | Toplama yöntemi |
|:---|:---|:---|
| [İşletim sistemi (konuk)](#operating-system-guest) | İşlem kaynakları işletim sisteminde ilgili veriler. | İstemci veri kaynakları Azure İzleyici VM'ler için destekleme bağımlılıkları toplamak için Azure İzleyici ve bağımlılık aracısını toplanması için Log Analytics aracısını yükleyin.<br>Azure sanal makineler için Azure İzleyici ile günlükleri ve ölçümleri toplamak için Azure tanılama uzantısını yükleyin. |
| [Uygulama kodu](#application-code) | Performansı ve işlevselliği gerçek uygulama ve performans izlemeleri, uygulama günlükleri ve kullanıcı telemetrisi gibi kod, ilgili veriler. | Application Insights'a veri toplamak için kodunuzu izleyin. |
| [Özel kaynaklar](#custom-sources) | Dış hizmetler veya diğer bileşenler veya cihazları verileri. | Günlük veya ölçüm verilerini Azure İzleyici ile herhangi bir diğer istemciden toplayın. |

## <a name="azure-tenant"></a>Azure kiracısı
Azure Active Directory gibi Kiracı genelinde hizmetlerden olmak üzere Azure kiracısı için ilgili telemetri toplanır.

![Azure Kiracı koleksiyonu](media/data-sources/tenant.png)

### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory denetim günlüklerini
[Azure Active Directory raporlama](../../active-directory/reports-monitoring/overview-reports.md) oturum açma etkinlik ve denetim izi belirli bir kiracıda yapılan değişikliklerin geçmişini içerir. 

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici günlüklerine | Azure AD günlükleri ile diğer izleme verilerini çözümlemek için Azure İzleyici'de toplanacak yapılandırın. | [Azure İzleyici günlüklerine (Önizleme) ile Azure AD günlükleri tümleştirme](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure Storage | Azure AD günlükleri arşivlemek için Azure Depolama'ya aktarın. | [Öğretici: Bir Azure depolama hesabına (Önizleme) Azure AD günlüklerini arşivleme](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Olay Hub'ı | Stream Azure AD, olay hub'larını kullanarak diğer konumlara günlüğe kaydeder. | [Öğretici: Azure Active Directory günlükleri (Önizleme) Azure olay hub'ına Stream](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md). |



## <a name="azure-subscription"></a>Azure aboneliği
Sistem durumu ve Azure aboneliğinizin işlemi ile ilgili telemetri.

![Azure aboneliği](media/data-sources/azure-subscription.png)

### <a name="azure-activity-log"></a>Azure Etkinlik günlüğü 
[Azure etkinlik günlüğü](activity-logs-overview.md) hizmet sağlık kaydı, Azure aboneliğinizdeki kaynaklar için yapılandırma değişiklikleri kayıtları ile birlikte içerir. Etkinlik günlüğü, tüm Azure kaynaklarını ve temsil kullanılabilir kendi _dış_ görünümü.

| Hedef | Açıklama | Başvuru |
|:---|:---|
| Etkinlik günlüğü | Etkinlik günlüğü, Azure İzleyici Menüsü'nden görüntülemek veya etkinlik günlüğü uyarıları oluşturma, kendi veri deposuna toplanır. | [Azure portalında etkinlik günlüğü sorgulama](activity-log-view.md#azure-portal) |
| Azure İzleyici günlüklerine | İle diğer izleme verilerini çözümlemek için etkinlik günlüğü toplamak için Azure İzleyici günlüklerine yapılandırın. | [Toplama ve Azure İzleyici'de Log Analytics çalışma alanında Azure etkinlik günlüklerini çözümleme](activity-log-collect.md) |
| Azure Storage | Etkinlik günlüğünü arşivleme için Azure Depolama'ya aktarın. | [Etkinlik günlüğünü arşivleme](activity-log-export.md#archive-activity-log)  |
| Event Hubs | Etkinlik günlüğünün Event Hubs kullanarak diğer konumlara Stream | [Stream etkinlik günlüğü olay hub'ına](activity-log-export.md#stream-activity-log-to-event-hub). |

### <a name="azure-service-health"></a>Azure Hizmet Durumu
[Azure hizmet durumu](../../service-health/service-health-overview.md) uygulama ve kaynakları kullanır, aboneliğinizdeki Azure hizmetlerinin durumu hakkında bilgi sağlar.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Etkinlik günlüğü<br>Azure İzleyici günlüklerine | Bunları Azure portalında görüntüleyebilir veya etkinlik günlüğü ile gerçekleştirebileceğiniz diğer etkinlikleri gerçekleştirmek için hizmet durumu kayıtları Azure etkinlik günlüğünde depolanır. | [Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme](service-notifications.md) |


## <a name="azure-resources"></a>Azure kaynakları
Ölçümler ve kaynak düzeyi tanılama günlükleri hakkında bilgi sağlar _iç_ Azure kaynaklarınızın işlemi. Bunlar çoğu Azure hizmeti için kullanılabilir ve izleme çözümleri ve öngörüleri belirli hizmetler için ek verileri toplayabilirsiniz.

![Azure kaynak koleksiyonunu](media/data-sources/azure-resources.png)


### <a name="platform-metrics"></a>Platform ölçümleri 
Çoğu Azure hizmeti gönderir [platform ölçümleri](data-platform-metrics.md) performans ve işlem ölçümlerini veritabanına doğrudan yansıtır. Belirli [ölçümleri, her kaynak türü için değişir](metrics-supported.md). 

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici ölçümleri | Platform ölçümleri, herhangi bir yapılandırma ile Azure İzleyici ölçüm veritabanına yazar. Ölçüm Gezgini erişim platform ölçümleri.  | [Azure ölçüm Gezgini ile çalışmaya başlama](metrics-getting-started.md)<br>[Azure İzleyici ile desteklenen ölçümler](metrics-supported.md) |
| Azure İzleyici günlüklerine | Platform ölçümleri, günlükleri Log Analytics kullanarak eğilim ve analiz için kopyalayın. | [Azure tanılama verilerini doğrudan Log Analytics'e bağlanma](collect-azure-metrics-logs.md#azure-diagnostics-direct-to-log-analytics) |
| Event Hubs | Event Hubs kullanarak diğer konumlara Stream ölçümleri. |[Stream Azure harici bir aracı tarafından veri tüketimi için olay hub'ına izleme](stream-monitoring-data-event-hubs.md) |

### <a name="diagnostic-logs"></a>Tanılama günlükleri
[Tanılama günlükleri](diagnostic-logs-overview.md) Öngörüler sağlayan _iç_ bir Azure kaynağının işlemi.  Tanılama günlükleri, varsayılan olarak etkin değildir. Bunları etkinleştirmek ve her kaynak için bir hedef belirtmeniz gerekir. 

Tanılama günlükleri içeriği ve yapılandırma gereksinimleri kaynak türüne göre farklılık gösterir ve tüm hizmetler henüz tanılama günlükleri oluşturun. Bkz: [desteklenen hizmetler, şemalar ve kategoriler için Azure tanılama günlükleri](diagnostic-logs-schema.md) her hizmet ve ayrıntılı yapılandırma yordamlara bağlantılar hakkında ayrıntılı bilgi için. Ardından bu hizmet şu anda bu makalede hizmet listelenmiyorsa, tanılama günlükleri yazma değil.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici günlüklerine | Tanılama günlükleri ile diğer toplanan günlük veri analizi için Azure İzleyici günlüklerine gönderin. Diğer Log Analytics çalışma alanınıza alınmadan önce bir depolama hesabına yazma sırasında bazı kaynaklar doğrudan Azure İzleyici yazabilirsiniz. | [Azure İzleyici'de Log Analytics çalışma alanına Stream Azure tanılama günlükleri](diagnostic-logs-stream-log-store.md)<br>[Azure Depolama'dan günlükleri toplamak için Azure portalını kullanma](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage)  |
| Depolama | Tanılama gönderme arşivlemek için Azure Depolama'ya günlükleri. | [Azure tanılama günlüklerini arşivleme](archive-diagnostic-logs.md) |
| Event Hubs | Event Hubs kullanarak diğer konumlara Stream tanılama günlükleri. |[Olay hub'ına Stream Azure tanılama günlükleri](diagnostic-logs-stream-event-hubs.md) |

## <a name="operating-system-guest"></a>İşletim sistemi (konuk)
Azure, diğer bulutlarda ve şirket içi bilgi işlem kaynaklarının izlemek için bir konuk işletim sistemi vardır. Bir veya daha fazla aracı yüklemesi ile Azure Hizmetleri ile aynı izleme araçlarıyla analiz etmek için Azure İzleyici ile Konuk telemetri toplayabilir.

![Azure işlem kaynak koleksiyonu](media/data-sources/compute-resources.png)

### <a name="azure-diagnostic-extension"></a>Azure tanılama uzantısı
Günlükleri toplamak için ve işlem kaynakları Azure bulut hizmeti (Klasik) Web ve çalışan rolleri, sanal makineler, sanal makine de dahil olmak üzere Azure konuk işletim sisteminden ölçümler sağlayan Azure sanal makineler için Azure tanılama uzantısını etkinleştirme Ölçek kümeleri ve Service Fabric.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Depolama | Tanılama uzantısını etkinleştirdiğinizde, varsayılan olarak bir depolama hesabına yazma. | [Azure Depolama’daki tanılama verilerini depolama ve görüntüleme](diagnostics-extension-to-storage.md) |
| Azure İzleyici ölçümleri | Performans sayaçları toplamak için tanılama uzantısını yapılandırdığınızda, bunlar Azure İzleyici ölçüm veritabanına yazılır. | [Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder](collect-custom-metrics-guestos-resource-manager-vm.md) |
| Application Insights günlükleri | Günlükleri ve performans sayaçları, uygulamanızın diğer uygulama verilerinizi Analiz edilecek destekleyen işlem kaynaktan toplayın. | [Bulut hizmeti, sanal makine ya da Service Fabric tanılama verilerini Application Insights'a gönderme](diagnostics-extension-to-application-insights.md) |
| Event Hubs | Event Hubs kullanarak diğer konumlara veri akışı tanılama uzantısını yapılandırın.  | [Event Hubs kullanarak Azure Tanılama verileri etkin yolu akış](diagnostics-extension-stream-event-hubs.md) |

### <a name="log-analytics-agent"></a>Log Analytics Aracısı 
Kapsamlı izleme ve yönetim, Windows veya Linux sanal makineleri için Log Analytics aracısını yükleyin. Azure, başka bir bulut veya şirket içi sanal makinenin çalışıyor olabilir.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici günlüklerine | Azure'da Log Analytics aracısını bağlanır, yapılandırdığınız veri kaynaklarından veya uygulamalara ek Öngörüler sağlayan çözümlerini izleme verileri toplamanıza olanak tanır ve doğrudan ya da System Center Operations Manager ile izleme Sanal makinede çalışıyor. | [Azure İzleyici aracı veri kaynakları](agent-data-sources.md)<br>[Operations Manager'ı Azure İzleyicisi ile bağlantı](om-agents.md) |


### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici 
[VM'ler için Azure İzleyici](../insights/vminsights-overview.md) hizmet durumu ve VM sistem durumu da dahil olmak üzere temel Azure İzleyici işlevsellikten dışında özellikler sağlayarak sanal makineler için özelleştirilmiş bir izleme deneyimi sağlar. Dış işlem bağımlılıklarını ve sanal makine üzerinde çalışan işlemler hakkında bulunan veri toplamak için Log Analytics aracısını ile tümleşen bir bağımlılık aracısını Windows ve Linux sanal makinelerinde gerektiriyor.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici günlüklerine | Aracıda işlemleri ve bağımlılıkları hakkında daha fazla veri depolar. | [Azure İzleyicisi'ni (Önizleme) Map uygulama bileşenleri anlamak için VM'ler için kullanma](../insights/vminsights-maps.md) |
| VM depolama | VM'ler için Azure İzleyici durumu bilgileri sistem durumu, özel bir konumda depolar. Bu yalnızca ek olarak Azure portalında sanal makineler için Azure İzleyici kullanılabilir [Azure kaynak durumu REST API](/rest/api/resourcehealth/). | [Azure sanal makinelerinizin durumunu anlama](../insights/vminsights-health.md)<br>[Azure kaynak durumu REST API](https://docs.microsoft.com/rest/api/resourcehealth/) |



## <a name="application-code"></a>Uygulama kodu
Azure İzleyicisi'nde ayrıntılı uygulama izleme ile yapılır [Application Insights](https://docs.microsoft.com/azure/application-insights/) çeşitli platformlarda çalışan uygulamalardan veri toplar. Uygulama, Azure, başka bir bulutta veya şirket içinde çalışıyor olabilir.

![Uygulama veri toplama](media/data-sources/applications.png)


### <a name="application-data"></a>Uygulama verileri
Bir izleme paketi yükleyerek bir uygulama için Application ınsights'ı etkinleştirdiğinizde, ölçüm ve günlükleri performans ve uygulama işlemi ilgili toplar. Application Insights, diğer veri kaynakları tarafından kullanılan Azure İzleyici veri platformundaki topladığı verileri depolar. Bu verileri analiz etmek için kapsamlı araçlar içerir, ancak, de, ölçüm Gezgini ve Log Analytics gibi araçları kullanarak diğer kaynaklardan verileri analiz edebilirsiniz.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici günlüklerine | Sayfa görüntülemeleri, uygulama istekleri, özel durumlar ve izlemeler gibi uygulamanız hakkında işletimsel verileri. | [Azure İzleyici'de günlük verilerini çözümleme](../log-query/log-query-overview.md) |
|                    | Telemetri bağıntısı ile Uygulama Haritası desteklemek için uygulama bileşenleri arasındaki bağımlılık bilgileri. | [Application ınsights telemetri bağıntısı](../app/correlation.md) <br> [Uygulama Eşlemesi](../app/app-map.md) |
|            | Kullanılabilirlik testleri, kullanılabilirlik ve yanıt hızı, uygulamanızın genel Internet üzerindeki farklı konumlardan test sonuçları. | [Web sitelerinin kullanılabilirliğini ve yanıt hızını izleme](../app/monitor-web-app-availability.md) |
| Azure İzleyici ölçümleri | Application Insights, performans ve uygulamanızda Azure İzleyici ölçüm veritabanına tanımladığınız özel ölçümler ek olarak uygulamanın açıklayan ölçümleri toplar. | [Günlük tabanlı ve önceden toplanan ölçümler Application ınsights](../app/pre-aggregated-metrics-log-metrics.md)<br>[Özel olaylar ve ölçümler için Application Insights API](../app/api-custom-events-metrics.md) |
| Azure Storage | Uygulama verileri arşivlemek için Azure depolama alanına gönderin. | [Application Insights’tan telemetriyi dışarı aktarma](../app/export-telemetry.md) |
|            | Kullanılabilirlik testleri ayrıntılarını Azure Depolama'da saklanır. Application Insights, yerel analiz için indirmek için Azure portalında kullanın. Kullanılabilirlik testi sonuçları, Azure İzleyici günlüklerine depolanır. | [Web sitelerinin kullanılabilirliğini ve yanıt hızını izleme](../app/monitor-web-app-availability.md) |
|            | Profiler izlemesi veriler Azure Depolama'da depolanır. Application Insights, yerel analiz için indirmek için Azure portalında kullanın.  | [Azure Application Insights ile profil üretim uygulamaları](../app/profiler-overview.md) 
|            | Hata ayıklama anlık görüntüsü özel durumlar bir alt kümesi için Yakalanan veriler Azure Depolama'da depolanır. Application Insights, yerel analiz için indirmek için Azure portalında kullanın.  | [Anlık görüntüleri nasıl çalışır?](../app/snapshot-debugger.md#how-snapshots-work) |

## <a name="monitoring-solutions-and-insights"></a>İzleme çözümleri ve öngörüleri
[İzleme çözümleri](../insights/solutions.md) ve [Insights](../insights/insights-overview.md) belirli hizmet veya uygulamanın işlemi ek Öngörüler sağlamak için veri toplama. Farklı uygulama katmanları ve hatta birden fazla katman kaynakları ele.

### <a name="monitoring-solutions"></a>İzleme çözümleri

| Hedef | Açıklama | Başvuru
|:---|:---|:---|
| Azure İzleyici günlüklerine | İzleme çözümleri verileri toplama Azure İzleyici günlüklerine burada olabilir sorgu dilini kullanarak Analiz veya [görünümleri](view-designer.md) genellikle çözümüne eklenmiş. | [Azure çözümlerini izleme için veri koleksiyonu ayrıntıları](../insights/solutions-inventory.md) |


### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
[Kapsayıcılar için Azure İzleyici](../insights/container-insights-overview.md) için özelleştirilmiş bir izleme deneyimi sağlar [Azure Kubernetes Service (AKS)](/azure/aks/). Bunu aşağıdaki tabloda açıklanan bu kaynakları ile ilgili ek bilgiler toplar.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici günlüklerine | Envanter, günlükler ve olaylar gibi AKS için izleme verilerini depolar. Ölçüm verilerini analiz işlevselliği portalında yararlanmak üzere günlükleri de depolanır. | [Kapsayıcılar için Azure İzleyici ile AKS kümesi performansını anlama](../insights/container-insights-analyze.md) |
| Azure İzleyici ölçümleri | Ölçüm verilerini sürücü Görselleştirme ve uyarılar ölçüm veritabanına depolanır. | [Ölçüm Gezgini'nde kapsayıcı ölçümlerini görüntüleme](../insights/container-insights-analyze.md#view-container-metrics-in-metrics-explorer) |
| Azure Kubernetes Service | Neredeyse gerçek zamanlı deneyime sırayla kapsayıcılar için Azure İzleyici Azure Portalı'nda verileri doğrudan Azure Kubernetes hizmeti sunar. | [Kapsayıcılar için Azure İzleyici ile kapsayıcı günlüklerini gerçek zamanlı olarak görüntüleme (önizleme)](../insights/container-insights-live-logs.md) |

### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
[VM'ler için Azure İzleyici](../insights/vminsights-overview.md) sanal makineleri izlemek için özelleştirilmiş bir deneyim sağlar. VM'ler için Azure İzleyici tarafından toplanan verileri açıklaması yer aldığı [işletim sistemi (konuk)](#operating-system-guest) yukarıdaki bölümde.

## <a name="custom-sources"></a>Özel kaynaklar
Standart bir uygulama katmanlarına ek olarak, diğer veri kaynaklarıyla toplanan telemetri olması diğer kaynakları izlemek gerekebilir. Bu kaynaklar için ölçüm veya Azure İzleyici API kullanarak günlükleri için bu verileri yazın.

![Özel Toplama](media/data-sources/custom.png)

| Hedef | Yöntem | Açıklama | Başvuru |
|:---|:---|:---|:---|
| Azure İzleyici günlüklerine | Veri Toplayıcı API’si | Herhangi bir diğer istemciden günlük verilerini toplamak ve Log Analytics çalışma alanında saklayın. | [Azure İzleyici HTTP veri toplayıcı API'sini (genel Önizleme) ile günlük verileri gönderin](data-collector-api.md) |
| Azure İzleyici ölçümleri | Özel ölçümler API'si | Herhangi bir REST istemcisi ve Azure İzleyici ölçüm veritabanı deposunda ölçüm verilerini toplayın. | [REST API'sini kullanarak bir Azure kaynağı için özel ölçümler Azure İzleyici ölçüm depoya gönder](metrics-store-custom-rest-api.md) |


## <a name="other-services"></a>Diğer hizmetler
Diğer Azure Hizmetleri, Azure İzleyici veri platformuna veri yazma. Bu, Azure İzleyici tarafından toplanan verilerle bu hizmetler tarafından toplanan verileri analiz etmek ve aynı çözümleme ve görselleştirme araçları yararlanmak sağlar.

| Hizmet | Hedef | Açıklama | Başvuru |
|:---|:---|:---|:---|
| [Azure Güvenlik Merkezi](/azure/security-center/) | Azure İzleyici günlüklerine | Azure Güvenlik Merkezi, Azure İzleyici tarafından toplanan diğer günlük verileri ile analiz izin veren bir Log Analytics çalışma alanında toplar güvenlik verileri depolar.  | [Azure Güvenlik Merkezinde veri toplama](../../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](/azure/sentinel/) | Azure İzleyici günlüklerine | Azure İzleyici tarafından toplanan diğer günlük verileri ile analiz izin veren bir Log Analytics çalışma alanında farklı veri kaynaklarından topladığı Azure Sentinel depolar.  | [Veri kaynağına bağlanın](/azure/sentinel/quickstart-onboard) |


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [türü izleme verilerinin Azure İzleyici tarafından toplanan](data-platform.md) ve görüntüleme ve bu verileri analiz etme.
- Liste [Azure kaynakları, veri depoladığınız farklı konumlara](data-locations.md) ve nasıl erişebilir. 
