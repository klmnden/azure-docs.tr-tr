---
title: "Azure operational güvenliğine genel bakış | Microsoft Docs"
description: "Bu makalede Azure işletimsel güvenlik genel bir bakış sağlar."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: f2153e783adb955cf9055b09ba9aa2592f51e4b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-operational-security-overview"></a>Azure operational güvenliğine genel bakış
Azure işletimsel güvenlik hizmetleri, denetimleri ve kullanıcılar için kullanılabilir özellikler verilerini, uygulamaları ve diğer Microsoft Azure varlıkları korumak için ifade eder. [Azure işlem güvenliği](https://docs.microsoft.com/azure/security/azure-operational-security) bilgi içeren bir çerçeve Microsoft Security Development Lifecycle (SDL), Microsoft Güvenlik Yanıt dahil olmak üzere Microsoft'a benzersiz yeteneklerini çeşitli elde edilen Merkezi program ve siber güvenlik tehdit derin tanıma.

Bu Azure işletimsel güvenlik genel bakış makalesi aşağıdaki alanlar üzerinde odaklanır:

- Azure Operations Management Suite
-   Azure Güvenlik Merkezi
-   Azure İzleyici
-   Azure Ağ İzleyicisi
-   Azure depolama çözümlemeleri
-   Azure Active directory

## <a name="azure-operations-management-suite"></a>Azure Operations Management Suite
BT işlemleri veri merkezi altyapı, uygulamaları ve verileri kararlılığını ve bu sistemlerin güvenliğini dahil olmak üzere, yönetmekle sorumlu. Ancak, karmaşık BT ortamları arasında genellikle artırma güvenlik bilgileri elde birlikte birden çok güvenlik ve yönetim sistemlerine ait verileri cobble kuruluşların gerektirir.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür.

OMS bulut tabanlı BT Yönetim BT Otomasyon, güvenlik ve uyumluluk, günlük analizi ve yedekleme ve kurtarma gibi birçok olanakları sunan çözümüdür. Bu nedenle, yönetmek ve BT altyapınızın korumak için mükemmel bir yardımcı olan — şirket içinde ve bulutta.

OMS’nin temel işlevleri Azure’da çalışan bir dizi hizmet tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz. İçerir:

-   Log Analytics
-   Otomasyon
-   Backup
-   Site Recovery

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics), yönetilen kaynaklardan toplanan verileri merkezi bir depoda birleştirerek OMS için izleme hizmetleri sağlar. Bu verilere olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler dahil olabilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir. Bu yöntem, çeşitli kaynaklardan gelen verileri birleştirerek Azure hizmetlerinizden topladığınız verileri mevcut şirket içi ortamınızdan toplananlarla birleştirmenize olanak tanır. Ayrıca, veri toplama işlemini veriler üzerinde gerçekleştirilen eylemden ayırarak tüm eylemlerin her tür veri üzerinde kullanılabilmesini mümkün kılar.

### <a name="automation"></a>Otomasyon
Microsoft [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) bir bulut ve Kurumsal ortamda sık gerçekleştirilen el ile uzun süreli, hatasız ve sık tekrarlanan görevleri otomatik hale getirmek bir yol sağlar. Bu zamandan tasarruf sağlar ve normal yönetim görevlerinin güvenilirliğini artırır ve hatta bunları düzenli aralıklarla otomatik olarak gerçekleştirilecek şekilde zamanlar. Runbook’ları kullanarak işlemleri otomatik hale getirebilir ya da İstenen Durum Yapılandırması’nı kullanarak yapılandırmayı otomatik hale getirebilirsiniz.

### <a name="backup"></a>Backup
[Azure yedekleme](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) yedekleme (veya korumak için) kullanabileceğiniz Azure tabanlı hizmet ve verilerinizi Microsoft bulutunda geri yükleyin. Azure Backup, var olan şirket içi veya şirket dışı yedekleme çözümünüzün yerine, güvenilir, güvenli ve maliyet açısından rekabetçi bir bulut tabanlı çözüm sunar. Azure Backup, indirdikten sonra uygun bilgisayar, sunucu veya buluta dağıtabileceğiniz birden fazla bileşene sahiptir. Dağıtacağınız bileşen veya aracı, korumak istediğiniz nesnelere göre değişiklik gösterir. Tüm Azure Backup bileşenleri (koruduğunuz veriler şirket içi veya bulut verileri olabilir), verileri Azure’daki bir Kurtarma Hizmetleri kasasına yedeklemek için kullanılabilir. Bkz: [Azure Backup bileşenleri tablo](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site kurtarma
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery), şirket içi sanal ve fiziksel makinelerin Azure’a veya ikincil bir konuma çoğaltılmasını düzenleyerek iş sürekliliği sağlar. Birincil konumunuzun kullanılamaması durumunda kullanıcılarınızın çalışmaya devam edebilmesi için ikincil konuma yük devreder; sistemler çalışır hale geldiğindeyse yeniden başlatma gerçekleştirirsiniz. Akıllı ve etkili tehdit algılama.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) Microsoft'un kapsamlı kimlik Service (Idaas) çözümü olarak:

