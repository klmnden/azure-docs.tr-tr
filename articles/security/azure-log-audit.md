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
ms.openlocfilehash: 2b8b5095fceaa369ae8b7a426ca04685c2d86109
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057946"
---
# <a name="azure-logging-and-auditing"></a>Azure günlüğe kaydetme ve denetleme
## <a name="introduction"></a>Giriş
### <a name="overview"></a>Genel Bakış
Anlama ve çeşitli güvenlikle ilgili özellikleri bulunan kullanarak ve Azure platformu çevreleyen geçerli ve gelecekteki Azure müşterilere yardımcı olmak için Microsoft teknik incelemeler, güvenlik genel bakışlar, en iyi yöntemler ve denetim listeleri bir dizi geliştirmiştir. Konular avantajlarına ve derinliği bakımından aralığı ve düzenli aralıklarla güncelleştirilir. Bu belge aşağıdaki soyut bölümünde özetlenen serisi bir parçası değil.
### <a name="azure-platform"></a>Azure platformu
Azure işletim sistemlerinin programlama dilleri, çerçeveleri, Araçlar, veritabanları ve cihazları, en geniş seçim destekleyen bir açık ve esnek bir bulut hizmeti platformudur.

Örneğin, şunları yapabilirsiniz:
-   Linux kapsayıcıları Docker Tümleştirmesi ile çalıştırın.

-   JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma

-   Yapı geri-iOS, Android ve Windows cihazları sona erer.

Azure genel bulut Hizmetleri aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven.

Oluşturmanıza veya BT varlıklar için geçiş, bir bulut sağlayıcısı uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızı güvenliği yönetmek için sağladıkları denetimleri ile verileri korumak için bir kuruluşun yeteneklerini öğesine bağlı.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Buna ek olarak, Azure’da çok çeşitli ve yapılandırılabilir güvenlik seçenekleri ile bunlar üzerinde denetim imkanı sunulmaktadır. Böylece, dağıtımlarınıza özel gereksinimleri karşılamak için güvenlik özelliklerini uyarlayabilirsiniz. Bu belge yardımcı olur, bu gereksinimleri karşılayan.

### <a name="abstract"></a>Özet
Denetim ve güvenlikle ilgili olaylar ve ilgili uyarıları günlük kaydını etkili verileri koruma stratejisi, önemli bileşenleridir. Güvenlik günlüklerini ve raporları kuşkulu etkinlikleri ve iç saldırıların yanı sıra ağ, denenen veya başarılı dış sızma gösterebilir desenlerini algılayabilir Yardım elektronik bir kayıtla sağlar. Denetim kullanıcı etkinliği, belge Mevzuat uyumluluğu izlemek, adli analiz gerçekleştirmek için kullanabilirsiniz. Güvenlik olayları oluştuğunda uyarılar anında bildirim sağlar.

Microsoft Azure Hizmetleri ve ürünleriyle denetim ve güvenlik ilkeleri ve mekanizmaları kapsamın açıklarını tanımlamanıza ve ihlallerinden önlemeye yardımcı olmak için bu boşluklar adres yardımcı olmak için seçenekleri günlük yapılandırılabilir güvenlik sağlar. Bazı Microsoft hizmetleri sunar (ve bazı durumlarda, tüm) aşağıdaki seçeneklerden birini: izleme, günlüğe kaydetme ve analiz sistemi sürekli görünürlük; sağlamak üzere merkezi zamanında uyarıları; ve büyük miktarda bilgi cihazları ve Hizmetleri tarafından oluşturulan yönetmenize yardımcı olacak raporlar.

Microsoft Azure günlük verilerini analiz için güvenlik olay ve Olay yönetimi (SIEM) sistemleri aktarılabilir ve üçüncü taraf denetim çözümler ile entegre olur.

Bu teknik oluşturma, toplama ve güvenlik günlüklerini Azure üzerinde barındırılan hizmetlerden analiz için bir giriş sağlar ve Azure dağıtımlarınızı güvenlik Öngörüler elde yardımcı olabilir. Bu teknik incelemede kapsamını uygulamalara sınırlıdır ve oluşturulan ve dağıtılan Azure Hizmetleri.

> [!Note]
> Burada yer alan bazı öneriler artan veri, ağ ya da işlem kaynağı kullanımına neden ve lisans ya da abonelik maliyetlerinizi artırabilir.

## <a name="types-of-logs-in-azure"></a>Azure günlükleri türleri
Bulut uygulamalarını birçok taşıma bölümleriyle karmaşıktır. Günlükleri, uygulamanızı kurma kalmasını sağlamak için veri ve sağlıklı bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur. Ayrıca, uygulamanız hakkında ayrıntılı Öngörüler elde etmek için günlük verilerini kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek.

Azure Azure her hizmet için ayrıntılı günlük kaydını üretir. Bu günlükler, bu ana türleri tarafından kategorilere ayrılır:
-   **Denetim/Yönetim günlüklerini** Azure Kaynak Yöneticisi oluşturma, güncelleştirme ve silme işlemleri görünürlük sağlar. [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) günlük bu tür bir örneğidir.

