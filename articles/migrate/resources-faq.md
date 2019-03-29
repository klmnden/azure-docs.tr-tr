---
title: Azure geçişi - sık sorulan sorular (SSS) | Microsoft Docs
description: Azure geçişi hakkında sık sorulan sorular adresleri
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: snehaa
ms.openlocfilehash: 366240c273feed559edb6e569640020046cc9471
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578654"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure geçişi - sık sorulan sorular (SSS)

Bu makale, Azure geçişi hakkında sık sorulan sorular içerir. Bu makaleyi okuduktan sonra başka sorgular varsa, gönderin [Azure geçişi Forumu](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Genel

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>Azure geçişi yalnızca VMware iş yüklerini değerlendirmesini destekliyor mu?

Evet, Azure geçişi şu anda yalnızca VMware iş yüklerini değerlendirmesini destekler. Destek Önizleme'de, Hyper-V için lütfen kaydolun [burada](https://aka.ms/migratefuture) Önizleme erişim elde etmek için. Fiziksel sunucular için destek gelecek etkinleştirilecektir.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure geçişi, vCenter sunucusunu bir VMware ortamını keşfetmeye gerekiyor mu?

Evet, Azure geçişi, vCenter sunucusunu bir VMware ortamını keşfetmeye gerektirir. Bulma işlemi bir vCenter Server tarafından yönetilmeyen bir ESXi ana bilgisayarları desteklemez.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Nasıl Azure geçişi Azure Site Recovery'den farkı nedir?

Azure geçişi, şirket içi iş yüklerinizi bulmak ve azure'a geçişinizi planlayın yardımcı olan bir değerlendirme hizmetidir. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), olağanüstü durum kurtarma çözümü olmasının yanı sıra, şirket içi iş yüklerini azure'da Iaas vm'lerine geçirmenize yardımcı olur.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>Azure geçişi değerlendirmeleri ve harita araç setini kullanma arasındaki fark nedir?

[Azure geçişi](migrate-overview.md) özellikle geçişe hazırlık durumunu ve şirket içi iş yüklerini azure'a değerlendirmesine yardımcı olmak üzere geçiş değerlendirmesi sağlar. [Microsoft Assessment ve planlama (eşleme) Araç Seti](https://www.microsoft.com/en-us/download/details.aspx?id=7826) geçiş Windows istemci ve sunucu işletim sistemleri ve yazılım kullanımını izleme daha yeni sürümleri için planlama gibi diğer işlevlere sahiptir. Bu senaryolarda, MAP Araç Kiti kullanmaya devam edin.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Nasıl Azure geçişi Azure Site Recovery dağıtım Planlayıcısı ' farklıdır?

Azure geçişi planlama aracı geçiş ve Azure Site Recovery dağıtım planlayıcısı aracı planlama bir olağanüstü durum kurtarma (DR).

**Vmware'den azure'a geçiş**: Şirket içi iş yüklerinizi Azure'a geçirmek istiyorsanız, Azure geçişi geçiş planlaması için kullanın. Azure geçişi, şirket içi iş yüklerini değerlendirir ve rehberlik, Öngörüler ve Azure'a geçirmenizde yardımcı mekanizmaları sağlar. Geçiş planınızla hazır olduktan sonra makineleri Azure'a geçirmek için Azure Site Recovery ve Azure veritabanı geçiş hizmeti gibi hizmetleri kullanabilirsiniz.

**Hyper-v'den azure'a geçiş**: Azure geçişi genel kullanıma sunulan sürümü değerlendirme VMware sanal makinelerini Azure'a geçiş için şu anda destekler. Desteklemek için Hyper-V şu anda üretim desteği Önizleme aşamasındadır. Önizlemenin çalışırken düşünüyorsanız, lütfen kaydolun [burada](https://aka.ms/migratefuture).

**Vmware'den/Hyper-v'den azure'a olağanüstü durum kurtarma**: Azure Site Recovery (Site Recovery) kullanarak azure'da olağanüstü durum kurtarma (DR) yapmak istiyorsanız, Site Recovery dağıtım Planlayıcısı planlama DR için kullanın. Site Recovery dağıtım Planlayıcısı, bir şirket içi ortamınızı kapsamlı ve ASR özgü değerlendirmesinin yapar. Bu, çoğaltma, yük devretme sanal makinelerinizin gibi başarılı DR işlemler için Site Recovery tarafından gerekli önerileri sağlar.  

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Hangi Azure coğrafyaları Azure geçişi tarafından destekleniyor mu?

Azure geçişi şu anda Avrupa, ABD ve Azure kamu proje coğrafi destekler. Geçiş projeleri yalnızca bu coğrafi bölgelerde oluşturabilirsiniz, ancak yine de makineleriniz için değerlendirebilirsiniz [birden çok hedef konumları](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). Proje Coğrafya, yalnızca bulunan meta verileri depolamak için kullanılır.

**Coğrafya** | **Meta veri depolama konumu**
--- | ---
Azure Kamu | ABD Devleti Virginia
Asya | Güneydoğu Asya
Avrupa | Kuzey Avrupa veya Batı Avrupa
Durumları sahip | Doğu ABD ve Batı Orta ABD

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Şirket içi siteyle Azure geçişi için nasıl bağlanıyor?

Bağlantı ortak eşleme ExpressRoute kullanabilir veya internet üzerinden olabilir.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Ben OVA şablonu ile ayarlanmış bir VM sağlamlaştırmak?

İletişim ve güvenlik duvarı kuralları çalışmak Azure geçişi Gereci için gerekli olduğu gibi bırakılır sürece ek bileşenler (örneğin, virüsten koruma) OVA şablonlarına eklenebilir.   

### <a name="to-harden-the-azure-migrate-appliance-what-are-the-recommended-antivirus-av-exclusions"></a>Azure geçişi Gereci sağlamlaştırmak için önerilen (AV) virüsten koruma dışlamaları nelerdir?

Virüsten koruma taraması için gereç bulunan aşağıdaki klasörler hariç yapmanız gerekir:

- Azure geçişi hizmeti için ikili dosyaları içeren klasör. Tüm alt klasörleri hariç tutun.
  %ProgramFiles%\ProfilerService  
- Azure geçişi Web uygulaması. Tüm alt klasörleri hariç tutun.
  %SystemDrive%\inetpub\wwwroot
- Veritabanı ve günlük dosyaları için yerel önbelleği. Azure geçişi hizmeti bu klasöre RW erişimi gerekir.
  %SystemDrive%\Profiler

## <a name="discovery"></a>Bulma

### <a name="what-data-is-collected-by-azure-migrate"></a>Azure geçişi tarafından verilerin ne toplanır?

Azure Geçişi, gereç tabanlı ve aracı tabanlı bulma olmak üzere iki tür bulma işlemini destekler.
Gereç tabanlı bulma şirket içi sanal makineleri ile ilgili meta verileri toplar, gereç tarafından toplanan meta verileri tam listesi aşağıda verilmiştir:

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

Aracı tabanlı bulma alet tabanlı bulma üzerinde sunulan bir seçenektir ve müşterilere yardımcı [bağımlılıkları görselleştirme](how-to-create-group-machine-dependencies.md) , şirket içi VM'ler. Bağımlılık aracısı FQDN, işletim sistemi, IP adresi, MAC adresi, VM içinde çalışan işlemler ve VM'den gelen/giden TCP bağlantıları gibi bilgileri toplar. Aracı tabanlı bulma isteğe bağlıdır ve sanal makinelerin bağımlılıklarını görselleştirin istemiyorsanız aracıları yüklememeyi seçebilirsiniz.

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Analiz edilen ESXi ana bilgisayar ortamının üzerinde bir performans etkisi olması?

Sürekli performans verilerin profilinin oluşturulması ile vCenter Server istatistik düzeyini performans tabanlı bir iç değerlendirme çalıştırmak değiştirmenize gerek yoktur. Toplayıcı Gereci, sanal makinelerin performans verileri şirket içi makinelerin profil. VCenter Server yanı sıra ESXi konakları bu neredeyse sıfır performans etkisi yoktur.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan verileri uzun depolanan ve için nasıl nerede?

Toplayıcı Gereci tarafından toplanan veriler, geçiş projesi oluştururken belirttiğiniz Azure konumunda depolanır. Veriler Microsoft abonelikte güvenli bir şekilde depolanır ve kullanıcı Azure geçişi projesini sildiğinde silinir.

Aracıları Vm'lerde yüklerseniz bağımlılık görselleştirmesi için bağımlılık aracısı tarafından toplanan verileri ABD kullanıcının abonelikte oluşturulan bir Log Analytics çalışma alanında depolanır. Aboneliğinizde Log Analytics çalışma alanını sildiğinizde, bu verileri silinir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="what-is-the-volume-of-data-which-is-uploaded-by-azure-migrate-in-the-case-of-continuous-profiling"></a>Azure geçişi tarafından sürekli olarak profil oluşturma söz konusu olduğunda karşıya veri birimi nedir?

Azure geçişi için gönderilen veri hacmini birkaç parametre göre farklılık gösterebilir. Bir göstergesi numarası vermek için on makineler (her bir disk ve bir NIC) sahip bir projeyi gönderin yaklaşık 50 MB / gün. Bu, yaklaşık bir değerdir ve NIC ve (gönderilen verilerin makineler, NIC'ler veya disk sayısını artırmak istiyorsanız doğrusal olmayan olacaktır) diskleri için veri noktalarının sayısına göre değiştirmeniz gerekir.

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Veriler beklerken ve aktarım sırasında şifrelenir?

Evet, toplanan verileri hem bekleyen hem aktarım sırasında şifrelenir. Gereç tarafından toplanan meta veriler güvenli bir şekilde Azure geçişi hizmetine internet üzerinden https gönderilir. Toplanan meta veriler depolanır [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) ve [Azure blob depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) Microsoft Abonelikteki ve bekleme sırasında şifrelenir.

Bağımlılık aracısı tarafından toplanan veriler de içinde şifrelenmiş Aktarım (güvenli bir https kanalı) ve bir kullanıcının aboneliğindeki Log Analytics çalışma alanında depolanır. Ayrıca, bekleme sırasında de şifrelenir.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Toplayıcı, vCenter Server ve Azure geçişi hizmeti ile nasıl iletişim?

Toplayıcı Gereci vCenter Server'a (bağlantı noktası 443) bağlanan gereç kullanıcı tarafından sağlanan kimlik bilgilerini kullanarak. Bu, vCenter Server vCenter Server tarafından yönetilen sanal makineleri ile ilgili meta verileri toplamak için VMware powerclı'yı kullanarak sorgular. Her iki yapılandırma verilerini VM'ler (çekirdekler, bellek, disk, NIC vb.) hakkında vCenter Server'dan tek tek son bir ay boyunca her VM'nin performans geçmişi yanı sıra toplar. Toplanan meta veriler daha sonra değerlendirmesi için Azure geçişi hizmeti (internet üzerinden https üzerinden) gönderilir. [Daha fazla bilgi](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>Birden fazla vCenter sunucunuz için aynı Toplayıcı gerecini bağlanabilir miyim?

Evet, bir tek Toplayıcı gerecini birden fazla vCenter sunucularını bulmak için kullanılabilir ancak aynı anda değil. Bulma birbiri ardına çalıştırmanız gerekir.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Site Recovery tarafından kullanılan OVA şablonu, Azure geçişi tarafından kullanılan OVA tümleşiktir?

Şu anda hiçbir tümleştirme yoktur. . OVA şablonu Site recovery'de VMware VM'LERİNİ/fiziksel sunucu çoğaltma için bir Site Recovery yapılandırma sunucusu kurmak için kullanılır. . Azure geçişi tarafından kullanılan OVA VMware vCenter sunucusu tarafından yönetilen geçiş Değerlendirme amacıyla Vm'leri bulmak için kullanılır.

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>Makine boyut değiştirdim. Değerlendirmeyi yeniden çalıştırabilir miyim?

Değerlendirmek istediğiniz VM ayarlarını değiştirirseniz, tetikleyici keşfedin yeniden Toplayıcı gerecini kullanarak. Gereci kullanın **koleksiyonu yeniden Başlat** Bunu yapmak için seçeneği. Koleksiyon tamamlandıktan sonra, güncelleştirilmiş değerlendirme sonuçlarını almak için portalda değerlendirmeye yönelik **Yeniden hesapla** seçeneğini belirleyin.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Azure Geçişi'ndeki çok kiracılı bir ortam nasıl bulabilecek kişileri?

Kiracılar genelinde paylaşılan bir ortamda varsa ve bir kiracının başka bir kiracının Abonelikteki sanal makinelerin keşfetmek istiyor musunuz, bulma kapsamı için toplayıcı gerecini kapsam alanı kullanabilirsiniz. Kiracıların konaklar paylaşıyorsanız, yalnızca belirli kiracıya ait sanal makinelerin salt okunur erişimi olan bir kimlik bilgisi oluşturmak ve ardından bu kimlik bilgisi Toplayıcı Gereci kullanabilir ve keşif yapmak için ana bilgisayarı olarak kapsamını belirtin. Alternatif olarak, vcenter Server (Klasör1 tenant1 için ve klasör2 tenant2 için diyelim), paylaşılan konak altında klasör oluşturabilirsiniz tenant2 ve Klasör1 içine tenant1 için Vm'leri klasör2 taşıyın ve bulmaları, Toplayıcı'ı uygun şekilde kapsam uygun bir klasör belirterek.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Kaç tane sanal makineyi bir tek bir geçiş projesi içinde bulunabilir?

Tek geçişi projesinde 1500 sanal makineler bulabilir. Daha fazla makine şirket içi ortamınızda varsa [daha fazla bilgi edinin](how-to-scale-assessment.md) hakkında Azure Geçişi'ndeki büyük bir ortamı nasıl bulabilir.


## <a name="assessment"></a>Değerlendirme

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Kurumsal Anlaşma (EA) Azure geçişi desteği maliyet tahmini mu?

Azure geçişi şu anda desteklemediği için maliyet tahmini [Kurumsal Anlaşma teklif](https://azure.microsoft.com/offers/enterprise-agreement-support/). Geçici çözüm, teklif ve indirim yüzdesindeki (abonelik için geçerli) değerlendirme Özellikleri 'İndirim' alanına el ile belirtme olarak Kullandıkça Öde belirtmektir.

  ![İndirim](./media/resources-faq/discount.png)

### <a name="what-is-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Olarak şirket içi boyutlandırma ve performans tabanlı boyutlandırma arasındaki fark nedir?

Olarak şirket içi olarak boyutlandırma ölçütü belirttiğinizde boyutlandırma, Azure Geçişi sanal makinelerin performans verilerini dikkate almaz ve şirket içi yapılandırmasını temel alan VM boyutları. Boyutlandırma ölçütü performans tabanlı olduğunda, boyutlandırma kullanım verilerine göre yapılır. Örneğin, 4 çekirdek içeren bir şirket içi sanal makine ve 8 GB bellek 50 CPU kullanımı % ve % 50 bellek kullanımı ile ise. 4 çekirdek içeren bir Azure VM SKU'su boyutlandırma şirket olarak boyutlandırma ölçütü ise ve 8 GB bellek önerilir, ancak boyutlandırma ölçütü performansa dayalı sanal makine SKU'su 2 Çekirdek ve 4 GB önerilen olarak kullanım yüzdesi kabul ederken boyutu önerme. Benzer şekilde, diskler için disk boyutlandırma ölçütü ve depolama türü boyutlandırma iki değerlendirme özelliklerine - bağlıdır. Boyutlandırma ölçütü performans tabanlı ve depolama türü otomatikse hedef disk türünü (Standart veya Premium) tanımlamak için diskin IOPS ve aktarım hızı değerleri göz önünde bulundurulur. Boyutlandırma ölçütü performans tabanlı ve depolama türü premium ise premium bir disk önerilir. Azure’daki premium disk SKU’su şirket içi diskin boyutuna göre seçilir. Boyutlandırma ölçütü şirket içi boyutlandırma ve depolama türü standart veya premium olduğunda aynı mantık kullanılır.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Boyut önerileri üzerinde performans geçmişi ve yüzdebirlik kullanımı nasıl bir etkisi var mı?

Bu özellikler yalnızca performans tabanlı boyutlandırma için geçerlidir. Azure Geçişi, şirket içi makinelerin performans geçmişini toplar ve bunları Azure’da VM boyutu ve disk türü önermek için kullanır. Toplayıcı aleti, her 20 saniyede bir gerçek zamanlı kullanım verilerini toplamak için sürekli olarak şirket içi ortamın profilini oluşturur. Alet, 20 saniyelik örnekler toparlar ve her 15 dakika için tek bir veri noktası oluşturur. Tek veri noktasını oluşturmak için alet tüm 20 saniyelik örneklerden en yüksek değerleri seçer ve Azure’a gönderir. Azure’da bir değerlendirme oluşturduğunuzda, Azure Geçişi performans süresi ve performans geçmişi yüzdebirlik değerine bağlı olarak Azure Geçişi etkili kullanım değerini hesaplar ve boyutlandırma için bunu kullanır. Performans süresi 1 gün ve 95 yüzdelik dilim değeri olarak ayarlarsanız, örneğin, Azure geçişi noktaları artan düzende sıralar ve 95. yüzdebirlik etkili ut olarak seçer. bu toplayıcı tarafından son bir gün için gönderilen 15 dakika örnek kullanır ilization. 95. yüzdebirlik 99. yüzdebirlik dilimde seçerseniz, gelebilir herhangi bir aykırı değer yoksayıyorsunuz sağlar. Dönemin en yüksek kullanımını seçmek ve aykırı değerleri kaçırmamak istiyorsanız 99. yüzdebirliği seçmelisiniz.

## <a name="dependency-visualization"></a>Bağımlılık görselleştirme

> [!NOTE]
> Bağımlılık görselleştirme işlevini Azure Kamu'da kullanılabilir değil.

### <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Bağımlılık görselleştirmesi kapsamında bir değerlendirmeyi çalıştırmadan önce makine bağımlılıklarını arası denetimi tarafından daha büyük bir güvenle geçiş için VM grupları değerlendirmek sağlar. Bağımlılık görselleştirmesi hiçbir şey arkasında, Azure'a geçiş yaptığınızda, beklenmeyen kesintilerin önleme bırakılır emin olmak için yardımcı olur. Azure geçişi, hizmet eşlemesi çözümünü bağımlılık görselleştirmesi etkinleştirmek için Azure İzleyici günlüklerine yararlanır.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Bağımlılık görselleştirmesi özelliğini kullanmak için ücret ödemem gerekiyor mu?

Hayır. Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için herhangi bir şey yüklemeniz gerekiyor mu?

Bağımlılık görselleştirmesi kullanmak için indirip değerlendirmek istediğiniz her bir şirket içi makineye aracılar yüklemeniz gerekir.

- [Microsoft Monitoring agent(MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) her makinede yüklü olması gerekir.
- [Bağımlılık aracısını](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) her makinede yüklü olması gerekir.
- Ayrıca, internet bağlantısı olmayan makineleriniz varsa, indirmek ve Log Analytics gateway yükler gerekir.

Bu aracılar, bağımlılık görselleştirmesi kullanmıyorsanız değerlendirmek istediğiniz makinelerde gerekmez.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için mevcut bir çalışma alanını kullanabilir miyim?

Evet, Azure geçişi, geçiş projesine mevcut bir çalışma alanı ekleyin ve bağımlılık görselleştirmesi için yararlanarak olanak sağlıyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, bağımlılık görselleştirme raporunu dışarı aktarmak mümkün değildir. Ancak, Azure geçişi kullandığı hizmet eşlemesi için bağımlılık görselleştirmeyi kullanabilirsiniz [hizmet eşlemesi REST API'lerini](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) bağımlılıkları bir json biçiminde alın.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükleme nasıl otomatikleştirebilirim?

[Burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) bağımlılık aracısını yükleme için kullanabileceğiniz bir komut dosyasıdır. [Burada](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) komut satırı veya otomatik yöntemlerini kullanarak MMA'yı nasıl yükleneceği hakkında yönergeler. MMA'yı için kullanılabilen bir komut dosyası da yararlanabilirsiniz [burada](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab) TechNet'teki.

Betiklerin yanı sıra System Center Configuration Manager (SCCM) gibi dağıtım araçları da yararlanabilirsiniz [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) vb. aracıları dağıtmak için.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>MMA'yı tarafından desteklenen işletim sistemleri nelerdir?

MMA'yı tarafından desteklenen Windows işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
MMA'yı tarafından desteklenen Linux işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Bağımlılık aracısı tarafından desteklenen işletim sistemleri nelerdir?

Bağımlılık aracısı tarafından desteklenen Windows işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
Bağımlılık aracısı tarafından desteklenen Linux işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Bağımlılıklar Azure Geçişi'ndeki bir saat süresinden daha fazla bilgi için görselleştirebilirsiniz?
Hayır, Azure geçişi, en fazla bir saatlik süre için bağımlılıkları görselleştirme sağlar. Azure geçişi belirli bir tarihe kadar son bir ay için geçmişte dönün sağlar, ancak için bağımlılıkları görselleştirebilirsiniz en fazla süre 1 saate kadar uzun. Örneğin, Dün için bağımlılıkları görüntülemek için bağımlılık Haritası saati süresi işlevleri kullanabilirsiniz ancak yalnızca bir için bir saat penceresinde görüntüleyebilirsiniz. Ancak, Azure İzleyici günlüklerine kullanabilirsiniz [bağımlılık verileri sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) üzerinden uzun bir süre.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>Bağımlılık görselleştirmesi 10'dan fazla Vm'leri gruplar için destekleniyor mu?
Yapabilecekleriniz [grupları için bağımlılıkları görselleştirme](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) en fazla 10 sanal sahip. 10'dan fazla vm'lerle grubunuz varsa, grupta küçük kullanıcı gruplarına bölün ve bağımlılıkları görselleştirme öneririz.


## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure geçişi genel bakış](migrate-overview.md)
- Öğrenin [bulma ve değerlendirme](tutorial-assessment-vmware.md) VMware ortamı
