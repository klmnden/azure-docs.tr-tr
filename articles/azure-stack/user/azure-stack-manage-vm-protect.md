---
title: Azure Stack üzerinde dağıtılan Vm'leri koruma | Microsoft Docs
description: Azure Stack üzerinde dağıtılan sanal makineleri koruma hakkında yönergeler.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: hector.linares
ms.openlocfilehash: 3c27aecf18fcb5e14347d8f02d71891b351292be
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341846"
---
# <a name="protect-virtual-machines-deployed-on-azure-stack"></a>Azure Stack üzerinde dağıtılan sanal makinelerini koruma

Bu makalede, kullanıcılarınız Azure Stack'te dağıtma, sanal makineler (VM) korumaya yönelik bir plan geliştirmek için bir kılavuz olarak kullanın.

Veri kaybı ve planlanmamış kesinti süreleri karşı korumak için kullanıcı uygulamaları ve verileri için bir yedekleme kurtarma veya olağanüstü durum kurtarma planınızı uygulamak gerekir. Bu plan, her uygulama için benzersiz olabilir ancak kuruluşunuzun kapsamlı iş sürekliliği ve olağanüstü durum kurtarma (DR/BC) stratejisi tarafından kurulan bir çerçeve izler. İyi bir başlangıç noktasıdır [Azure için dayanıklı uygulamalar tasarlama](https://docs.microsoft.com/azure/architecture/resiliency), sağlayan genel desenler ve uygulamalar için uygulama kullanılabilirliğini ve esnekliğini.

>[!IMPORTANT]
> Yedekleme kurtarma ve olağanüstü durum kurtarma planlarınızı düzenli olarak test edin. Bu emin olmak için gerekir:
> * Planlar çalışır
> * Planlar hala için tasarlanmış karşılar.

## <a name="azure-stack-infrastructure-recovery"></a>Azure Stack altyapısını kurtarma

Kullanıcıların kendi sanal makineleri ayrı ayrı Azure yığını'nın altyapı Hizmetleri'nden korumaktan siz sorumlusunuz.

Kurtarma planına Azure Stack altyapı hizmetleri için **yok** kullanıcı Vm'lerinin, depolama hesapları veya veritabanları kurtarma sayılabilir. Uygulama sahibi olarak, uygulamalarınız ve verileriniz için bir kurtarma planı uygulamak için sorumlu olursunuz.

Azure Stack Bulutu uzun bir süre çevrimdışı veya kalıcı olarak kurtarılamayan bir kurtarma planı içinde ihtiyacınız varsa, yerleştirin:

* En düşük kapalı kalma süresi sağlar
* Veritabanı sunucuları gibi kritik VM'lerin tutar
* Kullanıcı isteklerine hizmet tutmak uygulamaları etkinleştirir

Azure Stack bulut operatörü, temel alınan Azure Stack altyapı ve Hizmetleri için bir kurtarma planı oluşturmak için sorumludur. Daha fazla bilgi için makaleyi okuyun [geri dönülemez veri kaybından kurtarma](https://docs.microsoft.com/azure/azure-stack/azure-stack-backup-recover-data).

## <a name="sourcetarget-combinations"></a>Kaynak/hedef birleşimleri

Her Azure Stack Bulutu, bir veri merkezine dağıtılır. Ayrı bir ortam gerekli olduğundan, uygulamalarınızın kurtarabilirsiniz. Kurtarma Ortamı'nı, başka bir Azure Stack Bulutu farklı bir veri merkezinde veya Azure genel bulutunda olabilir. Kurtarma Ortamı'uygulamanız için veri gizlilik gereksinimleri ve veri egemenliği belirler. Her uygulama için korumayı etkinleştirin gibi her biri için belirli bir kurtarma seçeneği esnekliğine sahip olursunuz. Uygulamalar, başka bir veri merkezi için veri yedekleyen bir abonelikte olabilir. Başka bir abonelikte, Azure ortak bulutuna veri çoğaltma yapabilirsiniz.

Her uygulama için hedef belirlemek her bir uygulama için yedekleme kurtarma ve olağanüstü durum kurtarma stratejinizi planlayın. Bir kurtarma planı, kuruluşunuzun düzgün kapasite gerekli şirket içi depolama boyutu ve genel bulut tüketimi proje yardımcı olur.

|  | Genel Azure | CSP veri merkezi içinde dağıtılır ve CSP tarafından işletilen azure Stack | Müşteri veri merkezine dağıtılan ve müşteri tarafından işletilen azure Stack |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **CSP veri merkezi içinde dağıtılır ve CSP tarafından işletilen azure Stack** | Kullanıcı VM'ler için CSP tarafından işletilen Azure Stack dağıtılır. Kullanıcı Vm'lerinin yedeklemeden geri veya doğrudan Azure'a yük devretti. | CSP'de Azure Stack birincil ve ikincil örneklerini, kendi veri merkezlerinde çalışır. Kullanıcı Vm'leri geri veya iki Azure Stack örnekleri arasında yük devretti. | CSP, birincil sitenin Azure Stack'te çalışır. Müşterinin veri merkezi geri yükleme ya da yük devretme hedefidir. |
| **Müşteri veri merkezine dağıtılan ve müşteri tarafından işletilen azure Stack** | Kullanıcı VM'ler dağıtıldığı Azure Stack müşteri işletilir. Kullanıcı Vm'lerinin yedeklemeden geri veya doğrudan Azure'a yük devretti. | Müşteri, Azure Stack birincil ve ikincil örneklerini, kendi veri merkezlerinde çalışır. Kullanıcı Vm'leri geri veya iki Azure Stack örnekleri arasında yük devretti. | Müşteri, Azure Stack birincil sitede çalışır. CSP'ın datacenter geri yükleme ya da yük devretme hedefidir. |

![Kaynak-hedef birleşimleri](media\azure-stack-manage-vm-backup\vm_backupdataflow_01.png)

## <a name="application-recovery-objectives"></a>Uygulama Kurtarma hedefleri

Her uygulama için kuruluşunuzun tolere edebilen kapalı kalma süresi ve veri kaybı miktarını belirlemeniz gerekir. Kapalı kalma süresi ve veri kaybı niceleme tarafından kuruluşunuzdaki bir olağanüstü durum etkisini en aza indiren bir kurtarma planı oluşturabilirsiniz. Her uygulama için göz önünde bulundurun:

 - **Kurtarma süresi hedefi (RTO)**  
RTO, bir olay sonrasında uygulamanın kullanılamıyor olabilir kabul edilebilen maksimum süre ' dir. Örneğin, 90 dakikada bir RTO uygulamasının çalışır duruma 90 dakika içinde başından itibaren bir olağanüstü durum geri yükleme olanağınız olmalıdır anlamına gelir. Düşük bir RTO varsa, bölgesel bir kesintiye karşı korumak için bekleme sürekli çalıştıran ikinci bir dağıtım tutabilirsiniz.
 - **Kurtarma noktası hedefi (RPO)**  
RPO, olağanüstü bir durum sırasında kabul edilebilir veri kaybı maksimum süresidir. Örneğin, verileri başka bir veritabanına çoğaltma yapmadan tek bir veritabanında depoluyor ve saatlik yedeklemeler gerçekleştiriyorsanız bir saate kadar varan bir süreye ait verileri kaybedebilirsiniz.

RTO ve RPO iş gereksinimleridir. Uygulamanın RTO ve RPO tanımlamak için bir risk değerlendirmesi gerçekleştirin.

Başka bir ölçüm olan **ortalama süresi, kurtarılır** (MTTR) olan bir hatadan sonra uygulamanın geri yükleme için geçen ortalama süre. MTTR, bir sistem için Deneysel bir değerdir. MTTR, RTO’yu aşıyorsa bir hata halinde sistemin tanımlanmış RTO içerisinde geri yüklenmesi mümkün olmayacağından kabul edilemez iş kesintileri ortaya çıkar.

### <a name="backup-restore"></a>Yedekleme geri yükleme

Sanal makine tabanlı uygulamalar için en yaygın koruma şeması, yedekleme yazılımı kullanmaktır. Genellikle bir VM'yi yedekleme, işletim sistemi, işletim sistemi yapılandırması, uygulama ikili dosyalarını ve uygulama verilerini içerir. Yedeklemeler, birimlerin, diskler veya tüm VM anlık görüntüsünü alarak oluşturulur. Azure Stack ile gelen konuk işletim sistemi veya Azure Stack depolama bağlam içinde yedekleme esnekliğine sahip olursunuz ve API işlem. Azure Stack, hiper yönetici düzeyinde alma yedeklemeleri desteklemez.
 
![Yedekleme geri](media\azure-stack-manage-vm-backup\vm_backupdataflow_03.png)

Uygulama Kurtarma aynı buluta veya yeni bir bulut bir veya daha fazla sanal makine geri yükleme gerektirir. Veri merkezinizde veya genel bulut Bulutu hedefleyebilirsiniz. Seçtiğiniz bulut tamamen denetiminizin içinde olduğundan ve veri gizliliği ve özerkliği gereksinimlerinize göre alır.
 
 - RTO: Kapalı kalma süresini saat cinsinden.
 - RPO: Değişken veri kaybına (bağlı olarak yedekleme sıklığı)
 - Dağıtım topolojisi: Aktif/Pasif

#### <a name="planning-your-backup-strategy"></a>Yedekleme stratejisini planlama

Yedekleme stratejisini planlama ve ölçek gereksinimleri tanımlama, korunması gereken sanal makine örneği sayısını niceleme ile başlar. Azure'daki tüm sunucularda bir ortamdaki tüm sanal makineleri yedeklemeye genel bir stratejidir. Ancak, Azure Stack ile yedeklenmesi gereken bazı VM'ler vardır. Örneğin, bir ölçek kümesindeki sanal makineler, gelir ve bazen bir bildirim olmadan Git kısa ömürlü kaynaklar olarak kabul edilir. Korunması gereken dayanıklı bir veri bir veritabanı veya nesne deposu gibi ayrı bir deposunda depolanır.

Azure Stack üzerinde sanal makineleri yedeklemeye önemli noktalar:

 - **Kategorilere ayırma**
    - Burada kullanıcıları VM yedeklemesi için katılım modeli göz önünde bulundurun.
    - Uygulamaları veya iş etkisi önceliğini temel bir kurtarma hizmet düzeyi sözleşmesi (SLA) tanımlayın.
 - **Ölçeklendirme**
    - Düzenlenmiş yedeklemeleri çok sayıda yeni VM'ler (Yedekleme gerekliyse) kolaylaşmasına yaparken göz önünde bulundurun.
    - Kaynak içeriği çözümdeki en aza indirmek için yedekleme verileri verimli bir şekilde yakalayabilir ve yedekleme ürünleri değerlendirin.
    - Verimli bir şekilde ortamdaki tüm sanal makineler arasında tam yedeklemeler gereksinimini en aza indirmek için artımlı ya da fark yedeklemeler kullanarak yedekleme verilerini depolamak yedekleme ürünleri değerlendirin.
 - **Geri yükleme**
    - Yedekleme ürünleri, sanal diskler, varolan bir VM'yi veya tüm VM kaynak uygulama verilerini ve ilişkili sanal diskler geri yükleyebilirsiniz. Uygulamayı geri planınızı nasıl üzerinde ihtiyacınız geri yükleme düzeni bağlıdır ve kurtarma için uygulama sürenizi etkiler. Örneğin, SQL Server'dan bir şablonu yeniden dağıtın ve ardından VM kümesi ve tüm VM'yi geri yüklemek yerine veritabanlarını geri yüklemek daha kolay olabilir.

### <a name="replicationmanual-failover"></a>Çoğaltma/el ile yük devretme

Yüksek kullanılabilirliği desteklemek için alternatif bir yaklaşım, başka bir bulut uygulama Vm'lerinizi çoğaltın ve el ile yük devretme sırasında kullanan sağlamaktır. İşletim sistemi, uygulama ikili dosyalarını ve uygulama veri çoğaltma, VM düzeyinde veya konuk işletim sistemi düzeyinde gerçekleştirilebilir. Yük devretme, uygulamanın bir parçası olmayan ek yazılımları kullanılarak yönetilir.

Bu yaklaşım uygulamanın bir buluta dağıtılması ve kendi VM diğer buluta çoğaltılır. Bir yük devretme tetikleniyorsa, ikincil VM'ler ikinci buluta açık gerekir. Bazı senaryolarda yük devretme Vm'leri ve ekler diskleri bunlara oluşturur. Bu işlem, belirli bir başlangıç dizisi gerektiren özellikle bir katmanlı uygulamayla tamamlanması uzun zaman alabilir. Uygulama istekleri hizmet vermeye başlaması hazır olmadan önce çalıştırılması gereken adımları da olabilir.

![Çoğaltmayı elle yük devretme](media\azure-stack-manage-vm-backup\vm_backupdataflow_02.png)

 - RTO: Kapalı kalma süresini dakika cinsinden ölçülür.
 - RPO: Değişken veri kaybına (bağlı olarak çoğaltma sıklığı)
 - Dağıtım topolojisi: Aktif/Pasif beklemede
 
### <a name="high-availabilityautomatic-failover"></a>Yüksek kullanılabilirlik/otomatik yük devretme

Birkaç saniye veya dakika kapalı kalma süresini minimum düzeyde veri kaybı ve işletmeniz nerede tolere edebilen uygulamalar için yüksek kullanılabilirlik yapılandırmasında göz önünde bulundurmanız gerekir. Yüksek kullanılabilirlik uygulamaları hızla ve otomatik olarak hataları kurtarmak için tasarlanmıştır. Yerel donanım hatalarına için Azure Stack altyapısının fiziksel ağ raf üstü anahtar iki üst kullanarak yüksek kullanılabilirlik uygular. İşlem düzeyi hataları için Azure Stack bir ölçek birimi birden çok düğüm kullanır. VM düzeyinde düğüm hatalarına uygulamanızın almayan emin olmak için hata etki alanları ile ölçek kümeleri ile birlikte kullanabilirsiniz.

Ölçek kümeleri ile birlikte, yüksek kullanılabilirlik yerel olarak destekleyen veya kümeleme yazılımı kullanımını desteklemek uygulamanız gerekir. Örneğin, Microsoft SQL Server yüksek kullanılabilirlik veritabanları için zaman uyumlu tamamlama modunu kullanarak yerel olarak destekler. Ancak, yalnızca zaman uyumsuz çoğaltma destekleyebiliyorsa, ardından olacaktır bazı veriler kaybolabilir. Uygulamalar ayrıca burada uygulamasının otomatik yük devretme kümeleme yazılımı işleyen bir yük devretme kümesine dağıtılabilir.

Bu yaklaşımı kullanarak, uygulama yalnızca tek bir bulut etkindir, ancak yazılım için birden çok bulut dağıtılır. Diğer Bulutlar gelebilmek modda yük devretme tetiklendiğinde uygulamayı başlatmak hazır olur.

 - RTO: Kapalı kalma süresini saniye cinsinden ölçülür.
 - RPO: En düşük düzeyde veri kaybı
 - Dağıtım topolojisi: etkin/etkin bekleme

### <a name="fault-tolerance"></a>Hataya dayanıklılık

Azure Stack fiziksel yedeklilik ve altyapı hizmet kullanılabilirliği, yalnızca bir donanıma karşı korumak, böyle bir disk, güç, ağ bağlantı noktası veya düğüm hataları/hataları düzeyi. Ancak, uygulamanız her zaman kullanılabilir olması gerekir ve hiçbir zaman herhangi bir veri kaybı olabilir, hata toleransı yerel olarak uygulamanıza veya hataya dayanıklılığı etkinleştirmek için ek yazılım gerekir.

İlk olarak, VM ölçek kullanılarak dağıtılan uygulama düzey düğüm hatalarına karşı korumak için Ayarlar'ı emin olmanız gerekir. Böylece kesinti olmadan isteklere hizmet devam edebilirsiniz çevrimdışı duruma geçiyor bulut karşı korumak için aynı uygulama zaten farklı bir buluta dağıtılması gerekir. Bu model, genellikle bir aktif-aktif dağıtımına adlandırılır.

Bulut her zaman etkin bir altyapı açısından değerlendirilir, böylece her Azure Stack bulut birbirinden bağımsız olduğunu aklınızda bulundurun. Bu durumda, uygulamanın birden çok etkin örnekler için etkin bir veya daha fazla bulut dağıtılır.

 - RTO: Kapalı kalma süresi
 - RPO: Veri kaybı olmadan
 - Dağıtım topolojisi: etkin/etkin

### <a name="no-recovery"></a>Hiçbir kurtarma

Bazı uygulamalar, ortamınızdaki planlanmamış kesinti veya veri kaybına karşı koruma gerekmeyebilir. Örneğin, VM'ler, geliştirme için kullanılan ve test genellikle gerekmez kurtarılması. Bu, bir uygulama ya da belirli bir sanal makine için koruma olmadan yapmak için kararınız olur. Azure Stack yedekleme veya çoğaltma sanal makinelerinin temel altyapısından sunmaz. Benzer şekilde, Azure, her bir aboneliğinizde her VM için koruma katılımı yapmanız gerekir.

 - RTO: kurtarılamaz
 - RPO: Tam veri kaybının

## <a name="recommended-topologies"></a>Önerilen topolojileri

Azure Stack dağıtımınız için önemli noktalar:

|     | Öneri | Yorumlar |
|-------------------------------------------------------------------------------------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme/VM'ler veri merkezinizde zaten dağıtılmış bir dış yedekleme hedefine geri yükleme | Önerilen | Mevcut yedekleme altyapısı ve operasyonel becerilerinizi avantajlarından yararlanın. Ek sanal makine örnekleri korumak hazır olması için Yedekleme Altyapısı boyut emin olun. Yedekleme Altyapısı kaynağınızı yakın olmadığından emin olun. ' % S'kaynağına Azure yığını, ikincil bir Azure Stack örneği veya Azure Vm'leri geri yükleyebilirsiniz. |
| Yedekleme/Vm'leri Azure Stack için adanmış bir dış yedekleme hedefine geri yükleme | Önerilen | Azure Stack için yeni bir yedekleme altyapısı veya sağlama adanmış yedekleme altyapısı satın alabilirsiniz. Yedekleme Altyapısı kaynağınızı yakın olmadığından emin olun. ' % S'kaynağına Azure yığını, ikincil bir Azure Stack örneği veya Azure Vm'leri geri yükleyebilirsiniz. |
| Vm'lere genel Azure ya da güvenilir bir hizmet sağlayıcısına doğrudan yedekleme/geri yükleme | Önerilen | Veri gizliliği ve Mevzuat gereklilikleri karşılamak sürece genel Azure veya bir güvenilir hizmet sağlayıcısı, yedeklemelerinizi depolayabilirsiniz. İdeal olarak, geri yüklediğinizde işletimsel deneyimi tutarlılık elde hizmet sağlayıcısı ayrıca Azure Stack çalışıyor. |
| Çoğaltma/yük devretme VM'ler için ayrı bir Azure Stack örneği | Önerilen | Yük devretme durumunda, genişletilmiş uygulamaların kapalı kalma süresini önlemek ikinci bir Azure Stack bulut tam olarak işlevsel olması gerekir. |
| Çoğaltma/yük devretme sanal makineleri doğrudan Azure'da veya bir güvenilir hizmet sağlayıcısı | Önerilen | Veri gizliliği ve Mevzuat gereklilikleri karşılamak sürece genel Azure veya bir güvenilir hizmet sağlayıcısı verilerinizi çoğaltabilirsiniz. İdeal olarak yük devretme sonrasında işletimsel deneyimi tutarlılık elde hizmet sağlayıcısı ayrıca Azure Stack çalışıyor. |
| Yedekleme hedefi ile uygulama verilerinizi aynı Azure Stack Bulutu dağıtma | Önerilmez | Aynı Azure Stack Bulutu içinde yedeklemelerini depolamak kaçının. Bulutun Planlanmamış kapalı kalma süresi, birincil veri ve yedekleme verileri tutabilirsiniz. Yedekleme hedefi olarak bir sanal gereç (yedekleme ve geri yükleme için iyileştirme amacıyla) dağıtmayı seçerseniz, tüm verileri sürekli olarak bir dış yedekleme konumuna kopyalanır emin olmanız gerekir. |
| Azure Stack çözüm yüklendiği aynı raf fiziksel yedekleme uygulamasına dağıtma | Desteklenmiyor | Bu anda, herhangi bir cihaza özgün çözümün bir parçası olmayan raf üstü anahtarları üstüne bağlanamıyor. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Stack üzerinde dağıtılan kullanıcı Vm'leri korumak için genel yönergeler sağlanır. Kullanıcı Vm'leri korumak için Azure hizmetlerini kullanma hakkında daha fazla bilgi için bkz:

 - [Azure Stack'te dosyalarını ve uygulamaları yedeklemek için Azure Backup'ı kullanın](https://docs.microsoft.com/azure/backup/backup-mabs-files-applications-azure-stack)
 - [Azure Stack için Azure Backup sunucusu desteği](https://docs.microsoft.com/azure/backup/ ) 
 - [Azure Stack için Azure Site Recovery desteği](https://docs.microsoft.com/azure/site-recovery/)  

VM koruma Azure Stack'te teklif iş ortağı ürünleri hakkında daha fazla bilgi için bkz "[uygulamaları ve verileri Azure Stack'te koruma](https://azure.microsoft.com/blog/protecting-applications-and-data-on-azure-stack/)."
