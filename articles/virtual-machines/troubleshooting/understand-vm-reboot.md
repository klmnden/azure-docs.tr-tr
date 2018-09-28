---
title: Bir Azure sanal makinesi için sistemin yeniden başlatılma nedenini anlama | Microsoft Docs
description: Yeniden başlatmak bir VM'nin önerilmemesinin olayları listeler
services: virtual-machines
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: dd9a2c6a5b2183d4909b6bcfd24dc1fae401c785
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414696"
---
# <a name="understand-a-system-reboot-for-azure-vm"></a>Azure VM için sistemin yeniden başlatılma nedenini anlama

Azure sanal makineleri (VM'ler) bazen yok, yeniden başlatma işlemini başlattı, kanıt olmadan görünen bir nedenle yeniden başlatılması. Bu makalede, VM'lerin yeniden başlatılmasına neden olabilir ve beklenmeyen yeniden başlatma sorunları önlemenize veya bu sorunların etkisini azaltmak nasıl öngörü sağlayan olaylar ve Eylemler listeler.

## <a name="configure-the-vms-for-high-availability"></a>Yüksek kullanılabilirlik için Vm'leri yapılandırma
Bir VM'ye karşı Azure üzerinde çalışan bir uygulamayı korumak için en iyi yolu yeniden başlatır ve kapalı kalma süresi VM'ler yüksek kullanılabilirlik için yapılandırılır.

Bu düzeyde uygulamanıza yedeklilik sağlamak için iki veya daha fazla sanal makine bir kullanılabilirlik kümesinde gruplandırmanız önerilir. Bu yapılandırma ya da bir planlı veya Plansız bakım olayı sırasında en az bir VM kullanılabilir ve yüzde 99,95 karşıladığından emin olmanızı sağlar [Azure SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Kullanılabilirlik kümeleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [VM kullanılabilirliğini yönetme](../windows/manage-availability.md)
- [VM kullanılabilirliğini yapılandırma](../windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Kaynak durumu bilgilerine 
Azure kaynak durumu, bağımsız Azure kaynaklarının durumunu ortaya çıkaran ve sorunlarını gidermek için eyleme dönüştürülebilir rehberlik sağlayan bir hizmettir. Burada sunucular veya altyapı öğelerini doğrudan erişmek mümkün olmayan bir bulut ortamında kaynak durumu sorunları gidermeye harcadığınız süreyi azaltmak üzere hedefidir. Özellikle, bir yandan sorunun kök uygulama ya da Azure platformundaki bir olay olduğunda belirleme harcadığınız süreyi azaltmaktır. Daha fazla bilgi için [kavrama ve kullanma kaynak durumu](../../resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>Eylemler ve yeniden başlatmak VM'nin önerilmemesinin olayları

### <a name="planned-maintenance"></a>Planlı bakım
Microsoft Azure, dünya çapında güvenilirlik, performans ve Vm'leri altını aldığı konak altyapısının güvenliğini iyileştirmek için düzenli olarak güncelleştirmeler yapar. Bellek tasarruflu güncelleştirmeler dahil olmak üzere, bu güncelleştirmelerin çoğu sanal makinelerinizin herhangi bir etkisi olmadan gerçekleştirilen veya Bulut Hizmetleri.

Ancak bazı güncelleştirmeler yeniden başlatma gerektirir. Böyle durumlarda VM Altyapı Güncelleştirmesi ve sonra Vm'leri yeniden kapatılır.

Hangi Azure planlı Bakımı ve Linux sanal makinelerinizin kullanılabilirliğini nasıl etkileyebileceğini anlaması için burada listelenen makalelerine bakın. Makaleler, Azure hakkında arka plan bakım işlemi ve daha fazla etkiyi azaltmak için planlı bakım zamanlaması hakkında planlı sağlar.

- [Azure'da sanal makineler için planlı bakım](../windows/planned-maintenance.md)
- [Azure vm'lerinde planlı bakım zamanlaması hakkında](../windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Bellek koruma güncelleştirmeleri   
Bu güncelleştirmeleri Microsoft azure'da sınıfı için kullanıcılar kendi çalışan Vm'leri üzerinde herhangi bir etkisi deneyimi. Bu güncelleştirmelerin çoğu, çalışan örneği engellemeden güncelleştirilebilecek bileşenlere veya hizmetler içindir. Platform altyapı güncelleştirmelerinin VM'lerin yeniden başlatmadan uygulanabilir konak işletim sisteminde bazılarıdır.

Bu bellek koruma güncelleştirmeleri, yerinde dinamik geçiş olanağı sağlayan teknolojiyle gerçekleştirilir. Güncelleştirilmekte, VM yerleştirilen bir *duraklatıldı* durumu. Bu durum RAM belleğini korur ve bu arada alttaki konak işletim sistemi gerekli güncelleştirmeleri ve düzeltme eklerini alır. VM duraklatıldıktan sonra 30 saniye içinde devam ettirilir. VM devam ettirildikten sonra saati otomatik olarak eşitlenir.

Kısa duraklatma süresi nedeniyle güncelleştirmeler bu mekanizma aracılığıyla önemli ölçüde dağıtma, sanal makineler üzerindeki etkiyi azaltır. Ancak, tüm güncelleştirmelerin bu şekilde dağıtılabilir. 

Çok örnekli güncelleştirmeler (bir kullanılabilirlik kümesindeki VM'ler için), bir seferde bir güncelleme etki alanı şeklinde uygulanır.

> [!NOTE]
> Eski çekirdek sürümleri olan Linux makineler, bu güncelleştirme yöntemi sırasında bir çekirdek Panik tarafından etkilenir. Bu sorunu önlemek için çekirdek sürümü 3.10.0-327.10.1 veya sonraki bir sürüme güncelleştirin. Daha fazla bilgi için [bir konak düğümü yükseltmeden sonra bir Azure Linux sanal makinesinde 3.10 tabanlı bir çekirdek paniğiyle](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Kullanıcı tarafından başlatılan yeniden başlatma veya kapatma eylemi
 
Azure portalı, Azure PowerShell, komut satırı arabirimi veya REST API bir yeniden başlatma işlemi gerçekleştirirseniz, olayda bulabilirsiniz [Azure etkinlik günlüğü](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Sanal makinenin işletim sisteminden eylemini gerçekleştirirseniz, sistem günlüklerini Olay bulabilirsiniz.

Genellikle VM yeniden başlatılmasına neden diğer senaryolar birden fazla yapılandırma değişikliği Eylemler içerir. Normalde, sanal Makinenin bir yeniden başlatılmasına yol açacak belirli bir eylemi yürütürken belirten bir uyarı iletisi görürsünüz. VM yeniden boyutlandırma işlemleri yönetici hesabının parolasını değiştirme ve bir statik IP adresi ayarlama örneklerindendir.

### <a name="azure-security-center-and-windows-update"></a>Azure Güvenlik Merkezi ve Windows Update
Azure Güvenlik Merkezi, işletim sistemi güncelleştirmeleri eksik günlük Windows ve Linux sanal makineleri izler. Güvenlik Merkezi bir Windows sanal makine üzerinde yapılandırılmış hizmet bağlı olarak Windows Update veya Windows Server Update Services (WSUS) kullanılabilir güvenlik güncelleştirmeleri ve kritik güncelleştirmeler listesini alır. Güvenlik Merkezi, ayrıca Linux sistemleri için en son güncelleştirmeleri denetler. Sanal makinenizin sistem güncelleştirmesi eksikse, Güvenlik Merkezi sistem güncelleştirmelerini uygulayın önerir. Bu sistem güncelleştirmelerini uygulama, Azure Portal'da Güvenlik Merkezi aracılığıyla denetlenir. Bazı güncelleştirmeler uygulandıktan sonra VM yeniden başlatma gerekli olabilir. Daha fazla bilgi için [Azure Güvenlik Merkezi'nde sistem güncelleştirmelerini uygulayın](../../security-center/security-center-apply-system-updates.md).

Bu makineler kullanıcılarının yönetilmesine yönelik olduğundan şirket içi sunucular gibi Azure güncelleştirmeleri Windows Update'ten Windows Azure sanal makinelerine göndermez. Ancak, teşvik otomatik Windows güncelleştirme ayarı etkin bırakın. Windows Update'ten güncelleştirmeleri otomatik olarak yüklenmesini güncelleştirmeleri uygulandıktan sonra gerçekleşmesi yeniden başlatma da neden olabilir. Daha fazla bilgi için [Windows Update SSS](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>Sanal makinenizin kullanılabilirliğini etkileyen diğer durumlar
Hangi Azure etkin bir şekilde bir sanal makinenin askıya diğer durumlar vardır. Bu eylem önce temel alınan sorunları çözmek için bir fırsat gerekir böylece e-posta bildirimleri alırsınız. VM kullanılabilirliği etkileyen sorunları örnekleri, güvenlik ihlalleri ve sona erme tarihini ödeme yöntemlerini içerir.

### <a name="host-server-faults"></a>Konak sunucu hataları 
VM, Azure veri merkezi içinde çalıştıran fiziksel bir sunucuda barındırılır. Fiziksel sunucu, diğer Azure bileşenlerini birkaç yanı sıra konak Aracısı denilen ve aracıya çalıştırır. Bu Azure yazılım bileşenlerini fiziksel sunucuda yanıt vermemeye izleme sistemi yeniden başlatma kurtarma girişiminde konak sunucunun tetikler. VM, beş dakika içinde yeniden genellikle kullanılabilir ve dinamik olarak daha önce aynı konaktaki devam eder.

Sunucu hataları, genellikle bir sabit disk veya katı hal sürücüsü gibi donanım hatası tarafından kaynaklanır. Azure sürekli olarak bu durumları izler, temel alınan hataları tanımlar ve risk azaltma uygulanan ve test sonra güncelleştirme yapar.

Bazı ana sunucu hataları sunucuya belirli olabileceğinden, yinelenen bir VM yeniden başlatma durumu el ile VM'yi başka bir konak sunucuya yeniden dağıtılıyor iyileştirilebilir. Bu işlemi kullanarak tetiklenebilir **yeniden** seçeneği VM'nin veya durduruluyor ve Azure portalında VM'nin yeniden başlatılmasının Ayrıntıları sayfasında.

### <a name="auto-recovery"></a>Otomatik Kurtarma
Azure platformu, ana sunucu için herhangi bir nedenle yeniden olamaz, hatalı konak sunucunun araştırılması için rotasyon dışında olması için bir otomatik kurtarma eylemi başlatır. 

Konaktaki tüm sanal makineler otomatik olarak farklı ve iyi durumda konak sunucuya yeniden konumlandırılır. 15 dakika içinde genellikle bu işlemi tamamlanır. Otomatik Kurtarma işlemi hakkında daha fazla bilgi için bkz. [sanal makineleri otomatik kurtarma](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Plansız bakım
Nadir durumlarda, Azure operasyon ekibinin Azure platformunun genel durumunu emin olmak için bakım etkinlikleri gerçekleştirmesi gerekebilir. Bu davranış, VM'nin kullanılabilirlik etkileyebilir ve bu genellikle aynı otomatik kurtarma eylemi daha önce açıklandığı gibi sonuçlanır.  

Planlanmamış maintenances şunları içerir:

- Acil düğüm birleştirme
- Acil ağ anahtarı güncelleştirmeleri

### <a name="vm-crashes"></a>VM kilitlenmeleri
VM'ler, VM içindeki sorunları nedeniyle yeniden başlatılabilir. İş yükü veya VM'de çalışan rolü, konuk işletim sistemi içinde bir hata denetimi tetikleyebilir. Kilitlenme nedeni kullanılacağını belirlemek hakkında Yardım için sistem ve uygulama günlüklerini Windows Vm'leri için ve Linux Vm'leri seri günlüklerini görüntüleyin.

### <a name="storage-related-forced-shutdowns"></a>Depolama ile ilgili zorla kapatma
Azure'da VM'ler işletim sistemi ve veri depolama, Azure depolama altyapıda barındırılan sanal diskler kullanır. Azure platformu, 120 saniye boyunca kullanılabilirlik veya VM ile ilişkili sanal diskler arasında bağlantı etkilenen her veri bozulmasını önlemek için sanal makinelerin zorlanmış kapatma gerçekleştirir. Depolama bağlantı geri yüklendikten sonra VM'lerin yeniden otomatik olarak desteklenir. 

Kapatma süresi, beş dakika kısa olabilir ancak önemli ölçüde uzun olabilir. Depolama ile ilgili zorla kapatma ile ilişkili özel durumlarından biri aşağıda verilmiştir: 

**GÇ aşan sınırlar**

G/ç isteklerinin hacmi g/ç işlemi (IOPS) saniyede disk g/ç sınırları aştığı için tutarlı bir şekilde azaltılır, Vm'leri geçici olarak kapatılmış olabilir. (Standart disk depolama için 500 IOPS sınırlıdır.) Bu sorunu gidermek için disk bölümleme türüyle kullanmak veya konuk VM iş yüküne bağlı olarak depolama alanı yapılandırabilirsiniz. Ayrıntılar için bkz [yapılandırma Azure sanal makinelerini en uygun depolama performansı için](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Daha yüksek IOPS limitlerine Azure Premium depolama ile en fazla 80.000 IOPS aracılığıyla kullanılabilir. Daha fazla bilgi için [yüksek performanslı Premium depolama](../windows/premium-storage.md).

### <a name="other-incidents"></a>Diğer olaylar
Nadir durumlarda, yaygın bir sorun, Azure veri merkezindeki birden fazla sunucu etkileyebilir. Bu sorun oluşursa, Azure ekibi, etkilenen abonelikler için e-posta bildirimleri gönderir. Denetleyebilirsiniz [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/) ve devam eden kesintiler ve geçmiş olayları durumu için Azure portalı.
