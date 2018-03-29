---
title: Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz
description: Bu makalede, Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz açıklar
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 830f9c76d9d1bf11692fa9f2f5c49cbecdb69f25
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz 

[Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery yönetir ve şirket içi makineler ve Azure sanal makineleri (VM'ler) olağanüstü durum kurtarma düzenler. Olağanüstü durum kurtarma, çoğaltma, yük devretme ve kurtarma çeşitli iş yüklerinin içerir.

Bu makalede Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz açıklar ve çeşitli ortamlar uyacak şekilde diğer öneriler sağlar. 

- [Azure Iaas dosya sunucusu makineleri çoğaltma](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Site RECOVERY'yi kullanarak bir şirket içi dosya sunucusunu Çoğalt](#replicate-an-on-premises-file-server-by-using-site-recovery)

## <a name="file-server-architecture"></a>Dosya sunucu mimarisi
Coğrafi olarak dağıtılmış kullanıcılar grubunun verimli bir şekilde dosyalar üzerinde çalışması ve tutarlılığı gereksinimlerine zorlandığını garanti burada çalışabileceğiniz bir ortam sağlamak için bir açık dağıtılmış dosya paylaşımı sistem amacı olan. Çok sayıda eşzamanlı kullanıcı ve çok sayıda içerik öğelerini destekleyen tipik şirket içi bir dosya sunucusu ekosistem yinelemesinin zamanlanması ve bant genişliği azaltma için Dağıtılmış dosya sistemi çoğaltma (DFSR) kullanır. 

DFSR olarak uzaktan değişiklikleri sıkıştırma (dosyalarını sınırlı bant genişliğine sahip ağ üzerinden verimli bir şekilde güncelleştirmek için kullanılan RDC) bilinen bir sıkıştırma algoritması kullanır. Eklemeler, kaldırma işlemleri ve veri ilgili dosyalarında algılar. DFSR dosyalar güncelleştirildiğinde yalnızca değiştirilen dosya bloklarını etkinleştirilir. Dosya sunucu ortamları günlük yedeklemeler için olağanüstü durum gereksinimlerini karşılamak yoğun olmayan zamanlamaları burada alınır, vardır. DFSR uygulanmadı.

Aşağıdaki diyagramda uygulanan DFSR ile dosya sunucusu ortamı gösterilmektedir.
                
![DFSR mimarisi](media/site-recovery-file-server/dfsr-architecture.JPG)

Önceki diyagramda üyeleri etkin olarak adlandırılan birden çok dosya sunucuları arasında bir çoğaltma grubu dosyaları çoğaltmasında katılın. Üye çevrimdışı olması durumunda bile çoğaltılmış klasörün içeriğini ya da üyelerinin istekleri gönderen tüm istemciler tarafından kullanılabilir.

## <a name="disaster-recovery-recommendations-for-file-servers"></a>Dosya sunucuları için olağanüstü durum kurtarma önerileri

* **Site RECOVERY'yi kullanarak bir dosya sunucusunda çoğaltmak**: dosya sunucuları çoğaltılabilir için Azure Site Recovery kullanarak. Bir veya daha fazla şirket içi dosya sunucularına erişilemez durumda olduğunda, sanal makineleri kurtarma Azure'da getirilebilir. Sanal makineleri istekleri istemcilerden görebilir, var. sağlanan şirket içi, siteden siteye VPN bağlantısı olan ve Active Directory ile Azure yapılandırılır. Hiçbir DFSR ile DFSR yapılandırılmış bir ortam ya da bir basit dosya sunucu ortamı söz konusu olduğunda bu yöntemi kullanabilirsiniz. 

* **Bir Azure Iaas sanal DFSR genişletmek**: bir kümelenmiş dosya sunucusu ortamında uygulanan DFSR ile şirket içi DFSR Azure'a genişletebilirsiniz. Bir Azure VM ardından dosya sunucusu rolünü gerçekleştirmek için etkinleştirilmiş. 

    * Siteden siteye VPN bağlantısı ve Active Directory bağımlılıkları işlenir ve bir veya daha fazla şirket içi dosya sunucuları erişilemediğinde DFSR yerinde olduktan sonra istemcileri isteklere hizmet Azure VM için bağlanabilir.

    * Site Recovery tarafından desteklenmeyen yapılandırmalar Vm'leriniz varsa, bu yaklaşım kullanabilirsiniz. Dosya sunucusu ortamında bazen yaygın olarak kullanılan bir paylaşılan küme diski örneğidir. DFSR de Orta karmaşıklık oranı ile düşük bant genişlikli ortamlarda iyi çalışır. Bir Azure VM sahip ve her zaman çalışması ek maliyet dikkate almanız gerekir. 

* **Dosyalarınızı çoğaltmak için Azure dosya eşitleme kullanın**: Bulut kullanın veya zaten bir Azure VM kullanmayı düşünüyorsanız, Azure dosya eşitleme kullanabilirsiniz. Azure dosya eşitleme sunar, endüstri standardı erişilebilen tam olarak yönetilen dosya paylaşımlarının bulutta eşitleniyor [sunucu ileti bloğu](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (SMB) protokolü. Azure dosya paylaşımları, Windows, Linux ve macOS Bulut veya şirket içi dağıtımlar tarafından sonra bir eşzamanlı olarak bağlanabilir. 

Aşağıdaki diyagramda, dosya sunucu ortamınıza kullanmak için hangi stratejisi belirlemenize yardımcı olur.

![karar ağacı](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-in-your-decisions-about-disaster-recovery-to-azure"></a>Kararlarınıza Azure olağanüstü durum kurtarma hakkında dikkat edilecek noktalar

|Ortam  |Öneri  |Dikkate alınacak noktalar |
|---------|---------|---------|
|Dosya sunucusu ortamı ile veya olmadan DFSR|   [Site Recovery çoğaltma için kullanın](#replicate-an-on-premises-file-server-by-using-site-recovery)   |    Site Recovery paylaşılan disk kümelerini veya ağa bağlı depolama (NAS) desteklemiyor. Ortamınızı Bu yapılandırmalar kullanıyorsa, diğer yaklaşımlardan birini uygun şekilde kullanın. <br> Site Recovery, SMB 3.0 desteklemiyor. Yalnızca dosyalarda yapılan değişiklikler dosyalarının özgün konumda güncelleştirildiğinde çoğaltılmış VM'yi değişiklikleri içerir.
|DFSR ile dosya sunucusu ortamı     |  [Bir Azure Iaas sanal makineye DFSR genişletme](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR aşırı bant genişliği crunched ortamlarda iyi çalışır. Bu yaklaşım, çalışır durumda bir Azure VM ve her zaman çalışması gerekir. VM maliyet için planlamanızda hesabı gerekir.         |
|Azure Iaas VM     |     [File Sync ](#use-azure-file-sync-service-to-replicate-your-files)   |     Yük devretme sırasında bir olağanüstü durum kurtarma senaryosunda dosya eşitleme kullanırsanız, dosya paylaşımları saydam bir şekilde istemci makineye erişilebilir olduğundan emin olmak için el ile gerçekleştirilen eylemleri almanız gerekir. Dosya eşitleme bağlantı noktası 445'i istemci makineden açık olmasını gerektirir.     |


### <a name="site-recovery-support"></a>Site kurtarma desteği
Site Recovery çoğaltma uygulama belirsiz olduğundan, bu önerileri aşağıdaki senaryolar için doğru tutun beklenir.
| Kaynak    |İkincil bir siteye    |Azure'a
|---------|---------|---------|
|Azure| -|Evet|
|Hyper-V|   Evet |Evet
|VMware |Evet|   Evet
|Fiziksel sunucu|   Evet |Evet
 

> [!IMPORTANT]
> Aşağıdaki üç yaklaşımlardan birini ile devam etmeden önce bu bağımlılıklar dikkate emin olun.

**Siteden siteye bağlantı**: sunucuları arasında iletişim sağlamak için şirket içi site ile Azure ağı arasında doğrudan bir bağlantı kurulmalıdır. Olağanüstü durum kurtarma sitesi olarak kullanılan bir Azure sanal ağı güvenli bir siteden siteye VPN bağlantısı kullanın. Daha fazla bilgi için bkz: [bir şirket içi site ile Azure sanal ağı arasında siteden siteye VPN bağlantısı kuracak](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Active Directory**: DFSR üzerinde Active Directory bağlıdır. Bu, yerel etki alanı denetleyicileri ile Active Directory ormanı Azure olağanüstü durum kurtarma sitede genişletilir anlamına gelir. Hedeflenen kullanıcılar erişim verilir veya erişim için doğrulandı gerekiyorsa DFSR, kullanmadığınız olsa bile, şu adımları uygulamanız gerekir. Daha fazla bilgi için bkz: [Genişlet şirket içi Azure Active Directory'ye](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Azure Iaas sanal makineleri için olağanüstü durum kurtarma önerisi

Yapılandırmakta ve Azure Iaas Vm'leri üzerinde barındırılan dosya sunucularının olağanüstü durum kurtarmayı yönetme, iki seçenek arasından seçim yapabilirsiniz olursa, taşımak istediğiniz temel [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction):

* [Dosya eşitleme kullanın](#use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine)
* [Site Kurtarma'yı kullanma](#replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery)

## <a name="use-file-sync-to-replicate-files-hosted-on-an-iaas-virtual-machine"></a>Bir Iaas sanal makinede barındırılan dosya çoğaltmak için dosya eşitleme kullanın

Azure Dosyaları geleneksel şirket içi dosya sunucularını veya NAS cihazlarını tamamen değiştirmek veya desteklemek için kullanılabilir. Azure dosya paylaşımları Windows sunucuları, her iki şirket içi veya bulutta, performans ve dağıtılmış kullanıldığı verileri önbelleğe alma için dosya eşitleme ile aynı zamanda çoğaltılabilir. Aşağıdaki adımlar, Azure VM'ler, geleneksel dosya sunucuları aynı işlevleri gerçekleştirmek için olağanüstü durum kurtarma öneri açıklamaktadır:
* Site Recovery kullanarak makineler koruyun. Adımları [başka bir Azure bölgesine bir Azure VM çoğaltmak](azure-to-azure-quickstart.md).
* Bulut için dosya sunucusu gibi davranır VM'den dosya çoğaltmak için dosya eşitleme kullanın.
* Site Kurtarma'yı kullanmak [kurtarma planı](site-recovery-create-recovery-plans.md) komut dosyalarına eklemek için özellik [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makineniz paylaşımına erişim.

Aşağıdaki adımlar kısaca nasıl dosya eşitleme kullanılacağını açıklamaktadır:

1. [Bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Depolama hesapları için okuma erişimli coğrafi olarak yedekli depolama seçerseniz, okuma erişimi verilerinizi ikincil bölge kaybedildiği bir olağanüstü durumda alırlar. Daha fazla bilgi için bkz: [Azure dosya paylaşımı olağanüstü durum kurtarma stratejilerini](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Bir dosya paylaşımı oluşturmak](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Dosya eşitlemeyi Başlat](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) Azure dosya sunucunuzda.
4. Bir eşitleme grubu oluşturun. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş tutulur. Eşitleme grubunu bir Azure dosya paylaşımını gösteren en az bir bulut uç içermesi gerekir. Eşitleme grubu da bir Windows Server'da bir yol temsil eden bir sunucu endpoint içermelidir.
5. Dosyalarınızı şimdi, Azure dosya paylaşımı ve şirket içi sunucunuz arasında eşitleme tutulur.
6. Şirket içi ortamınızda bir olağanüstü durumda kullanarak bir yük devretme gerçekleştirme bir [kurtarma planı](site-recovery-create-recovery-plans.md). Komut dosyasına eklemek [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makineniz paylaşımına erişebilirsiniz.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-site-recovery"></a>Site RECOVERY'yi kullanarak bir Iaas dosya sunucusu sanal makinesi çoğaltma

Iaas dosya sunucusu sanal makineye erişim şirket içi istemcileriniz varsa, aşağıdaki adımları gerçekleştirin. Aksi takdirde, 3. adıma atlayın.

1. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun.
2. Şirket içi Active Directory genişletir.
3. [Olağanüstü durum kurtarma ayarlamak](azure-to-azure-tutorial-enable-replication.md) Iaas dosya sunucu makinesine ikincil bir bölgeye için.


İkincil bir bölgeye olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz: [bu makalede](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-by-using-site-recovery"></a>Site RECOVERY'yi kullanarak bir şirket içi dosya sunucusunu Çoğalt

Aşağıdaki adımlar, VMware sanal makine için çoğaltmayı açıklamaktadır. Bir Hyper-V VM çoğaltmak adımlar için bkz: [Bu öğretici](tutorial-hyper-v-to-azure.md).

1. [Azure kaynaklarını hazırlama](tutorial-prepare-azure.md) şirket içi makineler çoğaltılması için.
2. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun. 
3. Şirket içi Active Directory genişletir.
4. [Şirket içi VMware sunucuları hazırlama](tutorial-prepare-on-premises-vmware.md).
5. [Olağanüstü durum kurtarma ayarlamak](tutorial-vmware-to-azure.md) azure'a şirket içi VM'ler için.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Bir Azure Iaas sanal makineye DFSR genişletme

1. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun. 
2. Şirket içi Active Directory genişletir.
3. [Oluşturma ve bir dosya sunucusu VM sağlama](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) Azure sanal ağı üzerinde.
Sanal makine arası bağlantı şirket içi ortamı olan aynı Azure sanal ağı, eklendiğinden emin olun. 
4. Yükleme ve [DFSR yapılandırma](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) Windows Server'da.
5. [DFS ad alanı uygulamak](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6. DFS ad uygulanan DFS ad alanı klasörü hedefleri güncelleştirerek üretim paylaşılan klasörlerden olağanüstü durum kurtarma sitelere Yük Devretmesini yapılabilir. Sonra bu DFS ad alanı değişiklikleri Active Directory ile çoğaltmak kullanıcıların bağlı uygun klasör hedefleri saydam.

## <a name="use-file-sync-to-replicate-your-on-premises-files"></a>Şirket içi dosyalarınızı çoğaltmak için dosya eşitleme kullanın
Dosya eşitleme buluta nasıl dosya çoğaltmak için kullanabilirsiniz. Bir olağanüstü durum ve şirket içi dosya sunucusu olarak kullanım dışı kalması durumunda, istenen dosya konumları bulutta bağlama ve hizmet istekleri için istemci makinelerden devam edebilirsiniz.
Site Recovery ile dosya eşitleme tümleştirmeniz için:

* Site Recovery kullanarak dosya sunucusu makineleri koruyun. Adımları [Bu öğretici](tutorial-vmware-to-azure.md).
* Dosya eşitleme buluta bir dosya sunucusu olarak hizmet veren makineden dosya çoğaltmak için kullanın.
* Kurtarma planı özelliği Site kurtarma işlemi başarısız üzerinden dosya sunucusundaki Azure VM'de Azure dosya paylaşımını bağlama için komut dosyaları eklemek için kullanın.

Dosya eşitleme kullanmak için aşağıdaki adımları izleyin:

1. [Bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Okuma erişimli coğrafi olarak yedekli depolama (depolama hesapları için önerilir) seçerseniz, okuma erişimi verilerinizi ikincil bölge kaybedildiği bir olağanüstü durumda vardır. Daha fazla bilgi için bkz: [Azure dosya paylaşımı olağanüstü durum kurtarma stratejilerini](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
2. [Bir dosya paylaşımı oluşturmak](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Dosya eşitleme dağıtmak](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) , şirket içi dosya sunucusunda.
4. Bir eşitleme grubu oluşturun. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş tutulur. Eşitleme grubunu bir Azure dosya paylaşımını gösteren en az bir bulut uç içermesi gerekir. Eşitleme grubunu ayrıca şirket içi Windows server üzerinde bir yolu temsil eden bir sunucu endpoint içermesi gerekir.
5. Dosyalarınızı şimdi, Azure dosya paylaşımı ve şirket içi sunucunuz arasında eşitleme tutulur.
6. Şirket içi ortamınızda bir olağanüstü durumda kullanarak bir yük devretme gerçekleştirme bir [kurtarma planı](site-recovery-create-recovery-plans.md). Azure dosya paylaşımını bağlama ve sanal makineniz paylaşımına erişmek için komut dosyası ekleyin.

> [!NOTE]
> 445 bağlantı noktası açık olduğundan emin olun. Azure dosyaları, SMB protokolünü kullanır. SMB 445 numaralı TCP bağlantı noktası üzerinden iletişim kurar. Güvenlik duvarınızın istemci bir makineden TCP bağlantı noktası 445 engellemediğinden olmadığını denetleyin.


## <a name="do-a-test-failover"></a>Yük devretme testi yapın

1. Azure Portalı'na gidin ve kurtarma hizmeti kasanızı seçin.
2. Dosya sunucusu ortamı için oluşturduğunuz kurtarma planı seçin.
3. Seçin **yük devretme testi**.
4. Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
5. Yukarı ikincil ortamıdır sonra doğrulama gerçekleştirin.
6. Doğrulama tamamlandıktan sonra seçin **temizleme yük devretme testi** kurtarma planında ve yük devretme sınaması ortamı temizlendi.

Yük devretme testi gerçekleştirme hakkında daha fazla bilgi için bkz: [Site Recovery için yük devretme testi](site-recovery-test-failover-to-azure.md).

Active Directory ve DNS için yük devretme testi yapılması ile ilgili yönergeler için bkz: [Test Active Directory ve DNS için yük devretme konuları](site-recovery-active-directory.md).

## <a name="do-a-failover"></a>Bir yük devretme işlemi gerçekleştirin

1. Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.
2. Dosya sunucusu ortamı için oluşturduğunuz kurtarma planı seçin.
3. Seçin **yük devretme**.
4. Yük devretme işlemini başlatmak için kurtarma noktası seçin.

Bir yük devretme gerçekleştirme hakkında daha fazla bilgi için bkz: [Site Recovery yük](site-recovery-failover.md).
