---
title: "Azure Site Recovery en iyi uygulamaları | Microsoft Belgeleri"
description: "Bu makalede Azure Site Recovery dağıtımı için en iyi uygulamalar açıklanmaktadır"
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: a53e2a09fb2aabe96dd5347ea7509815d92bfdb8
ms.lasthandoff: 03/15/2017

---

# <a name="get-ready-to-deploy-azure-site-recovery"></a>Azure Site Recovery'yi dağıtmaya hazırlanma

Bu makalede Azure Site Recovery dağıtımına hazırlanma işlemi açıklanmaktadır.

## <a name="protecting-hyper-v-virtual-machines"></a>Hyper-V sanal makinelerini koruma

Hyper-V sanal makinelerini korumaya yönelik birkaç dağıtım seçeneğiniz vardır. Şirket içi Hyper-VM’leri Azure'a veya ikincil veri merkezine çoğaltabilirsiniz. Her dağıtım için farklı gereksinimler vardır.

**Gereksinim** | **Azure'a çoğaltma (VMM ile)** | **Hyper-V VM’lerini Azure’a çoğaltma (VMM olmadan)** | **Hyper-V VM'lerini ikincil siteye çoğaltma (VMM ile)** | **Ayrıntılar**
---|---|---|---|---
**VMM** | System Center 2012 R2 üzerinde çalışan VMM <br/><br/>Bir veya daha fazla VMM ana bilgisayar grubu içeren en az bir VMM bulutu. | NA | En azından en son güncelleştirmeleri içeren System Center 2012 SP1 üzerinde çalışan birincil ve ikincil sitelerdeki VMM sunucuları. <br/><br/> Her VMM sunucusunda en az bir bulut. Bulutların Hyper-V kapasite profili ayarlanmış olmalıdır.<br/><br/> Kaynak bulutu en az bir VMM ana bilgisayar grubuna sahip olmalıdır. | İsteğe bağlı. Hyper-V sanal makinelerini Azure’a çoğaltmak için System Center VMM’sinin dağıtılması gerekmez, ancak dağıtırsanız VMM sunucusunun doğru ayarlandığından emin olmanız gerekir. Buna doğru VMM sürümünün çalıştırıldığından ve bulutların ayarlandığından emin olma dahildir.
**Hyper-V** | Windows Server 2012 R2 veya sonraki sürümü çalıştıran şirket içi sitede en az bir Hyper-V ana bilgisayar sunucusu | En son güncelleştirmeleri içeren Windows Server 2012 veya sonraki sürümünde çalışıp İnternet’e bağlı olan kaynak ve hedef sitelerde en az bir Hyper-V sunucusu.<br/><br/> Hyper-V sunucularının VMM bulutundaki bir ana bilgisayar grubunda olması gerekir. | En son güncelleştirmeleri içeren Windows Server 2012 veya sonraki sürümünde çalışıp İnternet’e bağlı olan kaynak ve hedef sitelerde en az bir Hyper-V sunucusu.<br/><br/> Hyper-V sunucularının VMM bulutundaki bir ana bilgisayar grubunda bulunması gerekir. |
**Sanal makineler** | Kaynak Hyper-V ana bilgisayar sunucusunda en az bir VM | Kaynak VMM bulutundaki Hyper-V ana bilgisayar sunucusunda en az bir VM | Kaynak VMM bulutundaki Hyper-V ana bilgisayar sunucusunda en az bir VM. |  Azure'a çoğaltılan VM'ler, Azure sanal makine önkoşullarına uygun olmalıdır
**Azure hesabı** | Bir Azure hesabınızın olması ve Site Recovery hizmetine abone olmanız gerekir. | Bir Azure hesabınızın olması ve Site Recovery hizmetine abone olmanız gerekir. | NA | Bir hesabınız yoksa ücretsiz deneme sürümü ile başlayın.
**Azure depolama alanı** | Coğrafi çoğaltma özelliğinin etkin olduğu bir Azure depolama hesabına abone olmanız gerekir. | Coğrafi çoğaltma özelliğinin etkin olduğu bir Azure depolama hesabına abone olmanız gerekir. | NA | Hesap, Azure Site Recovery kasası ile aynı bölgede ve aynı abonelikle ilişkilendirilmiş olmalıdır.
**Ağ** | Aynı Azure ağın yük devreden tüm makinelerin, hangi kurtarma planında yer aldıklarına bakılmaksızın birbiriyle bağlantı kurabildiğinden emin olmak için ağ eşlemesi ayarlayın. Ayrıca, hedef Azure ağında bir ağ geçidi yapılandırılırsa sanal makineler diğer şirket içi sanal makinelere bağlanabilir. Ağ eşlemesini ayarlamazsanız, yalnızca aynı kurtarma planında yük devreden makineler bağlanabilir. | NA |  <br/><br/>Sanal makinelerin yük devretme sonrasında uygun ağlara bağlandığından ve bu çoğaltma sanal makinelerin Hyper-V ana bilgisayar sunucularına en uygun şekilde yerleştirildiğinden emin olmak üzere ağ eşlemesi ayarlayın. Ağ eşlemesini yapılandırmazsanız, çoğaltılmış makineler yük devretme sonrasında herhangi bir VM’ye bağlanmaz. |  VMM ile ağ eşlemesini ayarlamak için VMM mantığının ve VM ağlarının doğru yapılandırıldığından emin olmanız gerekir.
**Sağlayıcılar ve aracılar** | Dağıtım sırasında VMM sunucularına Azure Site Recovery Sağlayıcısı'nı yükleyin. VMM bulutlarındaki Hyper-V sunucularına Azure Kurtarma Hizmetleri aracısını yükleyin. | Dağıtım sırasında, Hyper-V ana bilgisayar sunucusuna veya kümesine Azure Site Recovery Sağlayıcısı ve Azure Kurtarma Hizmetleri aracısını yükleyin| Dağıtım sırasında VMM sunucularına Azure Site Recovery Sağlayıcısı'nı yükleyin. VMM bulutlarındaki Hyper-V sunucularına Azure Kurtarma Hizmetleri aracısını yükleyin. | Sağlayıcılar ve aracılar, şifrelenmiş bir HTTPS bağlantısı kullanarak İnternet üzerinden Site Recovery’ye bağlanır. Sağlayıcı bağlantısı için güvenlik duvarı özel durumları eklemeniz veya belirli bir proxy oluşturmanız gerekmez.
**İnternet bağlantısı** | Yalnızca VMM sunucuları için İnternet bağlantısı gereklidir | Yalnızca Hyper-V ana sunucuları için İnternet bağlantısı gereklidir | Yalnızca VMM sunucuları için İnternet bağlantısı gereklidir | Sanal makinelere herhangi bir şey yüklenmesi gerekmez ve doğrudan İnternet bağlantısı kurulmaz.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>VMware sanal makineleri veya fiziksel sunucularını koruma

