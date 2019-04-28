---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 42b6dde708e2a1dbda225fd95e3db964267ae48a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60613765"
---
## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>VM Yeniden Başlatma İşlemlerini Anlama - bakım ve kapalı kalma süresi
Sanal makine azure'da etkilenmesine neden olabilecek üç senaryo vardır: plansız Donanım bakımı, beklenmeyen kapalı kalma süresi ve planlı Bakım.

* **Plansız Donanım Bakımı Olayı**, Azure platformu donanımın veya fiziksel makineyle ilişkili herhangi bir platform bileşeninin arıza yapmak üzere olduğunu tahmin ettiğinde gerçekleşir. Platform bir arıza öngördüğünde, donanımda barındırılan sanal makineler üzerindeki etkiyi azaltmak amacıyla plansız donanım bakımı olayı düzenler. Azure, arızalı donanımdaki Sanal Makineleri sağlıklı bir fiziksel makineye geçirmek için Dinamik Geçiş teknolojisini kullanır. Dinamik Geçiş, Sanal Makineyi yalnızca kısa bir süre için duraklatan bir VM koruma işlemidir. Bellek, açık dosyalar ve ağ bağlantıları korunur, ancak olaydan önce ve/veya sonra performans azalabilir. Dinamik Geçişin kullanılamadığı durumlarda VM, aşağıda açıklanan Beklenmeyen Kapalı Kalma Süresi yaşar.


