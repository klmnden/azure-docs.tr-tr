---
title: Azure geçişi - sık sorulan sorular (SSS) | Microsoft Docs
description: Azure geçişi hakkında sık sorulan sorular adresleri
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: snehaa
ms.openlocfilehash: 3f035f38b1ad68e9e39d151ffad3fc650a0a1d80
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952758"
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

Azure geçişi şu anda Doğu ABD ve Batı Orta ABD geçiş projesi konumları destekler. Yalnızca Batı Orta ABD ve Doğu ABD geçiş projeleri oluşturabilirsiniz olsa da, makineleriniz için hala değerlendirebilirsiniz Not [birden çok hedef konumları](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). Proje konumu yalnızca bulunan verileri depolamak için kullanılır.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Şirket içi siteyle Azure geçişi için nasıl bağlanıyor?

Bağlantı ortak eşleme ExpressRoute kullanabilir veya internet üzerinden olabilir.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Ayarlama VM sağlamlaştırmak. OVA şablon?

Ek bileşenler (örneğin, virüsten koruma) içine eklenebilir. OVA şablonu çalışmak Azure geçişi Gereci için gerekli iletişim ve güvenlik duvarı kuralları olarak bırakılır sürece içindir.   

## <a name="discovery-and-assessment"></a>Keşif ve değerlendirme

### <a name="what-data-is-collected-by-azure-migrate"></a>Azure geçişi tarafından verilerin ne toplanır?

Azure geçişi, iki çeşit bulma, gereç tabanlı bulma ve aracı tabanlı bulma destekler.
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

Aracı tabanlı bulma alet tabanlı bulma üzerinde sunulan bir seçenektir ve müşterilere yardımcı [bağımlılıkları görselleştirme](how-to-create-group-machine-dependencies.md) şirket içi sanal makinelerin. Bağımlılık aracıları toplamak ayrıntıları gibi FQDN, işletim sistemi, IP adresi, MAC adresi, VM'den VM ve gelen ve giden TCP bağlantılarını içinde çalışan işlemler. Aracı tabanlı bulma isteğe bağlıdır ve sanal makinelerin bağımlılıklarını görselleştirin istemiyorsanız aracıları yüklememeyi seçebilirsiniz.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan verileri uzun depolanan ve için nasıl nerede?

Toplayıcı Gereci tarafından toplanan veriler, geçiş projesi oluştururken belirttiğiniz Azure konumunda depolanır. Veriler Microsoft abonelikte güvenli bir şekilde depolanır ve kullanıcı Azure geçişi projesini sildiğinde silinir.

Aracıları Vm'lerde yüklerseniz bağımlılık görselleştirmesi için bağımlılık aracısı tarafından toplanan verileri ABD kullanıcının abonelikte oluşturulan bir OMS çalışma alanına depolanır. Aboneliğinizde OMS çalışma alanını sildiğinizde, bu verileri silinir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

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

Değerlendirmek istediğiniz VM ayarlarını değiştirirseniz, tetikleyici keşfedin yeniden Toplayıcı gerecini kullanarak. Gereci kullanın **koleksiyonu yeniden Başlat** Bunu yapmak için seçeneği. Koleksiyon tamamlandıktan sonra seçin **yeniden hesapla** güncelleştirilmiş değerlendirme sonuçlarını almak için Portalı'nda değerlendirmesi için seçeneği.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Azure Geçişi'ndeki çok kiracılı bir ortam nasıl bulabilecek kişileri?

Kiracılar genelinde paylaşılan bir ortamda varsa ve bir kiracının başka bir kiracının Abonelikteki sanal makinelerin keşfetmek istiyor musunuz, bulma kapsamı için toplayıcı gerecini kapsam alanı kullanabilirsiniz. Kiracıların konaklar paylaşıyorsanız, yalnızca belirli kiracıya ait sanal makinelerin salt okunur erişimi olan bir kimlik bilgisi oluşturmak ve ardından bu kimlik bilgisi Toplayıcı Gereci kullanabilir ve keşif yapmak için ana bilgisayarı olarak kapsamını belirtin. Alternatif olarak, vcenter Server (Klasör1 tenant1 için ve klasör2 tenant2 için diyelim), paylaşılan konak altında klasör oluşturabilirsiniz tenant2 ve Klasör1 içine tenant1 için Vm'leri klasör2 taşıyın ve bulmaları, Toplayıcı'ı uygun şekilde kapsam uygun bir klasör belirterek.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Kaç tane sanal makineyi bir tek bir geçiş projesi içinde bulunabilir?

Tek geçişi projesinde 1500 sanal makineler bulabilir. Daha fazla makine şirket içi ortamınızda varsa [daha fazla bilgi edinin](how-to-scale-assessment.md) hakkında Azure Geçişi'ndeki büyük bir ortamı nasıl bulabilir.

## <a name="dependency-visualization"></a>Bağımlılık görselleştirme

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Bağımlılık görselleştirmesi özelliğini kullanmak için ücret ödemem gerekiyor mu?

Azure Geçişi ek ücret ödenmeden kullanılabilir. Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için mevcut bir çalışma alanını kullanabilir miyim?

Azure geçişi, bağımlılık görselleştirmesi için mevcut bir çalışma alanı kullanmayı desteklemez, ancak Microsoft Monitoring Agent (MMA) çoklu yönlendirmeyi destekler ve birden çok çalışma alanına veri göndermesini sağlar. Bu nedenle aracıları dağıttıktan ve yapılandırdıktan bir çalışma alanına zaten varsa, MMA aracısını, birden çok giriş yararlanın ve bunu Azure geçişi çalışma alanına (ek olarak var olan çalışma alanına) yapılandırabilir ve çalışması. [Burada](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) MMA Aracısı çoklu yönlendirmeyi nasıl olanak sağlayabileceğiniz üzerinde bir Web günlüğü.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure geçişi genel bakış](migrate-overview.md)
- Öğrenin [bulma ve değerlendirme](tutorial-assessment-vmware.md) VMware ortamı
