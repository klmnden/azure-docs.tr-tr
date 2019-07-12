---
title: Azure geçişi - sık sorulan sorular (SSS) | Microsoft Docs
description: Azure geçişi hakkında sık sorulan sorular adresleri
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: snehaa
ms.openlocfilehash: 08a0312f12b3daab8b7f5e88da118b5bcbeb2f4c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807318"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure geçişi - sık sorulan sorular (SSS)

Bu makale, Azure geçişi hakkında sık sorulan sorular içerir. Bu makaleyi okuduktan sonra başka sorgular varsa, gönderin [Azure geçişi Forumu](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Genel

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Hangi Azure coğrafyaları Azure geçişi tarafından destekleniyor mu?
Azure geçişi şu anda Azure geçişi projesini oluşturulabilir coğrafyalar destekler. Yalnızca bu coğrafyalardaki projeleri oluşturabilirsiniz, ancak yine de makineleriniz diğer hedef konumları için değerlendirebilirsiniz. Proje Coğrafya, yalnızca bulunan meta verileri depolamak için kullanılır.

**Coğrafya** | **meta veri depolama konumu** Azure kamu | ABD Devleti Virginia Asya | Güneydoğu Asya veya Doğu Asya, Avrupa | Güney Avrupa veya Amerika Birleşik Krallık Batı Avrupa | UK Güney veya UK Batı ABD | Orta ABD ve Batı ABD 2

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Nasıl Azure geçişi Azure Site Recovery'den farkı nedir?

Azure geçişi, keşfedin, değerlendirin ve makineler ve iş yüklerini Azure'a geçirmek için yardımcı araçlar sağlar. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) bir olağanüstü durum kurtarma çözümüdür. Her iki hizmet de, bazı bileşenler paylaşın.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure geçişi Gereci (VMware/fiziksel sunucuları)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Azure geçişi Gereci Azure'a nasıl bağlanıyor?

Genel eşdüzey hizmet sağlama ile Expressroute'u kullanabilirsiniz veya internet üzerinden bağlantı olabilir.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>Azure geçişi Server değerlendirme ve geçiş için hangi ağ bağlantısı gereksinimlerini gereklidir

URL'ler ve Azure geçişi için gereken bağlantı noktaları için Azure ile iletişim kurmak, gözden [VMWare](migrate-support-matrix-vmware.md) ve [Hyper-V](migrate-support-matrix-hyper-v.md) matrislerde destekler.

### <a name="can-i-harden-the-appliance-vmware-vm-i-set-up-with-the-ova-template"></a>Ben Gereci VMware VM OVA şablonla ayarlamak sağlamlaştırmak?

İletişim ve güvenlik duvarı kuralları sola re Azure geçişi Gereci için gerekli olduğu sürece ek bileşenler (örneğin, virüsten koruma) OVA şablonlarına eklenebilir.   

### <a name="to-harden-the-azure-migrate-appliance-what-are-the-recommended-antivirus-av-exclusions"></a>Azure geçişi Gereci sağlamlaştırmak için önerilen (AV) virüsten koruma dışlamaları nelerdir?

Gerecinde taramanın dışında aşağıdaki klasörleri dışarıda yapmanız gerekir:

- Azure geçişi hizmeti için ikili dosyaları içeren klasör. Tüm alt klasörleri hariç tutun.
- %ProgramFiles%\ProfilerService  
- Azure geçişi Web uygulaması. Tüm alt klasörleri hariç tutun.
- %SystemDrive%\inetpub\wwwroot
- Veritabanı ve günlük dosyaları için yerel önbelleği. Azure geçişi hizmeti okuma/bu klasöre yazma erişimi.
  - %SystemDrive%\Profiler

### <a name="what-data-is-collected-by-azure-migrate"></a>Azure geçişi tarafından verilerin ne toplanır?

Azure geçişi Gereci dahil olmak üzere şirket içi sanal makineleri için meta verileri toplar:

**Sanal makinenin yapılandırma verileri**
- VM görünen adı (temel, vCenter)
- VM envanteri yolu (konak/küme/klasör vcenter)
- IP adresi
- MAC adresi
- İşletim sistemi
- Çekirdek, disk, NIC sayısı
- Bellek boyutu, Disk boyutları

**VM'nin performans verileri**
- CPU kullanımı
- Bellek kullanımı
- Sanal Makineye eklenmiş her disk için:
  - Disk aktarım hızı okuma
  - Disk aktarım hızı yazar.
  - Disk okuma işlemleri / sn
  - Disk işlemleri / sn yazar.
- VM'ye bağlı her ağ bağdaştırıcısı için:
  - Ağ içine
  - Ağ dışına

Bağımlılık eşlemesini dağıtırsanız Tn ek olarak, bağımlılık eşlemesini aracıları makine FQDN, işletim sistemi, IP adresi ve MAC adresi, VM ve VM için gelen ve giden TCP bağlantılarını içinde çalışan işlemler içeren bilgiler toplayın. Bu bulma, bulma için bağımlılık eşlemesi etkinleştirirseniz, yalnızca kullanılan isteğe bağlıdır.

### <a name="is-there-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Bir performans etkisi çözümlenen ESXi ana bilgisayar ortamının mı?

Sürekli performans verilerini profil oluşturma ile Azure geçişi Gereci profilleri şirket içi makineleri VM performans verilerini ölçme. ESXi konaklarında yanı sıra vCenter Server neredeyse sıfır performans etkisi vardır.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan verileri uzun depolanan ve için nasıl nerede?

Azure geçişi Gereci tarafından toplanan veriler, geçiş projesi oluştururken, belirttiğiniz Azure konumunda depolanır. Veriler Microsoft abonelikte güvenli bir şekilde depolanır ve Azure geçişi projesi sildiğinizde silinir.

Aracıları Vm'lerde yüklerseniz, bağımlılık görselleştirmesi için Azure aboneliği için oluşturulan bir Log Analytics çalışma alanında ABD bağımlılık aracıları tarafından toplanan veriler depolanır. Aboneliğinizde Log Analytics çalışma alanını sildiğinizde, bu verileri silinir. [Daha fazla bilgi edinin](concepts-dependency-visualization.md).

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-during-continuous-profiling"></a>Azure geçişi tarafından sürekli olarak profil oluşturma sırasında yüklenen veri hacmi nedir?

Azure geçişi için gönderilen veri hacmini birkaç parametre göre değişir. 10 (her bir disk ve bir NIC ile), makineleri Azure geçişi projesini bir göstergesi numarası vermek için yaklaşık 50 MB / gün gönderir. Değişiklikleri NIC'ler için veri noktalarının sayısını temel alan bir yaklaşık değer ve diskleri (gönderilen verilerin makineler, NIC'ler veya disk sayısını artırmak istiyorsanız doğrusal olmayan) budur.

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Şifrelenmiş verilerin bekleyen ve aktarım sırasında mi?

