---
title: Günlüğe kaydetme ve denetim azure | Microsoft Docs
description: Nasıl verileri günlüğe kaydetmeye uygulamanız hakkında ayrıntılı Öngörüler elde etmek için kullanabileceğiniz hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: e4144ca0d87abda3d9f8de47e56af59d0e4af312
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938375"
---
# <a name="azure-logging-and-auditing"></a>Azure günlük kaydı ve denetim

Azure çok çeşitli denetim ve güvenlik ilkeleri ve mekanizmaları kapsamın açıklarını tanımlamanıza yardımcı olması için seçenekleri günlük yapılandırılabilir güvenlik sağlar. Bu makalede, oluşturma, toplama ve güvenlik günlüklerini Azure üzerinde barındırılan hizmetlerden analiz anlatılmaktadır.

> [!Note]
> Bu makaledeki bazı öneriler artan veri, ağ ya da işlem kaynağı kullanımına neden ve lisans ya da abonelik maliyetlerinizi artırabilir.

## <a name="types-of-logs-in-azure"></a>Azure günlükleri türleri
Çok sayıda taşıma bölümleriyle bulut uygulamalarını karmaşıktır. Günlükleri uygulamalarınızı çalışır durumda kalmasına yardımcı olmak için veri sağlar. Günlükler sorunlarını giderme veya olası olanları önlenmesine yardımcı olur. Ve uygulama performansı veya bakım artırmak ya da aksi takdirde el ile müdahale gerektiren Eylemler otomatikleştirmek yardımcı olabilir.

