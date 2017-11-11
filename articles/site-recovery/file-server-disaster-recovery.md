---
title: Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz
description: "Bu makalede, Azure Site Recovery kullanarak bir dosya sunucusu korunmasına yardımcı olmak açıklar"
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: cc585d6add9b9c5ce7f3379aeaf5ec79f5c61d51
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir dosya sunucusunu koruyabilmeniz 

[Azure Site Recovery](site-recovery-overview.md) hizmeti, iş uygulamalarınızı planlanan ve planlanmayan kesintiler sırasında kullanılabilir tutarak iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery yönetir ve olağanüstü durum kurtarma çoğaltma, yük devretme ve kurtarma, çeşitli iş yükleri de dahil olmak üzere şirket içi makineler ve Azure sanal makineleri (VM'ler) düzenler.

Bu makalede, Azure Site Recovery ve diğer öneriler ortamlarla uyacak şekilde kullanarak bir dosya sunucusu korunmasına yardımcı olmak açıklar.

## <a name="file-server-architecture"></a>Dosya sunucu mimarisi
Bir açık, dağıtılmış dosya paylaşımı sistem bütünlüğü gereksinimleri ödün vermeden coğrafi olarak dağıtılmış kullanıcılar dosyalarda burada çalışabileceğiniz bir ortam sağlar. 

Çok sayıda eşzamanlı kullanıcı ve içerik öğelerini destekleyen tipik şirket içi bir dosya sunucusu ekosistem yinelemesinin zamanlanması ve bant genişliği azaltma için Dağıtılmış dosya sistemi çoğaltma (DFSR) kullanır. DFSR dosyalarını sınırlı bant genişliğine sahip ağ üzerinden verimli bir şekilde güncelleştirmek için kullanılan uzaktan değişiklikleri sıkıştırma (RDC) sıkıştırma algoritması kullanır. RDC, eklemeler, kaldırma işlemleri ve veri ilgili dosyalarında algılar. Dosyalar güncelleştirildiğinde yalnızca değiştirilen dosya bloklarını DFSR sağlar. 

Bazı dosya sunucusu ortamlarda, olağanüstü durum kurtarma için yoğun olmayan saatlerde günlük yedeklemeler alınır. Bu ortamlar DFSR kullanmayın.

Aşağıdaki topoloji uygulanan DFSR ile bir dosya sunucu ortamı gösterir:
                
![DFSR mimarisi](media/site-recovery-file-server/dfsr-architecture.JPG)

Şekilde, birden çok dosya sunucuları (üye olarak adlandırılır) etkin bir çoğaltma grubu arasında dosyaları çoğaltmasında katılın. Üyelerinden çevrimdışı olması durumunda bile çoğaltılmış klasördeki içeriği istekleri ya da üyelerinin gönderdiğiniz tüm istemciler için kullanılabilir.

## <a name="disaster-recovery-strategies-for-file-servers"></a>Dosya sunucuları için olağanüstü durum kurtarma stratejileri

- **Bir dosya sunucusunda Azure Site Recovery kullanarak azure'a**: bir veya daha fazla şirket içi dosya sunucularına erişilemez durumda olduğunda, sanal makineleri kurtarma Azure'da getirilebilir. Sanal makineleri kurtarma sonra istekleri istemcilerinden, şirket içi, siteden siteye VPN bağlantısı olduğunu ve Active Directory ile Azure yapılandırılmışsa hizmet verebilir. DFSR yapılandırılmış bir ortamda veya hiçbir DFSR ile basit dosya bir sunucu ortamında bunu yapabilirsiniz. 

- **Bir Azure Iaas sanal DFSR genişletmek**: bir kümelenmiş dosya sunucusu ortamında uygulanan DFSR ile bir yaklaşım Azure için şirket içi DFSR genişletmek için önerilir. Bir Azure sanal makinesi, dosya sunucusu rolünü daha sonra gerçekleştirebilirsiniz. 

    Bir veya daha fazla şirket içi dosya sunucuları erişilemediğinde yerinde, siteden siteye VPN bağlantısı ve Active Directory bağımlılıkları işlenir ve DFSR sonra istemcilerin Azure VM'ye hala bağlanabilir. Azure VM isteklerine hizmet verir.

    Azure Site Recovery desteklemiyor yapılandırmaları Vm'leriniz varsa, bu yaklaşım öneririz. Bu tür bir yapılandırma dosyası sunucu ortamlarında yaygın olarak kullanılan paylaşılan bir küme diski örnektir. DFSR de Orta karmaşıklık oranı ile düşük bant genişlikli ortamlarda iyi çalışır. Bu yaklaşımda, bir Azure VM sahip ve her zaman çalışması için ek maliyet uyum sağlaması gerekmektedir.  

- **Dosyalarınızı çoğaltmak için Azure dosya eşitleme kullanın**: Yolculuğunuzun buluta hazırlığı ya da bir Azure VM zaten kullanıyorsanız, Azure dosya eşitleme kullanılmasını öneririz. Bu hizmeti sunan endüstri standardı erişilebilen tam olarak yönetilen dosya paylaşımı bulutta eşitleniyor [sunucu ileti bloğu ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)(SMB) protokolü. Bundan sonra bağlama Azure dosya paylaşımları Windows, Linux ve macOS şirket dağıtımları veya Bulut tarafından eşzamanlı olarak yapabilirsiniz. 

Aşağıdaki diyagramda, dosya sunucu ortamınıza kullanmak için hangi stratejisi karar vermenize yardımcı olabilir:

![karar ağacı](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-for-making-a-disaster-recovery-decision"></a>Bir olağanüstü durum kurtarma kararı Etkenler

|Ortam  |Öneri  |Dikkate alınacak noktalar |
|---------|---------|---------|
|Dosya sunucusu ortamı ile veya olmadan DFSR|   [Azure Site Recovery çoğaltma için kullanın](#replicate-an-on-premises-file-server-by-using-azure-site-recovery)   |    Site Recovery, paylaşılan disk kümelerini ya da NAS desteklemez. Ortamınızın aşağıdaki yapılandırmalardan birini kullanıyorsa, diğer yaklaşımlardan birini uygun şekilde kullanın. <br> Azure Site Recovery, SMB 3.0 desteklemiyor. Çoğaltılmış VM'yi dosyayı özgün konumda değişiklikleri güncelleştirildiğinde yalnızca dosyalarda yapılan değişiklikler dahil. 
|DFSR ile dosya sunucusu ortamı     |  [Bir Azure Iaas sanal makineye DFSR genişletme](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR bant genişliği crunched ortamlarda iyi çalışır. Bu yaklaşım, ancak her zaman bir Azure VM ve çalışıyor olması gerekir. VM maliyet için hesap planlama gerekir.         |
|Azure Iaas VM     |     [Azure dosya eşitleme kullanın](#use-azure-file-sync-to-replicate-your-on-premises-files)   |     Bir olağanüstü durum kurtarma senaryosunda, yük devretme sırasında Azure dosya eşitleme kullanıyorsanız, dosya paylaşımları istemci makine saydam bir şekilde erişilebilir olduğundan emin olmak için el ile gerçekleştirilen eylemleri almanız gerekir. Azure dosya eşitleme bağlantı noktası 445'i istemci makineden açık olmasını gerektirir.     |


### <a name="site-recovery-support"></a>Site kurtarma desteği
Site Recovery çoğaltma uygulama belirsiz olduğundan, aşağıdaki senaryolar için doğru tutmak için burada sağlanan öneriler bekleniyor:
| Kaynak    |İkincil bir siteye    |Azure'a
|---------|---------|---------|
|Azure| -|Evet|
|Hyper-V|   Evet |Evet
|VMware |Evet|   Evet
|Fiziksel sunucu|   Evet |Evet
 

> [!IMPORTANT]
> Aşağıdaki yaklaşımlardan birini ile devam etmeden önce adres bağımlılıkları emin olun.

**Siteden siteye bağlantı**: sunucuları arasında iletişim sağlamak için şirket içi site ile Azure ağı arasında doğrudan bir bağlantı kurulmalıdır. Olağanüstü durum kurtarma sitesi kullanılacak bir Azure sanal ağı güvenli bir siteden siteye VPN bağlantısı kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure portalında bir siteden siteye bağlantı oluşturmak](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Active Directory**: DFSR üzerinde Active Directory bağlıdır. Bu, yerel etki alanı denetleyicileri ile Active Directory ormanı Azure olağanüstü durum kurtarma sitede genişletilir anlamına gelir. Hedeflenen kullanıcılar için erişim izni/doğrulandı olması gerektiğinde, DFSR, bile kullanmıyorsanız, bu adımları uygulamanız gerekir.
Daha fazla bilgi için bkz: [Genişlet şirket içi Azure Active Directory'ye](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendations-for-azure-iaas-virtual-machines"></a>Azure Iaas sanal makineleri için olağanüstü durum kurtarma önerileri

Yapılandırma ve Azure Iaas Vm'leri üzerinde barındırılan dosya sunucularının olağanüstü durum kurtarmayı yönetme, iki seçenek arasından seçim yapabilirsiniz: Azure dosya eşitleme ve Azure Site Recovery. Karar, taşımak istediğiniz bağlıdır [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

### <a name="use-azure-file-sync-to-replicate-files-hosted-on-iaas-virtual-machines"></a>Iaas sanal makinelerinde barındırılan dosya çoğaltmak için Azure dosya eşitleme kullanın

Azure dosyaları tam olarak değiştirin veya Geleneksel şirket içi dosya sunucularında veya NAS cihazları desteklemek için kullanabilirsiniz. Azure dosya paylaşımları Windows sunucuları, her iki şirket içi veya bulutta, kullanıcı ve dağıtılmış burada kullanılıyor verileri önbelleğe alma için Azure dosya eşitleme ile aynı zamanda çoğaltılabilir. 

Aşağıdaki adımlar, geleneksel dosya sunucuları ile aynı işlevi gerçekleştirir Azure VM'ler için olağanüstü durum kurtarma öneri vermektedir:
1. Azure Site Recovery, burada sözü edilen adımları kullanarak makineler koruyun.

2. Bulut için dosya sunucusu gibi davranır VM'den dosya çoğaltmak için Azure dosya eşitleme kullanın.

3. Kullanım [kurtarma planı](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-create-recovery-plans) komut dosyalarına eklemek için Azure Site Kurtarma özelliği [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makineniz paylaşımına erişim.

Aşağıdaki adımlar, Azure dosya eşitleme hizmetini kullanmayı kısaca açıklamaktadır:

1. [Bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Depolama hesapları için okuma erişimli coğrafi olarak yedekli depolama seçerseniz kaybedildiği bir olağanüstü durumda ikincil bölgesinden verilerinize okuma erişimi alırsınız. Daha fazla bilgi için bkz: [Azure dosya paylaşımı olağanüstü durum kurtarma stratejilerini](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Bir dosya paylaşımı oluşturmak](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Azure dosya eşitleme dağıtmak](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) Azure dosya sunucunuzda.

4. Bir eşitleme grubu oluşturun. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş kalır. Eşitleme grubu, Azure dosya paylaşımı temsil eder, en az bir bulut uç noktası ve bir Windows Server'da bir yol temsil eden bir sunucu uç içermesi gerekir.  
    Dosyalarınızı şimdi, Azure dosya paylaşımı ve şirket içi sunucunuz arasında eşit şekilde kalır.

5. Şirket içi ortamınızda bir olağanüstü durum varsa, bir kurtarma planı kullanarak bir yük devretme gerçekleştirin. Komut dosyasına eklemek [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makineniz paylaşımına erişebilirsiniz.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir Iaas dosya sunucusu sanal makinesi çoğaltma

Iaas dosya sunucusu sanal makineye erişim şirket içi istemcileriniz varsa, aşağıdaki adımları gerçekleştirin. Aksi takdirde, yalnızca 3. adım gerçekleştirin.

1. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun.  

2. Şirket içi Active Directory genişletir.

3. [Olağanüstü durum kurtarma ayarlamak](azure-to-azure-tutorial-enable-replication.md) Iaas dosya sunucu makinesine ikincil bir bölgeye için.

İkincil bir bölgeye olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz: [Azure Azure çoğaltma mimarisi](concepts-azure-to-azure-architecture.md).

## <a name="disaster-recovery-recommendations-for-on-premises-virtual-machines"></a>Şirket içi sanal makineleri için olağanüstü durum kurtarma önerileri 

### <a name="replicate-an-on-premises-file-server-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir şirket içi dosya sunucusunu Çoğalt
Aşağıdaki adımlar bir VMware sanal makine için çoğaltmayı ayrıntılı olarak açıklanmaktadır. Bir Hyper-V VM çoğaltmak adımlar için bkz: [şirket içi Hyper-V sanal makineleri olağanüstü durum kurtarma Azure ayarlama](https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-hyper-v-to-azure).

1. [Azure kaynaklarını hazırlama](tutorial-prepare-azure.md) şirket içi makineler çoğaltılması için.

2. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun.  

3. Şirket içi Active Directory genişletir.

4. [Şirket içi VMware sunucuları hazırlama](tutorial-prepare-on-premises-vmware.md).

5. [Olağanüstü durum kurtarma ayarlamak](tutorial-vmware-to-azure.md) azure'a şirket içi VM'ler için.

### <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Bir Azure Iaas sanal makineye DFSR genişletme

1. Şirket içi site ile Azure ağı arasında bir siteden siteye VPN bağlantısı kurun. 

2. Şirket içi Active Directory genişletir.

3. [Oluşturma ve bir dosya sunucusu VM sağlama](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) Azure sanal ağı üzerinde.  
    Sanal makine arası bağlantı şirket içi ortamı olan aynı Azure sanal ağı, eklendiğinden emin olun. 

4. Yükleme ve [DFS Çoğaltma Yapılandırma](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) Windows Server'da.

5. [DFS ad alanı uygulamak](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).

6. DFS ad uygulanan, olağanüstü durum kurtarma sitelere üretimden paylaşılan klasörler üzerinde DFS ad alanı klasörü hedefleri güncelleştirerek başarısız olabilir. Sonra bu DFS ad alanı değişiklikleri Active Directory ile çoğaltmak kullanıcıların bağlı uygun klasör hedefleri saydam.

### <a name="use-azure-file-sync-to-replicate-your-on-premises-files"></a>Şirket içi dosyalarınızı çoğaltmak için Azure dosya eşitleme kullanın
Azure dosya eşitleme hizmetini kullanarak, istenen dosyaları buluta çoğaltabilirsiniz. Bir olağanüstü durum ve şirket içi dosya sunucusu olarak kullanım dışı kalması durumunda sonra bulutta istenen dosya konumları bağlama ve hizmet istekleri için istemci makinelerden devam edebilirsiniz.

Azure Site Recovery ile Azure dosya eşitleme tümleştirme önerilen yaklaşım şöyledir:
1. Azure Site Recovery kullanarak dosya sunucusu makineleri koruyun. Adımları [şirket içi VMware Vm'leri için olağanüstü durum kurtarma Azure ayarlama](tutorial-vmware-to-azure.md).

2. Buluta bir dosya sunucusu olarak hizmet veren makineden dosya çoğaltmak için Azure dosya eşitleme kullanın.

3. Kurtarma planı Özelliği Azure Site Recovery başarısız üzerinden dosya sunucusundaki Azure VM'de Azure dosya paylaşımını bağlama için komut dosyaları eklemek için kullanın.

Azure dosya eşitleme hizmetini kullanarak aşağıdaki adımları ayrıntıları:

1. [Bir depolama hesabı oluşturma](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Okuma erişimli coğrafi olarak yedekli depolama (depolama hesapları için önerilir) seçerseniz, kaybedildiği bir olağanüstü durumda ikincil bölgesinden verilerinize okuma erişimi gerekir. Daha fazla bilgi için bkz: [Azure dosya paylaşımı olağanüstü durum kurtarma stratejilerini](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Bir dosya paylaşımı oluşturmak](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Azure dosya eşitleme dağıtmak](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) , şirket içi dosya sunucusunda.

4. Bir eşitleme grubu oluşturun. Bir eşitleme grubundaki uç noktaları birbirleri ile eşitlenmiş kalır. Eşitleme grubu, Azure dosya paylaşımının temsil eder, en az bir bulut uç noktası ve şirket içi Windows server üzerinde bir yolu temsil eden bir sunucu uç nokta içermesi gerekir.  
    Dosyalarınızı şimdi, Azure dosya paylaşımı ve şirket içi sunucunuz arasında eşit şekilde kalır.

5. Şirket içi ortamınızda bir olağanüstü durum varsa, bir kurtarma planı kullanarak bir yük devretme gerçekleştirin. Azure dosya paylaşımını bağlama ve sanal makineniz paylaşımına erişmek için komut dosyası ekleyin.

> [!NOTE]
> 445 bağlantı noktası açık olduğundan emin olun. Azure dosyaları, SMB protokolünü kullanır. SMB 445 numaralı TCP bağlantı noktası üzerinden iletişim kurar. Güvenlik duvarını bağlantı noktası 445 istemci makineden engelleyip engellemediğini denetleyin.


## <a name="perform-a-test-failover"></a>Bir sınama yük devretmesi gerçekleştirme

1. Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.

2. Dosya sunucusu ortamı için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme testi**.

4. Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.

5. İkincil ortam yukarı olduğunda, doğrulama gerçekleştirin.

6. Doğrulamaları tamamlandığı zaman, seçebileceğiniz **temizleme yük devretme testi** kurtarma planında ve yük devretme sınaması ortamı temizlendi.

Yük devretme testi gerçekleştirme hakkında daha fazla bilgi için bkz: [Test yük devretme Azure Site Recovery](site-recovery-test-failover-to-azure.md).

Active Directory ve DNS için yük devretme testi gerçekleştirme hakkında yönergeler için bkz [Test Active Directory ve DNS için yük devretme konuları](site-recovery-active-directory.md).

## <a name="perform-a-failover"></a>Bir yük devretme gerçekleştirin.

1. Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.

2. Dosya sunucusu ortamı için oluşturduğunuz kurtarma planı seçin.

3. Seçin **yük devretme**.

4. Yük devretme işlemini başlatmak için kurtarma noktası seçin.

Bir yük devretme gerçekleştirme hakkında daha fazla bilgi için bkz: [Site Recovery yük](site-recovery-failover.md).