Hem Evet. Meta veriler için Azure geçişi hizmeti, https üzerinden internet üzerinden güvenli bir şekilde gönderilir. İçinde depolanan meta veriler bir [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest)hem de [Azure blob depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) Microsoft Abonelikteki ve çalışmıyorken şifrelendiğinden.

Bağımlılık aracısı tarafından toplanan veriler ayrıca içinde şifrelenmiş Aktarım (güvenli HTTPS) ve kullanıcı aboneliği bir Log Analytics çalışma alanında depolanır. Ayrıca çalışmıyorken şifrelendiğinden bir hizmettir.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Azure geçişi Gereci vCenter Server ve Azure geçişi hizmeti ile nasıl iletişim?

Gereci vCenter Server'a (bağlantı noktası 443) bağlanır, gereç ayarladığınızda sağlanan kimlik bilgilerini kullanarak. Bu, vCenter Server vCenter Server tarafından yönetilen sanal makineleri ile ilgili meta verileri toplamak için VMware powerclı'yı kullanarak sorgular. Geçen ay için her bir VM'nin performans geçmişi yanı sıra VM'ler (çekirdekler, bellek, disk, NIC vb.) hakkında her iki yapılandırma verilerini toplar. Toplanan meta veriler daha sonra (internet üzerinden HTTPS üzerinden) Azure geçişi Server değerlendirmesi için değerlendirme için gönderilir. 

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>Birden fazla vCenter sunucunuz için aynı gereç bağlanabilir miyim?

Evet, tek bir Azure geçişi Gereci birden fazla vCenter sunucularını bulmak için kullanılabilir ancak aynı anda değil. Bulmaları birbiri ardına çalıştırmanız gerekir.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Site Recovery tarafından kullanılan OVA şablonu, Azure geçişi tarafından kullanılan OVA tümleşiktir?

