---
title: Günlüğe kaydetme ve denetim azure | Microsoft Docs
description: Nasıl günlük verilerini uygulamanız hakkında ayrıntılı Öngörüler elde etmek için kullanabileceğiniz hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2019
ms.author: TomSh
ms.openlocfilehash: edadb369461bb3865dd6894c3329e7079fa9d13f
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752560"
---
# <a name="azure-logging-and-auditing"></a>Azure günlük kaydı ve denetim

Azure, geniş bir yelpazede denetim ve günlüğe kaydetme seçenekleri, güvenlik ilkeleri ve mekanizmaları boşlukları belirlemesine yardımcı olmak için yapılandırılabilir güvenlik sağlar. Bu makalede, oluşturma, toplama ve Azure'da barındırılan hizmetlerden güvenlik günlükleri çözümleme açıklanmaktadır.

> [!Note]
> Bu makaledeki bazı öneriler artan veri, ağ ya da işlem kaynağı kullanımına neden ve lisans ya da abonelik maliyetlerinizi artırabilir.

## <a name="types-of-logs-in-azure"></a>Azure'daki günlüklerin türleri

Bulut uygulamaları ile birçok hareketli parçadan karmaşık. Günlükleri uygulamalarınızı çalışır durumda tutma yardımcı olmak için veri sağlar. Günlükleri sorunlarını giderme veya olası olanları önlemeye yardımcı. Ve uygulama performansını veya bakım iyileştirmek veya aksi halde el ile müdahale gerektiren eylemleri otomatikleştirme yardımcı olabilir.

