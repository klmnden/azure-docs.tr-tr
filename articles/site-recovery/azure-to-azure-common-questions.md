---
title: Azure'dan Azure'a Azure Site Recovery ile olağanüstü durum kurtarma hakkında sık sorulan sorular
description: Bu makalede, Azure Vm'leri Azure Site Recovery kullanarak başka bir Azure bölgesine olağanüstü durum kurtarma hakkında sık sorulan sorular yanıtlanmaktadır.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 04/29/2019
ms.topic: conceptual
ms.author: asgan
ms.openlocfilehash: 271e3c31c3e08d170add84ca4995f4876d4d3a33
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753779"
---
# <a name="common-questions-azure-to-azure-disaster-recovery"></a>Sık sorulan sorular: Azure'dan Azure'a olağanüstü durum kurtarma

Bu makalede kullanarak Azure Vm'leri başka bir Azure bölgesine olağanüstü durum kurtarma hakkında sık sorulan sorulara yanıtlar sağlayan [Site Recovery](site-recovery-overview.md). 


## <a name="general"></a>Genel

### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?
Gözden geçirme [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) ayrıntıları.
### <a name="how-does-the-free-tier-for-azure-site-recovery-work"></a>Azure Site Recovery için ücretsiz katman nasıl çalışır?
Azure Site Recovery ile korunan her örnek, ilk 31 gün boyunca ücretsiz olarak korunur. 32\. günden itibaren örneğin korunması yukarıdaki fiyatlarla ücretlendirilir.
### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>İlk 31 gün boyunca başka herhangi bir Azure hizmeti için ücretlendirilir miyim?
Evet, Azure Site Recovery korunan bir örnek için ilk 31 gün boyunca ücretsiz olsa da Azure Depolama, depolama işlemleri ve veri aktarımı için ücretlendirilmeye devam edebilirsiniz. Korunan bir sanal makine için de Azure işlem ücretleri alınabilir. Fiyatlandırma hakkında tam bilgi almak [burada](https://azure.microsoft.com/pricing/details/site-recovery)

### <a name="where-can-i-find-best-practices-for-azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma için en iyi nerede bulabilirim? 
1. [Azure'dan Azure'a mimarisini anlama](azure-to-azure-architecture.md)
2. [Desteklenen ve desteklenmeyen yapılandırmalarını gözden geçirin](azure-to-azure-support-matrix.md)
3. [Azure Vm'leri için olağanüstü durum kurtarmayı ayarlayın](azure-to-azure-how-to-enable-replication.md)
4. [Yük devretme testi çalıştırma](azure-to-azure-tutorial-dr-drill.md)
5. [Yük devretme ve birincil bölgeye geri döndürme](azure-to-azure-tutorial-failover-failback.md)

### <a name="how-is-capacity-guaranteed-in-the-target-region"></a>Kapasite, hedef bölgede nasıl sağlanır?
Site Recovery ekibi yeterli altyapı kapasiteyi planlamak için Azure kapasitesine yönetim ekibi ile çalışır ve yük devretme başlatıldığında için Site Recovery tarafından korunan VM'ler sağlamaya yardımcı olmak için başarıyla dağıtılan hedef bölgede olacaktır.

## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>VM'ler ile Azure disk şifrelemesi etkin çoğaltabilirim?
Evet, bunları çoğaltabilirsiniz. Makaleye göz atın [sanal makineleri başka bir Azure bölgesine çoğaltmak için Azure disk şifrelemesi etkin](azure-to-azure-how-to-enable-replication-ade-vms.md). Şu anda Azure Site Recovery şifrelemesi ile Azure Active Directory (Azure AD) uygulamaları için yalnızca Azure Windows işletim sistemi çalıştıran ve etkin Vm'leri destekler.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Başka bir abonelik için sanal makineleri çoğaltabilir miyim?
Evet, aynı Azure AD kiracısı içinde farklı bir aboneliğe Azure Vm'lerini çoğaltabilirsiniz.
DR yapılandırma [abonelikler arasında](https://azure.microsoft.com/blog/cross-subscription-dr) basittir. Çoğaltma zaman başka bir abonelik seçebilirsiniz.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Bölgeye sabitlenmiş Azure Vm'lerini başka bir bölgeye çoğaltabilir miyim?
Evet, [bölgeye sabitlenmiş Vm'lerini çoğaltma](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) başka bir bölgeye.

### <a name="can-i-exclude-disks"></a>Diskleri çoğaltmanın dışında tutabilirsiniz?

Evet, PowerShell kullanarak koruma süresi diskleri dışlayabilirsiniz. Daha fazla bilgi için [makale](azure-to-azure-exclude-disks.md)

### <a name="can-i-add-new-disks-to-replicated-vms-and-enable-replication-for-them"></a>Çoğaltılan VM'ler için yeni bir disk ekleyin ve miyim makinelere yönelik çoğaltmayı etkinleştirir?

Evet, bu yönetilen disklerle Azure Vm'leri için desteklenir. Çoğaltma için etkin bir Azure VM için yeni bir disk eklediğinizde, VM için çoğaltma durumu VM'deki diskler bir veya daha fazla koruma için kullanılabilir olduğunu belirten bir not ile bir uyarı gösterir. Eklenen diskler için çoğaltmayı etkinleştirebilirsiniz.
- Eklenen diskleri için korumayı etkinleştirin, uyarı ilk çoğaltmadan sonra kaybolur.
- Diske ait çoğaltma etkinleştirmemeyi seçerseniz, uyarıyı Kapat seçeneğini belirleyebilirsiniz.
- Çoğaltma noktaları olan bir disk ekleyin ve çoğaltmayı etkinleştirebilirsiniz VM yük devretme, kurtarma için kullanılabilir olan diskler gösterir. Örneğin, bir sanal makine tek bir diske sahiptir ve yeni bir tane ekleyin, disk eklemeden önce oluşturulan çoğaltma noktaları "2 disk 1" çoğaltma noktası içerdiğini gösterir.

Site Recovery "Sık erişimli çoğaltılmış bir VM'den bir diski Kaldır" desteklememektedir. Sanal makine diskini kaldırırsanız, devre dışı bırakın ve ardından sanal makine için çoğaltmayı yeniden etkinleştirmeniz gerekir.


### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?
Azure Vm'lerini başka bir Azure bölgesine çoğaltma yapıyorsanız sürekli çoğaltma olur. Daha fazla bilgi için [Azure'dan Azure'a çoğaltma mimarisi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process).

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-to-migrate-vms"></a>Bir bölge içindeki sanal makineler çoğaltabilirim? Bu sanal makineleri geçirmek istiyorum.
Azure'dan Azure'a DR çözümü, bir bölgede Vm'lerini çoğaltmak için kullanamazsınız.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Herhangi bir Azure bölgesine Vm'lerini çoğaltabilir miyim?
Site Recovery ile çoğaltma ve aynı coğrafi kümedeki her iki bölge arasında sanal makineleri kurtarma. Coğrafi kümeleri, veri gecikme süresi ve özerkliği aklınızda ile tanımlanır. Daha fazla bilgi için bkz: Site Recovery [bölge destek matrisi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support).

### <a name="does-site-recovery-require-internet-connectivity"></a>Site Recovery, internet bağlantısı gerekiyor mu?

Hayır, Site Recovery, Internet bağlantısı gerektirmez. Ancak, Site Recovery hizmeti URL'lerine ve IP aralıklarının belirtildiği gibi erişmesi [bu makalede](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges).

### <a name="can-i-replicate-the-application-having-separate-resource-group-for-separate-tiers"></a>Ayrı bir kaynak grubu ayrı katmanlara yönelik olan uygulama çoğaltabilirim?
Evet, uygulama çoğaltma ve olağanüstü durum kurtarma yapılandırması ayrı bir kaynak grubunda çok tutun.
Örneğin, bir uygulama ile varsa her uygulama, db ve ayrı bir kaynak grubundaki web katmanlarını ardından tıklayarak [çoğaltma sihirbazını](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication#enable-replication) thrice tüm katmanlarda korumak için. Site Recovery, üç farklı kaynak gruplarında bu üç katmanda çoğaltır.

## <a name="replication-policy"></a>Çoğaltma ilkesi

### <a name="what-is-a-replication-policy"></a>Bir çoğaltma ilkesi nedir?
Kurtarma noktası bekletme geçmişine ve uygulamayla tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur:

* kurtarma noktası bekletme geçmişine için 24'yi tıklatın.
* uygulamayla tutarlı anlık görüntü sıklığını 60 dakikadır.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings).

### <a name="what-is-a-crash-consistent-recovery-point"></a>Kilitlenmeyle tutarlı kurtarma noktası nedir?
Kilitlenmeyle tutarlı kurtarma noktası, sanki VM kilitlenmesi veya güç kablosunu sunucudan anlık görüntünün alındığı zaman çekilmiştir diskteki verileri temsil eder. Bellek anlık görüntü alındığında olan herhangi bir şey içermez.

Bugün, çoğu uygulama iyi kilitlenme ile tutarlı anlık görüntülerden kurtarabilirsiniz. Kilitlenmeyle tutarlı kurtarma noktası yeterince genellikle no-veritabanı işletim sistemleri ve dosya sunucuları, DHCP sunucuları ve yazdırma sunucuları gibi uygulamalar içindir.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>Kilitlenmeyle tutarlı kurtarma noktası oluşturma işlemi sıklığını nedir?
Site Recovery, her 5 dakikada bir kilitlenme ile tutarlı kurtarma noktası oluşturur.

### <a name="what-is-an-application-consistent-recovery-point"></a>Uygulamayla tutarlı kurtarma noktası nedir?
Uygulamayla tutarlı kurtarma noktaları, uygulamayla tutarlı anlık görüntülerden oluşturulur. Uygulamayla tutarlı kurtarma noktaları, bellekteki tüm verileri ve tüm işlemleri ile kilitlenme ile tutarlı anlık görüntüler aynı verileri yakalayın.
Ek içeriklerini nedeniyle uygulamayla tutarlı anlık görüntüleri en ilgili ve tanımladığımız gerçekleştirmek için gerçekleştirin. Veritabanı işletim sistemleri ve SQL Server gibi uygulamalar için uygulamayla tutarlı kurtarma noktalarını öneririz.

### <a name="what-is-the-impact-of-application-consistent-recovery-points-on-application-performance"></a>Uygulamayla tutarlı kurtarma noktası uygulama performansı üzerindeki etkisi nedir?
Uygulamayla tutarlı kurtarma noktalarını yakalar tüm verileri bellek içinde ve işlem dikkate framework sessiz moda alın, uygulama için windows VSS gibi gerektirir. Bu, çok sık yapıldığında olabilir performans etkisi iş yükü çok meşgul ise. Genellikle düşük sıklık düzeyi veritabanı olmayan iş yükleri için ve veritabanı iş yükü için bile uygulama ile tutarlı kurtarma noktaları için 1 saat kullanmayı düşürebilmek önerilir.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>Uygulamayla tutarlı kurtarma noktası oluşturma işlemi en az sıklığı nedir?
Site Recovery oluşturur bir uygulamayla tutarlı kurtarma noktası ile en az bir sıklığı 1 saat içinde.

### <a name="how-are-recovery-points-generated-and-saved"></a>Nasıl kurtarma noktaları oluşturulur ve kaydedildiğini?
Nasıl Site Recovery kurtarma noktaları oluşturur anlamak için kurtarma 24 saatlik saklama aralığı ve 1 saatlik bir sıklığı uygulamayla tutarlı anlık görüntü noktası bir çoğaltma ilkesi örneği ele alalım.

Site Recovery, her 5 dakikada bir kilitlenme ile tutarlı kurtarma noktası oluşturur. Kullanıcı bu sıklığı değiştirilemiyor. Son 1 saat için 12 kilitlenmeyle tutarlı kullanıcı sahiptir; bu nedenle aralarından seçim noktaları ve 1 uygulama ile tutarlı gelin. Zaman ilerledikçe, saat başına tüm kurtarma noktalarını son 1 saat ve kaydeder yalnızca 1 kurtarma noktası, Site Recovery ayıklar.

Aşağıdaki ekran görüntüsünde, örnek gösterir. Ekran görüntüsünde:

1. Süre değerinden son 1 saat, 5 dakikalık bir sıklık ile kurtarma noktaları vardır.
2. Son 1 saat ötesinde daha fazla süre için Site Recovery yalnızca 1 kurtarma noktası tutar.

   ![Oluşturulan kurtarma noktalarının listesi](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarma gerçekleştirebilir miyim?
Kullanabileceğiniz en eski kurtarma noktası değer 72 saattir.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>Bir çoğaltma ilkesi 24 saat ve bir sorun varsa ne olacağını 24 saatten fazla bir süre için Site Recovery oluşturma kurtarma noktalarından önler? Önceki kurtarma noktalarıma kaybolacak?
Hayır, Site Recovery, önceki tüm kurtarma noktalarını tutar. Yalnızca yeni noktaları nesil ise kurtarma noktası bekletme penceresi sırasında bağlı olarak, 24 saat bu durumda, Site Recovery eski noktasını değiştirir. Bu durumda, bazı sorun nedeniyle oluşturulan tüm yeni kurtarma noktası olmayacağından biz bekletme penceresi ulaştıktan sonra tüm eski noktalarını değişmeden olarak kalır.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Bir VM üzerinde çoğaltmayı etkinleştirdikten sonra çoğaltma ilkesi nasıl değiştirebilirim?
Git **Site kurtarma kasası** > **Site Recovery altyapısı** > **çoğaltma ilkeleri**. Düzenle ve değişiklikleri kaydetmek istediğiniz ilkeyi seçin. Herhangi bir değişiklik var olan tüm çoğaltmalar için de geçerlidir.

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>Tüm kurtarma noktalarını VM ya da bir fark tam bir kopyasını misiniz?
Oluşturulan ilk kurtarma noktasını tam kopya vardır. Art arda gelen kurtarma noktalarını delta değişiklikler var.

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>Kurtarma noktalarının saklama süresini artırmak depolama maliyeti mu?
Evet. 24 saatten saklama süresini 72 saate artırmak istiyorsanız, Site Recovery kurtarma noktaları için ek bir 48 saat kaydedin. Eklenen süre depolama ücreti uygulanacaktır. Örneğin, bir tek bir kurtarma noktası değişiklikleri / 10 GB ve GB başına maliyet aylık $0,16 ek ücretler $1.6 * 48 aylık olacaktır.

## <a name="multi-vm-consistency"></a>Çoklu VM tutarlılığı

### <a name="what-is-multi-vm-consistency"></a>Çoklu VM tutarlılığını nedir?
Bu kurtarma noktasını tüm çoğaltılan sanal makineler arasında tutarlı olduğundan emin olmak anlamına gelir.
Site Recovery ", bu seçeneği belirlediğinizde, grubun parçası olan tüm makineleri birlikte çoğaltmak için bir çoğaltma grubu oluşturur çoklu VM tutarlılığı,", bir seçenek sunar.
Yük devretme zaman tüm sanal makineleri kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşılan.
Go için Bu öğreticide [çoklu VM tutarlılığını etkinleştirmek](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication-for-a-vm).

### <a name="can-i-failover-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Alabilirim bir çoklu VM tutarlılığı çoğaltma grubu içindeki tek sanal makine yük devretme?
"Çoklu VM tutarlılığı" seçeneğini belirleyerek, uygulama bir grup içindeki tüm sanal makineler üzerinde bir bağımlılık olduğunu belirten. Bu nedenle, tek sanal makine yük devretme izin verilmez.

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>Bir çoklu VM tutarlılığı çoğaltma grubunun bir parçası olarak kaç tane sanal makineyi çoğaltabilirim?
Bir çoğaltma grubunda birlikte 16 sanal makinelerini çoğaltabilirsiniz.

### <a name="when-should-i-enable-multi-vm-consistency-"></a>Çoklu VM tutarlılığı etkinleştirdiğinizde?
CPU bakımından yoğun olduğundan, çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir. Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden fazla makine arasında tutarlılık ihtiyacınız varsa kullanılmalıdır. Örneğin, bir uygulamada iki SQL Server örneği ve iki web sunucusu varsa, yalnızca SQL Server örnekleri için çoklu VM tutarlılığı olmalıdır.


## <a name="failover"></a>Yük devretme

### <a name="how-is-capacity-assured-in-target-region-for-azure-vms"></a>Nasıl kapasite Azure Vm'leri için hedef bölgede sağlanmıştır?
Site Recovery ekibi yeterli altyapı kapasiteyi planlamak üzere Azure kapasitesine yönetim ekibi ile çalışır, yük devretme başlatıldığında, olağanüstü durum kurtarma için etkinleştirilmiş olan VM'ler sağlamaya yardımcı olmak için başarıyla hedef bölgede dağıtılır.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?

Yük devretme işlemi otomatik değildir. Yük devretmeleri tek bir tıklamayla portaldan başlayın ya da kullanabilirsiniz [PowerShell](azure-to-azure-powershell.md) bir yük devretmeyi tetiklemek için.

### <a name="can-i-retain-a-public-ip-address-after-failover"></a>Yük devretme sonrasında genel IP adresi tutabilir miyim?

Üretim uygulamasının genel IP adresi, yük devretme sonrasında tutulamıyor.
- Yük devretme işleminin bir parçası hedef bölgede kullanılabilir olan bir Azure genel IP kaynağı atanması gerektiğinden getirdiği iş yükleri.
- El ile yapılması veya bir kurtarma planı otomatikleştirin.
- Bilgi edinmek için nasıl [yük devretme sonrasında genel IP adreslerini ayarlama](concepts-public-ip-address-with-site-recovery.md#public-ip-address-assignment-using-recovery-plan).  

### <a name="can-i-retain-a-private-ip-address-during-failover"></a>Yük devretme sırasında özel bir IP adresi tutabilir miyim?
Evet, özel bir IP adresi tutabilirsiniz. Azure Vm'leri için olağanüstü durum kurtarma etkinleştirdiğinizde, varsayılan olarak, Site Recovery hedef kaynaklar kaynak kaynak ayarları temel alarak oluşturur. -Statik IP adresleriyle yapılandırılmış Azure Vm'leri için Site Recovery kullanımda değilse hedef sanal makine, aynı IP adresi sağlamak çalışır.
Hakkında bilgi edinin [yük devretme sırasında IP adreslerini koruma](site-recovery-retain-ip-azure-vm-failover.md).

### <a name="after-failover-why-is-the-server-assigned-a-new-ip-address"></a>Yük devretmeden sonra neden sunucunun yeni bir IP adresi atanmış?

Site Recovery, yük devretme sırasında IP adresi sağlamak çalışır. Başka bir sanal makine bu adrese sürüyorsa, Site Recovery sonraki kullanılabilir IP adresi hedef olarak ayarlar.
Daha fazla bilgi edinin [ağ eşlemesini ve sanal ağlar için IP adresini ayarlama](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms).

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>Hangi **en son (en düşük RPO)** kurtarma noktaları?
**En son (en düşük RPO)** seçeneği ilk önce yük devretme için her VM için bir kurtarma noktası oluşturmak için Site Recovery hizmetine gönderilen tüm verileri işler. Yük devretme yük devretme tetiklendiğinde Site Recovery'ye çoğaltılan tüm verilere sahip olduktan sonra VM oluşturulduğundan, bu seçenek en düşük kurtarma noktası hedefi (RPO) sağlar.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>Yapmak **en son (en düşük RPO)** kurtarma noktaları RTO yük devretmede bir etkiye sahip?
Evet. Bu seçeneğe sahip diğer seçenekleri karşılaştırıldığında daha yüksek kurtarma süresi hedefi (RTO) için site kurtarma, yük devretmeyi önce tüm bekleyen verileri işler.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>Ne yaptığını **en son işlenen** kurtarma seçeneği işaret ortalama?
**Son işlenme** seçeneği başarısız en son kurtarma planındaki tüm sanal makineler üzerinde işlenen, Site kurtarma noktası. En son kurtarma için belirli bir VM'ye noktası olarak görmek için **en son kurtarma noktaları** VM ayarları. Bu seçenek, işlenmemiş verileri işlemeye zaman harcanmadığından düşük bir RTO sağlar.

### <a name="what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>My birincil bölgeye beklenmeyen bir kesinti oluşursa ne olur?
Kesinti bir yük devretme tetikleyebilirsiniz. Site Recovery, yük devretme gerçekleştirmek için birincil bölgeden bağlantı gerek yoktur.

### <a name="what-is-a-rto-of-a-vm-failover-"></a>VM yük devretme bir RTO nedir?
Site Recovery sahip bir [2 saat RTO SLA](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/). Ancak, çoğu zaman, Site Recovery başarısız yükü Devredilmiş sanal makineleri dakikalar içinde. RTO hesaplayabilirsiniz saati gösteren yük devretme için işleri giderek VM'yi getirmek için işlem. RTO için kurtarma planında, bölüme bakın.

## <a name="recovery-plans"></a>Kurtarma planları

### <a name="what-is-a-recovery-plan"></a>Kurtarma planı nedir?
Site Recovery kurtarma planında yük devretme VM'lerin kurtarılmasını sağlar. Bu, tutarlı bir şekilde doğru yinelenebilir ve Otomatik Kurtarma olmasına yardımcı olur. Bir kurtarma planı kullanıcı için aşağıdaki gereksinimleri ele alır:

- Bir grup sanal makinelerin birlikte yük devredebilmesi tanımlama
- Uygulama doğru bir şekilde gelir, böylece sanal makineler arasındaki bağımlılıkları tanımlama
- Kurtarma görevleri sanal makinelerin Yük Devretmesini dışında elde etmek için özel el ile yapılan Eylemler birlikte otomatikleştirme

[Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md) kurtarma planları hakkında.

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>Sıralama, bir kurtarma planında nasıl sağlanır?

Bir kurtarma planında sıralama elde etmek için çeşitli gruplar oluşturabilirsiniz. Her gruba aynı anda üzerinde başarısız olur. Aynı grubu başarısız parçası üzerinde birlikte, başka bir grubu tarafından izlenen VM'ler. Bir kurtarma planı kullanarak bir uygulama modelini öğrenmek için bkz. [kurtarma planlarıyla ilgili](recovery-plan-overview.md#model-apps).

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Bir kurtarma planı RTO nasıl bulabilirim?
Bir kurtarma planı RTO denetlemek için kurtarma planı için bir yük devretme testi yapın ve Git **Site Recovery işleri**.
Aşağıdaki örnekte, SAPTestRecoveryPlan adlı işi 8 dakika 59 tüm sanal makineler üzerinde başarısız olur ve belirtilen eylemleri gerçekleştirmek için saniye sürdü.

![Site Recovery işleri listesi](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kurtarma planına Otomasyon runbook'ları ekleyebilir miyim?
Evet, Azure Otomasyonu runbook'ları, kurtarma planına tümleştirebilirsiniz. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md).

## <a name="reprotection-and-failback"></a>Yeniden koruma ve yeniden çalışma

### <a name="after-a-failover-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Yük devretme sonrasında birincil bölgeden için bir olağanüstü durum kurtarma bölgesindeki VM'ler otomatik olarak korunan DR bölgesinde misiniz?
Hayır. Olduğunda, [yük devretme](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) Azure Vm'leri bir bölgeden diğerine Vm'leri başlatma DR bölgesinde korumasız bir durumda. Vm'leri birincil bölgeye yeniden çalışma için şunları yapmanız [yeniden koruma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) ikincil bölgedeki Vm'leri.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>Yeniden koruma anında Site Recovery tam veri birincil bölgeye ikincil bölgeden çoğaltabilir mu?
Bu, durumunuza bağlıdır. Örneğin, VM Kaynak bölgesi varsa, kaynak ve hedef disk arasında yalnızca değişiklikleri eşitlenir. Site Recovery, diskleri karşılaştırarak differentials hesaplar ve ardından verileri aktarır. Bu işlem genellikle birkaç saat sürer. Yeniden koruma sırasında neler olduğu hakkında daha fazla bilgi için bkz. [birincil bölgeye yeniden koruma başarısız Azure Vm'leri üzerinde]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).

### <a name="how-much-time-does-it-take-to-fail-back"></a>Ne kadar zaman mevcut yeniden çalışma için sınav zamanı?
Yeniden koruma sonra yeniden çalışma için süreyi genellikle birincil bölgeden ikincil bölgeye yük devretme için gerekli süreyi benzer.

## <a name="capacity"></a>Kapasite

### <a name="how-is-capacity-assured-in-target-region-for-azure-vms"></a>Nasıl kapasite Azure Vm'leri için hedef bölgede sağlanmıştır?
Site Recovery ekibi yeterli altyapı kapasiteyi planlamak üzere Azure kapasitesine yönetim ekibi ile çalışır, yük devretme başlatıldığında, olağanüstü durum kurtarma için etkinleştirilmiş olan VM'ler sağlamaya yardımcı olmak için başarıyla hedef bölgede dağıtılır.

### <a name="does-site-recovery-work-with-reserved-instances"></a>Site Recovery, ayrılmış örnekleri ile çalışır mı?
Evet, satın [rezerve örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/) olağanüstü durum kurtarma bölgesindeki ve Site Recovery yük devretme işlemlerini bunları kullanacaksınız. </br> Ek bir yapılandırma gerekmez.


## <a name="security"></a>Güvenlik

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  
Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, hem şifreleme-aktarım sırasında ve [durağan azure'da şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
* [Gözden geçirme](azure-to-azure-support-matrix.md) gereksinimlerini destekler.
* [Ayarlanan](azure-to-azure-tutorial-enable-replication.md) Azure'dan Azure'a çoğaltma.
- Bu makaleyi okuduktan sonra sorularınız varsa gönderin [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).