Şu anda hiçbir tümleştirme yoktur. . OVA şablonu Site recovery'de VMware VM'LERİNİ/fiziksel sunucu çoğaltma için bir Site Recovery yapılandırma sunucusu kurmak için kullanılır. . Azure geçişi tarafından kullanılan OVA değerlendirme ve geçiş amaçları doğrultusunda bir vCenter sunucusu tarafından yönetilen VMware Vm'leri bulmak için kullanılır.

### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>Makine boyut değiştirdim. Bir değerlendirmeyi yeniden çalıştırabilir miyim?
Azure geçişi Gereci sürekli olarak şirket içi ortamı hakkında bilgi toplar. Ancak, değerlendirme, şirket içi Vm'leri zaman içinde nokta anlık görüntüsüdür. Değerlendirmek istediğiniz VM ayarlarını değiştirirseniz, değerlendirme en son değişikliklerle güncelleştirmek için 'Yeniden Hesapla' seçeneğini kullanın.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Azure Geçişi'ndeki çok kiracılı bir ortam nasıl bulabilecek kişileri?

VMware için kiracılar genelinde paylaşılan bir ortamda sahip olduğunuz ve başka bir kiracının Abonelikteki bir kiracı Vm'leri bulmak istemiyorsanız, bulmak istediğiniz VM'lerin erişimi olan sunucu kimlik bilgileri vCenter oluşturun. Ardından Azure geçişi Gereci bulmayı devre dışı başlatma sırasında kimlik bilgilerini kullanın.

Hyper-V için bulma Hyper-V ana bilgisayar kimlik bilgileriyle kullanan VM'ler aynı Hyper-V ana bilgisayarı paylaşıyorsanız, şu anda bulma ayırmak için bir yolu yoktur.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>Kaç tane Vm'niz tek geçiş gereciyle bulunabilir?

VMware Vm'lerini 10.000 adede kadar ve tek bir geçiş gereciyle 5.000 Hyper-V Vm'leri bulabilir.  Daha fazla makine şirket içi ortamınızda varsa, nasıl ölçeklendireceğinizi öğrenin [Hyper-V](scale-hyper-v-assessment.md) ve [VMware](scale-vmware-assessment.md) değerlendirme.


## <a name="azure-migrate-server-assessment"></a>Azure geçişi: Server değerlendirmesi

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Azure geçişini sağlamaz: Server değerlendirmesi fiziksel sunucuları değerlendirmesini destekliyor?

Hayır, Azure geçişi şu anda fiziksel sunucularını değerlendirme desteklememektedir. 

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure geçişi, vCenter sunucusunu bir VMware ortamını keşfetmeye gerekiyor mu?

Evet, Azure geçişi, vCenter sunucusunu bir VMware ortamını keşfetmeye gerekir. Bunu, vCenter Server tarafından yönetilmeyen bir ESXi ana bilgisayarları bulmayı desteklemiyor.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>Azure Geçişi'nın kullanımı arasındaki fark nedir: Server değerlendirmesi ve Map Araç Kiti?

Azure geçişi: Server değerlendirmesi, geçiş değerlendirmesi, geçiş hazırlığı ve iş yüklerini azure'a geçiş için değerlendirmeyi yardımcı olmak için sağlar. [Microsoft Assessment ve planlama (eşleme) Araç Seti](https://www.microsoft.com/download/details.aspx?id=7826) geçiş Windows istemci ve sunucu işletim sistemleri ve yazılım kullanımını izleme daha yeni sürümleri için planlama gibi diğer işlevlere sahiptir. Bu senaryolarda, MAP Araç Kiti kullanmaya devam edin.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>Azure geçişi nasıl şöyledir: Azure Site Recovery dağıtım Planlayıcısı farklı Server değerlendirmesi?

Azure geçişi: Server değerlendirmesi planlama aracı geçiştir. Azure Site Recovery dağıtım planlayıcısı aracı planlama bir olağanüstü durum kurtarma ' dir.

- **Vmware'den/Hyper-v'den azure'a geçiş**: Şirket içi sunucularınızı Azure'a geçirmek istiyorsanız, Azure geçişi kullanın: Geçiş planlaması için sunucu değerlendirme aracı. Araç, şirket içi iş yüklerini değerlendirir ve rehberlik, Öngörüler ve Azure'a geçirmenizde yardımcı mekanizmaları sağlar. Geçiş planınızla hazır olduğunuzda, Azure geçişi gibi araçları kullanabilirsiniz: Sunucu geçiş, makineleri Azure'a geçirmek için.
- **Vmware'den/Hyper-v'den azure'a olağanüstü durum kurtarma**: Site Recovery kullanarak azure'a olağanüstü durum kurtarma için Site Recovery dağıtım Planlayıcısı, olağanüstü durum kurtarma planlaması için kullanın. Site Recovery dağıtım Planlayıcısı, şirket içi ortamınızın kapsamlı, Site Recovery özgü değerlendirme yapar. Bu, çoğaltma ve yük devretme sanal makinelerinin gibi başarılı olağanüstü durum işlemler için Site Recovery tarafından gerekli önerileri sağlar. 

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Azure geçişi, Kurumsal Anlaşma EA tabanlı maliyet tahmini destekliyor mu?