Azure günlükleri şu tür olarak kategorilere ayrılır:
* **Denetim/Yönetim günlükleri** Azure Resource Manager oluşturma, güncelleştirme ve silme işlemleri hakkında bilgi sağlar. Daha fazla bilgi için [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

* **Veri düzlemi günlükleri** bölümü Azure kaynak kullanımı oluşturulan olaylar hakkında bilgi sağlar. Bu tür bir günlük örnek güvenlik, Windows olay sistemi ve uygulama günlüklerine bir sanal makine (VM) ve [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) Azure İzleyici yapılandırılır.

* **İşlenen olaylar** sizin adınıza çözümlenmiş olaylar/işlenen uyarılar hakkında bilgi sağlar. Bu tür örnekleri [Azure Güvenlik Merkezi uyarılarını](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) burada [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) işlenir ve aboneliğinizi analiz ve kısa güvenlik uyarıları sağlar.

Aşağıdaki tablo, en önemli Azure'da kullanılabilen günlük türlerini listeler:

| Günlük kategorisi | Günlük türü | Kullanım | Tümleştirme |
| ------------ | -------- | ------ | ----------- |
|[Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Azure Resource Manager kaynaklarına denetim düzlemi olayları|   Aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.|    REST API, [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Azure Resource Manager kaynakların abonelik işlemi hakkında sık kullanılan veri|    Kaynak gerçekleştirilen işlemler hakkında bilgi sağlar.| Azure İzleyici [Stream](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Azure AD raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Günlüklerini ve raporları | Kullanıcı oturum açma etkinliği raporları ve kullanıcı ve Grup Yönetimi hakkında sistem etkinliği bilgileri.|[Graph API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Sanal makineler ve bulut Hizmetleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm)|Windows olay günlüğü hizmeti ve Linux Syslog|  Sistem verilerini ve sanal makinelerde günlük verilerini yakalar ve bu verileri, tercih ettiğiniz bir depolama hesabına aktarır.|   Windows (Windows Azure Tanılama'yı kullanarak [[WAD](https://docs.microsoft.com/azure/azure-diagnostics)] depolama) ve Azure İzleyici'de Linux|
|[Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Depolama günlüğü, depolama hesabı için ölçüm verileri sağlar|İzleme istekleri hakkında Öngörüler sağlar, kullanım eğilimlerini analiz eder ve depolama hesabınızla ilgili sorunlar tanılar.|   REST API veya [istemci kitaplığı](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Ağ güvenlik grubu (NSG) akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON biçiminde bir kural başına temelinde giden ve gelen akışları gösterir|Bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntüler.|[Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Application Insight](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Günlükler, özel durumlar ve özel tanılama|   Bir uygulama performansı izleme (APM) hizmeti birden çok platformda web geliştiricileri için sağlar.| REST API [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Verileri / güvenlik uyarıları|    Azure Güvenlik Merkezi uyarıları, Azure İzleyici günlükleri ve Uyarıları|    Güvenlik bilgileri ve uyarılar sağlar.|  REST API'ler, JSON|

### <a name="activity-logs"></a>Etkinlik günlükleri

[Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri daha önce "denetim günlüklerini" ve "işlem günlüklerini," olarak bilinen çünkü bunlar rapor [denetim düzlemi olayları](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) Abonelikleriniz için. 

Etkinlik günlükleri, belirlemenizi "ne, kim ve ne zaman" yazma işlemleri (diğer bir deyişle, PUT, gönderin veya Sil). Etkinlik, işlem ve ilgili diğer özellikleri durumunu anlamanıza yardım da günlüğe kaydeder. Etkinlik günlükleri, okuma (GET) işlemlerini içermez.

Bu makalede, PUT, POST ve DELETE kaynaklar üzerinde bir etkinlik günlüğü içeren tüm yazma işlemlerini bakın. Örneğin, etkinlik günlükleri ile ilgili sorunları giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcı bir kaynağı nasıl değiştirdiğini izlemek için kullanabilirsiniz.

![Etkinlik günlüğü diyagramı](./media/azure-log-audit/azure-log-audit-fig1.png)

Azure portalını kullanarak bir etkinlik günlüğünde olayları alabilirsiniz [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell cmdlet'leri ve [Azure İzleyici REST API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Etkinlik günlükleri, 90 günlük veri saklama süresine sahip olacak.

Tümleştirme senaryoları için bir etkinlik günlüğü olayında:

* [Bir etkinlik günlüğü olayı tarafından tetiklenen bir e-posta veya Web kancası uyarı oluşturma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email).

* [Olay hub'ına Stream](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) alımı üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.

* Power bı'da kullanarak çözümlemek [Power BI içerik paketi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Bir depolama hesabına arşivleme veya el ile İnceleme için Kaydet](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log). Günlük profilleri kullanılarak elde tutma süresi (gün cinsinden) belirtebilirsiniz.

* Sorgu ve Azure portalında görüntüleyebilirsiniz.

* PowerShell cmdlet, Azure CLI veya REST API sorgulayın.

* Günlük profilleri ile Etkinlik günlüğünü dışarı aktarma [Azure İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Bir depolama hesabı kullanabilir veya [olay hub'ı ad alanı](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) olmayan günlük yayma biri ile aynı abonelikte. Kişi ayarı yapılandırır uygun olmalıdır [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) iki abonelik erişimi.

### <a name="azure-diagnostics-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri, bu kaynağın işlemiyle ilgili zengin, sık kullanılan verilerini sağlayan bir kaynak tarafından gönderilir. Bu günlüklerin içeriği kaynak türüne göre değişir. Örneğin, [Windows olayı sistem günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) VM'ler için tanılama günlükleri kategorisi olan ve [blob, tablo ve kuyruk günlükleri](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) tanılama günlüklerinin depolama hesapları için kategoriler. Tanılama günlükleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlayan etkinlik günlüklerinden farklıdır.

![Diyagramları Azure tanılama günlükleri](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure tanılama günlükleri, Azure portalı, PowerShell, Azure CLI ve REST API gibi birden çok yapılandırma seçenekleri sunar.

**Tümleştirme senaryoları**

* Kaydetmek için bir [depolama hesabı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) denetim veya el ile İnceleme. Tanılama ayarlarını kullanarak, elde tutma süresi (gün cinsinden) belirtebilirsiniz.

* [Bunları event hubs'a Stream](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) üçüncü taraf hizmeti veya özel bir analiz çözümü, alımı için gibi [Powerbı](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Bunları analiz [Azure İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

**Desteklenen hizmetler, tanılama günlükleri ve desteklenen günlük kategorileri her kaynak türü için şema**


| Hizmet | Şema ve belgeler | Kaynak türü | Kategori |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Azure İzleyici günlüklerine yük dengeleyici (Önizleme)](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Ağ Güvenlik Grupları|[Ağ güvenlik grupları için Azure izleme günlükleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Uygulama ağ geçidi için tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Anahtar kasası günlükleri](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Etkinleştirme ve arama trafiği analizi kullanma](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Data Lake Store için tanılama günlüklerine erişme](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Denetim<br>İstekler|
|Azure Data Lake Analytics|[Data Lake Analytics için tanılama günlüklerine erişme](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Denetim<br>İstekler|
|Azure Logic Apps|[Logic Apps B2B özel izleme şeması](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|İş akışı WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch tanılama günlükleri](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Otomasyonu|[Azure otomasyonu için Azure izleme günlükleri](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Event Hubs tanılama günlükleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[İş tanılama günlükleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Yürütme<br>Yazma|
|Azure Service Bus|[Service Bus tanılama günlükleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory raporlama

Azure Active Directory (Azure AD) güvenlik, etkinlik ve Denetim raporları için bir kullanıcının dizini içerir. [Azure AD denetim raporu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) , kullanıcının Azure AD örneğinde oluştu ayrıcalıklı Eylemler belirlemenize yardımcı olur. Ayrıcalıklı Eylemler yükseltme değişiklikler (örneğin, rol oluşturma veya parola sıfırlama), değişen İlkesi yapılandırmalarını (örneğin, parola ilkelerini) veya dizin yapılandırması (örneğin, etki alanında Federasyon ayarlarını değişiklikler) değişiklikleri içerir.

Raporları, olay adı, değişiklik ve tarih ve saat (UTC) tarafından etkilenen hedef kaynak eylemi gerçekleştiren kullanıcı Denetim kaydını sağlar. Kullanıcılar, Azure AD yoluyla için denetim olaylarının listesi alabilirsiniz [Azure portalında](https://portal.azure.com/)anlatılan şekilde [denetim günlüklerinizi görüntülemek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

Dahil edilen raporlar aşağıdaki tabloda listelenmiştir:

| Güvenlik raporları | Etkinlik raporları | Denetim raporları |
| :--------------- | :--------------- | :------------ |
|Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri| Uygulama kullanımı: özet| Dizin denetimi raporu|
|Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri|  Uygulama kullanımı: ayrıntılı||
|Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri|    Uygulama panosu||
|Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri|   Hesap hazırlama hataları||
|Düzensiz oturum açma etkinliği|    Bireysel kullanıcı cihazları||
|Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri|   Bireysel kullanıcı etkinliği||
|Anormal oturum açma etkinliği gösteren kullanıcılar| Grup etkinlik raporu||
||Parola sıfırlama kayıt Etkinlik Raporu||
||Parola sıfırlama etkinliği||

Bu raporlardaki verileri güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleri, Denetim ve iş zekası araçları gibi uygulamalarınız için yararlı olabilir. Azure AD raporlama API'leri, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Bu çağrı [API'leri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) çeşitli programlama dilleri ve araçları.

Azure AD Denetim Raporu olayları 180 gün boyunca saklanır.

> [!Note]
> Rapor saklama hakkında daha fazla bilgi için bkz. [Azure AD rapor saklama ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Uzun, denetim olayları koruma içinde ilgileniyorsanız, düzenli olarak çekmek için raporlama API'sini kullanma [olaylarını denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) ayrı veri deposuna.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Sanal makine günlükleri, Azure Tanılama'yı kullanma

[Azure tanılama](https://docs.microsoft.com/azure/azure-diagnostics) azure'da dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlayan bir özelliktir. Tanılama uzantısını çeşitli kaynaklardan birini kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti web ve çalışan rolleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me).

![Sanal makine günlükleri, Azure Tanılama'yı kullanma](./media/azure-log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineslearnpathsdeploy-a-website-with-azure-virtual-machines-that-are-running-microsoft-windows-and-service-fabrichttpsdocsmicrosoftcomazureservice-fabricservice-fabric-overview"></a>[Azure sanal makineleri](/learn/paths/deploy-a-website-with-azure-virtual-machines/) Microsoft Windows çalıştıran ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)

Bir sanal makinede, aşağıdakilerden birini yaparak Azure tanılamayı etkinleştirebilirsiniz:

* [Azure sanal makinelerini izlemek için Visual Studio'yu kullanın.](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Azure tanılama ayarlama, Azure sanal makinesinde uzaktan ayarlayın](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

* [Azure sanal makineler'de tanılamayı ayarlamak için PowerShell kullanma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Bir Azure Resource Manager şablonu kullanarak izleme ve tanılama özellikli bir Windows sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Depolama Analizi

[Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydeder ve bir depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Depolama analizi günlük kaydı, kullanılabilir [Azure Blob, Azure kuyruk ve Azure tablo Depolama hizmetlerinin](https://docs.microsoft.com/azure/storage/storage-introduction). Depolama analizi, başarılı ve başarısız istekler hakkında ayrıntılı bilgi için bir depolama hizmetine kaydeder.

Bu bilgiler, tek tek istekleri izlemek için ve bir depolama hizmeti ile ilgili sorunları tanılamak için kullanabilirsiniz. Bir en iyi çaba ilkesine göre istekleri günlüğe kaydedilir. Hizmet uç noktasına karşı yapılan istekler varsa, günlük girişi oluşturulur. Örneğin, bir depolama hesabı, blob uç noktası ancak kendi tablo veya kuyruk uç etkinlik varsa, Blob Depolama hizmetine ait günlükleri oluşturulur.

Depolama analizi kullanmak için ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirin. İçinde etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com/). Daha fazla bilgi için [Azure portalında depolama hesabı izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Depolama analizi, her hizmet için ayrı ayrı etkinleştirmek için hizmeti özelliklerini ayarla işlemine kullanın.

Toplanan veriler (günlük için) iyi bilinen bir blob ve tablo depolama hizmeti API'leri ve Blob Depolama hizmetini kullanarak erişebilirsiniz (ölçüm) için iyi bilinen tablolara depolanır.

Depolama analizi, depolama hesabınız için toplam limiti bağımsızdır depolanan veri miktarına 20 terabayt (TB) sınırı vardır. Tüm günlükler depolanan [blok blobları](https://docs.microsoft.com/azure/storage/storage-analytics) $logs adlı bir kapsayıcı içinde otomatik olarak oluşturulan depolama analizi bir depolama hesabı için etkinleştirdiğinizde.

> [!Note]
> * Faturalandırma ve veri saklama ilkeleri hakkında daha fazla bilgi için bkz: [depolama analizi ve faturalandırma](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure depolama ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Depolama analizi, aşağıdaki türde kimliği doğrulanmış ve anonim istekler kaydeder:

| Kimlik doğrulaması  | Anonim|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|Başarısız istek, zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere | Başarılı ve başarısız istekleri dahil olmak üzere bir paylaşılan erişim imzası kullanarak istekleri |
| Başarılı ve başarısız istekleri dahil olmak üzere bir paylaşılan erişim imzası kullanarak istekleri |Hem istemci hem de sunucu zaman aşımı hataları |
|   Analiz veri istekleri |    304 (değiştirilmedi) hata koduyla başarısız GET istekleri |
| Depolama analizi kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi, işlemler ve durum iletileri günlüğe](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama analizi günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi, işlemler ve durum iletileri günlüğe](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama analizi günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure ağ günlükleri

Günlüğe kaydetme ve Azure'da izleme ağ kapsamlı ve iki geniş kategoriye kapsar:

* [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Senaryo tabanlı ağ izleme Ağ İzleyicisi özellikleri ile sağlanır. Bu hizmet içeren paket yakalama, sonraki atlama IP akışı doğrulama, güvenlik grubu görünümü, NSG akış günlükleri. Senaryo düzeyi izleme ağ kaynaklarını tek tek ağ kaynak izleme aksine bir uçtan uca görünümünü sağlar.

* [Kaynak İzleme](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Kaynak düzeyi izleme dört özellikleri, tanılama günlükleri, ölçümler, sorun giderme ve kaynak durumu oluşur. Bu özelliklerin tümü ağ kaynak düzeyinde oluşturulur.

![Azure ağ günlükleri](./media/azure-log-audit/azure-log-audit-fig4.png)

Ağ İzleyicisi, koşulları ağ senaryosu düzeyinde, azure'a veya azure'dan izlemenizi ve tanılamanızı sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme araçları Ağ İzleyicisi ile kullanılabilen anlamanıza, tanılamanıza ve ağınıza azure'da Öngörüler elde etmeye yardımcı olur.

### <a name="network-security-group-flow-logging"></a>Ağ güvenlik grubu akış günlüğe kaydetme

[NSG akış günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) Ağ İzleyicisi, giriş ve çıkış IP trafiğini bir NSG ile ilgili bilgileri görüntülemek için kullanabileceğiniz bir özelliğidir. Bu akış günlüklerini JSON biçiminde yazılır ve Göster:
* Kural başına temelinde giden ve gelen akışlar.
* Akış uygulandığı NIC.
* Akışla ilgili 5 demet bilgi: kaynak veya hedef IP, kaynak veya hedef bağlantı noktası ve protokol.
* Olup trafiklere izin verildiğini veya.

Hedef Nsg akış günlükleri olsa da, bunlar diğer günlükler aynı şekilde gösterilmez. Akış günlükleri yalnızca bir depolama hesabında depolanır.

Akış günlüklerini diğer günlüklerini görülen aynı bekletme ilkeleri uygulayın. Günlükleri ayarlayabileceğiniz 1 günden 365 güne kadar bir bekletme ilkesi vardır. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

**Tanılama günlükleri**

Düzenli ve spontaneous olaylar ağ kaynaklar tarafından oluşturulan ve depolama hesaplarında oturum ve bir olay hub'ı veya Azure İzleyici günlüklerine gönderilir. Günlükleri, kaynak durumu hakkında Öngörüler sağlar. Power BI ve Azure İzleyici günlükleri gibi Araçları'nda görüntülenebilir. Tanılama günlükleri görüntüleme hakkında bilgi edinmek için [Azure İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Tanılama günlükleri](./media/azure-log-audit/azure-log-audit-fig5.png)

Tanılama günlükleri için kullanılabilir [yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), yollar ve [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Ağ İzleyicisi, bir tanılama günlükleri görünümü sağlar. Bu görünüm, günlüğe kaydetme Tanılama'yı destekleyen tüm ağ kaynakları içerir. Bu görünümde, etkinleştirin ve ağ kaynakları kolayca ve hızlı bir şekilde devre dışı bırakın.


Yukarıda açıklanan günlük özelliklere ek olarak Ağ İzleyicisi şu anda aşağıdaki özellikleri içerir:
- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): Çeşitli bağlantılar ve bir kaynak grubundaki ağ kaynakları arasındaki ilişkileri gösteren bir ağ düzeyinde görünümü sağlar.

- [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): Bir sanal makine içine ve dışına paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve zaman ve boyut sınırlaması ayarları gibi ince ayar yapma denetimleri yönlülük sağlar. Paket verileri blob depolama veya yerel diskte depolanan *.cap* dosya biçimi.

- [IP akışı doğrulama](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): Bir paket izin verilip verilmediğini denetler akış bilgileri 5-tuple paket parametrelere (diğer bir deyişle, hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol) bağlı. Paket bir güvenlik grubu tarafından reddedilirse, kural ve paketi reddeden Grup döndürülür.

- [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Yapılandırılmış tüm kullanıcı tanımlı yollar tanılayabilirsiniz. böylece Azure ağ dokusunda yönlendirilen paketler için sonraki atlama belirler.

- [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): Bir VM'de uygulanan etkili ve uygulanan güvenlik kuralları alır.

- [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): Sanal ağ ağ geçitleriniz ve bağlantılarınızı gidermenize yardımcı olur.

- [Ağ aboneliği sınırı](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Ağ kaynak kullanımı sınırlarını karşı görüntülemenizi sağlar.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformlardaki web geliştiricilerine yönelik genişletilebilir bir APM hizmetidir. Canlı web uygulamaları izlemek için kullanın. Bunu, performans anormalliklerini otomatik olarak algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla aslında neler yaptığını anlamanıza yardımcı olan güçlü analiz araçları içerir.

Application Insights, performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak için tasarlanmıştır.

Bu, şirket içinde barındırılan bir geniş çeşitli platformlarda, bunlar ister .NET, Node.js ve Java EE dahil olmak üzere uygulamalar için veya bulutta çalışır. DevOps işleminizle tümleştirilir ve çeşitli geliştirme araçları ile bağlantı noktalarına sahip.

![Application Insights diyagramı](./media/azure-log-audit/azure-log-audit-fig6.png)

Geliştirme takımına yönelik olan Application Insights, uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olur. Şunları izler:

* **Oranları, yanıt süreleri ve hata oranları istek**: Hangi sayfaların günün hangi zamanlarda en popüler olduğunu öğrenmek ve kullanıcılarınızın bulunduğu. En iyi performansı hangi sayfaların gösterdiğini görün. Daha fazla istek olduğunda yanıt süreleriniz ve hata oranları yüksek giderseniz, oranlarınız bir sorun olabilir.

* **Bağımlılık oranları, yanıt süreleri ve hata oranları**: Olup dış hizmetler, yavaşlatmadan öğrenin.

* **Özel durumlar**: Toplu istatistikleri analiz edin veya belirli örnekler seçin ve yığın izlemesi ve ilgili isteklerin ayrıntılarına girmek. Hem sunucu hem de tarayıcı özel durumları raporlanır.

* **Sayfa görüntüleme sayısı ve yükleme performansı**: Kullanıcılarınızın tarayıcılarından raporları alın.

* **AJAX çağrıları**: Web sayfası oranları, yanıt süreleri ve hata oranları alın.

* **Kullanıcı ve oturum sayıları**.

* **Performans sayaçları**: Windows veya Linux sunucu makinelerinizin CPU, bellek ve ağ kullanımı gibi veri alın.

* **Konak tanılama**: Docker veya Azure veri alın.

* **Tanılama izleme günlükleri**: İzleme olayları ile istekler ilişkilendirebilmek, uygulamanızdan veri alın.

* **Özel olaylar ve ölçümler**: Kendiniz satılan öğeler gibi iş olaylarını izlemek için istemci veya sunucu kodu ya da kazanılan yazma verileri elde edersiniz.

Aşağıdaki tabloda, listeler ve tümleştirme senaryoları açıklanmıştır:

| Tümleştirme senaryosu | Açıklama |
| --------------------- | :---------- |
|[Uygulama Haritası](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Uygulamanızın bileşenlerinin yanı sıra önemli ölçüm ve uyarılar.|
|[Tanılama verileri örneği için arama](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| İstekler, özel durumlar, bağımlılık çağrıları, günlük izlemeleri ve sayfa görüntülemeleri gibi olaylarda arama yapın ve bunları filtreleyin.|
|[Toplu veriler için ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/app/metrics-explorer)|İstek, hata ve özel durum oranları; yanıt süreleri, sayfa yükleme süreleri gibi toplu verileri keşfedin, filtreleyin ve bölümlere ayırın.|
|[Panolar](https://docs.microsoft.com/azure/azure-monitor/app/overview-dashboard)|Birden çok kaynaktan toplanan verileri birleştirin ve başkalarıyla paylaşın. Çok bileşenli uygulamalar ve takım odasında sürekli görüntüleme için idealdir.|
|[Canlı ölçümleri Stream](https://docs.microsoft.com/azure/azure-monitor/app/live-stream)|Yeni bir derleme dağıttığınızda, her şeyin beklendiği gibi çalıştığından emin olmak için bu neredeyse gerçek zamanlı performans göstergelerini izleyin.|
|[Analizler](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Bu güçlü sorgulama dilini kullanarak uygulamanızın performansı ve kullanımıyla ilgili zor soruları yanıtlayın.|
|[Otomatik ve el ile uyarılar](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Otomatik uyarılar, uygulamanızın normal telemetri desenlerini uyum ve olduğunda ve normal desenin dışında bir şey tetiklenir. Belirli özel veya standart ölçüm düzeylerinde de uyarılar ayarlayabilirsiniz.|
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Koddaki performans verilerini görüntüleyin. Yığın izlemelerinden koda gidin.|
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Kullanım ölçümlerini diğer iş zekası verileriyle tümleştirin.|
|[REST API](https://dev.applicationinsights.io/)|Ölçümleriniz ve ham verileriniz üzerinde sorgu çalıştırmak için kod yazın.|
|[Sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Depolama için ham verileri dışarı aktarma geldiğinde toplu.|

### <a name="azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarıları

Azure Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağ ve bağlı iş ortağı çözümlerinden güvenlik bilgileri otomatik olarak toplayarak çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur. Daha fazla bilgi için [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro).

![Azure Güvenlik Merkezi diyagramı](./media/azure-log-audit/azure-log-audit-fig7.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri teknolojilerinde uygular ve [makine öğrenimi](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tüm bulut yapısındaki olayları değerlendirmek için teknolojileri. Bu şekilde, elle yaklaşımlar kullanılarak ve saldırıların gelişimi tahmin edilerek belirlenmesi imkansız olan tehditler algılar. Bu güvenlik analizleri şunlardır:

* **Tümleşik tehdit bilgileri**: Microsoft ürünleri ve Hizmetleri, Microsoft dijital Suçlar birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve dış akışların küresel tehdit zekasını uygulayarak bilinen kötü aktörleri arar.

* **Davranış analizi**: Kötü amaçlı davranışları bulmak için bilinen modelleri uygular.

* **anomali algılama**: Geçmiş taban çizgisi oluşturmak için istatistiksel profil oluşturmayı kullanır. Olası bir saldırı vektörüne uygun olan yerleşik taban çizgilerinden sapmalar konusunda uyarır.

Birçok güvenlik işlemleri ve olay yanıt ekiplerinin bir SIEM çözüm üzerinde güvenlik uyarılarının ve önceliklendirmek için başlangıç noktası olarak kullanır. Azure günlük Tümleştirmesi ile Güvenlik Merkezi uyarıları ve sanal makine, Azure tanılama ve Denetim günlükleri, Azure İzleyici günlüklerine veya neredeyse gerçek zamanlı SIEM çözüm tarafından toplanan güvenlik olaylarını eşitleyebilirsiniz.

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Azure İzleyici günlüklerine, azure'da bulut kaynaklar tarafından oluşturulan ve şirket içi Ortamlarınızdaki verileri toplayıp analiz yardımcı olan bir hizmettir. Bu işlem, tüm iş yüklerinizde ve sunucular, fiziksel konumlarından milyonlarca kaydı kolayca analiz etmek için tümleşik arama ve özel panoları kullanarak gerçek zamanlı Öngörüler sağlar.

![Azure İzleyici günlüklerine diyagramı](./media/azure-log-audit/azure-log-audit-fig8.png)

Log Analytics çalışma alanında, Azure'da barındırılan Azure İzleyici merkezi günlükleridir. Azure İzleyici günlüklerine veri kaynakları yapılandırılarak ve aboneliğinize çözümler ekleyerek bağlı kaynaklardan çalışma alanındaki veri toplar. Veri kaynaklarını ve çözümlerini her oluşturun farklı kayıt türleri, her biri kendi özellikler kümesini. Ancak, kaynakları ve çözümleri birlikte çalışma alanına sorgularda analiz edilebilir. Bu özellik, çeşitli çeşitli kaynaklardan toplanan veriler ile çalışmak için aynı araçları ve yöntemleri kullanın olanak tanır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Bağlı kaynaklar, bilgisayarları ve Azure İzleyici günlüklerine tarafından toplanan verileri oluşturan diğer kaynakları ' dir. Kaynakları, yüklü aracıları içerebilir [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) ve [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) doğrudan bağlanan bilgisayarlar veya aracıları [bağlı bir System Center Operations Manager yönetim grubu](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Azure İzleyici günlüklerine Ayrıca verileri toplayın bir [Azure depolama hesabı](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage).

[Veri kaynakları](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) çeşitli bağlı her kaynaktan toplanan veri türleridir. Kaynakları dahil olayları ve [performans verilerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) gelen [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) ve Linux aracılarından gibi kaynakları [IIS günlükler](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) ve [özelmetingünlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs). Toplamak istediğiniz her veri kaynağını yapılandırabilirsiniz. Yapılandırma, otomatik olarak bağlı her kaynağa dağıtılır.

İçin dört yolla [günlükleri ve Azure Hizmetleri için ölçümleri toplamak](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage):

* Azure tanılama doğrudan Azure İzleyici günlüklerine (**tanılama** aşağıdaki tabloda)

* Azure İzleyici için Azure depolama için Azure tanılama günlükleri (**depolama** aşağıdaki tabloda)

* Azure Hizmetleri için bağlayıcılar (**bağlayıcı** aşağıdaki tabloda)

* Toplamak ve daha sonra Azure İzleyici günlüklerine (aşağıdaki tabloda ve listelenmeyen Hizmetleri boş hücreler) içine veri göndermek için komut dosyaları

| Hizmet | Kaynak türü | Günlükler | Ölçümler | Çözüm |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft.Network/<br>applicationGateways|  Tanılama|Tanılama|    [Azure uygulama](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics) [ağ geçidi analizi](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics#azure-application-gateway-analytics-solution-in-azure-monitor)|
|Application Insights||     Bağlayıcı|  Bağlayıcı|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Bağlayıcısı (Önizleme)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Azure Otomasyon hesapları| Microsoft.Automation/<br>AutomationAccounts|    Tanılama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Azure Batch hesapları|  Microsoft.Batch/<br>batchAccounts|  Tanılama|    Tanılama||
|Klasik bulut Hizmetleri||       Depolama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Bilişsel Hizmetler|    Microsoft.CognitiveServices/<br>accounts|       Tanılama|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>accounts|   Tanılama|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Tanılama|||
|Azure olay hub'ı ad alanı| Microsoft.EventHub/<br>Ad alanları|  Tanılama|    Tanılama||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Tanılama||
|Azure Key Vault|   Microsoft.KeyVault/<br>kasaları|  Tanılama  || [Anahtar Kasası Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Azure Load Balancer|   Microsoft.Network/<br>Sonraki|    Tanılama|||
|Azure Logic Apps|  Microsoft.Logic/<br>İş akışları|  Tanılama|    Tanılama||
||Microsoft.Logic/<br>integrationAccounts||||
|Ağ Güvenlik Grupları|   Microsoft.Network/<br>networksecuritygroups|Tanılama||   [Azure ağ güvenlik grubu analizi](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics#azure-application-gateway-and-network-security-group-analytics)|
|Kurtarma kasaları|   Microsoft.RecoveryServices/<br>kasaları|||[Azure kurtarma Hizmetleri analizi (Önizleme)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Hizmet ara|   Microsoft.Search/<br>searchServices|    Tanılama|    Tanılama||
|Service Bus ad alanı| Microsoft.ServiceBus/<br>Ad alanları|    Tanılama|Tanılama|    [Service Bus analizi (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Depolama||    [Service Fabric analizi (Önizleme)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>sunucuları /<br>veritabanları||       Tanılama||
||Microsoft.Sql/<br>sunucuları /<br>elasticPools||||
|Depolama|||         Betik| [Azure depolama analizi (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure sanal makineleri|    Microsoft.Compute/<br>virtualMachines|  Dahili numara|  Dahili numara||
||||Tanılama||
|Sanal makine ölçek kümeleri|    Microsoft.Compute/<br>virtualMachines    ||Tanılama||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtualMachines||||
|Web sunucu grupları|Microsoft.Web/<br>serverfarms||   Tanılama
|Web siteleri|  Microsoft.Web/<br>Siteleri ||      Tanılama|    [Daha fazla bilgi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>Site /<br>yuvaları||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Şirket içi SIEM sistemlerine sahip günlük tümleştirmesi

Azure günlük Tümleştirmesi ile şirket içi SIEM sisteminizi (güvenlik bilgileri ve Olay yönetimi sistemi) ile Azure kaynaklarınızın ham günlükleri tümleştirebilirsiniz. AzLog yüklemeleri, 27 Haziran 2018'de devre dışı bırakıldı. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında rehberlik için [SIEM araçlarla tümleştirmek için kullanım Azure İzleyici](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

![Günlük tümleştirme diyagramı](./media/azure-log-audit/azure-log-audit-fig9.png)

Günlük tümleştirme, Windows sanal makineleri, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure kaynak sağlayıcısı günlükleri Azure tanılama verilerini toplar. Bu tümleştirme, bunlar şirket içinde veya bulutta, araya getirebilirsiniz böylece ilişkilendirin, çözümlemek ve güvenlik olayları için uyarı olup olmadığını, tüm varlıklar için birleştirilmiş bir Pano sağlar.

Windows olay günlükleri, Azure aboneliği, Azure Güvenlik Merkezi uyarıları, Azure tanılama günlükleri ve Azure AD ile Windows sanal makinelerden denetim günlükleri, günlük tümleştirmesi şu anda Azure etkinlik günlüklerini tümleştirmesini destekler.

| Günlük türü | Azure İzleyici (Splunk ArcSight ve IBM QRadar) destekleyen JSON günlüğe kaydeder. |
| :------- | :-------------------------------------------------------- |
|Azure AD denetim günlükleri|   Evet|
|Etkinlik günlükleri| Evet|
|Güvenlik Merkezi uyarıları |Evet|
|Tanılama günlükleri (kaynak günlükleri)|  Evet|
|Sanal makine günlükleri|   Evet, iletilen olaylar aracılığıyla hem de JSON üzerinden değil|

[Azure günlük tümleştirmesini kullanmaya başlama](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started): Bu öğretici, Azure günlük tümleştirmesi yüklenmesinde size kılavuzluk eder ve Azure depolama günlüklerinden tümleştirmek, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure AD denetim günlükleri.

Tümleştirme senaryoları için SIEM:

* [İş ortağı yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Bu blog gönderisini Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesi yapılandırma gösterilmektedir.

* [Azure günlük tümleştirmesi SSS](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq): Bu makalede, Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.

* [Azure günlük Tümleştirmesi ile Güvenlik Merkezi uyarılarını tümleştirme](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration): Bu makalede, Güvenlik Merkezi uyarıları, Azure tanılama günlükleri tarafından toplanan sanal makine güvenlik olaylarını eşitlenecek anlatılmaktadır ve Azure İzleyici günlüklerine ya da SIEM çözümünden ile Azure denetim günlükleri.

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim ve günlük](https://docs.microsoft.com/azure/security/security-management-and-monitoring-overview): Görünürlük koruma ve hızlı bir şekilde zamanında güvenlik uyarılarını yanıtlama verileri koruyun.

- [Azure'da güvenlik günlük kaydı ve denetim günlüğü koleksiyon](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): Azure örneklerinizin doğru güvenlik ve Denetim günlükleri toplamaya emin olmak için bu ayarları uygular.

- [Bir site koleksiyonu için denetim ayarları yapılandırma](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): Bir site koleksiyonu yöneticisi değilseniz, bireysel kullanıcıların eylemlerini geçmişini ve belirli bir tarih aralığı içinde gerçekleştirilen eylemlerin geçmişi alın. 

- [Office 365 güvenlik ve uyumluluk Merkezi'nde denetim günlüğü arama](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): Office 365 güvenlik ve uyumluluk Merkezi'nde birleşik denetim günlüğünde arama yapma ve Office 365 kuruluşunuzdaki kullanıcı ve yönetici etkinliğini görüntülemek için kullanın.