-   **Veri günlüklerini düzlemine** bir Azure kaynak kullanımını bir parçası olarak oluşturulan olaylara görünürlük sağlar. Bu günlük türü örnekler sistem, güvenlik, Windows olay ve uygulama günlüklerini bir sanal makine ve [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) Azure İzleyicisi aracılığıyla yapılandırılmış


-   **İşlenen olayların** sizin adınıza çözümlenen olaylar/işlenen Uyarıları hakkında bilgi verin. Bu tür örnekleri [Azure Güvenlik Merkezi uyarılarını](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) nerede [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) işlenir ve aboneliğinizi analiz ve kısa güvenlik uyarıları sağlar

Aşağıdaki tabloda Azure içinde kullanılabilir günlük en önemli türünü listeler.

| Günlük Kategorisi | Günlük türü | Kullanımları | Tümleştirme |
| ------------ | -------- | ------ | ----------- |
|[Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Azure Resource Manager kaynaklarını denetim düzlemi olayları|   Aboneliğinizi kaynaklarında gerçekleştirilen işlemler hakkında bilgi sağlar.| REST API & [Azure İzleyicisi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Azure Resource Manager kaynaklarını Abonelikteki işlemi hakkında sık veriler| Operations kaynağınız kendisini gerçekleştirilen bir anlayış sağlayın| Azure İzleyici [akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[AAD raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Günlüklerini ve raporları|Kullanıcı oturum açma etkinliklerini & kullanıcı ve Grup Yönetimi hakkında sistem etkinlik bilgileri|[Graph API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Sanal makine ve bulut Hizmetleri](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-collect-azurevm)|Windows olay günlüğü & Linux Syslog| Sistem verileri ve sanal makinelerde günlük verilerini yakalar ve bu verileri tercih ettiğiniz bir depolama hesabına aktarır.|   Windows kullanarak [WAD](https://docs.microsoft.com/azure/azure-diagnostics) (Windows Azure Diagnostics depolama) ve Linux Azure İzleyicisi|
|[Depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Depolama günlüğe kaydetme ve ölçüm verileri için bir depolama hesabı sağlar|Insight sağlar trace istekleri, kullanım eğilimlerini çözümleme ve depolama hesabınız ile ilgili sorunları tanılamak.|    REST API veya [istemci kitaplığı](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[NSG (ağ güvenlik grubu) akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON biçimi ve bir kural başına temelinde giden ve gelen akışları gösterir|Giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntüleyin|[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Uygulama Insight](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Günlükleri, özel durumlar ve özel tanılama|    Uygulama performansı Yönetimi (APM) hizmeti birden çok platformdaki web geliştiricileri için.| REST API [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Veri işleme / Güvenlik Uyarısı| Azure Güvenlik Merkezi uyarı, günlük analizi Uyarısı|   Güvenlik bilgileri ve Uyarıları.|   REST API'leri, JSON|

### <a name="activity-log"></a>Etkinlik Günlüğü
[Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar. Rapor beri etkinlik günlüğü daha önce "Denetim günlüklerini" veya "İşlem günlükleri," olarak biliniyordu [denetim düzlemi olayları](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) aboneliklerinizi için. Etkinlik günlüğü kullanarak, belirleyebilirsiniz "ne, kimin, ne zaman ve" herhangi bir yazma işlemleri (PUT, POST, DELETE) aboneliğinizi kaynaklarında alınan için. İşleminin durumunu ve ilgili diğer özellikleri de anlayabilirsiniz. Etkinlik günlüğü okuma (GET) işlemleri içermez.

Burada PUT, POST, DELETE tüm yazma işlemlerini etkinlik günlüğü kaynakları içeren ifade eder. Örneğin, etkinlik günlükleri sorunlarını giderirken hata bulmak için veya bir kullanıcı, kuruluşunuzdaki bir kaynak nasıl değişiklik izlemek için kullanabilirsiniz.

![Etkinlik Günlüğü](./media/azure-log-audit/azure-log-audit-fig1.png)


Olaylar, etkinlik Azure portalını kullanarak günlüğü alabilir [CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell cmdlet'leri ve [Azure İzleyici REST API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Etkinlik günlükleri 19 günlük veri saklama süresi vardır.

Tümleştirme senaryolarına
-   [Bir etkinlik günlüğü olay tetikler bir e-posta veya Web kancası uyarı oluşturabilir.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email)

-   [Olay Hub'ına akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.

-   Powerbı kullanarak Analiz [Power BI içerik paketi.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)

-   [Arşiv veya el ile İnceleme için bir depolama hesabı için kaydedin.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log) Günlük profilleri kullanarak bekletme süresi (gün) cinsinden belirtebilirsiniz.

-   Sorgulamak ve Azure portalında görüntüleyin.

-   PowerShell Cmdlet, CLI veya REST API sorgu.

-   Etkinlik günlüğü için günlük profilleriyle verme [Analytics oturum](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Bir depolama hesabı kullanabilir veya [olay hub'ı ad](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) olmayan bir verme günlük ile aynı abonelikte. Ayar yapılandıran kullanıcının uygun olmalıdır [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) her iki aboneliğin erişimi
### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri
Azure tanılama günlükleri, bu kaynakla ilgili zengin, sık sık veri sağlayan bir kaynak tarafından gösterilen. Bu günlükler içeriğini kaynak türüne göre değişir (örneğin, [Windows olayı sistem günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)VM'ler için bir kategori tanılama günlüğünün olan ve [blob, tablo ve kuyruk günlükleri](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) tanılama günlüklerini kategori depolama hesapları için) ve aboneliğinizi kaynaklarında gerçekleştirilen işlemler hakkında bilgi sağlayan etkinlik günlüğü farklıdır.

![Azure tanılama günlükleri](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure tanılama günlükleri, birden çok olan yapılandırma seçenekleri, PowerShell, komut satırı arabirimi (CLI) ve REST API kullanarak Azure portalında sunar.

**Tümleştirme senaryolarına**
-   Bunları kaydetmek bir [depolama hesabı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) denetim veya el ile İnceleme için. Tanılama ayarları kullanarak bekletme süresi (gün) cinsinden belirtebilirsiniz.

-   [Event Hubs'a akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) bir üçüncü taraf hizmeti veya gibi özel analiz çözümü tarafından alımı için [Powerbı.](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

-   Bunları ile analiz [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

**Tanılama günlüklerini ve kaynak türü başına desteklenen bir günlük kategorileri için şema Hizmetleri, desteklenen**


| Hizmet | Şema & belgeleri | Kaynak Türü | Kategori |
| ------- | ------------- | ------------- | -------- |
|Load Balancer| [Azure yük dengeleyici (Önizleme) için günlük analizi](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent|
|||Microsoft.Network/loadBalancers| LoadBalancerProbeHealthStatus
|Ağ Güvenlik Grupları|[Ağ güvenlik grupları (NSG’ler) için Log Analytics](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|
|||Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|
|Application Gatewayler|[Uygulama ağ geçidi için tanılama günlükleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|
|Key Vault|[Azure Anahtar Kasası Günlüğü](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Etkinleştirme ve arama trafiği Analytics kullanma](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Data Lake Store|[Azure Data Lake Store için tanılama günlüklerine erişme](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts|Denetim|
|Data Lake Analytics|[Azure Data Lake Analytics’te tanılama günlüklerine erişim](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts|Denetim|
|||Microsoft.DataLakeAnalytics/accounts|İstekler|
|||Microsoft.DataLakeStore/accounts|İstekler|
|Logic Apps|[Logic Apps B2B özel izleme şeması](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows|İş akışı WorkflowRuntime|
|||Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch Tanılama Günlüğü](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Otomasyonu|[Azure otomasyonu için günlük analizi](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts|JobLogs|
|||Microsoft.Automation/automationAccounts|JobStreams|
|Event Hubs|[Azure Event Hubs tanılama günlükleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces|ArchiveLogs|
|||Microsoft.EventHub/namespaces|OperationalLogs|
|Stream Analytics|[İş tanılama günlükleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs|Yürütme|
|||Microsoft.StreamAnalytics/streamingjobs|Yazma|
|Service Bus|[Azure Service Bus tanılama günlükleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory raporlama
Azure Active Directory (Azure AD), dizininize yönelik güvenlik, etkinlik ve denetim raporlarını içerir. [Azure Active Directory denetim raporu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) , müşterilerin kendi Azure Active Directory'de oluştu ayrıcalıklı Eylemler tanımlamak için yardımcı olur. Ayrıcalıklı Eylemler ayrıcalık değişiklikler (örneğin, rolü oluşturma veya parola sıfırlama), değişen ilke yapılandırmaları (örneğin, parola ilkelerinin) veya dizin yapılandırması (örneğin, etki alanı Federasyon ayarlarında yapılan değişiklikler) değişiklikleri içerir.

Raporlar, olay adı için değişiklik ve tarih ve saat (UTC) etkilenen hedef kaynak eylemi gerçekleştiren aktör denetim kaydını sağlar. Müşteriler kendi Azure Active Directory için denetim olayları listesini almak için [Azure portal](https://portal.azure.com/)açıklandığı gibi [denetim günlüklerini görüntülemek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Kapsama dahil olan raporların listesi şu şekildedir:

| Güvenlik raporları | Etkinlik raporları | Denetim raporları |
| :--------------- | :--------------- | :------------ |
|Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri| Uygulama kullanımı: özet| Dizin denetimi raporu|
|Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri|  Uygulama kullanımı: ayrıntılı||
|Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri|    Uygulama panosu||
|Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri|   Hesap hazırlama hataları||
|Düzensiz oturum açma etkinliği|    Bireysel kullanıcı cihazları||
|Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri|   Bireysel kullanıcı Etkinliği||
|Anormal oturum açma etkinliği gösteren kullanıcılar| Grup etkinlik raporu||
||Parola Sıfırlama Kayıt Etkinlik Raporu||
||Parola sıfırlama etkinliği|||

Bu raporlar veri SIEM sistemleri, Denetim ve iş zekası araçları gibi uygulamalarınıza yararlı olabilir. Azure AD raporlama API'leri, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Bu çağrı [API'leri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) çeşitli programlama dilleri ve araçları.

Azure AD Denetim Raporu olayları 180 gün boyunca saklanır.

> [!Note]
> Raporlarda bekletme hakkında daha fazla bilgi için bkz: [Azure Active Directory rapor bekletme ilkeleri.](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)

Bunların denetim olaylarını daha uzun bekletme dönemleri depolanırken ilgilenen müşteriler için düzenli olarak çekmesini raporlama API'si kullanılabilir [olaylarını denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) ayrı veri deposuna.

### <a name="virtual-machine-logs-using-azure-diagnostics"></a>Azure Tanılama'yı kullanarak sanal makine günlükleri
[Azure tanılama](https://docs.microsoft.com/azure/azure-diagnostics) dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir özelliktir. Tanılama uzantısını birkaç farklı kaynaklardan kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti Web ve çalışan rolleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me),

![Azure Tanılama'yı kullanarak sanal makine günlükleri](./media/azure-log-audit/azure-log-audit-fig3.png)

[Azure sanal makineleri](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) Microsoft Windows çalıştıran ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Azure tanılama kullanarak sanal makine etkinleştirebilirsiniz aşağıdaki:

-   Visual Studio kullanarak bkz [izleme Azure sanal makineler için Visual Studio'yu kullanın](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

-   [Bir Azure sanal makine uzaktan üzerinde Azure tanılama ayarlama](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

-   [Tanılama Azure sanal makineler üzerinde ayarlamak için PowerShell kullanın](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

-   [İzleme ve tanılama Azure Resource Manager şablonu kullanarak bir Windows sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Depolama Analizi
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler veriler için bir depolama hesabı sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Storage Analytics günlüğe kaydetme için kullanılabilir [Blob, kuyruk ve tablo hizmetlerine.](https://docs.microsoft.com/azure/storage/storage-introduction) Storage Analytics bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgi günlüğe kaydeder.

Bu bilgiler, istekleri ayrı ayrı izlemek ve depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. İstekleri en iyi çaba ilkesine göre günlüğe kaydedilir. Hizmet uç noktası karşı yapılan istekleri varsa günlük girişleri oluşturulur. Örneğin, bir depolama hesabı, Blob uç etkinlik vardır, ancak kendi tablo veya kuyruğu uç noktalarını, yalnızca için ilgili günlüğe değil Blob hizmeti oluşturulur.

Storage Analytics kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmelisiniz. İçinde etkinleştirebilirsiniz [Azure portal](https://portal.azure.com/); Ayrıntılar için bkz: [Azure portalında bir depolama hesabını izleme.](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) Storage Analytics REST API veya istemci kitaplığı yoluyla programlı olarak etkinleştirebilirsiniz. Storage Analytics her hizmet için ayrı ayrı etkinleştirmek için hizmet özelliklerini ayarlama işlemi kullanın.

Toplanan veriler (günlük için) iyi bilinen bir blob ve Blob hizmeti ve tablo hizmeti API'leri kullanılarak erişilebilecek (için ölçümleri), iyi bilinen tablolara depolanır.

Storage Analytics bir 20-TB depolama hesabınız için toplam sınırı bağımsızdır depolanan veri miktarına sahiptir. Tüm günlükler depolanmış [blok blobları](https://docs.microsoft.com/azure/storage/storage-analytics) $logs adlı bir kapsayıcıda, otomatik olarak oluşturulan depolama çözümlemeleri için bir depolama hesabı etkin olduğunda.

> [!Note]
> Faturalama ve veri bekletme ilkeleri hakkında daha fazla bilgi için bkz: [Storage Analytics ve faturalama.](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing)
>
> [!Note]
> Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri.](https://docs.microsoft.com/azure/storage/storage-scalability-targets)

Kimliği doğrulanmış ve anonim istek aşağıdaki türlerini günlüğe kaydedilir.



| Kimliği Doğrulandı  | Adsız|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu | Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istekleri |
| Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istekleri |İstemci ve sunucu zaman aşımı hataları |
|   Analiz verileri istekleri |    304 (değişiklik) hata koduyla başarısız olan GET istekleri |
| Storage Analytics kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analytics günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) Konular. | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analytics günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure ağ günlükleri
Günlüğe kaydetme ve Azure'da izleme ağ kapsamlı ve iki geniş kategorisi kapsar:

-   [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) -senaryo tabanlı ağ izleme Ağ İzleyicisi'deki özelliklerle sağlanır. Bu hizmet içeren paket yakalama, sonraki atlama IP akış, güvenlik grubu görünümü, NSG akış günlükleri doğrulayın. Senaryo düzeyi izleme kaynak tek tek ağ izleme aksine ağ kaynaklarına bir uçtan uca görünümünü sağlar.

-   [Kaynak İzleme](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring) -kaynak düzeyi izleme dört özellikleri, tanılama günlükleri, ölçümleri, sorun giderme ve kaynak durumu oluşur. Bu özelliklerin tümü ağ kaynak düzeyinde oluşturulur.

![Azure ağ günlükleri](./media/azure-log-audit/azure-log-audit-fig4.png)

Ağ İzleyicisi İzleme ve koşullar bir ağ düzeyinde senaryo içinde gelen ve giden Azure tanılama sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

**NSG akış günlük** -akış günlükleri ağ güvenlik grupları için izin verilen ya da grubu güvenlik kuralları tarafından reddedilen trafiği ilgili günlükleri yakalamanıza olanak sağlar. Bu akış günlükleri JSON biçiminde yazılır ve Kural başına temelinde, akış uygulanır, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini NIC giden ve gelen akışları gösterir ve trafiğe izin verilen veya reddedilen.

### <a name="network-security-group-flow-logging"></a>Ağ güvenlik grubu akışı günlüğe kaydetme

[Ağ güvenlik grubu akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek izin veren bir Ağ İzleyicisi bir özelliğidir. Bu akış günlükleri JSON biçiminde yazılır ve Kural başına temelinde, akış uygulanır, akış (kaynak/hedef IP, kaynak/hedef bağlantı noktası, Protokolü), 5-tanımlama grubu bilgilerini NIC giden ve gelen akışları gösterir ve trafiğe izin verilen veya reddedilen.

Hedef ağ güvenlik grupları akış günlükleri, ancak bunlar değil aynı diğer günlükler görüntülenir. Akış günlükleri yalnızca bir depolama hesabında depolanır.

Diğer günlükler üzerinde görülen olarak aynı bekletme ilkeleri, akış günlüklerine uygulanır. Günlükleri günden 1 gün 365 gün olarak ayarlanabilir bir bekletme ilkesi vardır. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

**Tanılama günlükleri**

Dönemsel ve spontaneous olayları ağ kaynakları tarafından oluşturulan ve bir olay hub'ı veya günlük analizi için gönderilen depolama hesaplarındaki günlüğe. Bu günlükleri bir kaynak sistem durumu fikir sağlar. Bu günlükler Power BI ve günlük analizi gibi araçları görüntülenebilir. Tanılama günlükleri görüntüleme konusunda bilgi için [günlük analizi.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)

![Tanılama günlükleri](./media/azure-log-audit/azure-log-audit-fig5.png)

Tanılama günlükleri için kullanılabilir [yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), yollar ve [uygulama ağ geçidi.](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

Ağ İzleyicisi görünümü bir tanılama günlükleri sağlar. Bu görünüm, tanılama günlüğünün destekleyen tüm ağ kaynaklarını içerir. Bu görünümden etkinleştirin ve ağ kaynaklarını kolayca ve hızlı bir şekilde devre dışı bırakın.


Günlüğe kaydetme özellikleri önceki yanı sıra Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:
- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -çeşitli bağlantılar ve ağ kaynakları bir kaynak grubunda arasındaki ilişkileri gösteren bir ağ düzey bir görünümünü sağlar.

- [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -bir sanal makine ve paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve süresini ayarlamak ve sınırlamaları boyutu yapamamasına gibi ince ayar denetimleri yönlülük sağlar. Paket verileri blob Mağazası'nda veya .cap biçiminde yerel diskte depolanabilir.

-   [IP akış doğrular](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -paket izin verilen veya reddedilen denetimleri temel akış bilgi 5-tanımlama grubu paket parametrelerine (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol). Paketi bir güvenlik grubu tarafından reddedilirse kural ve Paket reddedildi grubun döndürülür.

-   [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -kullanıcı tanımlı yollar herhangi tanılamak olanak sağlayarak Azure ağ yapıda yönlendirilen paketleri yanlış için sonraki atlama belirler.

-   [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.

-   [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -sanal ağ geçitleri ve bağlantıları sorun giderme olanağı sağlar.

-   [Ağ abonelik sınırları](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits) -ağ kaynak kullanımı sınırları karşı görüntülemenizi sağlar.

### <a name="application-insight"></a>Uygulama Insight

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformdaki web geliştiricileri için genişletilebilir bir uygulama performansı Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Ayrıca performans anormalliklerini otomatik olarak algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla aslında neler yaptığını anlamanıza yardımcı olan güçlü analiz araçları içerir.

 Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

 .NET, Node.js ve J2EE gibi çok çeşitli platformlarda, şirket içi veya bulutta barındırılan uygulamalar için yararlıdır. DevOps işleminizi ile tümleşir ve bağlantı noktaları için çeşitli geliştirme araçlarına sahiptir.

![Uygulama Insight](./media/azure-log-audit/azure-log-audit-fig6.png)

Geliştirme takımına yönelik olan Application Insights, uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olur. Şunları izler:

-   **İstek oranları, yanıt süreleri ve hata oranları**: Hangi sayfaların günün hangi saatlerinde popüler olduğunu ve kullanıcılarınızın konumunu öğrenin. En iyi performansı hangi sayfaların gösterdiğini görün. Daha fazla istek olduğunda yanıt süreleriniz ve hata oranlarınız yükseliyorsa bir kaynak atama sorununuz olabilir.

-   **Bağımlılık oranları, yanıt süreleri ve hata oranları**: Dış hizmetlerin sizi yavaşlatıp yavaşlatmadığını öğrenin.

-   **Özel durumlar** - toplu istatistikler çözümlemek veya belirli örnekleri seçin ve yığın izleme ve ilgili istekleri ayrıntıya gidin. Hem sunucu hem de tarayıcı özel durumları raporlanır.

-   **Sayfa görüntüleme sayısı ve yükleme performansı**: Kullanıcılarınızın tarayıcıları tarafından gerçekleştirilir.

-   Web sayfalarından **AJAX çağrıları**: Oranlar, yanıt süreleri ve hata oranları.

-   **Kullanıcı ve oturum sayıları**.

-   Windows veya Linux sunucu makinelerinizden CPU, bellek ve ağ kullanımı gibi **performans sayaçları**.

-   Docker veya Azure’dan **konak tanılama**.

-   Uygulamanızdan **tanılama izleme günlükleri**: İzleme olayları ile istekler arasında bağıntı kurmanıza imkan tanır.

-   Satılan öğeler ya da kazanılan maçlar gibi iş olaylarını izlemek için istemcide ya da sunucu kodunda kendi yazdığınız **özel olaylar ve ölçümler**.

**Tümleştirme senaryolarına ve açıklama listesi:**

| Tümleştirme senaryolarına | Açıklama |
| --------------------- | :---------- |
|[Uygulama eşlemesi](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Uygulamanızın bileşenlerinin yanı sıra önemli ölçüm ve uyarılar.||
|[Tanılama arama örneği için veri](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| İstekler, özel durumlar, bağımlılık çağrıları, günlük izlemeleri ve sayfa görüntülemeleri gibi olaylarda arama yapın ve bunları filtreleyin.||
|[Toplanan veriler için ölçüm Gezgini](https://docs.microsoft.com/azure/application-insights/app-insights-metrics-explorer)|İstek, hata ve özel durum oranları; yanıt süreleri, sayfa yükleme süreleri gibi toplu verileri keşfedin, filtreleyin ve bölümlere ayırın.||
|[Panolar](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards#dashboards)|Birden çok kaynaktan toplanan verileri birleştirin ve başkalarıyla paylaşın. Çok bileşenli uygulamalar ve takım odasında sürekli görüntüleme için idealdir.||
|[Canlı ölçümleri akış](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)|Yeni bir derleme dağıttığınızda, her şeyin beklendiği gibi çalıştığından emin olmak için bu neredeyse gerçek zamanlı performans göstergelerini izleyin.||
|[Analizler](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Bu güçlü sorgulama dilini kullanarak uygulamanızın performansı ve kullanımıyla ilgili zor soruları yanıtlayın.||
|[Otomatik ve el ile uyarıları](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Otomatik uyarılar, uygulamanızın normal telemetri desenlerine uyum sağlar ve normal desenin dışında bir durum gelişirse tetiklenir. Belirli özel veya standart ölçüm düzeylerinde de uyarılar ayarlayabilirsiniz.||
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Koddaki performans verilerini görün. Yığın izlemelerinden koda gidin.||
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Kullanım ölçümlerini diğer iş zekası verileriyle tümleştirin.||
|[REST API](https://dev.applicationinsights.io/)|Ölçümleriniz ve ham verileriniz üzerinde sorgu çalıştırmak için kod yazın.||
|[Sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Ham verileri, geldiğinde, depolama toplu verme.||

### <a name="azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarıları
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) otomatik olarak toplar, analiz eder ve Azure kaynaklarınızı, ağ ve gerçek tehditleri algılamak ve yanlış azaltmak için güvenlik duvarı ve endpoint protection çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini tümleştirir pozitif sonuç. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

Güvenlik Merkezi tehdit algılaması Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik verilerini otomatik olarak toplayarak çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur.

![Azure Güvenlik Merkezi](./media/azure-log-audit/azure-log-audit-fig7.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri sıçramalar ve [makine öğrenme](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojileri – elle yaklaşımlar kullanılarak ve saldırıların gelişimi tanımlamak imkansız olan tehditler tüm bulut yapısındaki olayları değerlendirmek için uygulanır. Bu güvenlik analizleri şunlardır:

-   **Tümleşik tehdit bilgileri:** Microsoft ürünleri ve Hizmetleri, Microsoft dijital Suçlar birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve dış akışların genel tehdit bilgisine uygulayarak bilinen kötü aktörleri arar.

-   **Davranış analizi:** kötü amaçlı davranışları bulmak için bilinen modelleri uygular.

-   **Anomali algılama:** geçmiş taban çizgisi oluşturmak için istatistiksel profil oluşturmayı kullanır. Olası bir saldırı vektörüne uygun olan yerleşik taban çizgilerinden sapmalar konusunda uyarır.


Çok sayıda güvenlik işlemleri ve olay yanıtı takımlar önceliklendirmek ve güvenlik uyarıları İnceleme için başlangıç noktası olarak bir güvenlik bilgileri ve Olay yönetimi (SIEM) çözümünü kullanır. Azure günlük Tümleştirmesi ile müşteriler Güvenlik Merkezi uyarılarını ve sanal makine güvenlik olayları, kullanıcıların günlük analizi ya da yakın gerçek zamanlı SIEM çözümünden birlikte Azure tanılama ve Azure denetim günlükleri tarafından toplanan eşitleyebilirsiniz.


## <a name="log-analytics"></a>Log Analytics

Günlük analizi toplamak ve bulut kaynakları tarafından oluşturulan verileri çözümlemek yardımcı olan Azure hizmetinde ve şirket içi ortamları. Tümleşik arama ve özel panolar kullanarak taşımalarına tüm iş yükleri ve fiziksel konumlarından sunucular arasında milyonlarca kayıt çözümlemek için gerçek zamanlı Öngörüler sunar.

![Log Analytics](./media/azure-log-audit/azure-log-audit-fig8.png)

Center, günlük analizi adresindeki Azure bulutta barındırılan günlük analizi çalışma alanıdır. Verileri çalışma alanına aboneliğinize yapılandırma veri kaynakları ve ekleme çözümleri tarafından bağlı kaynaklardan toplanır. Veri kaynakları ve çözümleri her kendi özellikleri vardır, ancak hala analiz farklı kayıt türleri birlikte çalışma alanına sorgularda oluşturur. Böylece farklı kaynaklar tarafından toplanan farklı veri türleriyle çalışmak için aynı araçları ve yöntemleri kullanabilirsiniz.

Bağlı kaynaklar, Log Analytics tarafından toplanan verileri oluşturan bilgisayarlar ve diğer kaynaklardır. Bu, yüklü aracıları içerebilir [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) ve [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) doğrudan bağlanan bilgisayarlar veya aracıları [bağlı bir System Center Operations Manager yönetim grubu.](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents) Günlük analizi de verileri toplamak [Azure depolama.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)

[Veri kaynakları](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources), bağlı her kaynaktan toplanan farklı veri türleridir. Bu olaylar içerir ve [performans verileri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) gelen [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) ve kaynakları gibi ek olarak Linux aracılarını [IIS günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs), ve [özel metin günlükleri.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs) Toplamak istediğiniz her veri kaynağını yapılandırabilirsiniz. Yapılandırma, otomatik olarak bağlı her kaynağa dağıtılır.

Dört farklı yolu vardır [günlüklerini ve Azure Hizmetleri için ölçümleri toplama:](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)
1.  Azure tanılama günlük analizi (aşağıdaki tabloda tanılama) doğrudan

2.  Günlük analizi (Depolama aşağıdaki tabloda) Azure depolama için Azure tanılama

3.  Azure Hizmetleri (aşağıdaki tabloda bağlayıcılar) bağlayıcıları

4.  Toplamak ve günlük analizi (boşlukları aşağıdaki tabloda ve için listelenmeyen Hizmetleri) içine veri göndermek için komut dosyaları

| Hizmet | Kaynak Türü | Günlükler | Ölçümler | Çözüm |
| :------ | :------------ | :--- | :------ | :------- |
|Uygulama ağ geçitleri|  Microsoft.Network/<br>applicationGateways|  Tanılama|Tanılama|    [Azure uygulama](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [ağ geçidi analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application Insights||     Bağlayıcı|  Bağlayıcı|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Bağlayıcısı (Önizleme)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Automation hesapları|   Microsoft.Automation/<br>AutomationAccounts|    Tanılama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Batch hesapları|    Microsoft.Batch/<br>batchAccounts|  Tanılama|    Tanılama||
|Klasik bulut Hizmetleri||       Depolama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Bilişsel hizmetler|    Microsoft.CognitiveServices/<br>accounts|       Tanılama|||
|Data Lake analizi|   Microsoft.DataLakeAnalytics/<br>accounts|   Tanılama|||
|Veri Gölü deposu|   Microsoft.DataLakeStore/<br>accounts|   Tanılama|||
|Olay Hub'ı ad alanı|   Microsoft.EventHub/<br>ad alanları|  Tanılama|    Tanılama||
|IoT Hub|  Microsoft.Devices/<br>IotHubs||     Tanılama||
|Key Vault| Microsoft.KeyVault/<br>kasaları|  Tanılama  || [KeyVault analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Yük Dengeleyiciler|    Microsoft.Network/<br>loadBalancers|    Tanılama|||
|Logic Apps|    Microsoft.Logic/<br>İş akışları|  Tanılama|    Tanılama||
||Microsoft.Logic/<br>integrationAccounts||||
|Ağ Güvenlik Grupları|   Microsoft.Network/<br>networksecuritygroups|Tanılama||   [Azure ağ güvenlik grubu analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Kurtarma kasaları|   Microsoft.RecoveryServices/<br>kasaları|||[Analytics (Önizleme) Azure kurtarma Hizmetleri](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Hizmet ara|   Microsoft.Search/<br>searchServices|    Tanılama|    Tanılama||
|Service Bus ad alanı| Microsoft.ServiceBus/<br>ad alanları|    Tanılama|Tanılama|    [Hizmet veri yolu Analytics (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Depolama||    [Service Fabric Analytics (Önizleme)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>sunucuları /<br>veritabanları||       Tanılama||
||Microsoft.Sql/<br>sunucuları /<br>elasticPools||||
|Depolama|||         Betik| [Azure depolama çözümlemeleri (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Virtual Machines|  Microsoft.Compute/<br>virtualMachines|  Dahili numara|  Dahili numara||
||||Tanılama||
|Sanal makine ölçek kümeleri|   Microsoft.Compute/<br>virtualMachines    ||Tanılama||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtualMachines||||
|Web sunucu grupları|Microsoft.Web/<br>ServerFarm öğesine verilir||   Tanılama
|Web Siteleri| Microsoft.Web/<br>siteler ||      Tanılama|    [Daha fazla bilgi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>siteleri /<br>yuvaları|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Şirket içi SIEM sistemleriyle günlük tümleştirme
[Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324) Azure kaynaklarınızı ham günlükleri, şirket içi tümleştirmenize olanak tanır **güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleri**.

![Günlük tümleştirme](./media/azure-log-audit/azure-log-audit-fig9.png)

Azure günlük tümleştirme Azure Tanılama'yı, Windows (WAD) sanal makinelerden, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını toplar ve Azure kaynak sağlayıcısı günlüğe kaydeder. Toplama, bağıntılı, çözümlemek ve güvenlik olayları için uyarı böylece bu tümleştirme tüm varlıklarınızı, şirket içi veya bulutta, birleştirilmiş bir Pano sağlar.



Azure Güvenlik Merkezi uyarılarını, Azure tanılama günlüklerini ve Azure Active Directory denetim günlükleri, Azure günlük tümleştirme şu anda Azure etkinlik günlükleri, Azure aboneliğinizde Windows sanal makine Windows olay günlüğünden tümleştirilmesi destekler.

| Günlük türü | Günlük analizi JSON (Splunk, ArcSight, Qradar) destekleme |
| :------- | :-------------------------------------------------------- |
|AAD denetim günlükleri|    evet|
|Etkinlik Günlükleri| Evet|
|ASC uyarıları |Evet|
|Tanılama günlükleri (kaynak günlükleri)|  Evet|
|VM günlükleri|   İletilen olaylar aracılığıyla ve JSON üzerinden değil Evet|


Aşağıdaki tabloda günlük kategori ve SIEM tümleştirme ayrıntı açıklanmaktadır.

[Azure günlük tümleştirme ile çalışmaya başlama](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started) - öğretici Azure günlük tümleştirme yükleme anlatılmaktadır ve Azure WAD depolama günlüklerinden tümleştirme, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını ve Azure Active Directory denetim günlükleri.

Tümleştirme senaryolarına

-   [Ortak yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – bu blog gönderisine Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesini yapılandırma gösterilmektedir.

-   [Azure günlük sık sorulan sorular (SSS) tümleştirme](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq) -bu SSS Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.

-   [Güvenlik Merkezi tümleştirme uyarıları Azure ile tümleştirme oturum](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration) – bu belge, günlük analizi ile Azure tanılama ve Azure denetim günlükleri tarafından toplanan sanal makine güvenlik olayları yanı sıra Güvenlik Merkezi uyarılarını eşitlemek gösterilmiştir veya SIEM çözümü.

## <a name="next-steps"></a>Sonraki Adımlar

- [Denetme ve günlüğe kaydetme](https://www.microsoft.com/trustcenter/security/auditingandlogging)

Görünürlüğü koruma ve hızlı bir şekilde zamanında güvenlik uyarılarını yanıt verilerini koruma

- [Güvenlik günlüğü ve Azure içindeki denetim günlük toplama](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/)

Azure örneklerinizi emin olmak için zorunlu kılmanız gerekiyorsa hangi ayarların doğru güvenlik topluyorsunuz ve denetim günlüklerini.

- [Bir site koleksiyonu denetim ayarlarını yapılandır](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US)

Bir site koleksiyonu yöneticisi olarak belirli bir kullanıcı tarafından gerçekleştirilen eylemler geçmişini alabilir ve belirli bir tarih aralığı içinde yapılan Eylemler geçmişini de alabilirsiniz. 

- [Office 365 güvenlik ve Uyumluluk Merkezi Denetim günlüğü arama](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)

Office 365 kuruluşunuzdaki kullanıcı ve yönetici etkinliğini görüntülemek için birleşik denetim günlüğü aramak için bir Office 365 güvenlik ve Uyumluluk Merkezi kullanabilirsiniz.