Azure geçişi şu anda desteklemez için maliyet tahmini [Kurumsal Anlaşma teklif](https://azure.microsoft.com/offers/enterprise-agreement-support/). Geçici çözüm, Kullandıkça Öde teklifi olarak ve el ile değerlendirme Özellikleri 'İndirim' alanına (abonelik için geçerli) indirim yüzdesindeki belirtin sağlamaktır.

  ![İndirim](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Olarak şirket içi boyutlandırma ve performans tabanlı boyutlandırma arasındaki fark nedir?

- İçinde şirket içi boyutlandırma, gibi Azure geçişi VM performans verilerini dikkate almaz. Bu, sanal makinelerinizi şirket yapılandırmasına göre boyutlandırır. Performansa dayalı boyutlandırma-boyutlandırma hakkında kullanım verileri temel alır.
- Örneğin, 4 çekirdek ve 8 GB bellek % 50 CPU kullanımı ve bellek kullanımı % 50 ile şirket içi VM varsa, şirket içi olarak boyutlandırma 4 çekirdek ve 8 GB bellek ile bir Azure VM SKU'su önerir. Kullanım yüzdesi olarak kabul edildiğinden performans tabanlı boyutlandırma, ancak, bir sanal makine SKU'su 2 Çekirdek ve 4 GB önerir.
- Benzer şekilde, disk boyutlandırma ölçütü ve depolama türü boyutlandırma iki değerlendirme özelliklerine - bağlıdır.
= Boyutlandırma ölçütü performansa dayalı ve depolama türünün otomatik olduğundan, diskin IOPS ve aktarım hızı değerleri hedef disk türünü (standart veya Premium) tanımlamak için kullanıldığında olarak kabul edilir.
- Boyutlandırma ölçütü performansa dayalı ve premium depolama türü ise, bir premium disk önerilir. Premium disk SKU seçili, şirket içi disk boyutuna bağlı. Aynı mantığı boyutlandırma, şirket içi boyutlandırma olarak boyutlandırma ölçütü olduğunda ve depolama türü standart veya premium disk için kullanılır.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Boyut önerileri üzerinde performans geçmişi ve yüzdebirlik kullanımı nasıl bir etkisi var mı?

Bu özellikler yalnızca performans tabanlı boyutlandırma için geçerlidir.

- Azure geçişi, şirket içi makinelerin performans geçmişi toplar ve Azure VM boyutu ve disk türü önermek için kullanır.
- Gereç her 20 saniyede gerçek zamanlı kullanım verilerini toplamak için şirket içi ortamı sürekli olarak profiller. Alet, 20 saniyelik örnekler toparlar ve her 15 dakika için tek bir veri noktası oluşturur. Tek veri noktasını oluşturmak için alet tüm 20 saniyelik örneklerden en yüksek değerleri seçer ve Azure’a gönderir.
- Azure geçişi (performans süresi ve performans geçmişi yüzdelik dilim değeri göre) azure'da bir değerlendirme oluşturmak, etkili kullanımı değeri hesaplar ve boyutlandırma için kullanır.
- Örneğin, performans süresini bir gün olacak şekilde ayarlayın ve 95. yüzdebirlik Azure geçişi için yüzdelik dilim değeri son gününün toplayıcı tarafından gönderilen 15 dakika örnek noktası kullanır, bunları artan düzende sıralar ve 95. yüzdebirlik olarak seçer etkili kullanımı.
- 95. yüzdebirlik 99. yüzdebirlik dilimde kullanırsanız, ortaya çıkabilecek herhangi bir aykırı değer yoksayıyorsunuz sağlar. Dönemin en yüksek kullanımını seçmek ve aykırı değerleri kaçırmamak istiyorsanız 99. yüzdebirliği seçmelisiniz.

### <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Bağımlılık görselleştirme daha büyük bir güvenle geçiş için VM grupları değerlendirmenize olanak tanır. Çapraz-makine bağımlılıklarını kapsamında bir değerlendirmeyi çalıştırmadan önce olup olmadığını denetler. Bağımlılık görselleştirme, hiçbir şey geride emin olun ve Azure'a geçirirken beklenmedik kesintileri önlemek yardımcı olur. Azure geçişi, bağımlılık görselleştirmesi etkinleştirmek için Azure İzleyici günlüklerine, hizmet eşlemesi çözümünü yararlanır.

> [!NOTE]
> Bağımlılık görselleştirme işlevini Azure Kamu'da kullanılabilir değil.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Bağımlılık görselleştirmesi özelliğini kullanmak için ücret ödemem gerekiyor mu?

Hayır. Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için herhangi bir şey yüklemeniz gerekiyor mu?

Bağımlılık görselleştirmesi kullanmak için indirip değerlendirmek istediğiniz her bir şirket içi makineye aracılar yüklemeniz gerekir.

- [Microsoft Monitoring agent(MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) her makinede yüklü olması gerekir.
- [Bağımlılık aracısını](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) her makinede yüklü olması gerekir.
- Ayrıca, internet bağlantısı olmayan makineleriniz varsa, indirmek ve Log Analytics gateway yükler gerekir.

Bağımlılık görselleştirmesi kullandığınız sürece bu aracıları gerekmez.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için mevcut bir çalışma alanını kullanabilir miyim?

Evet, mevcut bir çalışma geçiş projesine ekleyin ve bağımlılık görselleştirmesi için yararlanın. [Daha fazla bilgi edinin](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, bağımlılık görselleştirmesi dışarı aktarılamaz. Ancak, Azure geçişi kullandığı hizmet eşlemesi için bağımlılık görselleştirmeyi kullanabilirsiniz [hizmet eşlemesi REST API'lerini](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) bağımlılıkları bir json biçiminde alın.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükleme nasıl otomatikleştirebilirim?

[Bu betiği kullanın](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) aracıların yüklemesi için. [Bu yönergeleri izleyin](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) komut satırı veya Otomasyon kullanarak MMA'yı yüklemek için. MMA'yı için yararlanarak [bu betik](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Betiklerin yanı sıra dağıtım araçları gibi System Center Configuration Manager, kullanabileceğiniz [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) vb. aracıları dağıtmak için.

### <a name="what-operating-systems-are-supported-by-mma"></a>Hangi işletim sistemlerini MMA tarafından destekleniyor mu?

- [Gözden geçirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) MMA tarafından desteklenen Windows işletim sistemleri listesi.
- [İnceleyin] https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) MMA tarafından desteklenen Linux işletim sistemleri listesi.

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>Bağımlılık aracısı tarafından desteklenen işletim sistemleri nelerdir?

[Gözden geçirme](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) bağımlılık aracı tarafından desteklenen Windows işletim sistemleri.
[Gözden geçirme](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) bağımlılık aracı tarafından desteklenen Linux işletim sistemleri listesi.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Azure Geçişi'ndeki bağımlılıkları için bir saatten görselleştirebilir miyim?
Hayır, en fazla bir saatlik bağımlılıkları görselleştirebilirsiniz. Belirli bir tarihe geçmişindeki son ayda dönebilirsiniz ancak görselleştirme için en uzun süresi bir saattir. Örneğin, süre için dün bağımlılıkları görüntülemek için bağımlılık haritada kullanabilirsiniz ancak bir saatlik pencere için bir yalnızca görüntüleyebilir. Ancak, Azure İzleyici günlüklerine kullanabilirsiniz [bağımlılık veri sorgulama](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) üzerinden uzun bir süre.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-ten-vms"></a>Bağımlılık görselleştirme, ondan fazla Vm'leri gruplar için destekleniyor mu?
Yapabilecekleriniz [grupları için bağımlılıkları görselleştirme](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) en fazla on vm'lerle. On vm'lerle grubunuz varsa, biz grubunda küçük kullanıcı gruplarına ayırmak için önerilir ve ardından bağımlılıkları görselleştirin.

## <a name="azure-migrate-server-migration"></a>Azure geçişi: Sunucu geçiş

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>Azure geçişi nasıl şöyledir: Azure Site Recovery'den farklı sunucu geçişi?

Azure geçişi: Sunucu geçiş aracı tabanlı sunucuların geçişi, Azure için Site Recovery'nin çoğaltma motoru yararlanır.
## <a name="next-steps"></a>Sonraki adımlar
Okuma [Azure geçişi genel bakış](migrate-services-overview.md)
