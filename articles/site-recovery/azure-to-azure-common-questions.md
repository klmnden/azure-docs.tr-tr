---
title: Ortak soruları - Azure Site Recovery ile Azure'a olağanüstü durum kurtarma | Microsoft Docs
description: Azure Site Recovery ile Azure bölgesi için aonther Azure Vm'leri olağanüstü durum kurtarma ayarlama ayarladığınızda bu makalede, sık sorulan sorular özetler.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: ced306b00a5761ae096e918b43a8e67e649580dd
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106027"
---
# <a name="common-questions---azure-to-azure-replication"></a>Sık sorulan sorular - Azure'dan Azure'a çoğaltma

Bu makalede, Azure Vm'leri olağanüstü durum kurtarma için başka bir Azure bölgesine dağıtırken görüyoruz yaygın soruların yanıtları sağlanır. Bu makaleyi okuduktan sonra sorularınız varsa gönderin [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Genel
### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?
Gözden geçirme [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) ayrıntıları.

### <a name="how-to-configure-site-recovery-on-azure-vms-what-are-the-best-practices"></a>Site Recovery Azure Vm'leri üzerinde yapılandırılır. En iyi uygulamalar nelerdir?
1. [Azure'dan Azure'a mimarisini anlama](azure-to-azure-architecture.md)
2. [Desteklenen ve desteklenmeyen yapılandırmalarını gözden geçirin](azure-to-azure-support-matrix.md)
3. [Azure Vm'leri için olağanüstü durum kurtarmayı ayarlayın](azure-to-azure-how-to-enable-replication.md)
4. [Yük devretme testi çalıştırma](azure-to-azure-tutorial-dr-drill.md)
5. [Yük devretme ve birincil bölgeye geri dönme](azure-to-azure-tutorial-failover-failback.md)

## <a name="replication"></a>Çoğaltma

### <a name="can-i-replicate-azure-disk-encryption-enabled-vms"></a>Azure çoğaltma disk şifreleme etkin Vm'leri?
Evet, bunları çoğaltabilirsiniz. Başvuru [makale](azure-to-azure-how-to-enable-replication-ade-vms.md) Azure disk şifrelemesi (ADE) etkin çoğaltma Vm'leri etkinleştirmek için. Lütfen Windows işletim sistemi çalıştıran ve Azure AD uygulaması ile şifreleme etkin yalnızca Azure Vm'leri Azure Site Recovery tarafından şu anda desteklenmediğini unutmayın.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Başka bir abonelik için sanal makineleri çoğaltabilir miyim?
Evet, farklı bir abonelikte ile aynı Azure Active Directory kiracısı için Azure Vm'lerini çoğaltabilirsiniz.
DR yapılandırma [abonelikler arasında](https://azure.microsoft.com/blog/cross-subscription-dr) basittir. Çoğaltma zaman başka bir abonelik seçebilirsiniz.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Bölgeye sabitlenmiş Azure Vm'lerini başka bir bölgeye çoğaltma.
Evet, [bölgeye sabitlenmiş Vm'lerini çoğaltma](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) başka bir bölgeye.

### <a name="can-i-exclude-disks"></a>Diskleri çoğaltmanın dışında tutabilirsiniz?

Evet, güç Kabuğu'nu kullanarak koruma süresi diskleri dışarıda tutabilirsiniz. Başvuru [powershell Kılavuzu](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine) diski dışlamak için

###<a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?
Azure Vm'lerini başka bir Azure bölgesine çoğaltma yaparken çoğaltma sürekli olarak yapılır. Denetleme [Azure'dan Azure'a](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process) ayrıntılarını anlamak için çoğaltmayı mimarisi.

### <a name="can-i-replicate-virtual-machines-within-a-same-region-i-need-this-to-migrate-vms"></a>Aynı bölge içindeki sanal makineler çoğaltabilirim? Bu sanal makineleri geçirmek için ihtiyacım var?
Azure'dan Azure'a DR çözümü, aynı bölgede Vm'lerini çoğaltmak için kullanamazsınız.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Herhangi bir Azure bölgesine Vm'lerini çoğaltabilir miyim?
Site Recovery ile çoğaltma ve aynı coğrafi kümedeki her iki bölge arasında sanal makineleri kurtarma. Coğrafi kümeleri, veri gecikme süresi ve özerkliği göz önünde bulundurarak tanımlanır. Daha fazla bilgi için bkz: Site Recovery [bölge destek matrisi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support).

### <a name="does-site-recovery-require-internet-connectivity"></a>Site Recovery, internet bağlantısı gerekiyor mu?

Hayır, Site Recovery, ancak Site Recovery hizmeti URL'lerine ve IP aralıklarının erişimi internet bağlantısı bu belirtildiği gibi gerektirmez [makale](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges)

## <a name="replication-policy"></a>Çoğaltma İlkesi

### <a name="what-is-a-replication-policy"></a>Bir çoğaltma ilkesi nedir?
Kurtarma noktası bekletme geçmişine ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery, ' 24 saatliğine kurtarma noktası bekletme ' 60 dakika için uygulamayla tutarlı anlık görüntü sıklığını ve varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings) çoğaltma ilkesini yapılandırma]

### <a name="what-is-crash-consistent-recovery-point"></a>Kilitlenmeyle tutarlı kurtarma noktası nedir?
Kilitlenmeyle tutarlı kurtarma noktası, sanki VM kilitlenmesi veya güç kablosunu sunucudan anlık görüntünün alındığı zaman çekilmiştir diskteki verileri temsil eder. Bellek anlık görüntü alındığında olan herhangi bir şey içermez. Bugün, çoğu uygulama iyi kilitlenme tutarlı anlık görüntülerden kurtarabilirsiniz. Kilitlenmeyle tutarlı kurtarma noktası yeterince genellikle hiçbir veritabanı işletim sistemleri ve dosya sunucuları, DHCP sunucusu, yazdırma sunucuları ve benzeri gibi uygulamalar içindir.

### <a name="what-is-application-consistent-recovery-point"></a>Uygulamayla tutarlı kurtarma noktası nedir? 
Uygulamayla tutarlı kurtarma noktaları, bellekteki tüm verileri ve tüm işlemleri ile kilitlenme ile tutarlı anlık görüntüler aynı verileri yakalama uygulamayla tutarlı anlık görüntülerden oluşturulur. Ek içeriklerini nedeniyle uygulamayla tutarlı anlık görüntüleri en ilgili ve tanımladığımız gerçekleştirmek için gerçekleştirin. Veritabanı işletim sistemleri ve SQL gibi uygulamalar için uygulamayla tutarlı kurtarma noktalarını önerilir.

### <a name="how-are-recovery-points-generated-and-saved"></a>Nasıl kurtarma noktaları oluşturulur ve kaydedildiğini?
Anlamak için kurtarma noktası 24 saatlik saklama aralığı ve uygulama tutarlı sıklığı anlık görüntü 1 saatlik sahip Çoğaltma İlkesi ' nin bir örneği ele alalım nasıl Site Recovery kurtarma noktaları oluşturur olanak tanır.
Site Recovery, her 5 dakikada bir ve kullanıcı bu sıklığını değiştirmek için herhangi bir denetime sahip kilitlenme tutarlı noktası oluşturur. Bu nedenle, son için kilitlenmeyle tutarlı 12 punto ve aralarından seçim yapabileceğiniz 1 uygulama tutarlı nokta bir saatlik kullanıcı olacaktır. Zaman ilerledikçe, saat başına tüm kurtarma noktalarını ötesine son 1 saat ve yalnızca bir kurtarma noktası, Site Recovery ayıklar.
Çizim için aşağıdaki ekran görüntüsünde:


1. Küçüktür son 1 saat süreyle 5 dakikalık sıklığı kurtarma noktalarıyla vardır.
2. Son 1 saat ötesinde daha fazla süre için saat başına yalnızca bir kurtarma noktası tutulur görebiliriz.

  ![kurtarma noktası oluşturma](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarma gerçekleştirebilir miyim?
Kullanabileceğiniz en eski kurtarma noktası değer 72 saattir.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-there-is-no-recovery-points-generation-due-to-some-issue-for-more-than-24-hours-does-my-previous-recovery-points-will-be-pruned"></a>24 saatlik bir çoğaltma ilkesi sahibim ve kurtarma noktalarını oluşturmamak nedeniyle bazı sorun 24 saatten fazla için varsa ne. My önceki kurtarma noktalarını ayıklama mu?
Hayır, Site Recovery bu durumda, tüm önceki kurtarma noktalarını tutar. 

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Bir VM üzerinde çoğaltmayı etkinleştirdikten sonra çoğaltma ilkesi nasıl değiştirebilirim? 
Site Recovery kasası gidin > Site Recovery altyapısı > çoğaltma ilkeleri. Düzenle ve değişiklikleri kaydetmek istediğiniz ilkeyi seçin. Herhangi bir değişiklik var olan tüm çoğaltmalar için de geçerlidir. 

### <a name="are-all-the-recovery-points-complete-copy-of-the-vm-or-differential"></a>Tüm kurtarma noktalarını VM veya fark tam kopyasını misiniz?
Tam kopya olması durumunda ilk çoğaltma oluşturulan ilk kurtarma noktasını sahip olur ve delta değişikliklerinin art arda gelen kurtarma noktası gerekir.

### <a name="does-increasing-recovery-points-retention-windows-increases-the-storage-cost"></a>Mu kurtarma noktalarının bekletme pencerelerinin artırılması artırır depolama maliyeti?
Site Recovery kurtarma noktaları ekleme kaydedecek sonra 24 saat saklama süresini 72 saate artırırsanız Evet, hangi olacak 48 saat, depolama ücretleri. Delta değişikliklerinin / 10 GB ve GB başına maliyet tek bir kurtarma noktası varsa, aylık $1.6 * 48 ek ücret olacaktır sonra örnek, aylık $0,16 için.

## <a name="multi-vm-consistency"></a>Çoklu VM tutarlılığı 

### <a name="what-is-multi--vm-consistency"></a>Çoklu VM tutarlılığını nedir?
Bu kurtarma noktasını tüm çoğaltılan sanal makineler arasında tutarlı olduğundan emin olmak anlamına gelir.
Site Recovery, bir "Çoklu VM tutarlılığı" seçeneğini sağlar olan seçili grubun parçası olan tüm makineleri birlikte çoğaltmak için bir çoğaltma grubu oluşturur.
Tüm sanal makinelerin yük devretme sırasında kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşılan.
Go için Bu öğreticide ["Çoklu VM" tutarlılığını etkinleştirmek](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication).

### <a name="can-i-failover-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Alabilir miyim "Çoklu VM" tutarlılık çoğaltma grubundaki tek sanal makine yük devretme?
"Çoklu VM tutarlılığı" seçeneğini belirleyerek uygulamayı bir grup içindeki tüm sanal makineler üzerinde bir bağımlılık olduğunu belirten. Bu nedenle, tek sanal makine yük devretme izin verilmez. 

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-multi-vm-consistency-replication-group"></a>"Çoklu VM" tutarlılık çoğaltma grubunun bir parçası kaç tane sanal makineyi çoğaltabilirim?
Bir çoğaltma grubunda birlikte 16 sanal makine çoğaltabilirsiniz.

### <a name="when-should-i-enable-multi-vm-consistency-"></a>Çoklu VM tutarlılığı etkinleştirdiğinizde?
Çoklu VM tutarlılığını etkinleştirmek etkileyebilir iş yükü performansı (CPU kullanımı yoğun olduğu gibi) ve yalnızca makineler aynı iş yükünü çalıştıran ve birden fazla makine arasında tutarlılık ihtiyacınız varsa kullanılmalıdır. Örneğin, 2 sql sunucunuz varsa ve daha sonra uygulama 2 web sunucuları yalnızca sql sunucuları için "Çoklu VM" tutarlılık olması gerekir.


## <a name="failover"></a>Yük devretme

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?

Yük devretme işlemi otomatik değildir. Portaldaki tek tıklamayla yük devretmeleri başlatmak veya Site RECOVERY'yi kullanabilirsiniz [PowerShell](azure-to-azure-powershell.md) bir yük devretmeyi tetiklemek için. 

### <a name="can-i-retain-public-ip-address-after-failover"></a>Yük devretme sonrasında genel IP adresi tutabilir miyim?

Genel IP adresi bir üretim uygulamasının **yük devretmede tutulamıyor**. Yük devretme işleminin bir parçası bir hedef bölgede Azure genel IP kaynağı atanması gerektiğinden getirdiği iş yükleri. Bu adım ya da el ile yapılabilir veya kurtarma planları ile otomatik hale getirilmiştir. Başvuru [makale](https://docs.microsoft.com/azure/site-recovery/concepts-public-ip-address-with-site-recovery#public-ip-address-assignment-using-recovery-plan) kurtarma planı kullanarak genel IP adresi atamak için.  

### <a name="can-i-retain-private-ip-address-during-failover"></a>Özel IP adresi yük devretme sırasında tutabilir miyim?
Evet, özel IP adresi tutabilirsiniz. Azure Vm'leri için olağanüstü durum kurtarma etkinleştirdiğinizde, varsayılan olarak, Site Recovery hedef kaynaklar kaynak kaynak ayarları temel alarak oluşturur. Statik IP adresleriyle yapılandırılmış Azure Vm'leri için Site Recovery kullanımda değilse hedef sanal makine, aynı IP adresi sağlamak çalışır. Başvuru [makale](site-recovery-retain-ip-azure-vm-failover.md) farklı koşullar altında özel IP adresini korumak için.

### <a name="after-failover-the-server-does-not-have-the-same-ip-address-as-the-source-vm-why-is-it-assigned-with-a-new-ip-address"></a>Yük devretmeden sonra sunucunun VM kaynağı olarak aynı IP adresi yok. Neden yeni bir IP adresiyle atandıktan?

Site Recovery, IP adresi, yük devretme sırasında en iyi çaba ilkesine göre sağlamak çalışır. Sonraki kullanılabilir IP adresi, bazı başka bir sanal makine tarafından kullanıldığını durumda hedef olarak ayarlanır. Adresleme, Site Recovery nasıl işleyeceğini bir tam açıklama için [bu makaleyi gözden geçirin.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms)

### <a name="what-does-latestlowest-rpo-recovery-points-means"></a>En son (en düşük RPO) kurtarma yaptığı anlamına gelir işaret?
Bu seçenek ilk önce yük devretme için her VM için bir kurtarma noktası oluşturmak için Site Recovery hizmetine gönderilen tüm verileri işler. Yük devretme yük devretme tetiklendiğinde Site Recovery'ye çoğaltılan tüm verilere sahip sonra VM oluşturulduğundan, bu seçenek en düşük RPO (kurtarma noktası hedefi) sağlar.

### <a name="does-latest-lowest-rpo-recovery-points-have-impact-on-failover-rto"></a>(En düşük RPO) en son kurtarma noktaları, yük devretme RTO üzerinde etkisi var mı?
Site Recovery, yük devretme önce tüm bekleyen verileri işleyecek şekilde Evet, bu başkalarının karşılaştırıldığında daha yüksek bir RTO seçeneğiniz olur.

### <a name="what-does-latest-processed-option-in-recovery-points-mean"></a>Ne yaptığını en son kurtarma noktaları ortalama seçeneğinde işlenen?
Bu seçenek, Site Recovery tarafından işlenen en son kurtarma noktasına plandaki tüm VM'lerin yükünü başarısız olur. En son kurtarma noktası için belirli bir VM'ye görmek için en son kurtarma noktaları VM ayarlarını denetleyin. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.

### <a name="if-im-replicating-between-two-azure-regions-what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>İki Azure bölgeleri arasında çoğaltma yapıyorsam my birincil bölgeye beklenmeyen bir kesinti oluşursa ne olur?
Kesinti bir yük devretme tetikleyebilirsiniz. Site Recovery, yük devretme gerçekleştirmek için birincil bölgeden bağlantı gerek yoktur.

## <a name="recovery-plan"></a>Kurtarma planı

### <a name="what-is-a-recovery-plan-"></a>Bir kurtarma planı nedir?
Site Recovery kurtarma planlarında sanal makinelerinin yük devretme kurtarma düzenleyin. Bu, tutarlı bir şekilde doğru yinelenebilir ve Otomatik Kurtarma olmasına yardımcı olur. Bir kurtarma planı kullanıcı için aşağıdaki gereksinimleri ele alır:

- Bir grup sanal tanımlama, yük devretme birlikte makineleri.
- Uygulama doğru bir şekilde gelir, böylece sanal makineler arasındaki bağımlılıkları tanımlama.
- Böylece sanal makinelerin Yük Devretmesini dışındaki görevler de gerçekleştirilebilir özel el ile gerçekleştirilen eylemler ile birlikte kurtarma otomatikleştirme.

[Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md) kurtarma planları hakkında.

### <a name="how-does-sequencing-is-achieved-in-a-recovery-plan"></a>Nasıl sıralama, bir kurtarma planı'nda gerçekleştirilir mu?

Kurtarma planı'nda, sıralama elde etmek için çeşitli gruplar oluşturabilirsiniz. Her seferinde aynı grubun parçası olan Vm'leri anlamına gelir her grubu yük devretme birlikte başka bir grubu tarafından takip yük devri olur. Denetlemek nasıl [kurtarma planı kullanarak model uygulaması.](https://review.docs.microsoft.com/azure/site-recovery/recovery-plan-overview?branch=pr-en-us-61681#model-apps) 

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Bir kurtarma planı RTO nasıl bulabilirim?
Bir kurtarma planı RTO denetlemek için yük devretme kurtarma planı test etme ve Site Recovery işleri için gidin.
Örneğin aşağıda gösterildiği gibi SAP Test kurtarma planı 8 dakika 59 saniye, tüm sanal makineler için yük devretme sürdü ve belirtilen eylemleri gerçekleştirin.

  ![COM hatası](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Otomasyon runbook'ları kurtarma planı'na ekleyebilir miyim?
Evet, Azure Otomasyonu runbook'ları, kurtarma planına tümleştirebilirsiniz. [Daha fazla bilgi](site-recovery-runbook-automation.md)

## <a name="reprotect-and-failback"></a>Yeniden koruma ve yeniden çalışma 

### <a name="after-doing-a-failover-from-primary-region-to-disaster-recovery-region-does-vms-in-a-dr-region-gets-protected-automatically"></a>DR bölgesindeki VM'ler için olağanüstü durum kurtarma bölgesindeki birincil bölgeden yük devretme gerçekleştirmeden yaptıktan sonra otomatik olarak korunuyor?
Hayır, size [yük devretme](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) Azure Vm'leri bir bölgeden diğerine, VM'lerin önyükleme yukarı DR bölgesinde korumasız bir durumda. Vm'leri birincil bölgeye yeniden çalışma için şunları yapmanız [yeniden koruma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) ikincil bölgedeki Vm'leri.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-secondary-region-to-primary-region"></a>Yeniden koruma anında Site Recovery eksiksiz bir veri birincil bölgeye ikincil bölgeden çoğaltma mu?
Örneğin kaynak ve hedef disk arasında yalnızca değişiklikleri eşitlenir sonra VM'nin Kaynak bölgesi mevcut duruma bağlıdır. Differentials her iki diskte de karşılaştırılmasıyla hesaplanır ve daha sonra aktarılan. Bu genellikle işlemin tamamlanması birkaç saat. Başvuru [makale]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) fazla yeniden koruma sırasında neler olduğu hakkında bilgi edinmek için.

### <a name="how-much-time-does-it-take-to-failback"></a>Ne kadar zaman mevcut olması için yeniden çalışma?
Yeniden koruma bittikten sonra sonra genellikle, birincil bölgeden ikincil bölgeye yük devretme için benzer bir zaman miktarıdır. 

## <a name="security"></a>Güvenlik
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Çoğaltılan veriler Site Recovery hizmetine gönderilir mi?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  
Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, hem şifreleme-aktarım sırasında ve [azure'da şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
* [Gözden geçirme](azure-to-azure-support-matrix.md) gereksinimlerini destekler.
* [Ayarlanan](azure-to-azure-tutorial-enable-replication.md) Azure'dan Azure'a çoğaltma.