* **Beklenmeyen kapalı kalma süresi** donanım ya da fiziksel altyapı sanal makinesi için beklenmedik şekilde başarısız olduğunda. Bu, yerel ağ hatalarında, yerel disk hataları veya raf düzeyinde diğer hatalar içerebilir. Azure platformu algılandığında, otomatik olarak geçirir (heals) aynı veri merkezinde sağlıklı bir fiziksel makineye sanal makinenize. İyileştirme yordamı sırasında sanal makineler kapalı kalır (yeniden başlatma) ve bazı durumlarda geçici sürücü kaybı yaşar. Bağlı işletim sistemi ve veri diskleri her zaman korunur. 

  Sanal makineler yaşandığı nadir bir veri merkezinin tamamı veya tüm bir bölgeyi etkileyen bir kesinti veya kapalı kalma süresi de oluşabilir. Bu senaryolar için Azure dahil olmak üzere koruma seçeneklerini sunar. [kullanılabilirlik](../articles/availability-zones/az-overview.md) ve [eşleştirilmiş bölgeleri](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

* **Planlı Bakım olayları**, Microsoft tarafından sanal makinelerinizin çalıştığı platforma ait genel güvenilirlik, performans ve güvenliği artırmak amacıyla temel alınan Azure platformunda yapılan periyodik güncelleştirmelerdir. Bu güncelleştirmelerin çoğu Sanal Makine veya Bulut Hizmetlerinizi etkilemeden gerçekleştirilir (bkz. [VM Koruyucu Bakım](https://docs.microsoft.com/azure/virtual-machines/windows/preserving-maintenance)). Azure platformu mümkün olan tüm durumlarda VM Koruyucu Bakımı kullanmaya çalışsa da, gerekli güncelleştirmelerin temel alınan altyapıya uygulanması için bu güncelleştirmelerin sanal makineyi yeniden başlatmayı gerektirdiği nadir örnekler vardır. Bu durumda, uygun zaman penceresi içinde VM’lere yönelik bakımı başlatarak Maintenance-Redeploy işlemi ile Azure Planlı Bakımını gerçekleştirebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler için Planlı Bakım](https://docs.microsoft.com/azure/virtual-machines/windows/planned-maintenance/).


Bu olayların bir veya daha fazlası nedeniyle kapalı kalma süresinin etkisini azaltmak için, sanal makinelerinizde aşağıdaki yüksek kullanılabilirlik en iyi uygulamalarının kullanılması önerilir:

* [Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]
* [Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma]
* [Proaktif olarak yanıt etkileyen olayları VM için zamanlanmış olayları kullanın](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events)
* [Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]
* [Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]
* [Düzey veri merkezi arızasına karşı korumak için kullanılabilirlik alanlarını kullanma]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma
Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Bu yapılandırma bir veri merkezinde ya da bir planlı veya Plansız bakım olayı sırasında en az bir sanal makine kullanılabilir olduğundan ve % 99,95 oranında karşıladığından emin sağlar Azure SLA'sı. Daha fazla bilgi için bkz. [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Tek örnekli bir sanal makineyi bir kullanılabilirlik kümesinde tek başına bırakmaktan kaçının. Bu yapılandırmadaki sanal makineler değil bir SLA garantisine ve tek bir VM kullanırken dışında Azure planlı bakım olayları sırasında kapalı kalma süresi yüz [Azure premium SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd). Premium SSD kullanma tek VM'ler için Azure SLA'sı geçerlidir.

Kullanılabilirlik kümenizdeki her sanal makineye, temel alınan Azure platformu tarafından bir **güncelleme etki alanı** ve bir **hata etki alanı** atanır. Belirli bir kullanılabilirlik kümesi için, aynı anda yeniden başlatılabilecek sanal makine gruplarını ve temel alınan fiziksel donanımları göstermek üzere, kullanıcı tarafından yapılandırılabilen beş güncelleme etki alanı varsayılan olarak atanır (Resource Manager dağıtımları daha sonra en fazla 20 güncelleme etki alanı sağlayacak şekilde artırılabilir). Tek bir kullanılabilirlik kümesinde beşten fazla sanal makine yapılandırıldığında, altıncı sanal makine birinci sanal makine ile, yedinci sanal makine ikinci sanal makine ile aynı güncelleme etki alanına yerleştirilir ve yerleştirme işlemi bu düzende devam eder. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır. Yeniden başlatılmış bir güncelleme etki alanının kurtarılması için, farklı bir güncelleme etki alanında bakım başlatılmadan önce 30 dakika beklenir.

Hata etki alanları ortak bir güç kaynağı ve ağ anahtarını paylaşan sanal makine grubunu tanımlar. Varsayılan olarak, kullanılabilirlik kümenizde yapılandırılmış olan sanal makineler, Resource Manager dağıtımları için en fazla üç hata etki alanı (Klasik için iki etki alanı) arasında ayrılır. Sanal makinelerinizin bir kullanılabilirlik kümesine yerleştirilmesi uygulamanızı işletim sistemine veya uygulamaya özel hatalardan korumasa da, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkilerini sınırlar.

<!--Image reference-->
   ![Güncelleştirme etki alanı ve hata etki alanı yapılandırmasının kavramsal çizimi](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma
Şu anda yönetilmeyen disklere sahip VM’ler kullanıyorsanız, [Kullanılabilirlik Kümesindeki VM’leri Yönetilen Diskleri kullanacak şekilde dönüştürmeniz](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md) önemle tavsiye edilir.

[Yönetilen diskler](../articles/virtual-machines/windows/managed-disks-overview.md), bir Kullanılabilirlik Kümesindeki VM disklerinin tek arıza noktalarından kaçınmak üzere birbirinden yeterince ayrılmasını sağlayarak Kullanılabilirlik Kümeleri için daha fazla güvenilirlik sunar. Bunu VM hata etki alanı ile hizalayabilirsiniz ve diskleri farklı depolama hata etki alanları (depolama kümeleri) otomatik olarak yerleştirerek yapar. Depolama hata etki alanı, donanım veya yazılım arızasından dolayı başarısız olursa, yalnızca depolama hata etki alanı disklerle sanal makine örneği başarısız olur.
![Yönetilen diskler FD](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

> [!IMPORTANT]
> Yönetilen kullanılabilirlik kümelerine yönelik arıza etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki veya üç). Aşağıdaki tabloda bölge başına sayı gösterilmektedir

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

Yönetilmeyen diskleri olan VM'ler kullanmayı planlıyorsanız, aşağıdaki olarak sanal sabit diskleri (VHD'ler) sanal makinelerinin depolandığı depolama hesapları için en iyi uygulamayı izleyin [sayfa blobları](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs).

1. **Bir VM ile ilişkili tüm diskleri (işletim sistemi ve veri) aynı depolama hesabında tutma**
2. Bir depolama hesabına daha fazla VHD eklemeden önce **Depolama hesabındaki yönetilmeyen disk sayısına ilişkin [limitleri](../articles/storage/common/storage-scalability-targets.md) gözden geçirin**
3. **Bir Kullanılabilirlik Kümesindeki her VM için ayrı depolama hesabı kullanın.** Depolama hesaplarını aynı Kullanılabilirlik Kümesinde birden fazla VM ile paylaşmayın. Farklı kullanılabilirlik yukarıdaki en iyi deneyimler uygulanırsa, depolama hesaplarını paylaşabilir kümelerindeki vm'le ![yönetilmeyen diskler FD](./media/virtual-machines-common-manage-availability/umd-updated.png)

## <a name="use-scheduled-events-to-proactively-respond-to-vm-impacting-events"></a>VM etkileyen olayları proaktif olarak yanıt vermek için zamanlanmış olayları kullanın

Abone olduğunuzda [zamanlanmış olaylar](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-scheduled-events), sanal Makinenizin etkileyebilir yaklaşan bakımın olaylar hakkında bildirilir. Zamanlanmış olaylar etkinleştirildiğinde, sanal makinenizin bakım etkinliği gerçekleştiren önce geçmesi gereken süreyi en düşük düzeyde verilir. Örneğin, sanal makinenizin etkileyebilecek ana bilgisayar işletim sistemi güncelleştirmeleri etkisi belirten olayları, olarak hiçbir işlem yapılmadı, bakım gerçekleştirilir birer sıraya alınır. Azure iyileştirme ne zaman yapılması gereken karar vermenize olanak tanır, VM etkileyebilecek uyaran bir donanım hatası algıladığında zamanlama olayları da eklenirler. Müşteriler, olay durumu kaydetmeyi gibi önce bakım görevleri gerçekleştirmek için ikincil veritabanına yük devretmeyi kullanın ve benzeri. Düzgün bir şekilde bir bakım olayı işlemek için mantığınızı tamamladıktan sonra Bakım ve devam etmek platform izin vermek için bekleyen zamanlanmış olay onaylayabilirsiniz.

## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma
Sanal makinelerinizin neredeyse tümü aynıysa ve uygulamanız için aynı amaca hizmet ediyorsa, uygulamanızın her katmanı için bir kullanılabilirlik kümesi yapılandırmanız önerilir.  Aynı kullanılabilirlik kümesine iki farklı katman yerleştirirseniz, aynı uygulama katmanındaki tüm sanal makineler tek seferde yeniden başlatılabilir. Her katman için bir kullanılabilirlik kümesinde en az iki sanal makineyi yapılandırarak, her katmanda en az bir sanal makinenin kullanılabilir olmasını garanti edersiniz.

Örneğin, tüm sanal makineleri tek bir kullanılabilirlik kümesinde IIS, Apache, Nginx çalıştıran uygulamanızın ön ucuna yerleştirebilirsiniz. Yalnızca ön uç sanal makinelerin aynı kullanılabilirlik kümesine yerleştirildiğinden emin olun. Benzer şekilde, çoğaltılmış SQL Server sanal makineleriniz ya da MySQL sanal makineleriniz gibi yalnızca veri katmanı sanal makinelerinin kendi kullanılabilirlik kümelerine yerleştirildiğinden emin olun.

<!--Image reference-->
   ![Uygulama katmanları](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Yük dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme
En fazla uygulama dayanıklılığını elde etmek için [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md)’ı bir kullanılabilirlik kümesiyle birleştirin. Azure Load Balancer, birden fazla sanal makine arasında trafiği dağıtır. Standart katman sanal makinelerimize Azure Load Balancer dahildir. Tüm sanal makine katmanları Azure Load Balancer hizmetini içermez. Sanal makinelerinizde yük dengeleme hakkında daha fazla bilgi için bkz. [Sanal makinelerde yük dengeleme](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Yük dengeleyici birden fazla sanal makine arasında trafiği dengeleyecek şekilde yapılandırılmamışsa, planlı bakım olayları yalnızca trafik sunan sanal makineyi etkiler ve uygulama katmanınızda kesintiye neden olur. Aynı yük dengeleyici ve kullanılabilirlik kümesi altına aynı katmandaki birden fazla sanal makinenin yerleştirilmesi, trafiğin en az bir örnek tarafından sürekli olarak sunulmasını sağlar.

## <a name="use-availability-zones-to-protect-from-datacenter-level-failures"></a>Düzey veri merkezi arızasına karşı korumak için kullanılabilirlik alanlarını kullanma

[Kullanılabilirlik alanları](../articles/availability-zones/az-overview.md)kullanılabilirlik alternatif ayarlar, uygulamaları ve verileri sanal makinelerinizin kullanılabilirliğini sürdürmek için sahip olduğunuz denetimi düzeyini genişletin. Kullanılabilirlik Alanı, bir Azure bölgesinin içinde fiziksel olarak ayrılmış bir alandır. Üç kullanılabilirlik alanı desteklenen bir Azure bölgesi başına vardır. Her kullanılabilirlik alanı farklı bir sahip güç kaynağı, ağ ve soğutma ve diğer kullanılabilirlik alanlarından Azure bölgesi içinde mantıksal olarak ayrıdır. Çoğaltılan VM'ler bölgelerinde kullanılacak çözümlerinizi tasarlayarak, uygulamalarınızın ve verilerinizin bir veri merkezi kaybına karşı koruyabilirsiniz. Bir bölge tehlikedeyse, ardından çoğaltılan uygulamaları ve verileri başka bir bölgede anında kullanılabilir. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Dağıtma hakkında daha fazla bilgi bir [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) bir kullanılabilirlik alanında VM.


<!-- Link references -->
[Bir kullanılabilirlik kümesindeki birden fazla sanal makineyi yedeklilik için yapılandırma]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Her uygulama katmanını ayrı kullanılabilirlik kümeleri halinde yapılandırma]: #configure-each-application-tier-into-separate-availability-sets
[Yük Dengeleyiciyi kullanılabilirlik kümeleri ile birleştirme]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Bir kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma]: #use-managed-disks-for-vms-in-an-availability-set
[Düzey veri merkezi arızasına karşı korumak için kullanılabilirlik alanlarını kullanma]: #use-availability-zones-to-protect-from-datacenter-level-failures
