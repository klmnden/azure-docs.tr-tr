---
title: Azure Site Recovery kullanarak bir dosya sunucusu koruma
description: "Bu makalede, Azure Site RECOVERY'yi kullanarak bir dosya sunucusunu koruyabilmeniz açıklar"
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
ms.openlocfilehash: 8c9d8dadcd6181d9894ab6ee7110841afdec5708
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="protect-a-file-server-using-azure-site-recovery"></a>Azure Site Recovery kullanarak bir dosya sunucusu koruma 

[Azure Site Recovery](site-recovery-overview.md) hizmeti tarafından iş uygulamalarınızı çalışır halde tutmaktan planlanan ve planlanmayan kesintiler sırasında kullanılabilir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı. Site Recovery yönetir ve olağanüstü durum kurtarma çoğaltma, yük devretme ve kurtarma, çeşitli iş yükleri de dahil olmak üzere şirket içi makineler ve Azure sanal makineleri (VM'ler) düzenler.

Bu makalede, Azure Site Recovery ve diğer öneriler ortamlarla uyacak şekilde kullanarak bir dosya sunucusunu koruyabilmeniz açıklar.     

- [Azure Iaas dosya sunucusu makineleri çoğaltma](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Azure Site RECOVERY'yi kullanarak şirket içi dosya sunucusunu Çoğalt](#replicate-an-onpremises-file-server-using-azure-site-recovery)

## <a name="file-server-architecture"></a>Dosya sunucu mimarisi
Açık bir dağıtılmış dosya sistemi paylaşımı amacı, coğrafi olarak dağıtılmış kullanıcılar grubunun etkili bir şekilde dosyalar üzerinde çalışması ve tutarlılığı gereksinimlerine zorlandığını garanti burada çalışabileceğiniz bir ortam sağlamaktır. Çok sayıda eşzamanlı kullanıcı ve çok sayıda içerik öğelerini destekleyen tipik şirket içi bir dosya sunucusu ekosistem yinelemesinin zamanlanması ve bant genişliği azaltma için Dağıtılmış dosya sistemi çoğaltma (DFSR) kullanın. DFSR olarak uzaktan değişiklikleri sıkıştırma (dosyalarını sınırlı bant genişliğine sahip ağ üzerinden verimli bir şekilde güncelleştirmek için kullanılan RDC), bilinen bir sıkıştırma algoritması kullanır. Bu eklemeler, kaldırma işlemleri ve veri ilgili dosyalarında dosyalar güncelleştirildiğinde yalnızca değiştirilen dosya bloklarını DFSR etkinleştirme algılar. Dosya sunucusu ortamları, burada günlük yedeklemeler için olağanüstü durum gereksinimlerini karşılamak yoğun olmayan zamanlamaları alınır ve DFSR hiç uygulaması yoktur vardır.

Aşağıdaki topoloji uygulanan DFSR ile dosya sunucusu ortamı gösterir.
                
![DFSR architexture](media/site-recovery-file-server/dfsr-architecture.JPG)

Yukarıdaki başvurusunda birden çok dosya sunucuları arasında bir çoğaltma grubu dosyaları çoğaltmasında üye olarak etkin olarak katılmayı ifade. Çoğaltılmış klasörün içeriğini istekleri ya da üyelerinin biri bile çevrimdışına üyeleri durumunda gönderme tüm istemciler için kullanılabilir.

## <a name="disaster-recovery-recommendation-for-file-servers"></a>Dosya sunucuları için olağanüstü durum kurtarma öneri:

1.  Azure Site Recovery kullanan bir dosya sunucusunda çoğaltmak: dosya sunucuları için Azure Site Kurtarma'yı kullanarak Azure çoğaltılabilir. Bir veya daha fazla dosya sunucuları içi erişilemez durumda olduğunda kurtarma VM'ler için Azure'da getirilebilir sonra hizmet vermemesini istemcilerden gelen isteklerin var. sağlanan şirket içi, kullanıcının siteden siteye VPN bağlantısı ve Active directory ile Azure yapılandırılmış. Bu bir yapılandırılmış DFSR ortamı veya hiçbir DFSR basit dosya sunucusu ortamıyla durumunda yapılabilir. 

2.  Bir Azure Iaas sanal DFSR genişletmek:-bir kümelenmiş dosya sunucusu ortamında uygulanan DFSR ile bir yaklaşım Azure için şirket içi DFSR genişletmek için önerilir. Bir Azure sanal makinesi, ardından dosya sunucusu rolünü gerçekleştirmek için etkinleştirilir. 

    Bir veya daha fazla dosya sunucuları içi erişilemez durumda olduğunda yerinde, siteden siteye VPN bağlantısı ve Active directory bağımlılıkları işlenir ve DFSR sonra istemcileri isteklere hizmet Azure VM için hala bağlanabilir.

    Bu yaklaşım durumda Vm'lerinizi Azure Site Recovery tarafından gibi örneğin desteklenmeyen yapılandırmalara sahip olması önerilir: dosya sunucusu ortamlarda bazen sık kullanılan paylaşılan küme diski.  DFSR de Orta karmaşıklık oranı ile düşük bant genişlikli ortamlarda iyi çalışır. Bir Azure VM sahip ve her zaman çalışması ek maliyet, ayrıca bu barındırılabilmesi gerekir.  

3.  Dosyalarınızı çoğaltmak için Azure dosya eşitleme Hizmeti'ni kullanın: Yolculuğunuzun buluta hazırlığı ya da zaten bir Azure VM kullanıyorsanız varsa, erişilebilir v tam olarak yönetilen dosya paylaşımları bulutta eşitleniyor sunar Azure dosya eşitleme hizmeti kullanımını öneririz IA endüstri standardı [sunucu ileti bloğu ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)(SMB) protokolü. Azure dosya paylaşımları sonra aynı anda göre bağlanabilir Bulut veya şirket içi Windows, Linux ve macOS dağıtımları. 

Diyagram, dosya sunucu ortamınıza kullanmak için hangi stratejisi karar kolaylaştırma adresindeki amaçlayan resimsel temsili sağlar.

![decisiontree](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-while-making-decision-of-disaster-recovery-to-azure"></a>Olağanüstü durum kurtarma Azure karar verirken göz önünde bulundurmanız gereken Etkenler

|Ortam  |Öneri  |Dikkate alınacak noktalar |
|---------|---------|---------|
|Sunucu ortamı ile/DFSR olmadan dosya|   [Azure Site Recovery çoğaltma için kullanın](#replicate-an-onpremises-file-servers-using-azure-site-recovery)   |    Site Recovery paylaşılan disk küme, NAS desteklemez. Ortamınızın aşağıdaki yapılandırmalardan birini kullanıyorsa, diğer yaklaşımlardan birini uygun şekilde kullanın. <br> Azure Site Recovery yalnızca dosyalarda yapılan değişiklikler dosyayı özgün konumunda güncelleştirildiğinde çoğaltılmış VM'yi değişikliklerin uygulanması, yani SMB 3.0 desteklemiyor.
|DFSR ile dosya sunucusu ortamı     |  [DFSR bir Azure Iaas sanal makineye genişletin:](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |     DFSR son derece crunched bant genişliği ortamlarda iyi çalışır, bu yaklaşım ancak bir Azure VM'yi yedekleme ve her zaman çalışıyor olmasını gerektirir. VM maliyet planlamanızda olunması gerekir.         |
|Azure Iaas VM     |     [Azure dosya eşitleme](#use-azure-file-sync-service-to-replicate-your-files)   |     Azure dosya eşitleme kullanıyorsanız bir DR senaryosuna yük devretme sırasında el ile gerçekleştirilen eylemleri dosya istemci makine saydam bir şekilde erişilebilir olarak paylaşımları sağlamak için yapılması gerekir. AFS bağlantı noktası 445'i istemci makineden açık olmasını gerektirir.     |


### <a name="site-recovery-support"></a>Site kurtarma desteği
Site Recovery çoğaltma uygulama yükünden bağımsız olarak, aşağıdaki senaryolar için doğru tutmak için burada sağlanan öneriler bekleniyor:
| Kaynak    |İkincil bir siteye    |Azure'a
|---------|---------|---------|
|Azure| -|Evet|
|Hyper-V|   Evet |Evet
|VMware |Evet|   Evet
|Fiziksel sunucu|   Evet |Evet
 

> [!IMPORTANT]
> İle devam etmeden önce aşağıdaki bağımlılıkları dikkate üç yaklaşımlar emin olun:

**Siteden siteye bağlantı**: şirket içi site ile Azure ağı arasında bir bağlantı kurulacak, sunucular arasında iletişime izin verecek şekilde yönlendirir.  Bu Microsoft Azure sanal bir DR sitesi olarak kullanılacak ağa güvenli bir siteden siteye VPN bağlantısı tarafından güvence altına alınabilir.  
Başvurun: [şirket içi site ile Azure ağı arasında Ağımdaki siteden siteye VPN bağlantısı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Active Directory**: DFSR üzerinde Active Directory bağlıdır.  Bu, yerel etki alanı denetleyicileri ile Active Directory ormanı DR sitesi genişletilir anlamına gelir. Hedeflenen kullanıcılar gerekiyorsa verilir veya erişim gibi çoğu kuruluş için doğrulandı, DFSR bile kullanmıyorsanız, bu adımları gerçekleştirilmesi gerekir.
Başvurun: [Azure'a şirket içi Active Directory'yi genişletmeniz](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Azure Iaas sanal makineleri için olağanüstü durum kurtarma önerisi

Yapılandırmakta olduğunuz ve Azure Iaas Vm'leri üzerinde barındırılan dosya sunucularının olağanüstü durum kurtarmayı yönetme, iki seçenek arasından seçim yapabilirsiniz olursa, taşımak istediğiniz temel [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

1. [Azure dosya eşitleme kullanın](#use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine)
2. [Azure Site Recovery’yi kullanma](#replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery)

## <a name="use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine"></a>Iaas sanal makinede barındırılan dosya çoğaltmak için Azure dosya eşitleme Hizmeti'ni kullanın

**Azure dosyaları** tamamen değiştirin veya Geleneksel şirket içi dosya sunucularında veya NAS cihazları desteklemek için kullanılabilir. Azure Dosya paylaşımları ayrıca verilerin kullanıldığı yerde performanslı ve dağıtılmış bir şekilde önbelleğe alınması için Azure Dosya Eşitleme ile şirket içi veya bulut üzerindeki Windows sunucularında çoğaltılabilir. Aşağıdaki adımlar, Azure VM'ler, geleneksel dosya sunucuları aynı işlevleri gerçekleştirmek için DR öneri vermektedir:
1.  Belirtilen adımları kullanarak Azure Site Recovery kullanarak makine koru [burada](azure-to-azure-quickstart.md).
2.  Bulut için dosya sunucusu gibi davranır VM'den dosya çoğaltmak için Azure dosya eşitleme kullanın.
3.  Azure Site Recovery'nin kullanmak [kurtarma planı](site-recovery-create-recovery-plans.md) komut dosyalarına eklemek için özellik [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makineniz paylaşımına erişim.

Aşağıdaki adımları Azure dosya eşitleme hizmetini kullanmayı kısaca açıklanmaktadır:

1. [Bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Depolama hesapları için okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) seçerseniz, okuma erişimi verilerinizi ikincil bölge kaybedildiği bir olağanüstü durumda alırlar. Başvurmak [Azure dosya paylaşımı olağanüstü durum kurtarma stratejilerini](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) daha fazla bilgi için.
2. [Bir dosya paylaşımı oluşturmak](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Azure dosya eşitleme dağıtmak](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) Azure dosya sunucunuzda.
4. Bir eşitleme grubu oluşturun: bir eşitleme grubundaki uç noktaları korunur birbirleri ile eşitlenmiş. Eşitleme grubu, Azure dosya paylaşımı temsil eder, en az bir bulut uç ve bir Windows Server'da bir yol temsil eden bir sunucu uç içermesi gerekir.
5.  Dosyalarınızı şimdi, Azure dosya paylaşımı ve şirket içi sunucunuz arasında eşitleme tutulacak.
6.  Yük devretme olarak şirket içi ortamınızda bir olağanüstü durumda gerçekleştirin bir [kurtarma planı](site-recovery-create-recovery-plans.md) ve komut dosyasına eklemek [Azure dosya paylaşımını bağlama](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) ve sanal makineniz paylaşımına erişebilirsiniz.

### <a name="replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery"></a>Bir Iaas dosya sunucusu sanal makinesi Azure Site RECOVERY'yi kullanarak çoğaltma

Dosya sunucusu sanal makinesi gerçekleştirmek Iaas erişme şirket içi istemcileriniz varsa ilk iki adımı de başka 3. adıma geçebilirsiniz.

1. Şirket içi site ile Azure ağı arasında siteden siteye VPN bağlantısı kurun.
1. Şirket içi Active Directory genişletir.
1. [Olağanüstü durum kurtarma ayarlamak](azure-to-azure-tutorial-enable-replication.md) Iaas dosya sunucu makinesine ikincil bir bölgeye için.


İkincil bir bölgeye olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz [burada](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-using-azure-site-recovery"></a>Azure Site RECOVERY'yi kullanarak bir şirket içi dosya sunucusunu Çoğalt

Adımları ayrıntı çoğaltma bir Hyper-V VM çoğaltmak adımlar için bir VMware VM için aşağıda bkz [burada](tutorial-hyper-v-to-azure.md).

1.  [Azure kaynaklarını hazırlama](tutorial-prepare-azure.md) şirket içi makineler çoğaltılması için.
2.  Şirket içi site ile Azure ağı arasında siteden siteye VPN bağlantısı kurun.  
3. Şirket içi Active Directory genişletir.
4.  [Şirket içi VMware sunucuları hazırlama](tutorial-prepare-on-premises-vmware.md).
5.  [Olağanüstü durum kurtarma ayarlamak](tutorial-vmware-to-azure.md) azure'a şirket içi VM'ler için.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>DFSR bir Azure Iaas sanal makineye genişletin:

1.  Şirket içi site ile Azure ağı arasında siteden siteye VPN bağlantısı kurun. 
2.  Şirket içi Active Directory genişletir.
3.  [Oluşturma ve bir dosya sunucusu VM sağlama](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) Windows Azure sanal ağındaki.
Sanal makine ağa aynı Windows Azure sanal çapraz bağlantısı ile şirket içi ortamına sahip eklendiğinden emin olun. 
4.  Yükleme ve [DFS Çoğaltma Yapılandırma](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) Windows Server'da.
5.  [DFS Namespace uygulamak](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6.  DFS uygulanan Namespace ile üretim paylaşılan klasörlerden DR sitelere Yük Devretmesini DFS Namespace klasör hedefleri güncelleştirerek yapılabilir.  Active Directory ile bu DFS Namespace değişiklikleri çoğaltma sonra kullanıcılar için uygun klasör hedefleri şeffaf bir şekilde bağlı.

## <a name="use-azure-file-sync-service-to-replicate-your-on-premises-files"></a>Şirket içi dosyalarınızı çoğaltmak için Azure dosya eşitleme Hizmeti'ni kullanın:
Böylece olağanüstü durum ve şirket içi dosya sunucusu olarak kullanım dışı kalması durumunda, istenen dosya konumları için buluttan bağlayın ve isteklere hizmet devam Azure dosya eşitleme hizmeti kullanarak, istenen dosyaları buluta çoğaltabilirsiniz istemci makineler.
Azure Site Recovery ile Azure dosya eşitleme tümleştirme önerilen yaklaşım
1.  Belirtilen adımları kullanarak Azure Site RECOVERY'yi kullanarak dosya sunucusu makineleri korumak [burada](tutorial-vmware-to-azure.md).
2.  Buluta bir dosya sunucusu olarak hizmet veren makineden dosya çoğaltmak için Azure dosya eşitleme kullanın.
3.  Azure dosya paylaşımı azure'da DosyaSunucusu VM üzerinde başarısız oldu bağlamak için komut dosyası eklemek için Azure Site Recovery'nin kurtarma planı özelliğini kullanın.

Azure dosya eşitleme hizmetini kullanarak adımları ayrıntı altında:

1. [Bir depolama hesabı oluşturma](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). (Depolama hesapları için önerilir) okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) seçerseniz, okuma erişimi verilerinizi ikincil bölge kaybedildiği bir olağanüstü durumda vardır. Başvurmak [Azure dosya paylaşımı olağanüstü durum kurtarma stratejilerini](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) daha fazla bilgi için.
2. [Bir dosya paylaşımı oluşturmak](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).
3. [Azure dosya eşitleme dağıtmak](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) , şirket içi dosya sunucusunda.
4. Bir eşitleme grubu oluşturun: bir eşitleme grubundaki uç noktaları korunur birbirleri ile eşitlenmiş. Eşitleme grubu, Azure dosya paylaşımı temsil eder, en az bir bulut Endpoint ve şirket içi Windows Server bilgisayarında bir yolu temsil eden bir sunucu uç içermesi gerekir.
1. Dosyalarınızı şimdi, Azure dosya paylaşımı ve şirket içi sunucunuz arasında eşitleme tutulacak.
6.  Yük devretme olarak şirket içi ortamınızda bir olağanüstü durumda gerçekleştirin bir [kurtarma planı](site-recovery-create-recovery-plans.md) ve Azure dosya paylaşımını bağlama ve sanal makineniz paylaşımına erişmek için komut dosyası ekleyin.

> [!NOTE]
> 445 bağlantı noktası açık olduğundan emin olun: Azure dosyaları, SMB protokolünü kullanır. SMB, TCP bağlantı noktası 445 üstünden iletişim kurar. İstemci makinenizde güvenlik duvarının TCP bağlantı noktaları 445’i engellemediğinden emin olun.


## <a name="doing-a-test-failover"></a>Yük devretme sınamasını yapmak

1.  Azure Portalı'na gidin ve kurtarma hizmeti kasanızı seçin.
2.  DosyaSunucusu ortamınız için oluşturulmuş kurtarma planı tıklayın.
3.  'Test yük devretme üzerinde' tıklayın.
4.  Kurtarma noktası ve test yük devretme işlemini başlatmak için Azure sanal ağı seçin.
5.  İkincil ortamı kurma olduğunda, doğrulama gerçekleştirebilirsiniz.
6.  Doğrulama tamamlandıktan sonra kurtarma planı üzerinde 'Temizleme test yük devretme' tıklayın ve yük devretme sınama ortamı temizlendi.

Yük devretme testi gerçekleştirme hakkında daha fazla bilgi için bkz [burada](site-recovery-test-failover-to-azure.md).

AD için yük devretme testi yapılması konusunda yönergeler için ve DNS, başvurmak için [test yük devretme AD için ilgili önemli noktalar ve DNS](site-recovery-active-directory.md).

## <a name="doing-a-failover"></a>Bir yük devretme işleminden

1.  Azure Portalı'na gidin ve kurtarma Hizmetleri kasanız seçin.
2.  DosyaSunucusu ortamınız için oluşturulmuş kurtarma planı tıklayın.
3.  'Failover' üzerinde'ı tıklatın.
4.  Yük devretme işlemini başlatmak için kurtarma noktası seçin.

Bir yük devretme gerçekleştirme hakkında daha fazla bilgi için bkz [burada](site-recovery-failover.md).
