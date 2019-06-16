---
title: Azure operasyonel güvenliğine genel bakış | Microsoft Docs
description: Bu makalede, Azure operasyonel güvenliğine genel bakış sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: tomsh
ms.openlocfilehash: 38054d6ee3799296887726954ef1f096945aeaeb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586858"
---
# <a name="azure-operational-security-overview"></a>Azure operasyonel güvenliğine genel bakış

[Azure operasyonel güvenlik](https://docs.microsoft.com/azure/security/azure-operational-security) verilerini, uygulamalarını ve diğer varlıklardan Microsoft azure'da korumak için Hizmetleri, denetimleri ve kullanıcılara sunulan özellikleri gösterir. Çeşitli Microsoft'a özgü özellikler aracılığıyla edinilen bilgileri içeren bir çerçevedir. Bu özellikler, Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center program ve siber güvenlik tehditleri hakkındaki ayrıntılı tanıma içerir.

## <a name="azure-management-services"></a>Azure Yönetim Hizmetleri

Bir BT operasyon ekibinin, veri merkezi altyapı, uygulamaları ve verileri kararlılığını ve bu sistemlerin güvenliğini de dahil olmak üzere, yönetmekten sorumludur. Ancak, karmaşık BT ortamları arasında genellikle artan güvenlik öngörü kuruluşların verileri birden çok güvenlik ve yönetim sistemlerinden birlikte cobble gerektirir.

[Microsoft Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan bir bulut tabanlı BT yönetimi çözümüdür. Çekirdek işlevselliğini, Azure'da çalışan aşağıdaki hizmetleri tarafından sağlanır. Azure içeren birden çok yardımcı hizmetler yönetmek ve korumak şirket içi ve bulut altyapısı. Her hizmet belirli bir yönetim işlevi sağlar. Farklı yönetim senaryoları elde etmek için Hizmetleri birleştirebilirsiniz. 

### <a name="azure-monitor"></a>Azure İzleyici

[Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) merkezi veri depolarına yönetilen kaynaklardan veri toplar. Bu veriler, olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler içerebilir. Veriler toplandıktan sonra uyarı, analiz ve dışarı aktarma için kullanılabilir. 

Çeşitli kaynaklardan gelen verileri birleştirmenize ve mevcut şirket içi ortamınızla verileri birleştirerek Azure hizmetlerinizden. Azure İzleyici günlüklerine de açıkça ayıran veri koleksiyonunu bu verilerin üzerinde gerçekleştirilen eylemden tüm eylemlerin her tür veri için kullanılabilir olmasını sağlamak.

### <a name="automation"></a>Otomasyon

[Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) Bulut ve kurumsal bir ortamda yaygın olarak gerçekleştirilen el ile uzun süre çalışan, hata yapmaya açık ve sık sık tekrarlanan görevleri otomatik hale getirmek bir yol sağlar. Bu, zaman tasarrufu sağlar ve yönetim görevlerinin güvenilirliğini artırır. Hatta, düzenli aralıklarla otomatik olarak gerçekleştirilecek bu görevleri zamanlar. Runbook'ları kullanarak işlemleri otomatikleştirme ya da Desired State Configuration ' ı kullanarak yapılandırmayı otomatikleştirme.

### <a name="backup"></a>Backup

[Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) yedeklemek (veya korumak) için kullanabileceğiniz Azure tabanlı bir hizmettir ve Microsoft Cloud verilerinizi geri yükleyin. Azure yedekleme, mevcut şirket içi veya şirket dışı yedekleme çözümünüzü güvenilir, güvenli ve maliyet açısından rekabetçi bir bulut tabanlı Çözümle değiştirir. 

Azure Backup, indirin ve uygun bilgisayar veya sunucuda veya bulutta dağıtın bileşenleri sunar. Dağıtacağınız bileşen veya aracı, korumak istediğiniz nesnelere göre değişiklik gösterir. Tüm Azure Backup bileşenleri (koruduğunuz veriler şirket içi veya Bulut ortamında) verileri azure'daki bir Azure kurtarma Hizmetleri kasasına yedeklemek için kullanılabilir. 

Daha fazla bilgi için [Azure Backup bileşen tablosuna](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site Recovery

[Azure Site Recovery](https://azure.microsoft.com/documentation/services/site-recovery) şirket içi sanal ve fiziksel makineleri azure'a veya ikincil bir siteye çoğaltılmasını düzenleyerek iş sürekliliği sağlar. Birincil site kullanılamıyorsa, böylece kullanıcılar çalışmaya devam edebilirsiniz gelirse ikincil konuma yük devredersiniz. Sistemleri çalışma düzenine geri dönün, başarısız. Azure güvenlik daha akıllı ve etkili tehdit algılaması gerçekleştirmek üzere Merkezi'ni kullanın.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) olan kapsamlı bir kimlik hizmeti olan:

-   Kimlik ve erişim yönetimi (IAM) bulut hizmeti sağlar.
-   Merkezi erişim yönetimi, çoklu oturum açma (SSO) ve raporlama sağlar.
-   Destekleyen tümleşik access management için [uygulamaları binlerce](https://azure.microsoft.com/marketplace/active-directory/) Azure Marketi'nde, Salesforce, Google Apps, kutusu ve Concur gibi.

Azure AD de içeren eksiksiz [kimlik yönetimi özellikleri](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports), aşağıdakiler dahil olmak üzere:

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Self Servis parola yönetimi](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/)
- [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)
- [Ayrıcalıklı hesap yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
- [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Uygulama kullanımını izleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
- [Zengin denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Güvenlik İzleme ve uyarı](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)

Azure Active Directory'ye (işletme veya tüketici) müşteriler ve iş ortakları için yayımlama tüm uygulamalar aynı kimliğe sahip ve erişim yönetimi özellikleri. Bu, önemli ölçüde işletim maliyetlerini azaltmak sağlar.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) , önlemenize, algılamanıza ve Artırılmış görünürlük ile tehditleri (ve üzerinde denetim) yardımcı olur, Azure kaynaklarınızın güvenlik. Bu, aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde gözden kaçan geçebilir tehditleri algılamanıza yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

[Sanal makine (VM) veri koruma](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) azure'da sanal makinenizin güvenlik ayarlarını görünürlük sağlayarak ve tehditleri izleme. Güvenlik Merkezi, sanal makinelerinizi şu açılardan izleyebilir:

- Önerilen yapılandırma kuralları ile işletim sistemi güvenlik ayarları.
- Sistem güvenliği ve eksik olan kritik güncelleştirmeleri.
- Uç nokta koruması önerileri.
- Disk şifreleme doğrulaması.
- Ağ tabanlı saldırılara.

Güvenlik Merkezi kullanır [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). RBAC sağlar [yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) kullanıcıları, grupları ve Azure Hizmetleri için atanabilir.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca rol sahibi, katkıda bulunan veya okuyucu bir kaynağın ait olduğu kaynak grubu ve abonelik için atanan bir kaynakla ilgili bilgileri görebilirsiniz.

>[!Note]
>Roller hakkında daha fazla bilgi ve Güvenlik Merkezi'nde eylemlerine izin görmek için [Azure Güvenlik Merkezi'nde izinler](https://docs.microsoft.com/azure/security-center/security-center-permissions).

Güvenlik Merkezi Microsoft Monitoring Agent'ı kullanır. Azure İzleyici hizmeti kullanan aracının budur. Bu Aracıdan toplanan veriler, herhangi bir mevcut Log Analytics'te depolanır [çalışma](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) coğrafi konum VM'nin hesaba katılarak Azure aboneliğiniz veya yeni bir çalışma alanı ile ilişkili.

## <a name="azure-monitor"></a>Azure İzleyici

Bulut uygulamanızdaki performans sorunlarını işletmenizi etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sık sürümleri ile performans düşüşü yaşanması herhangi bir zamanda gerçekleşebilir. Ve bir uygulama geliştiriyorsanız, kullanıcılarınız genellikle test bulamadınız sorunları keşfedin. Bu sorunları hemen bilmeniz gerekenler ve araçları için sorun tanılanıp sahip olmalıdır.

[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) Azure üzerinde çalışan hizmetleri izlemek için temel araçtır. Aktarım hızı bir hizmet ve bulunduğu ortam hakkında altyapı düzeyinde veriler sağlar. Uygulamalarınızdaki tüm Azure ve ölçek büyütme veya küçültme kaynakları karar yönetiyorsanız, Azure İzleyici, başlangıç yerdir.

Uygulamanız hakkında ayrıntılı Öngörüler elde etmek için izleme verilerini de kullanabilirsiniz. Hakkında bilgi sahibi uygulama performans ve sürdürülebilirliği geliştirmek için yardımcı veya aksi halde el ile müdahale gerektiren eylemleri otomatikleştirme. 

Azure İzleyici, aşağıdaki bileşenleri içerir.

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü

[Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Abonelikleriniz için Denetim düzlemi olayları rapor ettiğinden, daha önce "Denetim günlüğü" veya "İşlem günlüğü" olarak biliniyordu.

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

[Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bir kaynak tarafından gönderilir ve bu kaynağın işlemiyle ilgili zengin, sık kullanılan veriler sağlar. Bu günlüklerin içeriği kaynak türüne göre değişir.

Windows olayı sistem günlükleri, tanılama günlüklerinin VM'ler için bir kategori ' dir. BLOB, tablo ve kuyruk günlükleri, tanılama günlüğü depolama hesapları için kategorileridir.

Tanılama günlükleri tümünden [etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Etkinlik günlüğü, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri işlemler kaynağınızın kendisi gerçekleştirdiği hakkında bilgi sağlar.

### <a name="metrics"></a>Ölçümler

Azure İzleyici, Azure'da iş yüklerinizi durumunu ve performansını görünürlük sunan telemetri sağlar. En önemli Azure telemetri verilerini türüdür [ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) (Ayrıca çağrılan performans sayaçları) çoğu Azure kaynakları tarafından yayılan. Azure İzleyicisi'ni yapılandırma ve izleme ve sorun giderme için bu ölçümleri kullanabilmeniz için birçok yol sağlar.

### <a name="azure-diagnostics"></a>Azure Tanılama

Azure Tanılama, dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlar. Çeşitli kaynaklardan tanılama uzantısını kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti rolleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure sanal makineleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) Microsoft Windows çalıştıran ve [Azure Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).

## <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Müşteriler, azure'da bir uçtan uca ağ Azure ExpressRoute, Azure Application Gateway sanal ağlar gibi ağ kaynakları oluşturma ve düzenleme oluşturun ve yük Dengeleyiciler. İzleme kaynakların her biri ağ üzerinde kullanılabilir.

Uçtan uca ağ karmaşık yapılandırmaları ve kaynakları arasındaki etkileşimler olabilir. Senaryo tabanlı aracılığıyla izlenmesi gereken karmaşık senaryolar sonucudur [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

Ağ İzleyicisi, izleme ve Tanılama, Azure ağı basitleştirir. Ağ İzleyicisi tanılama ve görselleştirme araçları kullanabilirsiniz:

- Azure sanal makinesinde uzaktan paket yakalamaları yararlanın.
- Akış günlüklerini kullanarak ağ trafiğiniz içgörüler.
- Azure VPN ağ geçidiniz ve bağlantıları tanılayın.

Ağ İzleyicisi şu anda aşağıdaki özellikleri içerir:

- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): Çeşitli bağlantılar ve bir kaynak grubundaki ağ kaynakları arasındaki ilişkileri bir görünümünü sağlar.
- [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): Bir sanal makine içine ve dışına paket verilerini yakalar. Zaman ve boyut sınırlamaları ayarlama gibi gelişmiş filtreleme seçenekleriyle ince ayarlı denetimler çok yönlülük getirir. Paket verileri blob depolama veya yerel diskte .cap biçimde depolanabilir.
- [IP akışı doğrulama](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): Akış bilgileri (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol) için 5 tanımlama grubu paket parametreleri temel paket izin verilip verilmediğini denetler. Bir güvenlik grubu paket reddederse, kural ve paketi reddeden Grup döndürülür.
- [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Paketler Azure ağ dokusunda, yanlış yapılandırılmış tüm kullanıcı tanımlı yollar tanılayabilirsiniz yönlendirilmek için sonraki atlama belirler.
- [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): Bir VM'de uygulanan etkili ve uygulanan güvenlik kuralları alır.
- [Ağ güvenlik grupları için NSG akış günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview): İzin verilen veya grubu güvenlik kuralları tarafından engellenen trafik ilgili günlükleri tutmak etkinleştirin. Akış 5-tuple bilgilere göre tanımlanır: kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol.
- [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): Sanal ağ geçitleri ve bağlantı sorunlarını giderme özelliği sağlar.
- [Ağ aboneliği sınırı](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Ağ kaynak kullanımı sınırlarını karşı görüntülemenizi sağlar.
- [Tanılama günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Etkinleştirme veya devre dışı bir kaynak grubundaki ağ kaynakları için tanılama günlüklerini tek bir panel sağlar.

Daha fazla bilgi için [yapılandırma Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="cloud-service-provider-access-transparency"></a>Bulut hizmeti sağlayıcısı erişim saydamlık

[Microsoft Azure müşteri kasa](https://azure.microsoft.com/blog/approve-audit-support-access-requests-to-vms-using-customer-lockbox-for-azure/) bir hizmet sorunu çözmek için verilerinize erişim için bir Microsoft destek mühendisine gerektiğinde, nadir örneğinde doğrudan denetim olanağı sağlar, Azure portalında tümleşiktir. Burada bir Microsoft destek mühendisine bu sorunu çözmek için yükseltilmiş izinler gerektirir bir hata ayıklama uzaktan erişim sorunu gibi çok az sayıda örneği vardır. Böyle durumlarda, Microsoft mühendisleri hizmete kısıtlı erişimle sınırlı, zamana bağlı yetkilendirme sağlayan tam zamanında Erişim hizmetini kullanın.  
Microsoft, her zaman erişim için müşteri onayı edinmiş olması, ancak müşteri kasa, artık birlikte inceleyip onaylayabilir veya Azure Portalı'ndan bu tür istekleri reddetme olanağı sağlar. İstek onaylanana kadar Microsoft destek mühendisleri erişim izni yok.

## <a name="standardized-and-compliant-deployments"></a>Standart ve uyumlu dağıtımları

[Azure bir Blueprint'i](../governance/blueprints/overview.md) bulut mimarları ve merkezi bilgi teknolojisi grupları uygulamak ve kuruluşun standartları, desenleri ve gereksinimlere uyması Azure kaynaklarını tekrarlanabilir bir kümesini tanımlamak etkinleştirin.  
Bu hızlı bir şekilde oluşturun ve yeni ortamlarını ve kullanıcılar bunları Kurumsal uyumluluğu koruyan altyapısıyla oluşturuyorsunuz güven DevOps takımları için mümkün kılar. Şemalar, çeşitli kaynak şablonlarını ve diğer yapıtlar gibi dağıtımını düzenlemek için bildirim temelli bir yöntemini sağlar: 

- Rol Atamaları
- İlke Atamaları
- Azure Resource Manager şablonları
- Kaynak Grupları

## <a name="devops"></a>DevOps

Önce [Geliştirici işlemlerini (DevOps)](https://www.visualstudio.com/learn/what-is-devops/) uygulama geliştirme ekipleri olan iş gereksinimlerini yazılım programı için veri toplama ve kod yazmaya sorumlu. Daha sonra ayrı bir QA takım program yalıtılmış geliştirme ortamında test. Gereksinimlerin karşılanıp karşılanmadığı, QA takım işlemlerinin dağıtmak kodu yayımladı. Dağıtım ekipleri, ağ ve veritabanı gibi gruplar halinde daha parçalanmış. Bir yazılım programının "bağımsız bir takıma yıkılan" her zaman performans sorunlarını eklendi.

DevOps, takımların daha hızlı ve daha hesaplı bir şekilde daha güvenli, yüksek kaliteli çözümler sunma olanağı sağlar. Müşteriler, yazılım ve Hizmetleri tüketirken dinamik ve güvenilir bir deneyim bekler. Takımlar Hızlı bir şekilde yazılım güncelleştirmelerini yinelemek ve güncelleştirmelerin etkisini ölçmek gerekir. Bunlar sorunlarını gidermek için yeni geliştirme yinelemeleriyle hızlı bir şekilde yanıt veya daha fazla değer sağlayın.  

Microsoft Azure gibi bulut platformları geleneksel performans sorunlarını kaldırmış ve altyapının metalaşmasına yardımcı oldu. Yazılım arasındaki temel farklardan ve iş sonuçlarını faktör olarak, tüm reigns. Hiçbir kuruluş, geliştirici veya BT çalışanı olabilir veya DevOps hareketinden kaçınmanız gerekir.

Deneyimli DevOps uygulayıcıları aşağıdaki uygulamalardan birkaçını benimseyin. Bu yöntemler [kişileri içeren](https://www.visualstudio.com/learn/what-is-devops-culture/) tarafından iş senaryolarını temel alan stratejiler için. Araçlar çeşitli uygulamaların otomatikleştirilmesine yardımcı olabilir.

- [Çevik planlama ve proje yönetimi](https://www.visualstudio.com/learn/what-is-agile/) teknikleri planlamak ve sprint'ler halinde yalıtmak, takım kapasitesini yönetmek ve takımların değişen iş gereksinimlerini hızla benimsemesine yardımcı olmak için kullanılır.
- [Genellikle Git ile sürüm denetimi](https://www.visualstudio.com/learn/what-is-git/), kaynak paylaşmak ve yayın işlem hattı otomatik hale getirmek için yazılım geliştirme araçları ile tümleştirmek için dünya genelindeki takımların sağlar.
- [Sürekli Tümleştirme](https://www.visualstudio.com/learn/what-is-continuous-integration/) sürekli olarak birleştirilmesini ve hataları erkenden bulunmasını sağlar kodu, test.  Birleştirme sorunlarıyla ve geliştirme takımları için anında geri bildirim harcama uğraşmaya daha az zaman diğer avantajları içerir.
- [Sürekli teslim](https://www.visualstudio.com/learn/what-is-continuous-delivery/) yazılım çözümlerinin üretim ve test ortamları yardımcı olur, kuruluşların hızla hataları düzeltin ve durmaksızın değişen iş gereksinimlerine yanıt vermesine.
- [İzleme](https://www.visualstudio.com/learn/what-is-monitoring/) --çalışan uygulamaları dahil olmak üzere üretim ortamları için müşteri kullanımını--yanı sıra uygulama durumunu yardımcı izlenmesi sayesinde kuruluşların bir hipotez hızla doğrulamak ya stratejileri disprove.  Zengin veriler yakalanır ve çeşitli günlük biçimlerinde depolanır.
- [Kod olarak altyapı (Iac)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) Otomasyon doğrulama oluşturulması ve kapatılması işlemlerinin ve ağlar ve sanal makineleri, güvenli, kararlı uygulama barındırma platformları yardımcı sağlayan bir uygulamadır.
- [Mikro Hizmetler](https://www.visualstudio.com/learn/what-are-microservices/) mimarisi iş kullanımı örnekleri, yeniden kullanılabilir küçük hizmetler yalıtılır için kullanılır.  Bu mimari, ölçeklenebilirlik ve verimlilik sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik ve denetim çözümü hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Güvenlik ve uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)
- [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview)