-   Bulut hizmeti olarak IAM sağlar
-   Merkezi erişim yönetimi, çoklu oturum açma (SSO) ve raporlama sağlar
-   Destekleyen tümleşik erişim yönetimi için [uygulamaları binlerce](https://azure.microsoft.com/marketplace/active-directory/) uygulama galerisinde Salesforce, Google Apps, kutusunda, Concur ve daha fazlası dahil olmak üzere.

Azure AD de içeren tam dizisi [kimlik yönetimi özelliklerini](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports) dahil olmak üzere [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), [aygıt kaydı]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview), [ Self Servis parola yönetimi](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/), [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), [ayrıcalıklı hesap yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is), [uygulama kullanımını izleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), [zengin denetim](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), ve [izleme ve uyarma güvenlik](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).

Azure Active Directory ile tüm uygulamalar, iş ortakları için yayımlayın ve müşteriler (iş veya tüketici) aynı kimliğe sahip ve erişim yönetimi özellikleri. Bu, önemli ölçüde işletim maliyetlerini azaltmak sağlar.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi
[Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-get-started), Azure kaynaklarınızın güvenliğine yönelik artırılmış görünürlük ve denetim yoluyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

[Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) , koruma azure'da sanal makine verilerinin sanal makinenizin güvenlik ayarlarını görünürlük sağlayarak ve tehditleri için izleme yardımcı olur. Güvenlik Merkezi, sanal makinelerinizi şu açılardan izleyebilir:

-   Önerilen yapılandırma kuralları ile birlikte İşletim Sistemi (OS) güvenlik ayarları
-   Sistem güvenliği ve eksik olan kritik güncelleştirmeler
-   Uç nokta koruması önerileri
-   Disk şifreleme doğrulaması
-   Ağ tabanlı saldırılara

Azure Güvenlik Merkezi kullanan [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure), sağlayan [yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) kullanıcıları, grupları ve Azure Hizmetleri için atanabilir.

Güvenlik Merkezi güvenlik sorunları ve güvenlik açıklarını tanımlamak için kaynaklarınızı yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca kaynak sahibi, katkıda bulunan veya okuyucu rolü abonelik veya kaynak ait olduğu kaynak grubu için atandığında ilgili bilgileri görebilirsiniz.

