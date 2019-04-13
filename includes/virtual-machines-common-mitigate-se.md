---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 4c5b4c5eacd4be751004af551e3753a61873c7a7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551663"
---
**En son Güncelleştirmesi'ni belge**: 14 Ağustos 2018 10: 00'te Pasifik saati.

Açıklanması bir [CPU güvenlik açıklarından yeni sınıf](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) kurgusal yürütme yan kanal saldırıları olarak bilinen daha fazla netlik dağıtımınızla müşterilerden gelen soruları sonuçlandı.  

Microsoft, tüm bulut hizmetlerimizin arasında azaltmaları dağıtmıştır. Azure çalışan ve müşteri iş yüklerinin birbirinden yalıtan altyapı korunur. Bu altyapıyı kullanarak olası bir saldırganın bu güvenlik açıklarına kullanarak uygulamanızı saldırı olamaz anlamına gelir.

Azure kullanarak [Bakımı koruma bellek](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#maintenance-not-requiring-a-reboot) mümkün olduğunda, müşteri etkiyi en aza indirmek ve yeniden başlatma gereksinimini ortadan kaldırmak. Azure ana bilgisayara yükleyebilecek güncelleştirmeler yaparken bu yöntemleri yararlanmaya devam ve müşterilerimizi korumak.

Güvenlik her yönüyle Azure ile nasıl tümleştirildiği hakkında daha fazla bilgi edinilebilir [Azure güvenlik belgeleri](https://docs.microsoft.com/azure/security/) site. 

> [!NOTE] 
> Bu belge ilk kez yayınlandığı bu güvenlik açığı sınıfın birden çok çeşitleri duyurulmuştur. Microsoft, müşterilerimizin koruma ve rehberlik sağlama yatırım yoğun devam eder. Yayın başka düzeltmeleri devam ederken bu sayfa güncelleştirilir. 
> 
> 14 Ağustos 2018 tarihinde sektör olarak bilinen yeni bir kurgusal yürütme yan kanal güvenlik açığını duyurulmuş [L1 Terminal hata](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018) (L1TF) atanmış olan birden çok CVEs ([CVE-2018-3615, CVE-2018-3620 ve CVE-2018-3646](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00161.html)). Bu güvenlik açığını Intel Core® işlemcileri ve Intel® Xeon® İşlemci etkiler. Microsoft azaltmaları güçlendirmek müşteriler arasında yalıtım müşterilerimize bulut hizmetlerimizle arasında Dağıttı. Lütfen aşağıda L1TF ve önceki güvenlik açıklarına karşı korumaya yönelik ek yönergeler için okuma ([Spectre değişken 2 CVE-2017-5715 ve Meltdown değişken 3 CVE-2017-5754](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution)).
>  






## <a name="keeping-your-operating-systems-up-to-date"></a>İşletim sistemlerinizi güncel tutma

Bir işletim sistemi güncelleştirme diğer Azure müşterilerinin Azure üzerinde çalışan, uygulamaları yalıtmak için gerekli olmasa da, bilgisayarınızı güncel tutmak için en iyi uygulama her zaman olduğu. İçin en son güvenlik toplamaları Windows birkaç kurgusal yürütme yan kanal güvenlik açığına yönelik risk azaltma işlemleri içerir. Benzer şekilde, Linux dağıtımları, bu güvenlik açıklarına değinen birden çok güncelleştirme yayımladık. İşletim sisteminizi güncelleştirmeniz için sunduğumuz önerilen eylemler şunlardır:

| Teklifi | Önerilen Eylem  |
|----------|---------------------|
| Azure Cloud Services  | Etkinleştirme [otomatik güncelleştirme](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal) veya en yeni konuk işletim sistemi çalıştırdığınızdan emin olun. |
| Azure Linux Sanal Makineleri | Güncelleştirmeleri, işletim sistemi Sağlayıcısı'ndan yükleyin. Daha fazla bilgi için [Linux](#linux) bu belgenin devamındaki. |
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

Sanal makine veya Bulut hizmeti içinde ek güvenlik özellikleri etkinleştirebilirsiniz.

### <a name="windows"></a>Windows 

Hedef işletim sisteminiz bu ek güvenlik özelliklerini etkinleştirmek için güncel olması gerekir. Çok sayıda kurgusal yürütme yan kanal azaltmaları varsayılan olarak etkindir ancak aşağıda açıklanan ek özellikler el ile etkinleştirilmelidir ve performans düşüşüne neden olabilir. 

**1. adım**: [Azure Destek ekibiyle iletişime geçin](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) güncelleştirilmiş sunmaya bellenimi (mikro kod), sanal makinelere için. 

**2. adım**: Çekirdek sanal adres gölgeleme (KVAS) ve dal hedef ekleme (BTI) işletim sistemi desteğini etkinleştirin. Bölümündeki yönergeleri [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) korumaları aracılığıyla etkinleştirmek için `Session Manager` kayıt defteri anahtarları. Bir yeniden başlatma gerekiyor. 

**3. adım**: Kullanan dağıtımlar için [iç içe sanallaştırma](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (D3 ve yalnızca E3): Bir Hyper-V konağı olarak kullandığınız VM'nin içindeki bu yönergeleri uygulayın. 

1. Bölümündeki yönergeleri [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) korumaları aracılığıyla etkinleştirmek için `MinVmVersionForCpuBasedMitigations` kayıt defteri anahtarları.  
 
1. Hiper yönetici Zamanlayıcı türü kümesine **çekirdek** yönergeleri izleyerek [burada](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types). 

**4. adım**: Bölümündeki yönergeleri [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) korumaları etkin kullanarak doğrulamak için [SpeculationControl](https://aka.ms/SpeculationControlPS) PowerShell modülü. 

> [!NOTE]
> Bu modülün daha önce indirdiyseniz, en yeni sürümünü yüklemeniz gerekir.
>

Tüm VM'lerin göstermelidir:

```
branch target injection mitigation is enabled: True

kernel VA shadow is enabled: True  

L1TFWindowsSupportEnabled: True
```


### <a name="linux"></a>Linux

<a name="linux"></a>Kümesi içinde ek güvenlik özelliklerini etkinleştirme, hedef işletim sistemini tam olarak güncel olmasını gerektirir. Bazı risk azaltma işlemleri varsayılan olarak etkinleştirilir. Aşağıdaki bölümde, varsayılan olarak ve/veya donanım desteği (mikro kod) sayfalarında devre dışı olan özellikleri açıklar. Bu özellikleri etkinleştirmek, performans düşüşüne neden olabilir. Daha fazla yönerge için işletim sistemi sağlayıcınızın belgeleri başvurusu
 
**1. adım**: [Azure Destek ekibiyle iletişime geçin](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) güncelleştirilmiş sunmaya bellenimi (mikro kod), sanal makinelere için.
 
**2. adım**: CVE-2017-5715 (Spectre değişken 2), işletim sistemi sağlayıcının belgelerine izleyerek azaltmak dal hedef ekleme (BTI) işletim sistemi desteğini etkinleştirin. 
 
**3. adım**: CVE-2017-5754 (Meltdown değişken 3) azaltmak için çekirdek sayfası tablosu yalıtım (KPTI), işletim sistemi sağlayıcının belgelerine izleyerek etkinleştirin. 
 
Daha fazla bilgi işletim sisteminizin sağlayıcıdan kullanılabilir:  
 
- [RedHat ve CentOS](https://access.redhat.com/security/vulnerabilities/speculativeexecution) 
- [SuSE](https://www.suse.com/support/kb/doc/?id=7022512) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/SpectreAndMeltdown) 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure müşterilerini CPU Güvenlik Açığı](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
