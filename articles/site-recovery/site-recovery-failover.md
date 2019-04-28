---
title: Yük devretme sırasında Azure Site Recovery ile olağanüstü durum kurtarma | Microsoft Docs
description: Döndürme hakkında üzerinde Vm'leri ve fiziksel sunucuları Azure Site Recovery hizmeti ile olağanüstü durum kurtarma sırasında öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 1/18/2019
ms.author: mayg
ms.openlocfilehash: 8f76d4e54133e4e899e707e666703a67310e8702
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61280495"
---
# <a name="fail-over-vms-and-physical-servers"></a>Vm'leri ve fiziksel sunucuları başarısız 

Bu makalede nasıl yük devretme sanal makinelere ve fiziksel Site Recovery tarafından korunan sunucular açıklanır.

## <a name="prerequisites"></a>Önkoşullar
1. Yük devri gerçekleştirmeden önce yapmanız bir [yük devretme testi](site-recovery-test-failover-to-azure.md) her şeyin beklendiği gibi çalıştığından emin olmak için.
1. [Ağ hazırlama](site-recovery-network-design.md) yük devri gerçekleştirmeden önce hedef konumunda.  

Azure Site Recovery tarafından sağlanan yük devretme seçenekleri hakkında bilgi edinmek için aşağıdaki tabloyu kullanın. Bu seçenekler, farklı bir yük devretme senaryosu da listelenir.

| Senaryo | Uygulama Kurtarma gereksinimi | Hyper-v iş akışı | VMware için iş akışı
|---|--|--|--|
|Yaklaşan bir veri merkezinde bir kesinti nedeniyle planlı yük devretme| Sıfır veri kaybıyla planlı bir etkinliği gerçekleştirildiğinde uygulama| Hyper-V için kullanıcı tarafından belirtilen bir kopyalama sıklığı veri ASR çoğaltır. Planlı yük devretme sıklığını geçersiz kıl ve bir yük devretme başlatılmadan önce son değişiklikleri çoğaltmak için kullanılır. <br/> <br/> 1.    Bir bakım penceresi işletmenizin değişiklik Yönetimi sürecini göre planlayın. <br/><br/> 2, yaklaşan bir kapalı kalma süresi kullanıcılara bildirin. <br/><br/> 3. Kullanıcıya yönelik uygulamayı çevrimdışı duruma getirin.<br/><br/>4 ASR portalı kullanarak planlı yük devretme başlatın. Şirket içi sanal makine otomatik olarak kapalı.<br/><br/>Etkili uygulama veri kaybı = 0 <br/><br/>Bir günlük kurtarma noktası, daha eski bir kurtarma noktasına kullanmak isteyen bir kullanıcı için bir saklama aralığı içinde de sağlanır. (24 saat bekletme için Hyper-V). Bekletme süresi çerçeve dışına çoğaltma durduruldu, müşterilerin en son kullanılabilir kurtarma noktalarını kullanarak yük devretme mümkün olabilir. | VMware için sürekli olarak CDP kullanarak verileri ASR çoğaltır. Yük devretme kullanıcı yük devretme seçeneğini (post uygulama kapalı dahil) en son verileri sağlar.<br/><br/> 1. Değişiklik Yönetimi sürecinin göre bir bakım penceresi planlama <br/><br/>2 kullanıcılara yaklaşan bir kapalı kalma süresi bildir <br/><br/>3.    Kullanıcıya yönelik uygulamayı çevrimdışı duruma getirin. <br/><br/>4.  Planlanan uygulamayı çevrimdışı duruma geldikten sonra en son tarihli ASR portal'ı kullanarak yük devretme başlatın. Portal'daki "Planlanmamış yük devretme" seçeneğini kullanın ve en son noktaya yük devretme seçin. Şirket içi sanal makine otomatik olarak kapalı.<br/><br/>Etkili uygulama veri kaybı = 0 <br/><br/>Kurtarma noktası bekletme penceresi içinde bir günlük daha eski bir kurtarma noktasına kullanmak isteyen bir müşteri için sağlanır. (72 saate kadar bekletme VMware için). Bekletme süresi çerçeve dışına çoğaltma durduruldu, müşterilerin en son kullanılabilir kurtarma noktalarını kullanarak yük devretme mümkün olabilir.
|Yük devretmesi nedeniyle bir datacenter Planlanmamış kapalı kalma süresi (doğal veya BT olağanüstü durum) | Uygulama için en düşük düzeyde veri kaybı | 1. kuruluşun BCP planı başlatma <br/><br/>2. ASR portalından en son veya noktası için bekletme süresinin (günlük) kullanarak planlanmamış yük devretme başlatın.| 1. Kuruluşun BCP planı başlatın. <br/><br/>2.  ASR portalından en son veya noktası için bekletme süresinin (günlük) kullanarak, planlanmamış yük devretme başlatın.


