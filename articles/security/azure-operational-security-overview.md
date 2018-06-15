---
title: Azure operational güvenliğine genel bakış | Microsoft Docs
description: Bu makalede Azure işletimsel güvenlik genel bir bakış sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: c0413678aad16105f732ef23fb60c61fddcdad45
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34365721"
---
# <a name="azure-operational-security-overview"></a>Azure operational güvenliğine genel bakış
[Azure işlem güvenliği](https://docs.microsoft.com/azure/security/azure-operational-security) verilerini, uygulamaları ve diğer Microsoft Azure varlıkları korumak için Hizmetleri, denetimleri ve kullanıcılar için kullanılabilir özellikleri gösterir. Microsoft'a özgü özellikleri çeşitli elde edilen bilgilerden içeren bir çerçevedir. Bu yetenekler Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center program ve siber güvenlik tehdit derin tanıma içerir.

## <a name="operations-management-suite"></a>Operations Management Suite
Bir BT işletim ekibi, veri merkezi altyapı, uygulamaları ve kararlılık ve bu sistemlerin güvenlik dahil olmak üzere veri yönetilmesinden sorumludur. Ancak, karmaşık BT ortamları arasında genellikle artırma güvenlik bilgileri elde birlikte birden çok güvenlik ve yönetim sistemlerine ait verileri cobble kuruluşların gerektirir.

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan bir bulut tabanlı BT yönetimi çözümüdür. Çekirdek işlevselliğini Azure'da çalışan aşağıdaki hizmetleri tarafından sağlanır. Her hizmet belirli yönetim işlevi sağlar. Farklı yönetim senaryoları elde etmek için Hizmetleri birleştirebilirsiniz. 

### <a name="log-analytics"></a>Log Analytics
[Azure günlük analizi](http://azure.microsoft.com/documentation/services/log-analytics) merkezi bir depoya yönetilen kaynaklarından veri toplayarak Operations Management Suite için izleme hizmetleri sağlar. Bu veriler, olayları, performans verileri veya API aracılığıyla sağlanan özel verileri içerebilir. Veriler toplandıktan sonra uyarı, analiz ve dışarı aktarma için kullanılabilir. 

Çeşitli kaynaklardan veri birleştirmek ve verileri, mevcut şirket içi ortamınız ile Azure hizmetlerinden birleştirin. Tüm eylemleri tüm veri türleri için kullanılabilir olacak şekilde, günlük analizi verilerin toplanması de açıkça bu veriler üzerinde gerçekleştirilecek eylemi ayırır.

### <a name="automation"></a>Otomasyon
[Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) bir bulut ve Kurumsal ortamda sık gerçekleştirilen el ile uzun süreli, hatasız ve sık tekrarlanan görevleri otomatik hale getirmek bir yol sağlar. Zamandan tasarruf sağlar ve yönetim görevlerinin güvenilirliğini artırır. Bile, bu görevleri düzenli aralıklarla otomatik olarak gerçekleştirilecek zamanlar. Runbook'ları kullanarak işlemleri otomatik hale ya da istenen durum yapılandırması kullanarak yapılandırma yönetimini otomatik hale getirme.

### <a name="backup"></a>Backup
[Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) yedekleme (veya korumak için) kullanabileceğiniz Azure tabanlı hizmet ve verilerinizi Microsoft Cloud geri yükleyin. Azure yedekleme, mevcut şirket içi veya şirket dışı yedekleme çözümünüzün güvenilir, güvenli ve maliyet açısından rekabetçi bir bulut tabanlı çözüm ile değiştirir. 

Azure yedekleme indirin ve uygun bilgisayar veya sunucuda ya da bulutta dağıtın bileşenleri sunar. Dağıtacağınız bileşen veya aracı, korumak istediğiniz nesnelere göre değişiklik gösterir. (Tüm Azure Backup bileşenleri verileri şirket içi koruduğunuz olup olmadığını veya bulutta) Azure Azure kurtarma Hizmetleri kasasına verileri yedeklemek için kullanılabilir. 

Daha fazla bilgi için bkz: [Azure Backup bileşenleri tablo](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) şirket içi sanal ve fiziksel makineleri azure'a veya ikincil bir siteye çoğaltılmasını düzenleyerek iş sürekliliği sağlar. Birincil site kullanılamıyorsa, böylece kullanıcılar çalışmaya devam edebilirsiniz, ikincil konuma yük devredin. Geri sistemleri çalışma siparişe döndüğünüzde, başarısız. Azure güvenlik daha akıllı ve etkili tehdit algılama gerçekleştirmek için Merkezi'ni kullanın.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) olan Identity hizmetinin:

-   Kimlik ve erişim yönetimi (IAM) bir bulut hizmeti sağlar.
-   Merkezi erişim yönetimi, çoklu oturum açma (SSO) ve raporlama sağlar.
-   Destekleyen tümleşik erişim yönetimi için [uygulamaları binlerce](https://azure.microsoft.com/marketplace/active-directory/) Salesforce, Google Apps, kutusunu ve Concur Azure Marketi'nde de dahil olmak üzere.

Azure AD de içeren tam dizisi [kimlik yönetimi özelliklerini](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports), aşağıdakiler dahil olmak üzere:

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Cihaz kaydı]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview)
- [Self Servis parola yönetimi](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/)
- [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)
- [Ayrıcalıklı hesap yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
- [Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Uygulama kullanımını izleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
- [Zengin denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Güvenlik İzleme ve uyarma](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)

Azure Active Directory ile iş ortakları ve müşterileri (iş veya tüketici) için yayımlama tüm uygulamalar aynı kimliğe sahip ve yönetim özellikleri erişebilirsiniz. Bu, önemli ölçüde işletim maliyetlerini azaltmak sağlar.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi
[Azure Güvenlik Merkezi](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) engellemenize, algılamanıza ve Artırılmış görünürlük aracılığı ile tehditleri yanıt (ve üzerinden denetlemesine) yardımcı olan Azure kaynaklarınızın güvenliğini. Bu tümleşik güvenlik izleme ve ilke yönetimi sağlar. Kaçabilecek tehditleri algılayabilir yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle ile çalışır.

[Sanal makine (VM) veri koruma](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) Azure sanal makinenizin güvenlik ayarlarını görünürlük sağlayarak ve tehditleri için izleme. Güvenlik Merkezi, sanal makinelerinizi şu açılardan izleyebilir:

-   Önerilen yapılandırma kuralları ile işletim sistemi güvenlik ayarları.
-   Sistem güvenliği ve eksik olan kritik güncelleştirmeler.
-   Endpoint protection öneriler sunar.
-   Disk şifreleme doğrulama.
-   Ağ tabanlı saldırılara.

Güvenlik Merkezi kullanır [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). RBAC sağlar [yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) kullanıcıları, grupları ve Azure Hizmetleri için atanabilir.

Güvenlik Merkezi güvenlik sorunları ve güvenlik açıklarını tanımlamak için kaynaklarınızı yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca sahibi, katkıda bulunan veya abonelik veya kaynak ait olduğu kaynak grubu için okuyucu rolüne atanmış oldukları bir kaynağa ilgili bilgileri görebilirsiniz.

>[!Note]
>Rolleri hakkında daha fazla bilgi edinin ve Güvenlik Merkezi, Eylemler izin görmek için [Azure Güvenlik Merkezi'nde izinleri](https://docs.microsoft.com/azure/security-center/security-center-permissions).

Güvenlik Merkezi, Microsoft Monitoring Agent kullanır. Operations Management Suite ve günlük analizi hizmeti kullanan aynı aracı budur. Bu Aracıdan toplanan verileri herhangi bir varolan günlük analizi içinde depolanan [çalışma](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) VM coğrafi konuma dikkate alarak, Azure aboneliğinizin veya yeni bir çalışma alanı ile ilişkili.

## <a name="azure-monitor"></a>Azure İzleyici
Performans sorunlarını bulut uygulamanızda iş etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sürümleri ile degradations herhangi bir zamanda oluşabilir. Ve kullanıcılarınızın bir uygulama geliştiriyorsanız, genellikle testinde bulamadı sorunları keşfedin. Bu sorunla ilgili hemen bilmeniz gerekenler ve sorunlarını tanılamak ve araçları sahip olmalıdır.

[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) Azure üzerinde çalışan hizmetleri izlemek için temel araçtır. Bir hizmet ve çevresindeki ortamı üretimini ilgili altyapı düzeyinde verileri sağlar. Uygulamalarınızda tüm Azure ve yukarı veya aşağı kaynakları ölçeklendirmek karar verme yönetiyorsanız, Azure İzleyicisi'ni başlatmak için yerdir.

Uygulamanız hakkında ayrıntılı Öngörüler elde etmek için izleme verilerini de kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek. 

Azure İzleyici aşağıdaki bileşenleri içerir.

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü
[Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar. Denetim düzlemi olayları aboneliklerinizi için rapor ettiğinden "Denetim günlüğü" veya "İşlem günlüğü," olarak daha önce biliniyordu.

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri
[Azure tanılama günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bir kaynak tarafından gösterilen ve bu kaynağın işlemi hakkında zengin, sık veriler sağlar. Bu günlükler içeriğini kaynak türüne göre değişir.

Windows olayı sistem günlükleri tanılama günlüklerinin VM'ler için bir kategori ' dir. BLOB, tablo ve kuyruk günlükleri depolama hesapları için tanılama günlükleri, kategoriler bulunur.

Tanılama günlüklerini farklı [etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Etkinlik günlüğü aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri işlemleri kaynağınız kendisini gerçekleştirilen bir anlayış sağlar.

### <a name="metrics"></a>Ölçümler
Azure İzleyicisi, performans ve sistem durumu, iş yüklerinin görünürlük Azure üzerinde verir telemetri sağlar. En önemli Azure telemetri verilerini türüdür [ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) (Ayrıca çağrılan performans sayaçları) çoğu Azure kaynaklar tarafından gösterilen. Azure İzleyicisi'ni yapılandırma ve izleme ve sorun giderme için bu ölçümleri kullanmak için çeşitli yöntemler sağlar.

### <a name="azure-diagnostics"></a>Azure Tanılama
Azure tanılama dağıtılan bir uygulama Tanılama verileri koleksiyonu sağlar. Çeşitli kaynaklardan tanılama uzantısını kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti rollerinizi](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure sanal makineleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) Microsoft Windows çalıştıran ve [Azure Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi
Müşteriler, Azure ExpressRoute, Azure uygulama ağ geçidi, sanal ağlar gibi tek tek ağ kaynaklarına oluşturma ve yönetme tarafından azure'da bir uçtan uca ağ oluştur ve yük Dengeleyiciler. İzleme kaynakların her biri ağ üzerinde kullanılabilir.

Uçtan uca ağ karmaşık yapılandırmaları ve kaynakları arasındaki etkileşimler sahip olabilir. Senaryo tabanlı üzerinden izlenmesi gereken karmaşık senaryolar sonucudur [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

Ağ İzleyicisi, izleme ve Tanılama, Azure ağı basitleştirir. Ağ İzleyicisi için tanılama ve görselleştirme araçları kullanabilirsiniz:
- Uzak paket yakalamaları bir Azure sanal makinede alın.
- Akış günlükleri kullanarak ağ trafiğinizi kazanç Öngörüler.
- Azure VPN ağ geçidiniz ve bağlantıları tanılayın.

Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:

- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): çeşitli bağlantılar ve ağ kaynakları bir kaynak grubunda arasındaki ilişkilendirmeleri bir görünümünü sağlar.
-   [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): bir sanal makine ve paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve işlemlere denetimleri, süresini ayarlamak ve kısıtlamalar, boyut yeteneği gibi çok yönlülük sağlar. Paket verileri blob Mağazası'nda veya .cap biçiminde yerel diskte depolanabilir.
-   [IP akış doğrulayın](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): 5-tanımlama grubu paket parametrelerinin akışı bilgileri (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol) için temel bir paket izin verilen veya reddedilen gerekmediğini denetler. Bir güvenlik grubu paket engellediği, kural ve Paket reddedildi Grup döndürülür.
-   [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): kullanıcı tanımlı yollar herhangi tanılamak için Azure ağ yapısında yönlendirilen paketleri yanlış için sonraki atlama belirler.
-   [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.
-   [Ağ güvenlik grupları için NSG akış günlüklerine](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview): izin verilen veya grubu güvenlik kuralları tarafından reddedilen trafiği ilgili günlükleri yakalama olanak sağlar. Akış 5-tanımlama grubu bilgilere göre tanımlanır: kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol.
-   [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): sanal ağ geçitlerini ve bağlantıları sorun giderme olanağı sağlar.
-   [Ağ abonelik sınırları](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): ağ kaynak kullanımı sınırları karşı görüntülemenizi sağlar.
-   [Tanılama günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): etkinleştirme veya devre dışı bir kaynak grubunda ağ kaynakları için tanılama günlükleri tek bir bölme sağlar.

Daha fazla bilgi için bkz: [Ağ İzleyicisi'ni yapılandırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="devops"></a>DevOps
Önce [Geliştirici işlemleri (DevOps)](https://www.visualstudio.com/learn/what-is-devops/) uygulama geliştirme ekipleri olan bir yazılım programı için iş gereksinimlerini toplama ve kod yazma sorumlu. Daha sonra ayrı bir QA takım program yalıtılmış geliştirme ortamında test. Gereksinimlerin karşılanıp karşılanmadığı, QA takım dağıtmak işlemler için kod yayımladı. Dağıtım ekipleri daha fazla ağ ve veritabanı gibi gruplar halinde parçalanmış. Bir yazılım programının "Duvar bağımsız bir ekip için bir durum oluştu" her zaman performans sorunlarını eklendi.

DevOps daha hızlı ve daha ailenin daha güvenli, daha yüksek kaliteli çözümler sunmak takımlar sağlar. Müşteriler, yazılım ve hizmetlerini kullanırken dinamik ve güvenilir bir deneyim bekler. Takımlar Hızlı bir şekilde yazılım güncelleştirmelerini yinelemek ve güncelleştirmeleri etkisini ölçüyor gerekir. Bunlar sorunları gidermek için yeni geliştirme yineleme ile hızlı yanıt veya daha fazla değer sağlamanız gerekir.  

Microsoft Azure gibi bulut platformlarıyla geleneksel performans sorunlarını kaldırıldı ve altyapı commoditize Yardım. Her iş temel fark yatan unsur ve işletme sonuçlarını faktörünü yazılım reigns. Hiçbir kuruluş, geliştirici ve BT çalışan olabilir veya DevOps taşıma kaçınmalısınız.

Olgun DevOps uygulayıcıları aşağıdaki yöntemleri çeşitli benimser. Bu yöntemler [kişileri içeren](https://www.visualstudio.com/learn/what-is-devops-culture/) iş senaryolarını temel alarak form stratejileri için. Araç, çeşitli yöntemler otomatikleştirmek yardımcı olabilir.

-   [Çevik planlama ve proje yönetimi](https://www.visualstudio.com/learn/what-is-agile/) teknikleri planlama ve iş sprint içine yalıtmak, ekip kapasitesi yönetmek ve hızla değişen işletme ihtiyaçlarına uyum takımlar yardımcı olmak için kullanılır.
-   [Sürüm denetimi, genellikle Git ile](https://www.visualstudio.com/learn/what-is-git/), kaynak paylaşma ve yayın ardışık düzen otomatik hale getirmek için yazılım geliştirme araçları ile tümleştirmek için dünyanın her yerdeki takımlar sağlar.
-   [Sürekli Tümleştirme](https://www.visualstudio.com/learn/what-is-continuous-integration/) devam eden birleştirme ve kusurları erken bulmak için müşteri adayları kod sınama sürücüler.  Birleştirme sorunlar ve hızlı geri bildirim geliştirme ekipleri için mücadele küçülttüğü iyi bir şekilde daha az zaman diğer avantajlar şunlardır.
-   [Kesintisiz teslim](https://www.visualstudio.com/learn/what-is-continuous-delivery/) üretim ve test ortamları yardımcı olan yazılım çözümleri kuruluşlar hızla hataları düzeltin ve sürekli değişen iş gereksinimlerini yanıt.
-   [İzleme](https://www.visualstudio.com/learn/what-is-monitoring/) çalışan uygulamaları--dahil olmak üzere üretim ortamları müşteri kullanımı--yanı sıra uygulama sistem için bir varsayım form ve hızlı bir şekilde doğrulamak veya stratejileri disprove için kuruluşlara yardımcı olur.  Zengin veri yakalanan ve çeşitli günlük biçimlerde depolanır.
-   [Kod (IaC) olarak altyapı](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) otomasyon ve doğrulama oluşturma ve ağlar ve sanal makineleri, güvenli ve sağlam uygulama platformları barındırma teslim etmeye yardımcı olmak için erdirme sağlayan bir uygulamadır.
-   [Mikro](https://www.visualstudio.com/learn/what-are-microservices/) mimarisi iş kullanımı durumlarını küçük yeniden kullanılabilir Hizmetleri içine yalıtmak için kullanılır.  Bu mimari, ölçeklenebilirlik ve verimlilik sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Operations Management Suite güvenlik ve denetim çözümü hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Güvenlik ve uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [İzleme ve Operations Management Suite güvenlik ve denetim çözümünde güvenlik uyarılarını yanıtlama](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)
- [Operations Management Suite güvenlik ve denetim çözümüne kaynakları izleme](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources)