Azure günlükleri aşağıdaki türlerine ayrılır:
* **Denetim/Yönetim günlüklerini** Azure Kaynak Yöneticisi oluşturma, güncelleştirme ve silme işlemleri hakkında bilgi sağlar. Daha fazla bilgi için bkz: [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

* **Veri günlüklerini düzlemine** bölümü Azure kaynak kullanım olarak başlatılan olayları hakkında bilgi sağlar. Bu günlük türü örnekler güvenlik, Windows olay sistem ve uygulama günlüklerini bir sanal makine (VM) ve [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) Azure İzleyicisi aracılığıyla yapılandırılır.

* **İşlenen olayların** sizin adınıza çözümlenen olaylar/işlenen Uyarıları hakkında bilgi sağlar. Bu tür örnekleri [Azure Güvenlik Merkezi uyarılarını](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) nerede [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) işlenir ve aboneliğinizi analiz ve kısa güvenlik uyarıları sağlar.

Aşağıdaki tabloda en önemli Azure'da kullanılabilen günlük türlerini listeler:

| Günlük kategorisi | Günlük türü | Kullanım | Tümleştirme |
| ------------ | -------- | ------ | ----------- |
|[Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Azure Resource Manager kaynaklarını denetim düzlemi olayları|   Aboneliğinizi kaynaklarında gerçekleştirilen işlemler hakkında bilgi sağlar.|    REST API, [Azure İzleyicisi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Azure Resource Manager kaynaklarını Abonelikteki işlemi hakkında sık veriler|    Kaynak gerçekleştirilen işlemler hakkında bilgi sağlar.| Azure İzleyici [akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Azure AD raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Günlüklerini ve raporları | Kullanıcı oturum açma etkinlikleri ve sistem etkinlik bilgileri kullanıcı ve Grup Yönetimi hakkında raporlar.|[Graph API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Sanal makineler ve bulut Hizmetleri](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-collect-azurevm)|Windows olay günlüğü hizmeti ve Linux Syslog|    Sistem verileri ve sanal makinelerde günlük verilerini yakalar ve bu verileri tercih ettiğiniz bir depolama hesabına aktarır.|   Windows (Windows Azure Tanılama'yı kullanarak [[WAD](https://docs.microsoft.com/azure/azure-diagnostics)] depolama) ve Linux Azure İzleyicisi|
|[Azure depolama çözümlemeleri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Depolama günlüğe kaydetme için bir depolama hesabı ölçüm verileri sağlar|İzleme istekleri bir anlayış sağlar, kullanım eğilimlerini analiz eder ve depolama hesabınız ile ilgili sorunları tanılar.|   REST API veya [istemci kitaplığı](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Ağ güvenlik grubu (NSG) akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON biçimi, bir kural başına temelinde giden ve gelen akışları gösterir|Giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgiler görüntüler.|[Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Uygulama Insight](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Günlükleri, özel durumlar ve özel tanılama|   Bir uygulama performansı izleme (APM) hizmeti birden çok platformdaki web geliştiricileri için sağlar.| REST API [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Veri işleme / Güvenlik Uyarıları|    Azure Güvenlik Merkezi uyarılarını, Azure günlük analizi uyarıları|   Güvenlik bilgileri ve uyarılar sağlar.|  REST API'leri, JSON|

### <a name="activity-logs"></a>Etkinlik günlükleri
[Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri eski adıyla "denetim günlüklerini" veya "işlem günlükleri," olarak bunlar raporladığından [denetim düzlemi olayları](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) aboneliklerinizi için. 

Etkinlik günlükleri saptamanıza yardım "ne, kimin, ne zaman ve" (diğer bir deyişle, PUT, POST veya DELETE) yazma işlemleri için. Etkinlik durumunu işlemi ve ilgili diğer özellikleri anlamanıza yardım de kaydeder. Etkinlik günlükleri okuma (GET) işlemleri dahil etmeyin.

Bu makalede, PUT, POST ve DELETE bir etkinlik günlüğü kaynakları içeren tüm yazma işlemlerini bakın. Örneğin, etkinlik günlükleri sorunlarını giderirken hata bulmak için veya bir kullanıcı, kuruluşunuzdaki bir kaynak nasıl değişiklik izlemek için kullanabilirsiniz.

![Etkinlik günlüğü diyagramı](./media/azure-log-audit/azure-log-audit-fig1.png)

Azure portal kullanarak bir etkinlik günlüğünden olayları alabilirsiniz [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell cmdlet'leri ve [Azure İzleyici REST API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Etkinlik günlükleri 19 günlük veri saklama süresi vardır.

Tümleştirme senaryolarına bir etkinlik günlüğü olayı için:

* [Bir etkinlik günlüğü olayı tarafından tetiklenen bir e-posta veya Web kancası uyarı oluşturma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email).

* [Olay hub'ına akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.

* Kullanarak Powerbı çözümlemek [Power BI içerik paketi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Arşiv veya el ile İnceleme için bir depolama hesabı kaydetmeye](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log). Günlük profilleri kullanılarak bekletme süresi (gün) cinsinden belirtebilirsiniz.

* Sorgulamak ve Azure portalında görüntüleyin.

* PowerShell cmdlet, Azure CLI veya REST API sorgu.

* Etkinlik günlüğü için günlük profilleriyle verme [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Bir depolama hesabı kullanabilir veya [olay hub'ı ad](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) olmayan günlük yayma biri ile aynı abonelikte. Aktaranın ayarı yapılandırır uygun olmalıdır [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) her iki aboneliğin erişimi.

### <a name="azure-diagnostics-logs"></a>Azure tanılama günlükleri
Azure tanılama günlükleri, bu kaynakla ilgili zengin, sık sık veri sağlayan bir kaynak tarafından gösterilen. Bu günlükler içeriğini kaynak türüne göre değişir. Örneğin, [Windows olayı sistem günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) VM'ler için tanılama günlükleri kategorisine olan ve [blob, tablo ve kuyruk günlükleri](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) depolama hesapları için tanılama günlükleri kategori. Tanılama günlüklerini aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlayan etkinlik günlükleri farklıdır.

![Diyagramları Azure tanılama günlükleri](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure tanılama günlükleri, Azure portalını, PowerShell'i, Azure CLI ve REST API gibi birden çok yapılandırma seçenekleri sunar.

**Tümleştirme senaryolarına**

* Bunları kaydetmek bir [depolama hesabı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) denetim veya el ile İnceleme için. Tanılama ayarları kullanarak bekletme süresi (gün) cinsinden belirtebilirsiniz.

* [Event hubs'a akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) bir üçüncü taraf hizmeti veya özel analiz çözümü tarafından alımı için gibi [Powerbı](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Bunları ile analiz [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

**Desteklenen hizmetler, tanılama günlüklerini ve kaynak türü başına desteklenen bir günlük kategoriler şeması**


| Hizmet | Şema ve belgeleri | Kaynak türü | Kategori |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Yük Dengeleyici (Önizleme) için günlük analizi](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers| LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Ağ Güvenlik Grupları|[Ağ güvenlik grupları için günlük analizi](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Uygulama ağ geçidi için tanılama günlükleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Anahtar kasası günlükleri](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Etkinleştirme ve arama trafiği Analytics kullanma](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Data Lake Store erişim tanılama günlükleri](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Denetim<br>İstekler|
|Azure Data Lake Analytics|[Data Lake Analytics erişim tanılama günlükleri](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Denetim<br>İstekler|
|Azure Logic Apps|[Logic Apps B2B özel izleme şeması](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|İş akışı WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch tanılama günlükleri](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Otomasyonu|[Azure otomasyonu için günlük analizi](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Olay hub'ları tanılama günlükleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[İş tanılama günlükleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Yürütme<br>Yazma|
|Azure Service Bus|[Hizmet veri yolu tanılama günlükleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory raporlama
Azure Active Directory (Azure AD), güvenlik, etkinlik ve bir kullanıcının dizini için Denetim raporları içerir. [Azure AD denetim rapor](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) kullanıcının Azure AD örneğinde oluştu ayrıcalıklı Eylemler belirlemenize yardımcı olur. Ayrıcalıklı Eylemler ayrıcalık değişiklikler (örneğin, rolü oluşturma veya parola sıfırlama), değişen ilke yapılandırmaları (örneğin, parola ilkeleri) veya dizin yapılandırması (örneğin, etki alanı Federasyon ayarlarında yapılan değişiklikler) değişiklikleri içerir.

Raporlar, olay adı için değişiklik ve tarih ve saat (UTC) etkilenen hedef kaynak eylem gerçekleştiren kullanıcı Denetim kaydını sağlar. Kullanıcıların Azure AD için denetim olayları listesini alabilir [Azure portal](https://portal.azure.com/)açıklandığı gibi [denetim günlüklerini görüntülemek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

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
||Parola sıfırlama etkinliği|||

Bu raporlardaki verileri güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleri, Denetim ve iş zekası araçları gibi uygulamalarınıza yararlı olabilir. Azure AD raporlama API'leri, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Bu çağrı [API'leri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) çeşitli programlama dilleri ve araçları.

Azure AD Denetim Raporu olayları 180 gün boyunca saklanır.

> [!Note]
> Rapor bekletme hakkında daha fazla bilgi için bkz: [Azure AD rapor bekletme ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Uzun, denetim olayları korunuyor düşünüyorsanız, düzenli olarak çekmesini raporlama API'si kullanmak [olaylarını denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) ayrı veri deposuna.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Azure Tanılama'yı sanal makine günlükleri
[Azure tanılama](https://docs.microsoft.com/azure/azure-diagnostics) dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir özelliktir. Tanılama uzantısını çeşitli kaynaklardan birini kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti web ve çalışan rolleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me).

![Azure Tanılama'yı sanal makine günlükleri](./media/azure-log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineshttpsazuremicrosoftcomdocumentationlearning-pathsvirtual-machines-that-are-running-microsoft-windows-and-service-fabrichttpsdocsmicrosoftcomazureservice-fabricservice-fabric-overview"></a>[Azure sanal makineleri](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) Microsoft Windows çalıştıran ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)

Aşağıdakilerden birini yaparak, bir sanal makinede Azure tanılama etkinleştirebilirsiniz:

* [Azure sanal makinelerini izlemek için Visual Studio'yu kullanma](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Azure tanılama uzaktan bir Azure sanal makinede ayarlayın](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

* [Azure sanal makinelerde tanılama ayarlamak için PowerShell kullanın](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Bir Azure Resource Manager şablonu kullanarak bir Windows sanal makine izleme ve Tanılama ile oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Depolama Analizi
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydeder ve bir depolama hesabı için ölçüm verilerini sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Storage Analytics günlüğe kaydetme için kullanılabilir [Azure Blob, Azure kuyruk ve Azure Table depolama hizmetleri](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgi günlüğe kaydeder.

Bu bilgiler, istekleri ayrı ayrı izlemek ve depolama hizmeti ile ilgili sorunları tanılamak için kullanabilirsiniz. İstekleri en iyi çaba ilkesine göre günlüğe kaydedilir. Hizmet uç noktası karşı yapılan istekleri varsa günlük girişleri oluşturulur. Örneğin, bir depolama hesabı blob uç ancak kendi tablo veya kuyruğu uç noktaları etkinlik varsa, Blob Depolama hizmetine ait günlükler oluşturulur.

Storage Analytics kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirin. İçinde etkinleştirebilirsiniz [Azure portal](https://portal.azure.com/). Daha fazla bilgi için bkz: [Azure portalında bir depolama hesabını izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Storage Analytics REST API veya istemci kitaplığı yoluyla programlı olarak etkinleştirebilirsiniz. Storage Analytics her hizmet için ayrı ayrı etkinleştirmek için hizmet özelliklerini ayarlama işlemi kullanın.

Toplanan veriler (günlük için) iyi bilinen bir blob ve Blob Depolama hizmetinin ve tablo depolama hizmeti API'lerini kullanarak erişebilirsiniz (için ölçümleri), iyi bilinen tablolara depolanır.

Storage Analytics, depolama hesabınız için toplam sınırı bağımsızdır depolanan veri miktarına 20 terabayt (TB) sınıra sahiptir. Tüm günlükler depolanmış [blok blobları](https://docs.microsoft.com/azure/storage/storage-analytics) $logs adlı bir kapsayıcı içinde otomatik olarak oluşturulan Storage Analytics bir depolama hesabı için etkinleştirdiğinizde.

> [!Note]
> * Faturalama ve veri bekletme ilkeleri hakkında daha fazla bilgi için bkz: [depolama çözümlemeleri ve faturalama](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Storage Analytics kimliği doğrulanmış ve anonim istek aşağıdaki türlerini kaydeder:

| Kimliği Doğrulandı  | Anonim|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu | Başarılı ve başarısız istekleri dahil olmak üzere bir paylaşılan erişim imzası kullanarak istekleri |
| Başarılı ve başarısız istekleri dahil olmak üzere bir paylaşılan erişim imzası kullanarak istekleri |İstemci ve sunucu için zaman aşımı hataları |
|   Analiz verileri istekleri |    304 (değişiklik) hata koduyla başarısız olan GET istekleri |
| Storage Analytics kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama çözümlemeleri işlemler ve durum iletilerini günlüğe](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama çözümlemeleri günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama çözümlemeleri işlemler ve durum iletilerini günlüğe](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama çözümlemeleri günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure ağ günlükleri
Günlüğe kaydetme ve Azure'da izleme ağ kapsamlı ve iki geniş kategorisi kapsar:

* [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher): Senaryo tabanlı ağ izleme Ağ İzleyicisi'deki özelliklerle sağlanır. Bu hizmet içeren paket yakalama, sonraki atlama IP akış, güvenlik grubu görünümü, NSG akış günlükleri doğrulayın. Senaryo düzeyi izleme kaynak tek tek ağ izleme aksine ağ kaynaklarına bir uçtan uca görünümünü sağlar.

* [Kaynak İzleme](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring): kaynak düzeyi izleme dört özellikleri, tanılama günlükleri, ölçümleri, sorun giderme ve kaynak durumu oluşur. Bu özelliklerin tümü ağ kaynak düzeyinde oluşturulur.

![Azure ağ günlükleri](./media/azure-log-audit/azure-log-audit-fig4.png)

Ağ İzleyicisi İzleme ve koşullar bir ağ düzeyinde senaryo içinde gelen ve giden Azure tanılama sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

### <a name="network-security-group-flow-logging"></a>Ağ güvenlik grubu akışı günlüğe kaydetme

[NSG akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) Ağ İzleyicisi, bir NSG giriş ve çıkış IP trafiğinin hakkındaki bilgileri görüntülemek için kullanabileceğiniz bir özelliğidir. Bu akış günlükleri, JSON biçiminde yazılmıştır ve Göster:
* Giden ve gelen akış kuralı başına temelinde.
* Akış uygulandığı NIC.
* Akış 5-tanımlama grubu bilgilerini: kaynak veya hedef IP, kaynak veya hedef bağlantı noktası ve protokolü.
* Olup trafiğe izin verilen veya reddedilen.

Hedef Nsg'ler akış günlükleri karşın, bunların diğer günlükler aynı şekilde görüntülenmez. Akış günlükleri yalnızca bir depolama hesabında depolanır.

Diğer açtığında görülen aynı bekletme ilkeleri, akış günlüklerine uygulanır. Günlükleri 365 gün sayısı 1 günden ayarlayabileceğiniz bir bekletme ilkesi vardır. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

**Tanılama günlükleri**

Düzenli ve spontaneous olayları ağ kaynakları tarafından oluşturulan ve depolama hesaplarında oturum ve bir olay hub'ı veya günlük analizi için gönderilir. Günlükleri, bir kaynak sistem durumu fikir sağlar. Power BI ve günlük analizi gibi Araçları'nda görüntülenebilir. Tanılama günlüklerini görüntülemek öğrenmek için bkz: [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Tanılama günlükleri](./media/azure-log-audit/azure-log-audit-fig5.png)

Tanılama günlükleri için kullanılabilir [yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), yollar ve [uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Ağ İzleyicisi görünümü bir tanılama günlüklerini sağlar. Bu görünüm Tanılama Günlüğü destekleyen tüm ağ kaynaklarını içerir. Bu görünümden etkinleştirin ve ağ kaynaklarını kolayca ve hızlı bir şekilde devre dışı bırakın.


Yukarıda açıklanan günlüğe kaydetme özellikleri yanı sıra Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:
- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): çeşitli bağlantılar ve ağ kaynakları bir kaynak grubunda arasındaki ilişkileri gösteren bir ağ düzeyinde görünümü sağlar.

- [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): bir sanal makine ve paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve zaman ve boyutu sınırlaması ayarları gibi ince ayar yapma denetimleri yönlülük sağlar. Paket verileri blob Mağazası'nda veya yerel diskte depolanan *.cap* dosya biçimi.

* [IP akış doğrulama](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): bir paket izin verilip verilmediğini görmek için denetimleri temel akış bilgi 5-tanımlama grubu paket parametrelerinin üzerinde (diğer bir deyişle, hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol). Paketi bir güvenlik grubu tarafından reddedilirse kural ve Paket reddedildi grubun döndürülür.

* [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): kullanıcı tanımlı yollar herhangi tanılamak böylece Azure ağ yapısında yönlendirilen paketleri yanlış için sonraki atlama belirler.

* [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.

* [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): sanal ağ geçitleri ve ağ bağlantıları gidermenize yardımcı olur.

* [Ağ abonelik sınırları](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits): ağ kaynak kullanımı sınırları karşı görüntülemenizi sağlar.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformdaki web geliştiricileri için genişletilebilir bir APM hizmetidir. Bunu, dinamik web uygulamaları izlemek için kullanabilirsiniz. Ayrıca performans anormalliklerini otomatik olarak algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla aslında neler yaptığını anlamanıza yardımcı olan güçlü analiz araçları içerir.

Application Insights sürekli performansını ve kullanımını geliştirmemize yardımcı olmak için tasarlanmıştır.

Bu, çok çeşitli platformlar oldukları olup olmadığını .NET, Node.js ve J2EE, dahil olmak üzere, uygulamaları şirket içi barındırılan veya bulutta çalışır. DevOps işleminizi ile tümleşir ve çeşitli geliştirme araçları ile bağlantı noktalarını içerir.

![Uygulama Öngörüler diyagramı](./media/azure-log-audit/azure-log-audit-fig6.png)

Geliştirme takımına yönelik olan Application Insights, uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olur. Şunları izler:

* **İstek oranları, yanıt sürelerini ve başarısızlık oranları**: hangi sayfaların, günün hangi zamanlarında en popüler olduğunu bulmak ve kullanıcılarınızın bulunduğu. En iyi performansı hangi sayfaların gösterdiğini görün. Daha fazla istekleri olduğunda yanıt sürelerini ve başarısızlık oranları yüksek giderseniz, resourcing bir sorun olabilir.

* **Bağımlılık oranları, yanıt sürelerini ve başarısızlık oranları**: olup dış hizmetler, yavaşlamadan öğrenin.

* **Özel durumlar**: Toplu istatistikler çözümlemek veya belirli örnekleri seçin ve yığın izleme ve ilgili istekleri ayrıntıya gidin. Hem sunucu hem de tarayıcı özel durumları raporlanır.

* **Sayfa görünümleri ve yük performans**: kullanıcılarınızın tarayıcılarından raporlar alın.

* **AJAX çağrıları**: Web sayfası oranları, yanıt sürelerini ve başarısızlık oranları Al.

* **Kullanıcı ve oturum sayıları**.

* **Performans sayaçları**: Windows veya Linux sunucusu makinelerinizden, CPU, bellek, gibi veri alma ve ağ kullanımı.

* **Konak tanılama**: Docker ya da Azure veri alın.

* **Tanılama izleme günlüklerine**: izleme olayları istekleriyle ilişkilendirebilirsiniz böylece veri, uygulamanızdan alma.

* **Özel olayları ve ölçümleri**: kendiniz öğeleri satışı gibi iş olaylarını izlemek için istemci veya sunucu kodu ya da Kazanılan oyun yazma veri alma.

Aşağıdaki tabloda listelenmekte ve tümleştirme senaryolarına açıklanmaktadır:

| Tümleştirme senaryosu | Açıklama |
| --------------------- | :---------- |
|[Uygulama eşlemesi](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Uygulamanızın bileşenlerinin yanı sıra önemli ölçüm ve uyarılar.||
|[Tanılama veri örneği için arama](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| İstekler, özel durumlar, bağımlılık çağrıları, günlük izlemeleri ve sayfa görüntülemeleri gibi olaylarda arama yapın ve bunları filtreleyin.||
|[Toplanan veriler için ölçüm Gezgini](https://docs.microsoft.com/azure/application-insights/app-insights-metrics-explorer)|İstek, hata ve özel durum oranları; yanıt süreleri, sayfa yükleme süreleri gibi toplu verileri keşfedin, filtreleyin ve bölümlere ayırın.||
|[Panolar](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards#dashboards)|Birden çok kaynaktan toplanan verileri birleştirin ve başkalarıyla paylaşın. Çok bileşenli uygulamalar ve takım odasında sürekli görüntüleme için idealdir.||
|[Canlı ölçümleri akış](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)|Yeni bir derleme dağıttığınızda, her şeyin beklendiği gibi çalıştığından emin olmak için bu neredeyse gerçek zamanlı performans göstergelerini izleyin.||
|[Analizler](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Bu güçlü sorgulama dilini kullanarak uygulamanızın performansı ve kullanımıyla ilgili zor soruları yanıtlayın.||
|[Otomatik ve el ile uyarıları](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Otomatik uyarılar, uygulamanızın normal telemetri desenlerini uyum ve olduğunda normal düzeni dışında bir şey tetiklenir. Belirli özel veya standart ölçüm düzeylerinde de uyarılar ayarlayabilirsiniz.||
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Performans verileri kodda görüntüleyin. Yığın izlemelerinden koda gidin.||
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Kullanım ölçümlerini diğer iş zekası verileriyle tümleştirin.||
|[REST API](https://dev.applicationinsights.io/)|Ölçümleriniz ve ham verileriniz üzerinde sorgu çalıştırmak için kod yazın.||
|[Sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Ham verileri, geldiğinde, depolama toplu verme.||

### <a name="azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarıları
Azure Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağ ve bağlı iş ortağı çözümlerinden güvenlik bilgileri otomatik olarak toplayarak çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro).

![Azure Güvenlik Merkezi diyagramı](./media/azure-log-audit/azure-log-audit-fig7.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri sıçramalar uygular ve [makine öğrenme](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tüm bulut yapısındaki olayları değerlendirmek için teknolojileri. Bu şekilde, elle yaklaşımlar kullanılarak ve saldırıların gelişimi tanımlamak imkansız olan tehditler algılar. Bu güvenlik analizleri şunlardır:

* **Tümleşik tehdit bilgileri**: Microsoft ürünleri ve Hizmetleri, Microsoft dijital Suçlar birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve dış akışların genel tehdit bilgisine uygulayarak bilinen kötü aktörleri arar.

* **Davranış analizi**: kötü amaçlı davranışları bulmak için bilinen modelleri uygular.

* **Anomali algılama**: geçmiş taban çizgisi oluşturmak için istatistiksel profil oluşturmayı kullanır. Olası bir saldırı vektörüne uygun olan yerleşik taban çizgilerinden sapmalar konusunda uyarır.

Çok sayıda güvenlik işlemleri ve olay yanıtı takımlar SIEM çözüm üzerinde önceliklendirmek ve güvenlik uyarıları İnceleme için başlangıç noktası olarak kullanır. Azure günlük Tümleştirmesi ile Güvenlik Merkezi uyarılarını ve sanal makine güvenlik olayları, Azure tanılama ve Denetim günlükleri, yakın gerçek zamanlı günlük analizi veya SIEM çözümünüzü ile tarafından toplanan eşitleyebilirsiniz.

## <a name="log-analytics"></a>Log Analytics 

Günlük analizi toplamak ve bulut kaynakları tarafından oluşturulan ve şirket içi ortamları verileri çözümlemek yardımcı olan Azure bir hizmettir. Bu yedeğe tüm iş yükleri ve fiziksel konumlarından sunucular arasında milyonlarca kayıt çözümlemek için tümleşik arama ve özel panolar kullanarak gerçek zamanlı bilgiler verir.

![Günlük analizi diyagramı](./media/azure-log-audit/azure-log-audit-fig8.png)

Center, günlük analizi adresindeki Azure üzerinde barındırılan günlük analizi çalışma alanıdır. Günlük analizi çalışma alanındaki bağlı kaynaklardan veri kaynaklarını yapılandırmak ve çözümleri aboneliğinize ekleyerek toplar. Veri kaynakları ve her oluşturduğunuz farklı çözümler kayıt türleri, her biri kendi özellikler kümesi. Ancak kaynakları ve çözümleri hala birlikte çalışma alanına sorgularda çözümlenebilir. Bu özellik, çeşitli kaynakları tarafından toplanan verileri çeşitli çalışmak için aynı araçları ve yöntemleri kullanmanıza olanak sağlar.

Bağlı kaynakları bilgisayarları ve günlük analizi tarafından toplanan verileri oluşturmak diğer kaynakları ' dir. Kaynakları yüklenen aracılar dahil edebileceğiniz [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) ve [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) doğrudan bağlanan bilgisayarlar ya da aracıları [bağlı bir System Center Operations Manager yönetim grubu](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Günlük analizi de verileri toplayan bir [Azure depolama hesabı](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage).

[Veri kaynakları](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) çeşitli bağlı her kaynaktan toplanan veri türleridir. Kaynakları dahil olayları ve [performans verileri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) gelen [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) ve kaynakları gibi ek olarak Linux aracılarını [IIS günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) ve [özel metin günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs). Toplamak istediğiniz her veri kaynağını yapılandırabilirsiniz. Yapılandırma, otomatik olarak bağlı her kaynağa dağıtılır.

Dört yolla [günlüklerini ve Azure Hizmetleri için ölçümleri toplamak](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage):
* Azure tanılama yönlendirmek için günlük analizi (**tanılama** aşağıdaki tabloda)

* Azure günlük analizi depolama için Azure tanılama (**depolama** aşağıdaki tabloda)

* Azure Hizmetleri için bağlayıcıları (**bağlayıcı** aşağıdaki tabloda)

* Toplamak ve günlük analizi (aşağıdaki tabloda ve için listelenmeyen Hizmetleri boş hücreler) içine veri göndermek için komut dosyaları

| Hizmet | Kaynak türü | Günlükler | Ölçümler | Çözüm |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft.Network/<br>applicationGateways|  Tanılama|Tanılama|    [Azure uygulama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [ağ geçidi analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application Insights||     Bağlayıcı|  Bağlayıcı|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Bağlayıcısı (Önizleme)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Azure Automation hesapları| Microsoft.Automation/<br>AutomationAccounts|    Tanılama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Azure Batch hesapları|  Microsoft.Batch/<br>batchAccounts|  Tanılama|    Tanılama||
|Klasik bulut Hizmetleri||       Depolama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Bilişsel Hizmetler|    Microsoft.CognitiveServices/<br>accounts|       Tanılama|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>accounts|   Tanılama|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Tanılama|||
|Azure Event Hub'ad alanı| Microsoft.EventHub/<br>ad alanları|  Tanılama|    Tanılama||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Tanılama||
|Azure Key Vault|   Microsoft.KeyVault/<br>kasaları|  Tanılama  || [Anahtar Kasası Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Azure Load Balancer|   Microsoft.Network/<br>loadBalancers|    Tanılama|||
|Azure Logic Apps|  Microsoft.Logic/<br>İş akışları|  Tanılama|    Tanılama||
||Microsoft.Logic/<br>integrationAccounts||||
|Ağ Güvenlik Grupları|   Microsoft.Network/<br>networksecuritygroups|Tanılama||   [Azure ağ güvenlik grubu analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Kurtarma kasaları|   Microsoft.RecoveryServices/<br>kasaları|||[Analytics (Önizleme) Azure kurtarma Hizmetleri](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Hizmet ara|   Microsoft.Search/<br>searchServices|    Tanılama|    Tanılama||
|Service Bus ad alanı| Microsoft.ServiceBus/<br>ad alanları|    Tanılama|Tanılama|    [Hizmet veri yolu Analytics (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Depolama||    [Service Fabric Analytics (Önizleme)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>sunucuları /<br>veritabanları||       Tanılama||
||Microsoft.Sql/<br>sunucuları /<br>elasticPools||||
|Depolama|||         Betik| [Azure depolama çözümlemeleri (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure Sanal Makineler|    Microsoft.Compute/<br>virtualMachines|  Dahili numara|  Dahili numara||
||||Tanılama||
|Sanal makine ölçek kümeleri|    Microsoft.Compute/<br>virtualMachines    ||Tanılama||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtualMachines||||
|Web sunucu grupları|Microsoft.Web/<br>ServerFarm öğesine verilir||   Tanılama
|Web Siteleri|  Microsoft.Web/<br>siteler ||      Tanılama|    [Daha fazla bilgi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>siteleri /<br>yuvaları|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Şirket içi SIEM sistemleriyle günlük tümleştirme
İle [Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324), Azure kaynaklarınızdan ham günlükleri ile şirket içi SIEM sisteminizi tümleştirebilirsiniz.

![Günlük tümleştirme diyagramı](./media/azure-log-audit/azure-log-audit-fig9.png)

Günlük tümleştirme Azure Tanılama'yı, Windows sanal makineler, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını ve Azure kaynak sağlayıcısı günlüklerine toplar. Bunlar şirket içi olduğunuz ister bulutta araya getirebilirsiniz böylece ilişkilendirmek, çözümlemek ve güvenlik olayları için uyarı Bu tümleştirme birleştirilmiş bir pano, tüm varlıklar için sunar.

Windows olay günlüklerini Azure abonelik, Azure Güvenlik Merkezi uyarılarını, Azure tanılama günlüklerini ve Azure AD ile Windows sanal makinelerden denetim günlükleri, günlük tümleştirme şu anda Azure etkinlik günlüğü tümleştirmeyi destekler.

| Günlük türü | Günlük analizi (Splunk, ArcSight ve IBM QRadar) JSON destekleme |
| :------- | :-------------------------------------------------------- |
|Azure AD denetim günlüklerini|   Evet|
|Etkinlik günlükleri| Evet|
|Güvenlik Merkezi uyarıları |Evet|
|Tanılama (kaynak) günlükleri|  Evet|
|VM günlükleri|   Evet, iletilen olaylar aracılığıyla ve JSON üzerinden değil|

[Azure günlük tümleştirme ile çalışmaya başlama](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started): Bu öğretici, Azure günlük tümleştirme yüklenmesinde size yol gösterir ve Azure depolama günlüklerinden tümleştirme, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını ve Azure AD denetim günlükleri.

Tümleştirme senaryolarına SIEM için:

* [Ortak yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Bu blog gönderisine Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesini yapılandırma gösterilmektedir.

* [Azure günlük tümleştirme SSS](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq): Bu makalede Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.

* [Güvenlik Merkezi uyarılarını Azure günlük tümleştirme ile tümleştirme](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration): Bu makalede Güvenlik Merkezi uyarılarını, sanal makine güvenlik olaylarını Azure tanılama günlükleri tarafından toplanan eşitlemenin nasıl ele ve Azure denetim günlükleri için günlük analizi veya SIEM çözümü.

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim ve günlük](https://docs.microsoft.com/azure/security/security-management-and-monitoring-overview): Görünürlük koruma ve hızlı bir şekilde zamanında güvenlik uyarılarını yanıt tarafından veri koruma.

- [Güvenlik günlüğü ve denetim günlüğü koleksiyonu Azure içinde](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): Azure örneklerinizi doğru güvenlik ve Denetim günlükleri topluyorsunuz emin olmak için bu ayarları zorlamak.

- [Bir site koleksiyonu denetim ayarlarını yapılandırmak](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): bir site koleksiyonu yöneticisi değilseniz, bireysel kullanıcılar eylemlerin geçmişini ve belirli bir tarih aralığı içinde yapılan Eylemler geçmişini almak. 

- [Office 365 güvenlik ve Uyumluluk Merkezi Denetim günlüğü arama](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): Office 365 güvenlik ve Uyumluluk Merkezi birleşik denetim günlüğü aramak ve Office 365 kuruluşunuzdaki kullanıcı ve yönetici etkinliğini görüntülemek için kullanın.