VMware sanal makinelerini veya Windows/Linux fiziksel sunucularını korumaya yönelik birkaç dağıtım seçeneği mevcuttur. Bunları Azure’a veya ikincil veri merkezine çoğaltabilirsiniz. Her dağıtım için farklı gereksinimler vardır.

**Gereksinim** | **VMware VM’lerini/fiziksel sunucuları Azure’a çoğaltma)** | * **VMware VM’lerini/Fiziksel sunucuları ikincil siteye çoğaltma**  
---|---|---
**Birincil site** | **İşlem sunucusu**: ayrılmış bir Windows sunucusu (fiziksel veya sanal) | **İşlem sunucusu**: ayrılmış bir Windows sunucusu (fiziksel veya VMware sanal makinesi)<br/><br/>  
**Şirket içi ikincil site** | NA | **Yapılandırma sunucusu**: ayrılmış bir Windows sunucusu (fiziksel veya sanal) <br/><br/> **Ana hedef sunucu**: ayrılmış bir sunucu (fiziksel veya sanal). Windows makinelerini korumak için Windows, Linux makinelerini korumak için Linux ile yapılandırın.
**Azure** | **Abonelik**: Site Recovery hizmeti için bir abonelik gerekir. <br/><br/> **Depolama hesabı**: Coğrafi çoğaltmanın etkin olduğu bir depolama hesabınızın olmalıdır. Hesap, Site Recovery kasası ile aynı bölgede ve aynı abonelikle ilişkilendirilmiş olmalıdır. <br/><br/> **Yapılandırma sunucusu**: Yapılandırma sunucusunu bir Azure VM olarak ayarlamanız gerekir <br/><br/> **Ana hedef sunucu**: Ana hedef sunucuyu bir Azure VM olarak ayarlamanız gerekir <br/><br/> Windows makinelerini korumak için Windows, Linux makinelerini korumak için Linux ile yapılandırın.<br/><br/> **Azure sanal ağı**:  Yapılandırma sunucusu ve ana hedef sunucunun dağıtılacağı bir Azure sanal ağı gerekir. Ağın, Azure Site Recovery kasasıyla aynı abonelik ve aynı bölgede olması gerekir | NA  
**Sanal makineler/fiziksel sunucular** | En az bir VMware sanal makinesi veya fiziksel Windows/Linux sunucusu.<br/><br/>Dağıtım sırasında Mobility hizmeti her makineye yüklenir| En az bir VMware VM veya fiziksel Windows/Linux sunucusu.<br/><br/> Dağıtım sırasında her makineye Birleşik aracı yüklenir.




