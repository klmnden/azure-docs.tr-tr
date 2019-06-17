---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/04/2019
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 46ade0ecb0e2e081585803a0b1bc7eab989e21e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735207"
---
**En son Güncelleştirmesi'ni belge**: 4 Haziran 2019 15:00 PST.

Açıklanması bir [CPU güvenlik açıklarından yeni sınıf](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) kurgusal yürütme yan kanal saldırıları olarak bilinen daha fazla netlik dağıtımınızla müşterilerden gelen soruları sonuçlandı.  

Microsoft, tüm bulut hizmetlerimizin arasında azaltmaları dağıtmıştır. Azure çalışan ve müşteri iş yüklerinin birbirinden yalıtan altyapı korunur. Bu altyapıyı kullanarak olası bir saldırganın bu güvenlik açıklarına kullanarak uygulamanızı saldırı olamaz anlamına gelir.

Azure kullanarak [Bakımı koruma bellek](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#maintenance-not-requiring-a-reboot) mümkün olduğunda, müşteri etkiyi en aza indirmek ve yeniden başlatma gereksinimini ortadan kaldırmak. Azure ana bilgisayara yükleyebilecek güncelleştirmeler yaparken bu yöntemleri yararlanmaya devam ve müşterilerimizi korumak.

Güvenlik her yönüyle Azure ile nasıl tümleştirildiği hakkında daha fazla bilgi edinilebilir [Azure güvenlik belgeleri](https://docs.microsoft.com/azure/security/) site. 

> [!NOTE] 
> Bu belge ilk kez yayınlandığı bu güvenlik açığı sınıfın birden çok çeşitleri duyurulmuştur. Microsoft, müşterilerimizin koruma ve rehberlik sağlama yatırım yoğun devam eder. Yayın başka düzeltmeleri devam ederken bu sayfa güncelleştirilir. 
> 
> 14 Mayıs 2019 tarihinde [duyurulmuş Intel](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00233.html) kurgusal yürütme yan kanal güvenlik açığı mikro mimari veri örnekleme bilinen yeni bir dizi (MDS bkz. Microsoft Güvenlik rehberi [ADV190013](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV190013)), birden çok CVEs atandı: 
> - CVE-2019-11091 - mikro mimari (MDSUM) Uncacheable bellek örnekleme verileri
> - CVE-2018-12126 - mikro mimari Store verilerini arabelleğe (MSBDS) örnekleme 
> - CVE-2018-12127 - mikro mimari yük bağlantı noktası verileri (MLPDS) örnekleme
> - CVE-2018-12130 - mikro mimari dolgu verilerini arabelleğe (MFBDS) örnekleme
>
> Bu güvenlik açığını Intel Core® işlemcileri ve Intel® Xeon® İşlemci etkiler.  Microsoft Azure, Intel tarafından müşterilerimizi bu yeni güvenlik açıklarına karşı korumak için sunduğumuz Filo boyunca kullanılabilir olarak yeni mikro kod dağıtıyor ve işletim sistemi güncelleştirmeleri yayımladı.   Azure, test ve yeni mikro kodun platformunda, resmi sürümden önce doğrulamak için Intel ile yakından çalışmaktadır. 
>
> **Güvenilmeyen çalıştıran müşteriler, VM içinde kod** tüm kurgusal yürütme yan kanal güvenlik açıklarına (Microsoft önerileri ADV hakkında ek yönergeler için aşağıdaki okuyarak bu güvenlik açıklarına karşı korumak için işlem yapması gerekmez [180002](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), [180018](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/adv180018), ve [190013](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV190013)).
>
> Diğer müşteriler, bu güvenlik açıklarından derinliği perspektifinde savunma değerlendirmek ve kendi seçtiğiniz yapılandırma, güvenlik ve performans etkilerini göz önünde bulundurun.



## <a name="keeping-your-operating-systems-up-to-date"></a>İşletim sistemlerinizi güncel tutma

Bir işletim sistemi güncelleştirme diğer Azure müşterilerinin Azure üzerinde çalışan, uygulamaları yalıtmak için gerekli olmasa da, bilgisayarınızı güncel tutmak için en iyi uygulama her zaman olduğu. İçin en son güvenlik toplamaları Windows birkaç kurgusal yürütme yan kanal güvenlik açığına yönelik risk azaltma işlemleri içerir. Benzer şekilde, Linux dağıtımları, bu güvenlik açıklarına değinen birden çok güncelleştirme yayımladık. İşletim sisteminizi güncelleştirmeniz için sunduğumuz önerilen eylemler şunlardır:

| Teklifi | Önerilen Eylem  |
|----------|---------------------|
| Azure bulut Hizmetleri  | Etkinleştirme [otomatik güncelleştirme](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal) veya en yeni konuk işletim sistemi çalıştırdığınızdan emin olun. |
| Azure Linux sanal makineleri | Güncelleştirmeleri, işletim sistemi Sağlayıcısı'ndan yükleyin. Daha fazla bilgi için [Linux](#linux) bu belgenin devamındaki. |
| Azure Windows sanal makineleri  | En son güvenlik paketi yükleyin.
| Diğer Azure PaaS Hizmetleri | Bu hizmetler kullanan müşteriler için gereken herhangi bir işlem yoktur. Azure, işletim sistemi sürümleri otomatik olarak güncel tutar. |

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Güvenilmeyen kod çalıştırılıyorsa ek yönergeler 

Müşteriler güvenilmeyen kullanıcıların rastgele kod yürütmek bazı ek güvenlik özellikleri, Azure sanal makineler veya Cloud Services içinde uygulamak isteyebilirsiniz. Bu özellikler, kurgusal yürütmeyi açıklarına açıklayan işlem içi açığa vektörlerine karşı koruma.

Ek güvenlik özelliklerini burada önerilen örnek senaryolar:

- Çalıştırmak için sanal makine içinde güvenmediğiniz kod sağlar.  
    - *Örneğin, bir ikili dosya karşıya yükleme veya, betik sonra uygulamanızın içinden yürütme müşterilerinize biri izin*. 
- Düşük ayrıcalıklı hesapları kullanarak oturum açmak için VM'de oturum güvenmediğiniz kullanıcılar tanır.   
    - *Örneğin, Uzak Masaüstü veya SSH kullanarak düşük ayrıcalıklı Vm'lerinizi birine oturum açmasına izin*.  
- Güvenilmeyen kullanıcıların sanal makineleri, iç içe sanallaştırma uygulanan erişim izni.  
    - *Örneğin, Hyper-V ana bilgisayar denetim ancak Vm'leri güvenilmeyen kullanıcılara tahsis*. 

Müşteriler güvenilmeyen kod içeren bir senaryo kullanılmaz, bu ek güvenlik özelliklerini etkinleştirmek gerekmez. 

## <a name="enabling-additional-security"></a>Ek güvenlik etkinleştirme 

Güvenilmeyen kod çalıştırıyorsanız VM veya Bulut hizmeti içinde ek güvenlik özellikleri etkinleştirebilirsiniz. Paralel olarak, işletim sisteminin sanal makine veya Bulut hizmeti içindeki güvenlik özelliklerini etkinleştirmek için güncel olduğundan emin olun.

### <a name="windows"></a>Windows 

Hedef işletim sisteminiz bu ek güvenlik özelliklerini etkinleştirmek için güncel olması gerekir. Çok sayıda kurgusal yürütme yan kanal azaltmaları varsayılan olarak etkindir ancak aşağıda açıklanan ek özellikler el ile etkinleştirilmelidir ve performans düşüşüne neden olabilir. 


**1. adım: Hyper-threading sanal makinede devre dışı** -güvenilmeyen kod hiper iş parçacıklı bir VM'de çalıştıran müşteriler, hyper-threading devre dışı bırakın veya olmayan-hiper iş parçacıklı bir VM boyutu için taşımak gerekir. Başvuru [bu belge](https://docs.microsoft.com/azure/virtual-machines/windows/acu) hiper iş parçacıklı VM boyutları (vCPU core'a oranını olduğu 2:1) listesi. Sanal makinenizin hiper izleğin-etkin olup olmadığını denetlemek için bkz aşağıdaki betiği kullanarak Windows komut satırından VM içinde.

Tür `wmic` etkileşimli arabirimi girmek için. Ardından fiziksel miktarını görüntülemek için aşağıdaki ve mantıksal yazın VM üzerindeki.

```console
CPU Get NumberOfCores,NumberOfLogicalProcessors /Format:List
```

Mantıksal işlemci sayısı (çekirdek) fiziksel işlemci sayısından büyükse, ardından hyper-threading etkinleştirilir.  Hiper iş parçacıklı bir VM çalıştırıyorsanız, lütfen [Azure desteğine başvurun](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) hiper iş parçacığı için alma devre dışı.  Hyper-threading devre dışı bırakıldığında **desteği tam VM yeniden başlatma gerektiren**. Lütfen [çekirdek sayısı](#core-count) neden, VM çekirdek sayısı azaldıkça anlamak için.


**2. adım**: 1. adım için paralel olarak yönergeleri [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) korumaları etkin kullanarak doğrulamak için [SpeculationControl](https://aka.ms/SpeculationControlPS) PowerShell modülü.

> [!NOTE]
> Bu modülün daha önce indirdiyseniz, en yeni sürümünü yüklemeniz gerekir.
>


PowerShell betiğinin çıktısı olması doğrulamak için değerleri bu güvenlik açıklarına karşı koruma etkin:

```
Windows OS support for branch target injection mitigation is enabled: True
Windows OS support for kernel VA shadow is enabled: True
Windows OS support for speculative store bypass disable is enabled system-wide: False
Windows OS support for L1 terminal fault mitigation is enabled: True
Windows OS support for MDS mitigation is enabled: True
```

Çıkış gösteriliyorsa `MDS mitigation is enabled: False`, lütfen [Azure desteğine başvurun](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) kullanılabilir azaltma seçenekleri.



**3. adım**: Çekirdek sanal adres gölgeleme (KVAS) ve dal hedef ekleme (BTI) işletim sistemi desteğini etkinleştirmek için yönergeleri izleyin. [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) korumaları kullanarak etkinleştirmek için `Session Manager` kayıt defteri anahtarları. Bir yeniden başlatma gerekiyor.


**4. adım**: Kullanan dağıtımlar için [iç içe sanallaştırma](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (D3 ve yalnızca E3): Bir Hyper-V konağı olarak kullandığınız VM'nin içindeki bu yönergeleri uygulayın.

1.  Bölümündeki yönergeleri [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) korumaları kullanarak etkinleştirmek için `MinVmVersionForCpuBasedMitigations` kayıt defteri anahtarları.
2.  Hiper yönetici Zamanlayıcı türü kümesine `Core` yönergeleri izleyerek [burada](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).


### <a name="linux"></a>Linux

<a name="linux"></a>Kümesi içinde ek güvenlik özelliklerini etkinleştirme, hedef işletim sistemini tam olarak güncel olmasını gerektirir. Bazı risk azaltma işlemleri varsayılan olarak etkinleştirilir. Aşağıdaki bölümde, varsayılan olarak ve/veya donanım desteği (mikro kod) sayfalarında devre dışı olan özellikleri açıklar. Bu özellikleri etkinleştirmek, performans düşüşüne neden olabilir. Daha fazla yönerge için işletim sistemi sağlayıcınızın belgeleri başvurusu


**1. adım: Hyper-threading sanal makinede devre dışı** -güvenilmeyen kod hiper iş parçacıklı bir VM'de çalıştıran müşteriler, hyper-threading devre dışı bırakın veya olmayan-hiper iş parçacıklı bir VM'ye taşımak gerekir.  Başvuru [bu belge](https://docs.microsoft.com/azure/virtual-machines/linux/acu) hiper iş parçacıklı VM boyutları (vCPU core'a oranını olduğu 2:1) listesi. Hiper iş parçacıklı bir VM çalıştırıyorsanız denetlemek için çalıştırın `lscpu` Linux VM'de komutu. 

Varsa `Thread(s) per core = 2`, ardından hyper-threading etkinleştirildi. 

Varsa `Thread(s) per core = 1`, ardından hyper-threading devre dışı bırakıldı. 

 
Hiper izleğin-etkin bir VM için örnek çıktı gösterilmektedir: 

```console
CPU Architecture:      x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1

```

Hiper iş parçacıklı bir VM çalıştırıyorsanız, lütfen [Azure desteğine başvurun](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) hiper iş parçacığı için alma devre dışı.  Hyper-threading devre dışı bırakıldığında **desteği tam VM yeniden başlatma gerektiren**. Lütfen [çekirdek sayısı](#core-count) neden, VM çekirdek sayısı azaldıkça anlamak için.



**2. adım**: Herhangi bir karşı azaltmak için kurgusal yürütme yan kanal güvenlik açıkları, işletim sistemi sağlayıcının belgelerine bakın:   
 
- [RedHat ve CentOS](https://access.redhat.com/security/vulnerabilities) 
- [SUSE](https://www.suse.com/support/kb/?doctype%5B%5D=DT_SUSESDB_PSDB_1_1&startIndex=1&maxIndex=0) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/) 


### <a name="core-count"></a>Çekirdek sayısı

Hiper iş parçacıklı bir VM oluşturulduğunda, Azure tarafından ayrılan çekirdek başına 2 iş parçacığı - bunlar Vcpu olarak adlandırılır. Hyper-threading devre dışı bırakıldığında, Azure, bir iş parçacığı ve yüzeyleri tek iş parçacıklı çekirdek (fiziksel çekirdekler)'kurmak kaldırır. CPU vCPU oranını 2:1., bu nedenle bir kez hyper-threading devre dışı bırakıldı, CPU sayısı VM tarafından yarım azaltılabilir için görünür. Örneğin, bir D8_v3 hiper iş parçacıklı VM 8 Vcpu (çekirdek x 4 çekirdeği başına 2 iş parçacığı) üzerinde çalışan bir vm'dir.  Hyper-threading devre dışı bırakıldığında, CPU çekirdeği başına 1 iş parçacığı ile 4 fiziksel çekirdek için kaldıracağız. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makale için kılavuzluk sağlar. çok sayıda modern işlemciye etkileyen kurgusal yürütme yan kanal saldırıları aşağıda:

[Spectre ve Meltdown](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180002):
- CVE-2017-5715 - dal hedef ekleme (BTI)  
- CVE-2017-5754 - çekirdek sayfa tablosu yalıtım (KPTI)
- CVE-2018-3639 – kurgusal Store atlama (KPTI) 
 
[L1 Terminal hata (L1TF)](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180018):
- CVE-2018-3615 - Intel yazılım koruma Uzantıları (Intel SGX)
- CVE-2018-işletim sistemlerini (OS) ve sistem yönetim modu (SMM) 3620-
- CVE-2018-3646 – Virtual Machine Manager (VMM) etkiler

[Mikro mimari veri örnekleme](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190013): 
- CVE-2019-11091 - mikro mimari (MDSUM) Uncacheable bellek örnekleme verileri
- CVE-2018-12126 - mikro mimari Store verilerini arabelleğe (MSBDS) örnekleme
- CVE-2018-12127 - mikro mimari yük bağlantı noktası verileri (MLPDS) örnekleme
- CVE-2018-12130 - mikro mimari dolgu verilerini arabelleğe (MFBDS) örnekleme








