---
title: Azure geçişi - sık sorulan sorular (SSS) | Microsoft Docs
description: Azure geçişi hakkında sık sorulan sorular adresleri
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: snehaa
ms.openlocfilehash: e39cf260cc4931fc0dddc4922479522cb521d08e
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407070"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure geçişi - sık sorulan sorular (SSS)

Bu makale, Azure geçişi hakkında sık sorulan sorular içerir. Bu makaleyi okuduktan sonra başka sorgular varsa, gönderin [Azure geçişi Forumu](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Genel

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>Azure geçişi yalnızca VMware iş yüklerini değerlendirmesini destekliyor mu?

Evet, Azure geçişi şu anda yalnızca VMware iş yüklerini değerlendirmesini destekler. Hyper-V ve fiziksel sunucular için destek gelecek etkinleştirilecektir.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure geçişi, vCenter sunucusunu bir VMware ortamını keşfetmeye gerekiyor mu?

Evet, Azure geçişi, vCenter sunucusunu bir VMware ortamını keşfetmeye gerektirir. Bulma işlemi bir vCenter Server tarafından yönetilmeyen bir ESXi ana bilgisayarları desteklemez.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Nasıl Azure geçişi Azure Site Recovery'den farkı nedir?

Azure geçişi, şirket içi iş yüklerinizi bulmak ve azure'a geçişinizi planlayın yardımcı olan bir değerlendirme hizmetidir. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), olağanüstü durum kurtarma çözümü olmasının yanı sıra, şirket içi iş yüklerini azure'da Iaas vm'lerine geçirmenize yardımcı olur.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>Azure geçişi değerlendirmeleri ve harita araç setini kullanma arasındaki fark nedir?

[Azure geçişi](migrate-overview.md) özellikle geçişe hazırlık durumunu ve şirket içi iş yüklerini azure'a değerlendirmesine yardımcı olmak üzere geçiş değerlendirmesi sağlar. [Microsoft Assessment ve planlama (eşleme) Araç Seti](https://www.microsoft.com/en-us/download/details.aspx?id=7826) diğer işlevlere sahiptir. Örneğin, Windows istemci ve sunucu işletim sistemleri, daha yeni sürümleri için planlama geçiş yazılımı vb. kullanımını izleme. Bu senaryolarda, MAP Araç Kiti kullanmaya devam edin.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Nasıl Azure geçişi Azure Site Recovery dağıtım Planlayıcısı ' farklıdır?

Azure geçişi planlama aracı geçiş ve Azure Site Recovery dağıtım planlayıcısı aracı planlama bir olağanüstü durum kurtarma (DR).

**Vmware'den azure'a geçiş**: şirket içi iş yüklerinizi Azure'a geçirmek istiyorsanız, Azure geçişi geçiş planlaması için kullanın. Azure geçişi, şirket içi iş yüklerini değerlendirir ve rehberlik, Öngörüler ve Azure'a geçirmenizde yardımcı mekanizmaları sağlar. Geçiş planınızla hazır olduktan sonra makineleri Azure'a geçirmek için Azure Site Recovery ve Azure veritabanı geçiş hizmeti gibi hizmetleri kullanabilirsiniz.

**Hyper-v'den azure'a geçiş**: Azure geçişi şu anda yalnızca destekler değerlendirme VMware sanal makinelerini Azure'a geçiş için. Hyper-V desteği, Azure geçişi için yol haritası açıktır. Bu arada, Site Recovery dağıtım Planlayıcısı'nı kullanabilirsiniz. Hyper-V desteği, Azure Geçişi'nde etkinleştirildikten sonra Azure geçişi Hyper-V iş yüklerinin geçişini planlama için kullanabilirsiniz.

**Vmware'den/Hyper-v'den azure'a olağanüstü durum kurtarma**: Azure Site Recovery (Site Recovery) kullanarak azure'da olağanüstü durum kurtarma (DR) yapmak istiyorsanız, Site Recovery dağıtım Planlayıcısı planlama DR için kullanın. Site Recovery dağıtım Planlayıcısı, bir şirket içi ortamınızı kapsamlı ve ASR özgü değerlendirmesinin yapar. Bu, çoğaltma, yük devretme sanal makinelerinizin gibi başarılı DR işlemler için Site Recovery tarafından gerekli önerileri sağlar.  

### <a name="which-azure-regions-are-supported-by-azure-migrate"></a>Hangi Azure bölgeleri, Azure geçişi tarafından destekleniyor mu?

Azure geçişi şu anda Doğu ABD ve Batı Orta ABD geçiş projesi konumları destekler. Yalnızca Batı Orta ABD ve Doğu ABD geçiş projeleri oluşturabilirsiniz, ancak yine de makineleriniz için değerlendirebilirsiniz [birden çok hedef konumları](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). Proje konumu yalnızca bulunan verileri depolamak için kullanılır.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Şirket içi siteyle Azure geçişi için nasıl bağlanıyor?

Bağlantı ortak eşleme ExpressRoute kullanabilir veya internet üzerinden olabilir.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Ayarlama VM sağlamlaştırmak. OVA şablon?

Ek bileşenler (örneğin, virüsten koruma) içine eklenebilir. OVA şablonu çalışmak Azure geçişi Gereci için gerekli iletişim ve güvenlik duvarı kuralları olarak bırakılır sürece içindir.   

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

Aracı tabanlı bulma, gereç tabanlı bulma ile birlikte kullanılabilecek bir seçenektir ve müşterilerin şirket içi VM'ler üzerindeki [bağımlılıkları görselleştirmesine](how-to-create-group-machine-dependencies.md) yardımcı olur. Bağımlılık aracısı FQDN, işletim sistemi, IP adresi, MAC adresi, VM içinde çalışan işlemler ve VM'den gelen/giden TCP bağlantıları gibi bilgileri toplar. Aracı tabanlı bulma isteğe bağlıdır ve sanal makinelerin bağımlılıklarını görselleştirin istemiyorsanız aracıları yüklememeyi seçebilirsiniz.

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Analiz edilen ESXi ana bilgisayar ortamının üzerinde bir performans etkisi olması?

Durumunda, [zaman bulma yaklaşımlardan](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods)performans verilerini toplamak için vCenter sunucusundaki istatistik düzeyini 3 olarak ayarlamanız gerekir. Bu düzeyini ayarlamak vCenter Server veritabanında depolanan veriler, sorun giderme işlemlerinin büyük bir miktar toplayın. Bu nedenle vcenter Server'da bazı performans sorunlarına neden olabilir. ESXi ana bilgisayarındaki göz ardı edilebilir etkisi olacaktır.

(Önizleme aşamasında olan) performans verilerinin sürekli profil oluşturma ekledik. Sürekli profil oluşturma ile artık yoktur vCenter Server istatistik düzeyini performans tabanlı bir değerlendirme çalıştırmak için değiştirmeniz gerekmez. Toplayıcı gerecini artık sanal makinelerin performans verileri şirket içi makinelerin profil. VCenter Server yanı sıra ESXi konakları bu neredeyse sıfır performans etkisi yoktur.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan verileri uzun depolanan ve için nasıl nerede?

Toplayıcı Gereci tarafından toplanan veriler, geçiş projesi oluştururken belirttiğiniz Azure konumunda depolanır. Veriler Microsoft abonelikte güvenli bir şekilde depolanır ve kullanıcı Azure geçişi projesini sildiğinde silinir.

Aracıları Vm'lerde yüklerseniz bağımlılık görselleştirmesi için bağımlılık aracısı tarafından toplanan verileri ABD kullanıcının abonelikte oluşturulan bir Log Analytics çalışma alanında depolanır. Aboneliğinizde Log Analytics çalışma alanını sildiğinizde, bu verileri silinir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Veriler beklerken ve aktarım sırasında şifrelenir?

Evet, toplanan verileri hem bekleyen hem aktarım sırasında şifrelenir. Gereç tarafından toplanan meta veriler güvenli bir şekilde Azure geçişi hizmetine internet üzerinden https gönderilir. Toplanan meta veriler depolanır [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) ve [Azure blob depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) Microsoft Abonelikteki ve bekleme sırasında şifrelenir.

Bağımlılık aracısı tarafından toplanan veriler de içinde şifrelenmiş Aktarım (güvenli bir https kanalı) ve bir kullanıcının aboneliğindeki Log Analytics çalışma alanında depolanır. Ayrıca, bekleme sırasında de şifrelenir.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Toplayıcı, vCenter Server ve Azure geçişi hizmeti ile nasıl iletişim?

Toplayıcı Gereci vCenter Server'a (bağlantı noktası 443) bağlanan gereç kullanıcı tarafından sağlanan kimlik bilgilerini kullanarak. Bu, vCenter Server vCenter Server tarafından yönetilen sanal makineleri ile ilgili meta verileri toplamak için VMware powerclı'yı kullanarak sorgular. Her iki yapılandırma verilerini VM'ler (çekirdekler, bellek, disk, NIC vb.) hakkında vCenter Server'dan tek tek son bir ay boyunca her VM'nin performans geçmişi yanı sıra toplar. Toplanan meta veriler daha sonra değerlendirmesi için Azure geçişi hizmeti (internet üzerinden https üzerinden) gönderilir. [Daha fazla bilgi](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>Birden fazla vCenter sunucunuz için aynı Toplayıcı gerecini bağlanabilir miyim?

Evet, bir tek Toplayıcı gerecini birden fazla vCenter sunucularını bulmak için kullanılabilir ancak aynı anda değil. Bulmaları birbiri ardına çalıştırmanız gerekir.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Olduğu. Site Recovery tarafından kullanılan OVA şablonu ile tümleştirilmiştir. Azure geçişi tarafından kullanılan OVA?

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
  

## <a name="dependency-visualization"></a>Bağımlılık görselleştirme

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Bağımlılık görselleştirmesi özelliğini kullanmak için ücret ödemem gerekiyor mu?

Azure Geçişi ek ücret ödenmeden kullanılabilir. Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için mevcut bir çalışma alanını kullanabilir miyim?

Evet, Azure geçişi, geçiş projesine mevcut bir çalışma alanı ekleyin ve bağımlılık görselleştirmesi için yararlanarak olanak sağlıyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, bağımlılık görselleştirme raporunu dışarı aktarmak mümkün değildir. Ancak, Azure geçişi kullandığı hizmet eşlemesi için bağımlılık görselleştirmeyi kullanabilirsiniz [hizmet eşlemesi REST API'lerini](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) bağımlılıkları bir json biçiminde alın.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükleme nasıl otomatikleştirebilirim?

[Burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) bağımlılık aracısını yükleme için kullanabileceğiniz bir komut dosyasıdır. MMA için [burada](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab) yararlanabileceğiniz TechNet üzerindeki bir komut dosyası kullanılabilir.

Betiklerin yanı sıra System Center Configuration Manager (SCCM) gibi dağıtım araçları da yararlanabilirsiniz [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) vb. aracıları dağıtmak için.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>MMA'yı tarafından desteklenen işletim sistemleri nelerdir?

MMA'yı tarafından desteklenen Windows işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
MMA'yı tarafından desteklenen Linux işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Bağımlılık aracısı tarafından desteklenen işletim sistemleri nelerdir?

Bağımlılık aracısı tarafından desteklenen Windows işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
Bağımlılık aracısı tarafından desteklenen Linux işletim sistemleri listesi [burada](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Bağımlılıklar Azure Geçişi'ndeki bir saat süresinden daha fazla bilgi için görselleştirebilirsiniz?
Hayır, Azure geçişi, en fazla bir saatlik süre için bağımlılıkları görselleştirme sağlar. Azure geçişi belirli bir tarihe kadar son bir ay için geçmişte dönün sağlar, ancak için bağımlılıkları görselleştirebilirsiniz en fazla süre 1 saate kadar uzun. Örneğin, Dün için bağımlılıkları görüntülemek için bağımlılık Haritası saati süresi işlevleri kullanabilirsiniz ancak yalnızca bir için bir saat penceresinde görüntüleyebilirsiniz.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>Bağımlılık görselleştirmesi 10'dan fazla Vm'leri gruplar için destekleniyor mu?
Yapabilecekleriniz [grupları için bağımlılıkları görselleştirme](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) sahip yukarı 10 VM için 10'dan fazla vm'lerle grubunuz varsa öneririz, grupta küçük kullanıcı gruplarına bölün ve bağımlılıklarını görselleştirin.


## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure geçişi genel bakış](migrate-overview.md)
- Öğrenin [bulma ve değerlendirme](tutorial-assessment-vmware.md) VMware ortamı