## <a name="azure-virtual-machine-requirements"></a>Azure sanal makine gereksinimleri

Azure tarafından desteklenen herhangi bir işletim sistemi çalıştıran sanal makineleri ve fiziksel sunucuları çoğaltmak için Site Recovery’yi dağıtabilirsiniz. Buna çoğu Windows ve Linux sürümü dahildir. Korumak istediğiniz şirket içi sanal makinelerin Azure gereksinimlerine uygun olduğundan emin olmanız gerekir.


## <a name="optimizing-your-deployment"></a>Dağıtımınızı en iyi duruma getirme

Dağıtımınızı en iyi duruma getirmek ve ölçeklendirmek için aşağıdaki ipuçlarını kullanın.

- **İşletim sistemi birim boyutu**: Bir sanal makineyi Azure’a çoğaltırken işletim sistemi birimi 1 TB’tan küçük olmalıdır. Bundan fazla biriminiz varsa, dağıtımı başlatmadan önce bu birimleri farklı bir diske el ile taşıyabilirsiniz.
- **Veri disk boyutu**: Azure'a çoğaltma yapıyorsanız, bir sanal makine üzerinde her biri en fazla 1 TB olan en fazla 32 veri diskiniz olabilir. Yaklaşık&32; TB’lık bir sanal makineyi etkili bir şekilde çoğaltabilir ve yük devredebilirsiniz.
- **Kurtarma planı sınırları**: Site Recovery, binlerce sanal makineyi ölçeklendirebilir. Kurtarma planları birlikte yük devretmesi gereken uygulamalar için model olarak tasarlanmıştır, bu nedenle bir kurtarma planındaki makinelerin sayısı 50 ile sınırlıdır.
- **Azure hizmet sınırları**: Her Azure aboneliği, çekirdeklere, bulut hizmetlerine vb. yönelik varsayılan sınırlamalarla sunulur. Aboneliğinizdeki kaynakların kullanılabilirliğini doğrulamak için bir yük devretme testi çalıştırmanız önerilir. Bu limitleri Azure desteği aracılığıyla değiştirebilirsiniz.
- **Kapasite planlama**: Ölçekleme ve performans için planlama.
- **Çoğaltma bant genişliği**: Çoğaltma bant genişliği azaldıysa şunları unutmayın:
    - **ExpressRoute**: Site Recovery, Riverbed gibi Azure ExpressRoute ve WAN iyileştiricileri ile birlikte çalışır.
    - **Çoğaltma trafiği**: Site Recovery, VHD’nin tamamı yerine yalnızca veri bloklarını kullanarak akıllı bir ilk çoğaltma gerçekleştirir. Devam eden çoğaltma sırasında yalnızca değişiklikler çoğaltılır.
    - **Ağ trafiği**: Hedef IP adresi ve bağlantı noktasını temel alan bir ilke ile Windows QoS ayarlayarak, çoğaltma için kullanılan ağ trafiğini denetleyebilirsiniz.  Ayrıca, Azure Backup aracısını kullanarak Azure Site Recovery’ye çoğaltma yapıyorsanız Bu aracı için azaltma yapılandırabilirsiniz.
- **RTO**: Site Recovery ile beklediğiniz kurtarma süresi hedefini (RTO) ölçmek istiyorsanız, bir yük devretme testi çalıştırmanız ve Site Recovery işlerini görüntüleyerek işlemleri tamamlamanın ne kadar sürdüğünü analiz etmeniz önerilir. Azure’a yük devrediyorsanız, en iyi RTO için Azure otomasyonu ve kurtarma planlarıyla tümleştirerek, el ile gerçekleştirilen tüm eylemleri otomatikleştirmeniz önerilir.
- **RPO**: Site Recovery, Azure'a çoğaltma yaptığınızda neredeyse zaman uyumlu bir kurtarma noktası hedefini (RPO) destekler. Bu seçenek, veri merkeziniz ile Azure arasında yeterli bant genişliği olduğunu varsayar.

