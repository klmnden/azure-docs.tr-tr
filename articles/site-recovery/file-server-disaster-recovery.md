---
title: Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz
description: Bu makalede, Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz açıklanır
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 0b6d5dccbce30c55e259e4bb3f8ae4194a02b646
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916892"
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz 

[Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, yönetir ve şirket içi makinelerin ve Azure sanal makineleri (VM'ler) olağanüstü durum kurtarma işlemlerini yönetir. Çoğaltma, yük devretme ve kurtarma çeşitli iş yükleri, olağanüstü durum kurtarma içerir.

Bu makalede Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz açıklar ve çeşitli ortamlar uyacak şekilde diğer öneriler sağlar. 

- [Azure Iaas dosya sunucusu makineleri çoğaltma](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Site Recovery kullanarak bir şirket içi dosya sunucusunu Çoğalt](#replicate-an-on-premises-file-server-by-using-site-recovery)

## <a name="file-server-architecture"></a>Dosya sunucu mimarisi
Bir açık dağıtılmış dosya paylaşım sistemi amacı verimli bir şekilde dosyaları üzerinde çalışın ve tutarlılığı gereksinimlerine uygulanmasını garanti coğrafi olarak dağıtılmış bir kullanıcı grubu burada imkan tanıyacak bir ortamı sağlamaktır. Çok sayıda eşzamanlı kullanıcıyı ve çok sayıda içerik öğeleri destekleyen tipik şirket içinde bir dosya sunucusu ekosistem yinelemesinin zamanlanması ve bant genişliği azaltma için Dağıtılmış dosya sistemi çoğaltma (DFSR) kullanır. 

DFSR olarak uzaktan değişiklikleri sıkıştırma (dosyaları sınırlı bant genişliğine sahip ağ üzerinden verimli bir şekilde güncelleştirmek için kullanılan RDC) bilinen bir sıkıştırma algoritması kullanır. Eklemeler, çıkarma ve ilgili veri dosyalarında algılar. DFSR dosyaları güncelleştirildiğinde yalnızca değiştirilen dosya bloklarını çoğaltmak için etkinleştirilir. Dosya sunucu ortamlarında, günlük yedeklemeler olağanüstü durum ihtiyaçlarınıza göre değiştirebileceğiniz yoğun olmayan zamanlamaları, nerede alınır vardır. DFSR uygulanmadı.

Aşağıdaki diyagram, uygulanan DFSR ile dosya sunucusu ortamı gösterir.
                
![DFSR mimarisi](media/site-recovery-file-server/dfsr-architecture.JPG)

Önceki diyagramda, üyeleri etkin olarak adlandırılan birden çok dosya sunucuları arasında bir çoğaltma grubu dosyaları çoğaltmasında katılın. Üye çevrimdışı olsa bile çoğaltılmış klasörün içeriğini üyeleri birini istekleri göndermek tüm istemciler tarafından kullanılabilir.

## <a name="disaster-recovery-recommendations-for-file-servers"></a>Dosya sunucuları için olağanüstü durum kurtarma önerileri

* **Site Recovery kullanarak bir dosya sunucusu çoğaltma**: dosya sunucularında çoğaltabilirsiniz için Azure Site Recovery kullanarak. Bir veya daha fazla şirket içi dosya sunucularını erişilemediğinde Vm'leri kurtarma Azure'da getirilebilir. Vm'leri istekleri istemcilerden görebilir, var. sağlanan şirket içinde siteden siteye VPN bağlantısı olan ve Active Directory Azure'da yapılandırılır. Hiçbir DFSR ile bir DFSR yapılandırılmış ortam ya da bir basit dosya sunucusu ortamı söz konusu olduğunda bu yöntemi kullanabilirsiniz. 

* **Azure Iaas VM'LERİNİ DFSR genişletmek**: uygulanan DFSR ile bir kümelenmiş dosya sunucusu ortamı şirket içi DFSR Azure'a genişletebilirsiniz. Bir Azure VM ardından dosya sunucusu rolünü gerçekleştirmek için etkinleştirilmiş. 

    * Siteden siteye VPN bağlantısı ve Active Directory bağımlılıkları işlenir ve bir veya daha fazla şirket içi dosya sunucularını erişilemediğinde DFSR yerinde olduktan sonra istemciler isteklere hizmet Azure VM için bağlanabilir.

    * Sanal makinelerinizin Site Recovery tarafından desteklenmeyen yapılandırmalar varsa bu yaklaşımı kullanabilirsiniz. Dosya sunucusu ortamında bazen yaygın olarak kullanılan bir paylaşılan küme diskine buna bir örnektir. DFSR da Orta karmaşıklık oranı ile düşük bant genişliğine sahip ortamlarda iyi çalışır. Bir Azure VM sahip olunması ve çalıştıran her zaman ek maliyeti göz önünde bulundurmanız gerekir. 

* **Dosyalarınızı çoğaltmak için Azure dosya eşitleme kullanmak**: Bulut veya zaten bir Azure VM kullanın planlıyorsanız, Azure dosya eşitleme kullanabilirsiniz. Azure dosya eşitleme sunan sektör standardı erişilebilen bulutta tamamen yönetilen dosya paylaşımları eşitleme [sunucu ileti bloğu](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (SMB) protokolü. Azure dosya paylaşımlarını ardından Windows, Linux ve macOS Bulut ve şirket içi dağıtımları tarafından aynı anda bağlanabilir. 

Aşağıdaki diyagramda, dosya sunucu ortamınıza kullanmak için hangi stratejisini belirlemenize yardımcı olur.

![Karar ağacı](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-in-your-decisions-about-disaster-recovery-to-azure"></a>İçinde kararlarınızı azure'a olağanüstü durum kurtarma hakkında dikkat edilecek noktalar

|Ortam  |Öneri  |Dikkat edilmesi gereken noktalar |
|---------|---------|---------|
|Dosya sunucusu ortamı ile veya olmadan DFSR|   [Site Recovery çoğaltması için kullanın.](#replicate-an-on-premises-file-server-by-using-site-recovery)   |    Site Recovery, paylaşılan disk kümelerini veya ağa bağlı depolama (NAS) desteklememektedir. Herhangi diğer bir yaklaşım, ortamınızı Bu yapılandırmalar kullanıyorsa, uygun şekilde kullanın. <br> Site Recovery, SMB 3.0 desteklememektedir. Yalnızca dosyalarda yapılan değişiklikleri dosyalarının özgün konumda güncelleştirildiğinde çoğaltılan sanal Makineye değişiklikleri içerir.
|DFSR ile dosya sunucusu ortamı     |  [Bir Azure Iaas sanal makinesine DFSR genişletme](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR de son derece bant genişliği crunched ortamlarda çalışır. Bu yaklaşım, Azure VM ve her zaman çalışması gerektirir. VM maliyetini planlamanızda hesabı gerekir.         |
|Azure Iaas VM     |     [Dosya eşitleme ](#use-azure-file-sync-service-to-replicate-your-files)   |     Dosya eşitleme bir olağanüstü durum kurtarma senaryosunda kullanırsanız, yük devretme sırasında saydam bir şekilde dosya paylaşımlarını istemci makine için erişilebilir olduğundan emin olmak için el ile işlemler yapmanız gerekir. Dosya eşitleme bağlantı noktası 445'in istemci makinesinden açık olmasını gerektirir.     |


### <a name="site-recovery-support"></a>Site kurtarma desteği
Site Recovery çoğaltma uygulamadan bağımsız olduğundan, bu öneriler aşağıdaki senaryolar için doğru tutun beklenir.
| Kaynak    |İkincil bir siteye    |Azure'a
|---------|---------|---------|
|Azure| -|Evet|
|Hyper-V|   Evet |Evet
|VMware |Evet|   Evet
|Fiziksel sunucu|   Evet |Evet
 

> [!IMPORTANT]
> Aşağıdaki üç yaklaşımlardan birini ile devam etmeden önce bu bağımlılıklar dikkate emin olun.

**Siteden siteye bağlantı**: sunucular arasındaki iletişime izin vermek için şirket içi site ile Azure ağı arasında doğrudan bir bağlantı kurulması gerekir. Olağanüstü durum kurtarma siteniz olarak kullanılan bir Azure sanal ağa güvenli bir siteden siteye VPN bağlantısı kullanın. Daha fazla bilgi için [bir şirket içi site ve bir Azure sanal ağ arasında siteden siteye VPN bağlantısı kuracak](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Active Directory**: Active Directory DFSR bağlıdır. Başka bir deyişle, yerel etki alanı denetleyicileri ile Active Directory ormanı için azure'da olağanüstü durum kurtarma sitesi genişletilir. Hedeflenen kullanıcılar erişim verilen veya erişimi için doğrulanmış gerekiyorsa, DFSR kullanmıyorsanız bile, bu adımları atmanız gerekir. Daha fazla bilgi için [Genişlet şirket içi Azure Active Directory'ye](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Azure Iaas sanal makineler için olağanüstü durum kurtarma önerisi

Yapılandırmakta ve Azure Iaas Vm'lerinde barındırılan dosya sunucuları için olağanüstü durum kurtarmayı yönetme, iki seçenek arasından seçim yapabilirsiniz, taşımak istediğinize bağlı [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction):

* [Dosya eşitleme kullanın](#use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine)
* [Site RECOVERY'yi kullanın](#replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery)

## <a name="use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine"></a>Bir Iaas sanal makinesinde barındırılan dosya çoğaltmak için dosya eşitleme kullanın

Azure Dosyaları geleneksel şirket içi dosya sunucularını veya NAS cihazlarını tamamen değiştirmek veya desteklemek için kullanılabilir. Azure dosya paylaşımları Windows sunucuları, şirket içinde veya bulutta, performans ve verilerin kullanıldığı dağıtılmış önbelleğe alma için dosya eşitleme ile de çoğaltılabilir. Aşağıdaki adımlar, geleneksel dosya sunucuları aynı işlevi gerçekleştiren Azure Vm'leri için olağanüstü durum kurtarma öneri göstermektedir:
* Site Recovery kullanarak makineleri koruyun. Bağlantısındaki [bir Azure VM'yi başka bir Azure bölgesine çoğaltma](azure-to-azure-quickstart.md).
* Dosya eşitleme bulut dosya sunucusu olarak davranan VM'den dosyaları çoğaltmak için kullanın.
* Site RECOVERY'yi [kurtarma planı](site-recovery-create-recovery-plans.md) betiklere Ekle özelliğini [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makinenizde paylaşıma erişme.

Aşağıdaki adımlar, dosya eşitleme'yi kullanmayı kısaca açıklayın:

1. [Azure'da bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Depolama hesaplarınız için okuma erişimli coğrafi olarak yedekli depolama'yı seçerseniz, okuma erişimi verilerinize bir olağanüstü durum oluşması halinde ikincil bölgeden olursunuz. Daha fazla bilgi için [Azure dosya paylaşımı olağanüstü durum kurtarma stratejileri](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Dosya paylaşımı oluşturma](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Dosya eşitlemeyi başlatmak](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) Azure dosya sunucunuzda.
4. Bir eşitleme grubu oluşturma. Bir eşitleme grubu içindeki uç noktalar birbiriyle eşitlenmiş olarak tutulur. Azure dosya paylaşımını temsil eden en az bir bulut uç noktası, bir eşitleme grubu içermelidir. Bir eşitleme grubu da bir Windows sunucusundaki bir yola temsil eden bir sunucu uç noktası, içermelidir.
5. Dosyalarınız artık Azure dosya paylaşımınızı ve şirket içi sunucunuz arasında eşitlenmiş durumda tutulur.
6. Şirket içi ortamınızdaki bir olağanüstü durumda kullanarak bir yük devretme gerçekleştirme bir [kurtarma planı](site-recovery-create-recovery-plans.md). Betiğe ekleme [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makinenizde paylaşıma erişme.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery"></a>Site Recovery kullanarak bir Iaas dosya sunucusu sanal makine çoğaltma

Iaas dosya sunucusu sanal makine erişen şirket içi istemcileriniz varsa, aşağıdaki adımları atın. Aksi takdirde, 3. adımına geçin.

1. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun.
2. Şirket içi Active Directory'ye genişletin.
3. [Olağanüstü durum kurtarma ayarlama](azure-to-azure-tutorial-enable-replication.md) Iaas dosya sunucu makinesine ikincil bir bölgeye için.


İkincil bir bölgeye olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz. [bu makalede](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-by-using-site-recovery"></a>Site Recovery kullanarak bir şirket içi dosya sunucusunu Çoğalt

Aşağıdaki adımlar, bir VMware sanal makinesi için çoğaltma açıklar. Bir Hyper-V VM çoğaltmak adımlar için bkz: [Bu öğreticide](tutorial-hyper-v-to-azure.md).

1. [Azure kaynaklarını hazırlama](tutorial-prepare-azure.md) şirket içi makinelerin çoğaltması için.
2. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun. 
3. Şirket içi Active Directory'ye genişletin.
4. [Şirket içi VMware sunucularını hazırlama](tutorial-prepare-on-premises-vmware.md).
5. [Olağanüstü durum kurtarma ayarlama](tutorial-vmware-to-azure.md) için şirket içi Vm'leri azure'a.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Bir Azure Iaas sanal makinesine DFSR genişletme

1. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun. 
2. Şirket içi Active Directory'ye genişletin.
3. [Oluşturma ve dosya sunucusu VM sağlama](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) Azure sanal ağı üzerinde.
Çapraz bağlantısı şirket içi ortamı olan aynı Azure sanal ağ, sanal makine eklendiğinden emin olun. 
4. Yükleme ve [DFSR yapılandırma](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) Windows Server'da.
5. [DFS ad alanı uygulamak](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6. Uygulanan DFS ad alanı ile DFS ad alanı klasörü hedefleri güncelleştirerek üretimden paylaşılan klasörler için olağanüstü durum kurtarma sitelerinde yük devretmesi gerçekleştirebilirsiniz. Sonra bu DFS ad alanı değişiklikleri Active Directory çoğaltma kullanıcıların bağlı için uygun klasör hedeflerinin şeffaf bir şekilde.

## <a name="use-file-sync-to-replicate-your-on-premises-files"></a>Dosya eşitleme, şirket içi dosya çoğaltmak için kullanın
Dosya eşitleme dosyaları buluta çoğaltmak için kullanabilirsiniz. Olağanüstü bir durum ve şirket içi dosya sunucunuzu kullanım dışı kalması durumunda, istenen dosya konumları buluttan bağlama ve hizmet istekleri için istemci makinelerden devam edebilirsiniz.
Dosya eşitleme Site Recovery ile tümleştirmek için:

* Site Recovery kullanarak dosya sunucusu makineleri koruyun. Bağlantısındaki [Bu öğreticide](tutorial-vmware-to-azure.md).
* Dosya, buluta bir dosya sunucusu olarak görev yapan makineden çoğaltmak için dosya eşitleme'yi kullanın.
* Kurtarma planı özelliği Site Recovery, azure'da VM başarısız dosya sunucusundaki Azure dosya paylaşımını bağlayabilmeniz için komut dosyası eklemek için kullanın.

Dosya eşitleme kullanmak için aşağıdaki adımları izleyin:

1. [Azure'da bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Okuma erişimli coğrafi olarak yedekli depolama (depolama hesaplarınız için önerilir) seçerseniz, bir olağanüstü durum oluşması halinde ikincil bölgeden verilerinize okuma erişimine sahip. Daha fazla bilgi için [Azure dosya paylaşımı olağanüstü durum kurtarma stratejileri](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Dosya paylaşımı oluşturma](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Dosya eşitleme işlemi dağıtma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) , şirket içi dosya sunucusunda.
4. Bir eşitleme grubu oluşturma. Bir eşitleme grubu içindeki uç noktalar birbiriyle eşitlenmiş olarak tutulur. Azure dosya paylaşımını temsil eden en az bir bulut uç noktası, bir eşitleme grubu içermelidir. Eşitleme grubu Ayrıca, şirket içi Windows server üzerinde bir yolu temsil eden bir sunucu uç noktası, içermesi gerekir.
5. Dosyalarınız artık Azure dosya paylaşımınızı ve şirket içi sunucunuz arasında eşitlenmiş durumda tutulur.
6. Şirket içi ortamınızdaki bir olağanüstü durumda kullanarak bir yük devretme gerçekleştirme bir [kurtarma planı](site-recovery-create-recovery-plans.md). Azure dosya paylaşımını bağlama ve sanal makinenizde paylaşımına erişmek için betiği ekleyin.

> [!NOTE]
> Bağlantı noktası 445'in açık olduğundan emin olun. Azure dosyaları SMB protokolünü kullanır. SMB, TCP bağlantı noktası 445 üzerinden iletişim kurar. Güvenlik duvarınızı bir istemci bir makineden TCP bağlantı noktası 445'i engellemediğinden denetleyin.


## <a name="do-a-test-failover"></a>Yük devretme testi yapın

1. Azure portalına gidin ve kurtarma Hizmetleri kasanızı seçin.
2. Dosya sunucusu ortamı için oluşturduğunuz kurtarma planı seçin.
3. Seçin **yük devretme testi**.
4. Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
5. İkincil ortamı kurma olduktan sonra doğrulamaları gerçekleştirin.
6. Doğrulama tamamlandıktan sonra seçin **yük devretme testini Temizle** kurtarma planı ve test yük devretmesi ortam temizlendiği.

Yük devretme testi gerçekleştirme hakkında daha fazla bilgi için bkz. [Site recovery'ye yük devretme testi](site-recovery-test-failover-to-azure.md).

Active Directory ve DNS için yük devretme testi yapılması ile ilgili yönergeler için bkz: [Test Active Directory ve DNS için yük devretme konuları](site-recovery-active-directory.md).

## <a name="do-a-failover"></a>Bir yük devretme yapın

1. Azure portalına gidin ve kurtarma Hizmetleri kasanızı seçin.
2. Dosya sunucusu ortamı için oluşturduğunuz kurtarma planı seçin.
3. Seçin **yük devretme**.
4. Yük devretme işlemini başlatmak için kurtarma noktasını seçin.

Bir yük devretme gerçekleştirme hakkında daha fazla bilgi için bkz. [Site recovery'de yük devretme](site-recovery-failover.md).