>[!Note]
>Bkz: [izinleri Azure Güvenlik Merkezi'nde](https://docs.microsoft.com/azure/security-center/security-center-permissions) rolleri ve Güvenlik Merkezi'nde izin verilen eylemleri hakkında daha fazla bilgi için.

Güvenlik Merkezi, Microsoft Monitoring Agent kullanır: Operations Management Suite ve günlük analizi hizmeti tarafından kullanılan aynı aracı budur. Bu Aracıdan toplanan verileri herhangi bir varolan günlük analizi içinde depolanan [çalışma](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) VM coğrafi konuma dikkate alarak, Azure aboneliğinizin veya yeni bir çalışma alanları ile ilişkilendirilmiş.

## <a name="azure-monitor"></a>Azure İzleyici
Performans sorunlarını bulut uygulamanızda iş etkileyebilir. Birden çok birbirine bağlı bileşenleri ve sık sürümleri ile degradations herhangi bir zamanda oluşabilir. Ve kullanıcılarınızın bir uygulama geliştiriyorsanız, genellikle testinde bulamadı sorunları keşfedin. Bu hemen bilmeniz ve sorunlarını tanılamak ve araçları olması gerekir.

[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) Azure üzerinde çalışan hizmetleri izlemek için temel araçtır. Bir hizmet ve çevresindeki ortamı üretimini ilgili altyapı düzeyinde verileri sağlar. Uygulamalarınızda tüm Azure yönetiyorsanız, yukarı veya aşağı kaynaklar, Ölçek karar sonra Azure İzleyicisi, başlatmak için kullandığınız sağlar.

Ayrıca, uygulamanız hakkında ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek. Aşağıdakileri içerir:

-   Azure etkinlik günlüğü
-   Azure tanılama günlükleri
-   Ölçümler
-   Azure Tanılama

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü
Aboneliğinizi kaynaklarında gerçekleştirilen işlemler hakkında bilgi sağlayan bir günlüktür. [Etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliklerinizi denetim düzlemi olaylarını raporları olduğundan daha önce "Denetim günlüklerini" veya "İşlem günlükleri," olarak biliniyordu.

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri
[Azure tanılama günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bir kaynak tarafından gösterilen ve bu kaynağın işlemi hakkında zengin, sık veriler sağlar. Bu günlükler içeriğini kaynak türüne göre değişir.

Örneğin, Windows olayı sistem günlükleri tanılama günlüğünün VM'ler ve blob, tablo için bir kategori, ve sıra günlükleri tanılama günlüklerini kategorileri depolama hesapları için.

Tanılama günlüklerini farklı [etkinlik günlüğü (önceki adıyla denetim günlüğü veya işlem günlüğü olarak bilinir)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Etkinlik günlüğü aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri işlemleri kaynağınız kendisini gerçekleştirilen bir anlayış sağlar.

### <a name="metrics"></a>Ölçümler
Azure İzleyicisi, performans ve sistem durumu, iş yüklerinin Azure üzerinde görünürlük elde etmek için telemetri kullanmasına olanak sağlar. En önemli Azure telemetri verileri çoğu Azure kaynaklar tarafından gösterilen (performans sayaçlarını olarak da bilinir) ölçümleri türüdür. Azure İzleyicisi, yapılandırmak ve bunlar kullanmak için çeşitli yollar sağlar [ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) izleme ve sorun giderme için.

### <a name="azure-diagnostics"></a>Azure Tanılama
Dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir özelliktir. Çeşitli farklı kaynaklardan tanılama uzantısını kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti Web ve çalışan rolleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure sanal makineleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) Microsoft Windows çalıştıran ve [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="network-watcher"></a>Ağ İzleyicisi
Müşterilerin bir uçtan uca ağ Azure VNet, ExpressRoute, uygulama ağ geçidi, yük Dengeleyiciler ve daha fazlasını gibi çeşitli tek tek ağ kaynaklarına oluşturma ve yönetme tarafından oluşturun. İzleme kaynakların her biri ağ üzerinde kullanılabilir.

Uçtan uca ağ karmaşık yapılandırmalar ve senaryo tabanlı ağ izlemekte gereken karmaşık senaryolar oluşturma kaynakları arasındaki etkileşimler sahip olabilir.

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) izleme ve Tanılama, Azure ağı basitleştirir. Tanılama ve görselleştirme araçları ile Ağ İzleyicisi'ni etkinleştir kullanılabilir uzaktan paket almak için bir Azure sanal makinesinde yakalar, ağınızı kazanç Öngörüler akış günlüklerini kullanarak trafiği ve VPN ağ geçidi ve bağlantıları tanılama.

Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:

- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -çeşitli bağlantılar ve ağ kaynakları bir kaynak grubunda arasındaki ilişkileri gösteren bir ağ düzey bir görünümünü sağlar.
-   [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -bir sanal makine ve paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve süresini ayarlamak ve sınırlamaları boyutu yapamamasına gibi ince ayar denetimleri yönlülük sağlar. Paket verileri blob Mağazası'nda veya .cap biçiminde yerel diskte depolanabilir.
-   [IP akışları doğrulayın](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -paket izin verilen veya reddedilen denetimleri temel akış bilgi 5-tanımlama grubu paket parametrelerine (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol). Paketi bir güvenlik grubu tarafından reddedilirse kural ve Paket reddedildi grubun döndürülür.
-   [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -kullanıcı tanımlı yollar herhangi tanılamak olanak sağlayarak Azure ağ yapıda yönlendirilen paketleri yanlış için sonraki atlama belirler.
-   [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.
-   [NSG akış günlük](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) -akış günlükleri ağ güvenlik grupları için izin verilen ya da grubu güvenlik kuralları tarafından reddedilen trafiği ilgili günlükleri yakalamanıza olanak sağlar. Akış bir 5-tanımlama grubu bilgileriyle – kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol tanımlanır.
-   [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -sanal ağ geçitleri ve bağlantıları sorun giderme olanağı sağlar.
-   [Ağ abonelik sınırları](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -ağ kaynak kullanımı sınırları karşı görüntülemenizi sağlar.
-   [Tanılama günlük yapılandırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) – etkinleştirme veya devre dışı bir kaynak grubunda ağ kaynakları için tanılama günlükleri tek bir bölme sağlar.

Ağ İzleyicisi bakın, yapılandırma konusunda daha fazla bilgi için [Ağ İzleyicisi'ni yapılandırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="developer-operations-devops"></a>Geliştirici işlemleri (DevOps)
DevOps uygulama geliştirme önce bir yazılım programı için iş gereksinimlerini toplama ve kod yazma sorumlu takımlar yoktu. Gereksinimler karşılanmadı ve dağıtmak işlemler için kod yayımları ayrı bir QA takım program bir yalıtılmış geliştirme ortamında sınar. Dağıtım ekipleri daha fazla ağ ve veritabanı gibi siloed gruplar halinde parçalanmış. Bir yazılım programının "Duvar bağımsız bir takıma, oluşturulan" her zaman performans sorunlarını ekler.

[DevOps](https://www.visualstudio.com/learn/what-is-devops/) daha hızlı ve ucuz daha güvenli, daha yüksek kaliteli çözümler sunmak takımlar sağlar. Müşteriler, yazılım ve hizmetlerini kullanırken dinamik ve güvenilir bir deneyim bekler.  Takımlar yazılım güncelleştirmelerini, hızlı bir şekilde yinelemek gerekir güncelleştirmeleri etkisini ölçüyor ve yeni geliştirme yineleme sorunları gidermek için hızlı bir şekilde yanıt veya daha fazla değer sağlayın.  Microsoft Azure gibi bulut platformlarıyla geleneksel performans sorunlarını kaldırıldı ve altyapı commoditize Yardım. Her iş temel fark yatan unsur ve işletme sonuçlarını faktörünü yazılım reigns. Hiçbir kuruluş, geliştirici ve BT çalışan olabilir veya DevOps taşıma kaçınmalısınız.

Olgun DevOps uygulayıcıları aşağıdaki yöntemleri çeşitli benimser. Bu yöntemler [kişileri içeren](https://www.visualstudio.com/learn/what-is-devops-culture/) iş senaryolarını temel alarak form stratejileri için.  Araç, çeşitli yöntemler otomatikleştirmek yardımcı olabilir:

-   [Çevik planlama ve proje yönetimi](https://www.visualstudio.com/learn/what-is-agile/) teknikleri planlama ve iş sprint içine yalıtmak, ekip kapasitesi yönetmek ve hızla değişen işletme ihtiyaçlarına uyum takımlar yardımcı olmak için kullanılır.
-   [Sürüm denetimi, genellikle Git ile](https://www.visualstudio.com/learn/what-is-git/), kaynak paylaşma ve yayın ardışık düzen otomatik hale getirmek için yazılım geliştirme araçları ile tümleştirmek için dünyanın her yerdeki takımlar sağlar.
-   [Sürekli Tümleştirme](https://www.visualstudio.com/learn/what-is-continuous-integration/) devam eden birleştirme ve kusurları erken bulmak için müşteri adayları kod sınama sürücüler.  Birleştirme sorunlar ve hızlı geri bildirim geliştirme ekipleri için mücadele küçülttüğü iyi bir şekilde daha az zaman diğer avantajlar şunlardır.
-   [Kesintisiz teslim](https://www.visualstudio.com/learn/what-is-continuous-delivery/) üretim ve test ortamları Yardım için yazılım çözümleri kuruluşlar hızla hataları düzeltin ve sürekli değişen iş gereksinimlerini yanıt.
-   [İzleme](https://www.visualstudio.com/learn/what-is-monitoring/) çalışan uygulamalar üretim ortamları için uygulama durumu da dahil olmak üzere müşteri kullanım Yardım kuruluşlar form olarak bir varsayım yanı sıra ve hızlı bir şekilde doğrulamak veya stratejileri disprove.  Zengin veri yakalanan ve çeşitli günlük biçimlerde depolanır.
-   [Kod (IaC) olarak altyapı](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) otomasyon ve doğrulama oluşturma ve ağlar ve sanal makineleri, güvenli ve sağlam uygulama platformları barındırma teslim etmeye yardımcı olmak için erdirme sağlayan bir uygulama.
-   [Mikro](https://www.visualstudio.com/learn/what-are-microservices/) mimarisi işlevden küçük yeniden kullanılabilir Hizmetleri içine iş kullanımı durumlarını ayırmak için.  Bu mimari, ölçeklenebilirlik ve verimlilik sağlar.

## <a name="next-steps"></a>Sonraki adımlar
OMS güvenlik ve denetim çözüm hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Operations Management Suite | Güvenlik ve Uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [İzleme ve güvenlik uyarılarını Operations Management Suite güvenlik ve denetim çözümü yanıt](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-responding-alerts).
- [Operations Management Suite güvenlik ve denetim çözümü kaynakları izleme](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-monitoring-resources).
