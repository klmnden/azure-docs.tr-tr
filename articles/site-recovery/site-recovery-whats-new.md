---
title: Azure Site Recovery'de yenilikler | Microsoft Docs
description: Azure Site Recovery'de sunulan yeni özellikleri bir özetini sunar
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: raynew
ms.openlocfilehash: 61bcc0565d57f9c64c453f79f319fc56d5a6de18
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925055"
---
# <a name="whats-new-in-site-recovery"></a>Site Recovery'deki yenilikler

[Azure Site Recovery](site-recovery-overview.md) hizmeti güncelleştirildi ve sürekli olarak Gelişmiş. Yeniliklerden haberdar olun yardımcı olmak için bu makalede, en son sürümlerde, yeni özellikler ve yeni içerik hakkında bilgi sağlar. Bu sayfayı düzenli olarak güncelleştirilir.

Site Recovery özellikleri için önerileriniz varsa, öğrenmek isteriz [Geri bildirimlerinizi](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>S1 2019 

### <a name="update-rollup-34-february-2019"></a>Güncelleştirme paketi 34 (Şubat 2019)

[Güncelleştirme paketi 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorunu giderir** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)



### <a name="update-rollup-33-february-2019"></a>Güncelleştirme paketi 33 (Şubat 2019)

[Güncelleştirme paketi 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorunu giderir** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)
**Ağ eşlemesi** | Azure VM'LERİNDE olağanüstü durum kurtarma için çoğaltmayı etkinleştirdiğinizde herhangi bir kullanılabilir hedef ağ artık kullanabilirsiniz. 
**Standart SSD** | Kullanarak Azure Vm'leri için olağanüstü durum kurtarma artık ayarlayabilirsiniz [standart SSD disk](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Depolama alanları doğrudan** | Kullanarak Azure VM uygulamalar üzerinde çalışan uygulamalar için olağanüstü durum kurtarma ayarlayabilirsiniz [depolama alanları doğrudan](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) yüksek kullanılabilirlik için.
**BRTFS dosya sistemi** | Ek Azure Vm'leri, VMware Vm'leri için desteklenmiyor.<br/><br/> Varsa desteklenmez: Çoğaltmayı etkinleştirdikten sonra BTRFS dosya sistemi alt birimi değiştirildi, dosya sistemi birden çok disk dağıldığında veya dosya sistemi BTRFS RAID destekler.



### <a name="update-rollup-32-january-2019"></a>Güncelleştirme paketi 32 (Ocak 2019)

[Güncelleştirme Paketi 31](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorunu giderir** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)
**Linux için olağanüstü durum kurtarma** | **Azure sanal makineleri**: RedHat iş istasyonu 7/6; Ubuntu, Debian ve SUSE yeni kernel sürümleri için destek.<br/><br/> **VMware Vm'lerini/fiziksel sunucuları**: Red Hat Enterprise Linux 7.6; RedHat iş istasyonu 7/6; Oracle Linux 6.10/7.6; Ubuntu, Debian ve SUSE yeni kernel sürümleri için spport.


### <a name="update-rollup-31-january-2019"></a>Güncelleştirme Paketi 31 (Ocak 2019)

[Güncelleştirme Paketi 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorunu giderir** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)
**Linux için olağanüstü durum kurtarma** | **Azure sanal makineleri**: Oracle Linux 6,8 ve 6.9/7.0; UEK5 çekirdekler için destek.<br/><br/> **VMware Vm'lerini/fiziksel sunucuları**: Oracle Linux 6,8 ve 6.9/7.0; UEK5 çekirdek için destek.
**BRTFS dosya sistemi** | Azure sanal makineler için desteklenir.
**LVM'Yİ** | LVM'yi ve LVM2 birimleri için eklenen destek.<br/><br/> Disk bölümü ve LVM birimlerde makinesiyse dizin desteklenir.
**Dizinler** | Destek ayrı bölümlerde ya da aynı sistem diskinde değil dosya sistemleri için bu dizinleri seet toplanır: / (root), makinesiyse, / usr, / usr/local, /var, / etc.
**Windows Server 2008** | Dinamik diskler için eklenen destek.
**VMware VM yük devretmesi** | Burada storvsc ve vsbus önyükleme sürücüleri olmayan VMware Vm'leri için yük devretme süresi geliştirildi.
**UEFI desteği** | Azure sanal makineler önyükleme türü UEFI desteklemez. Şimdi, UEFI ile şirket içi fiziksel sunucuları Azure Site Recovery ile geçirebilirsiniz. Site kurtarma, sunucunun geçişten önce için BIOS önyükleme türü dönüştürerek geçirir. Site Recovery daha önce bu dönüştürme VM'ler için yalnızca desteklenen. Windows Server 2012 çalıştıran fiziksel sunucular için veya üzeri destek sunulur.
**Kullanılabilirlik alanları, Azure Vm'leri** | Başka bir bölgeye çoğaltma kullanılabilirlik alanında dağıtılan Azure VM'ler için etkinleştirebilirsiniz. OU artık bir Azure VM için çoğaltmayı etkinleştirin ve tek bir VM örneği, bir sanal makine bir kullanılabilirlik kümesinde veya bir kullanılabilirlik alanında VM için yük devretme için hedef ayarlayın. Ayar çoğaltmasını etkilemez. [Okuma](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) duyuruyu.


## <a name="q4-2018"></a>S4 2018

### <a name="update-rollup-30-october-2018"></a>Güncelleştirme paketi 30 (Ekim 2018)

[Güncelleştirme paketi 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorunu giderir** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)
**Bölge desteği** | Avustralya Orta 1 ve Avustralya Orta 2 için eklenen Destek sitesi.
**Disk şifrelemesi desteği** | Azure AD uygulaması ile Azure Disk şifrelemesi (ADE) ile şifrelenmiş Azure Vm'leri olağanüstü durum kurtarma için eklenen destek. [Daha fazla bilgi edinin](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Disk dışlama** | Unitialized diskler artık Azure VM çoğaltma sırasında otomatik olarak dışlanır.
**Güvenlik Duvarı etkin depolama** | İçin eklenen Destek [Güvenlik Duvarı etkin depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Yönetilmeyen diskler başka bir Azure bölgesine olağanüstü durum kurtarma için Güvenlik Duvarı etkin depolama hesapları ile Azure Vm'lerini çoğaltabilirsiniz.<br/><br/> Yönetilmeyen diskler için hedef depolama hesapları, Güvenlik Duvarı etkin depolama hesapları kullanabilirsiniz.<br/><br/> Yalnızca PowerShell kullanılarak desteklenir.


### <a name="update-rollup-29-october-2018"></a>Güncelleştirme paketi 29 (Ekim 2018)

[Güncelleştirme paketi 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorunu giderir** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Mobility hizmeti uzantısının otomatik güncelleştirmeler

Site Recovery Mobility hizmeti uzantısı için otomatik güncelleştirmeler için bir seçenek eklendi. Mobility hizmeti uzantısı, Azure Site Recovery tarafından çoğaltılmış her VM'de yüklenir. Çoğaltmayı etkinleştirdiğinizde Site Recovery, uzantı için güncelleştirmeleri yönetmek izin verilip verilmeyeceğini seçin. Güncelleştirmeleri VM yeniden başlatma gerektirmez ve çoğaltma etkilemez. [Daha fazla bilgi edinin](azure-to-azure-autoupdate.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Olağanüstü Durum Kurtarma kullanarak sanal makineler için accelerated networking

Hızlandırılmış ağ, ağ performansını iyileştirme, bir sanal makineye tek kökte g/ç sanallaştırma (SR-IOV) etkinleştir. Bir Azure sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery, hızlandırılmış ağ etkin olup olmadığını algılar. Site Recovery otomatik olarak yapılandırır yük devretme hedef çoğaltma Azure VM, her ikisi için accelerated networking sonra bu doğruysa [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) ve [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Daha fazla bilgi edinin](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma için fiyatlandırma hesaplayıcı

Azure Vm'leri için olağanüstü durum kurtarma sanal makine lisanslama maliyetleri ve ağ ve depolama maliyetleri doğurur. Azure sağlayan bir [fiyatlandırma hesaplayıcısını](https://aka.ms/a2a-cost-estimator) yardımcı olmak için bu maliyetleri şekil. Site Recovery artık sağlar bir [tahmin fiyatlandırma örneği](https://aka.ms/a2a-cost-estimator) altı VM'den 12 standart HDD diskleri ve 6 Premium SSD diskleri ile kullanarak bir üç katmanlı uygulama göre bir örnek dağıtım fiyatlar. Örnek, bir gün için standart ve premium için 20 GB / 10 GB veri değişikliği varsayar. Belirli dağıtımınız için maliyetlerini tahmin etmek istediğiniz değişkenleri değiştirebilirsiniz. Beklenen toplam veri değişim oranı VM'ler arasında beklenen ve VM'lerin ve yönetilen disk sayısını belirtebilirsiniz. Buna ek olarak, bant genişliği maliyetlerini tahmin etmek için bir sıkıştırma faktörü uygulayabilirsiniz. [Okuma](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) duyuruyu.



## <a name="q3-2018"></a>2018 Ç3 


### <a name="update-rollup-28-august-2018"></a>Güncelleştirme Paketi (Ağustos 2018) 28

[Güncelleştirme paketi 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Linux için olağanüstü durum kurtarma** | **Azure sanal makineleri**: RedHat Enterprise Linux 6.10 için desteklenen eklendi; CentOS 6.10.<br/><br/> **VMware Vm'lerini**: RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> Linux tabanlı VM'ler GUID bölümleme tablosu (GPT) bölüm stilini eski BIOS uyumluluk modunda kullanmak, artık desteklenmektedir. Bkz: [Azure Iaas VM diskleri hakkında sık sorulan sorular](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks) daha fazla bilgi için. 
**Bulut desteği** | Almanya Azure Vm'leri için olağanüstü durum kurtarma desteklenir.
**Çapraz abonelik olağanüstü durum kurtarma** | Başka bir bölgede aynı Azure Active Directory kiracısı içinde farklı bir abonelikte bir bölgedeki Azure Vm'lerini çoğaltma desteği. [Daha fazla bilgi edinin](https://aka.ms/cross-sub-blog).
**Windows Server 2008** | Geçiş desteği, çalışan Windows Server 2008 R2/2008 64 bit ve 32-bit makineleri.<br/><br/> Geçiş yalnızca (çoğaltma ve yük devretme). Yeniden çalışma desteklenmez.

### <a name="update-rollup-27-july-2018"></a>Güncelleştirme Paketi 27 (Temmuz 2018)

[Güncelleştirme Paketi 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Linux için olağanüstü durum kurtarma** | **Azure sanal makineleri**: Red Hat Enterprise Linux 7.5<br/><br/> **VMware Vm'lerini/fiziksel sunucuları**: Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12




## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirmeler hakkında güncel [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=site-recovery) sayfası.




 