## <a name="run-a-failover"></a>Yük devretme çalıştırma
Bu yordam, bir yük devretme için çalıştırma işlemi açıklanır bir [kurtarma planı](site-recovery-create-recovery-plans.md). Alternatif olarak, tek sanal makineleri veya fiziksel sunucudan için yük devretme çalıştırabilirsiniz **çoğaltılan öğeler** sayfasında açıklandığı gibi [burada](vmware-azure-tutorial-failover-failback.md#run-a-failover-to-azure).


![Yük devretme](./media/site-recovery-failover/Failover.png)

1. Seçin **kurtarma planları** > *recoveryplan_name*. Tıklayın **yük devretme**
2. Üzerinde **yük devretme** ekranındayken bir **kurtarma noktası** Yük Devretmesini. Şu seçeneklerden birini kullanabilirsiniz:
   1. **En son**: Bu seçenek, Site Recovery hizmetine gönderilen tüm verileri işleyerek işi başlatır. Veri işleme, her sanal makine için bir kurtarma noktası oluşturur. Bu kurtarma noktasını sanal makine için yük devretme sırasında kullanılır. Bu seçenek en düşük RPO (kurtarma noktası hedefi) sanal yük devretme tüm verilere sahip sonra oluşturulan yük devretme tetiklendiğinde Site Recovery hizmetine çoğaltılan makineyi sağlar.
   1. **En son işlenen**: Bu seçenek tüm sanal makineler kurtarma planının Site Recovery hizmeti tarafından zaten işlenmiş en son kurtarma noktasına devreder. Bir sanal makinenin yük devretme Testi gerçekleştirirken, en son işlenen kurtarma noktasının zaman damgası da gösterilir. Bir kurtarma planı yük devretme yapıyorsanız, tek tek sanal makineye gidin ve bakmak **en son kurtarma noktaları** bu bilgileri almak için bir kutucuk. İşlenmemiş verileri işlemek için zaman harcanmadığından gibi bu seçenek, düşük bir RTO (Kurtarma süresi hedefi) yük devretme seçeneği sağlar.
   1. **Uygulamayla tutarlı olan sonuncu**: Bu seçenek tüm sanal makineler kurtarma planının Site Recovery hizmeti tarafından zaten işlenmiş en son uygulamayla tutarlı kurtarma noktasına devreder. Bir sanal makinenin yük devretme Testi gerçekleştirirken, en son uygulamayla tutarlı kurtarma noktası zaman damgasını da gösterilir. Bir kurtarma planı yük devretme yapıyorsanız, tek tek sanal makineye gidin ve bakmak **en son kurtarma noktaları** bu bilgileri almak için bir kutucuk.
   1. **En son işlenen VM'li**: Bu seçenek yalnızca, çoklu VM tutarlılığı üzerinde en az bir sanal makineyle olan kurtarma planları için kullanılabilir. Çoğaltma grubu yük devretme için en son genel çoklu VM tutarlı kurtarma parçası olan sanal makineleri işaret edin. Diğer sanal makinelerin yük devretme, işlenen en son kurtarma noktasına.  
   1. **En son çoklu VM uygulamayla tutarlı**: Bu seçenek yalnızca, çoklu VM tutarlılığı üzerinde en az bir sanal makineyle olan kurtarma planları için kullanılabilir. Çoğaltma grubu yük devretme için en son genel çoklu VM uygulamayla tutarlı kurtarma parçası olan sanal makineleri işaret edin. Diğer sanal makinelerin yük devretme, en son uygulamayla tutarlı kurtarma noktası.
   1. **Özel**: Bir sanal makine test yük devretmesi yapıyorsanız, belirli bir kurtarma noktasına yük devretme için bu seçeneği kullanabilirsiniz.

      > [!NOTE]
      > Bir kurtarma noktası seçim yapma olanağı, yalnızca Azure'a üzerinden başarısız olduğu halde kullanılabilir.
      >
      >


1. Bazı sanal makineler kurtarma planında üzerinden önceki bir çalıştırmada başarısız ve sanal makineler hem kaynak hem de hedef konumunda etkin olan artık kullanabileceğiniz **yönünü değiştirme** hangi yönde karar vermek için seçeneği Yük devretme gerçekleşmelidir.
1. Azure'a devretmek ve veri şifreleme (yalnızca Hyper-v sanal makinelerini VMM sunucusundan korunan olduğunda geçerlidir) bulut için etkin değilse, **şifreleme anahtarı** , etkinleştirildiğinde verilmiş sertifikayı seçin VMM sunucusunda kurulumu sırasında veri şifrelemesi.
1. Seçin **kapalı makine yük devretmeye başlamadan önce** Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemek istiyorsanız. Kapalı başarısız olsa bile yük devretme devam eder.  

    > [!NOTE]
    > Hyper-v sanal makineler korunursa de kapalı seçeneğini henüz hizmete yük devretmeyi tetiklemeden önce gönderilmedi şirket içi verileri eşitlemek çalışır.
    >
    >

1. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz. Planlanmamış yük devretme sırasında bir hata oluşmamasına olsa da, kurtarma planı tamamlanana kadar çalışır.
1. Yük devretmeden sonra sanal makine tarafından günlük ona doğrulayın. Sanal makinenin başka bir kurtarma noktasına geçiş yapmak istediğinizi sonra kullanabileceğiniz **kurtarma noktasını Değiştir** seçeneği.
1. Yük devredilmiş sanal makineden memnun kaldığınızda, yük devretmeyi **Yürütebilirsiniz**. **İşleme hizmette kullanılabilir olan tüm kurtarma noktalarını siler** ve **kurtarma noktasını Değiştir** seçenek, artık kullanılabilir.

## <a name="planned-failover"></a>Planlı yük devretme
Site Recovery ayrıca desteği kullanılarak korunan sanal makineler/fiziksel sunucuları **planlı yük devretme**. Planlı yük devretme sıfır veri kaybı yük devretme seçeneğini ' dir. Planlı yük devretme tetiklendiğinde, önce kaynak sanal makine kapalı, en son verileri eşitlenir ve ardından bir yük devretme tetiklenir.

> [!NOTE]
> Bir şirket içi siteden başka bir şirket içi siteye Hyper-v sanal makinelerin yük devretme sırasında birincil şirket içi siteye geri dönmeniz için ilk sahip olduğunuz **ters çoğaltma** sanal makine yedekleme için birincil site ve ardından bir yük devretme tetikleyin. Birincil sanal makine için önce başlangıç kullanılabilir değilse, **ters çoğaltma** sanal makine bir yedekten geri yüklemek zorunda.   
> 
> 
> ## <a name="failover-job"></a>Yük devretme işi

![Yük devretme](./media/site-recovery-failover/FailoverJob.png)

Bir yük devretme tetiklendiğinde, aşağıdaki adımları içerir:

1. Önkoşulları denetleyin: Bu adım, yük devretme için gerekli tüm koşulların karşılandığından sağlar
1. Yük devretme: Bu adım verileri işler ve böylece dışında bir Azure sanal makinesi oluşturulabilir hazır kolaylaştırır. Seçtiyseniz **son** kurtarma noktası, bu adım bir kurtarma noktası oluşturur hizmete gönderilen verilerden.
1. Başlat: Bu adım, önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturur.

> [!WARNING]
> **Devam eden işlemini iptal etmeyin yük devretme**: Yük devretme başlatılmadan önce sanal makine için çoğaltma durdurulur. Varsa, **iptal** bir ilerleme işinde yük devretme durdurulur, ancak sanal makine başlamayacak çoğaltmak. Çoğaltma yeniden başlatılamıyor.
>
>

## <a name="time-taken-for-failover-to-azure"></a>Azure'a yük devretme için harcanan süre

Bazı durumlarda, sanal makinelerin yük devretmesi, genellikle tamamlanması yaklaşık 8-10 dakika sürer fazladan ara adım gerektirir. Aşağıdaki durumlarda yük devretme için geçen süre işlem normalden biraz daha yüksek olur:

* Mobility hizmeti 9.8 eski bir sürümü kullanarak VMware sanal makineleri
* Fiziksel sunucular
* VMware Linux sanal makineleri
* Fiziksel sunucuları olarak korunan Hyper-V sanal makineler
* VMware sanal makinelerini burada aşağıdaki sürücüler olarak mevcut olmayan sürücüler önyükleme
    * storvsc
    * vmbus
    * storflt
    * intelide
    * Atapi
* VMware sanal makinelerini, DHCP hizmeti, DHCP veya statik kullanarak bağımsız olarak etkin olmayan IP adresleri

Diğer tüm durumlarda, bu ara adım gerekli değildir ve yük devretme için geçen süre düşüktür.

## <a name="using-scripts-in-failover"></a>Yük devretme kümesinde komut dosyalarını kullanma
Bir yük devretme yaparken belirli eylemleri otomatikleştirmek isteyebilirsiniz. Komut dosyalarını kullanabilirsiniz veya [Azure Otomasyonu runbook'ları](site-recovery-runbook-automation.md) içinde [kurtarma planları](site-recovery-create-recovery-plans.md) Bunu yapmak için.

## <a name="post-failover-considerations"></a>Yük devretme konuları gönderin
Aşağıdaki önerileri göz önünde bulundurun isteyebileceğiniz yük devretme sonrası:
### <a name="retaining-drive-letter-after-failover"></a>Yük devretmeden sonra sürücü harfi korunuyor
Yük devretmeden sonra sanal makinelerde sürücü harfini korumak için ayarlayabileceğiniz **SAN ilkesinin** sanal makine için **OnlineAll**. [Daha fazla bilgi edinin](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretme sonrasında RDP/SSH kullanarak Azure VM'lerine bağlanmak istiyorsanız [buradaki](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) tabloda özetlenen gereksinimleri izleyin.

Yük devretme sonrasında karşılaştığınız bağlantı sorunlarını gidermek için [burada](site-recovery-failover-to-azure-troubleshoot.md) anlatılan adımları izleyin.


## <a name="next-steps"></a>Sonraki adımlar

> [!WARNING]
> Sanal makineler üzerinde başarısız olan ve şirket içi veri merkezi kullanılabilir sonra yapmanız gerekenler [ **yeniden koruma** ](vmware-azure-reprotect.md) VMware sanal makinelerini yedeklemek için şirket içi veri merkezi.

Kullanım [ **planlı yük devretme** ](hyper-v-azure-failback.md) seçeneğini **geri dönme** azure'dan Hyper-v sanal makinelerini şirket içi geri dön.

Üzerinden bir Hyper-v sanal makine başka bir veri merkezinde VMM sunucusu tarafından yönetilen şirket içinde ve birincil veri merkezinde kullanılabilir, başarısız olursa, ardından **ters çoğaltma** tekrar birincil veri için çoğaltma başlatma seçeneği Merkezi.
