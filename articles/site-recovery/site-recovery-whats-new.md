---
title: Azure Site Recovery'de yenilikler | Microsoft Docs
description: Azure Site Recovery'de sunulan yeni özellikleri bir özetini sunar
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: raynew
ms.openlocfilehash: e2145fbbb5fa09aa3321742ca8a786822f6f0641
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148657"
---
# <a name="whats-new-in-site-recovery"></a>Site Recovery'deki yenilikler

[Azure Site Recovery](site-recovery-overview.md) hizmeti güncelleştirildi ve sürekli olarak Gelişmiş. Yeniliklerden haberdar olun yardımcı olmak için bu makalede, en son sürümlerde, yeni özellikler ve yeni içerik hakkında bilgi sağlar. Bu sayfayı düzenli olarak güncelleştirilir.

Site Recovery özellikleri için önerileriniz varsa, öğrenmek isteriz [Geri bildirimlerinizi](https://feedback.azure.com/forums/256299-site-recovery).


## <a name="updates-march-2019"></a>Güncelleştirmeler (Mart 2019)

### <a name="update-rollup-35"></a>Güncelleştirme paketi 35

[Güncelleştirme paketi 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde) için bir güncelleştirme
**Sorun düzeltmeleri/iyileştirmeleri** | Bir dizi düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde)

#### <a name="vmwarephysical-server-disaster-recovery"></a>VMware/fiziksel sunucu olağanüstü durum kurtarma
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Yönetilen diskler** | Azure yönetilen diskler için doğrudan şirket içi VMware Vm'leri ve fiziksel sunucuların çoğaltma sunulmuştur. Azure önbellek depolama hesabına gönderilen veri ve kurtarma noktaları, yönetilen diskler hedef konumda oluşturulur şirket içi. Bu, birden fazla hedef depolama hesabı yönetmeniz gerekmez sağlar.
**Yapılandırma sunucusu** | Site Recovery artık birden çok NIC içeren bir yapılandırma sunucuları destekler. Yapılandırma sunucusunu kasaya kaydetmeden önce VM yapılandırma sunucusuna ek bağdaştırıcılar eklemeniz gerekir. Sonradan eklerseniz sunucu kasaya yeniden kaydetmeniz gerekir.


## <a name="updates-february-2019"></a>Güncelleştirmeler (Şubat 2019)

### <a name="update-rollup-34"></a>Güncelleştirme paketi 34 

[Güncelleştirme paketi 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.


### <a name="update-rollup-33"></a>Güncelleştirme paketi 33 

[Güncelleştirme paketi 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.


#### <a name="azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma 
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Ağ eşlemesi** | Azure VM'LERİNDE olağanüstü durum kurtarma için çoğaltmayı etkinleştirdiğinizde herhangi bir kullanılabilir hedef ağ artık kullanabilirsiniz. 
**Standart SSD** | Kullanarak Azure Vm'leri için olağanüstü durum kurtarma artık ayarlayabilirsiniz [standart SSD disk](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd).
**Depolama alanları doğrudan** | Kullanarak Azure VM uygulamalar üzerinde çalışan uygulamalar için olağanüstü durum kurtarma ayarlayabilirsiniz [depolama alanları doğrudan](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) yüksek kullanılabilirlik için.  Depolama alanları doğrudan (S2D) Site Recovery ile birlikte kullanarak Azure VM iş yüklerini kapsamlı korumasını sağlar. S2d'yi bir konuk küme azure'da barındırmanıza olanak tanır. SAP ASCS katmanı, SQL Server veya genişleme dosya sunucusu gibi kritik bir uygulamayı barındıran bir VM olduğunda bu kullanışlıdır.


#### <a name="vmwarephysical-server-disaster-recovery"></a>VMware/fiziksel sunucu olağanüstü durum kurtarma
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux BRTFS dosya sistemi** | Site Recovery, VMware sanal makinelerinin çoğaltma BRTFS dosya sistemiyle artık destekler. Çoğaltma durumunda desteklenmez:<br/><br/>-Çoğaltmayı etkinleştirdikten sonra BTRFS dosya sistemi alt birimi değiştirilir.<br/><br/>-Dosya sistemi birden çok disk yayılır.<br/><br/>-RAID BTRFS dosya sistemini destekler.
**Windows Server 2019** | Windows Server 2019 çalışan makineler için eklenen destek.


## <a name="updates-january-2019"></a>Güncelleştirmeler (Ocak 2019)

### <a name="accelerated-networking-azure-vms"></a>Hızlandırılmış ağ (Azure Vm'leri)

Hızlandırılmış ağ, ağ performansını iyileştirme, bir sanal makineye tek kökte g/ç sanallaştırma (SR-IOV) etkinleştir. Bir Azure sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery, hızlandırılmış ağ etkin olup olmadığını algılar. Site Recovery otomatik olarak yapılandırır yük devretme hedef çoğaltma Azure VM, her ikisi için accelerated networking sonra bu doğruysa [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) ve [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms).

[Daha fazla bilgi edinin](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="update-rollup-32"></a>Güncelleştirme paketi 32 

[Güncelleştirme paketi 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.

#### <a name="azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma

Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | Destek, Ubuntu, Debian ve SUSE RedHat iş istasyonu 7/6 ve yeni çekirdek sürümleri eklendi.
**Depolama alanları doğrudan** | Site Recovery, depolama alanları doğrudan (S2D) kullanarak Azure Vm'leri destekler.

#### <a name="vmware-vmsphysical-servers-replication"></a>VMware Vm'lerini/fiziksel sunucuları çoğaltma 
**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | Redhat Enterprise Linux 7.6, RedHat iş istasyonu 6 7, Oracle Linux 6.10/7.6 ve yeni kernel sürümleri için destek Ubuntu, Debian ve SUSE eklendi.


### <a name="update-rollup-31"></a>Güncelleştirme Paketi 31 

[Güncelleştirme Paketi 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.

#### <a name="vmware-vmsphysical-servers-replication"></a>VMware Vm'lerini/fiziksel sunucuları çoğaltma 
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | UEK5 çekirdek ve Oracle Linux 6,8 6.9/7.0 için destek eklendi.
**LVM'Yİ** | LVM'yi ve LVM2 birimleri için eklenen destek.<br/><br/> Disk bölümü ve LVM birimlerde makinesiyse dizin artık desteklenmektedir.
**Dizinler** | Bu dizinler ayrı bölümlerde ya da aynı sistem diskinde değil dosya sistemleri olarak ayarlamak için destek eklendi:<br/><br/> / (root), makinesiyse, / usr, / usr/local, /var, / etc.
**Windows Server 2008** | Dinamik diskler için eklenen destek.
**Yük devretme** | Burada storvsc ve vsbus önyükleme sürücüleri olmayan VMware Vm'leri için yük devretme süresi geliştirildi.
**UEFI desteği** | Azure sanal makineler önyükleme türü UEFI desteklemez. Şimdi, UEFI ile şirket içi fiziksel sunucuları Azure Site Recovery ile geçirebilirsiniz. Site kurtarma, sunucunun geçişten önce için BIOS önyükleme türü dönüştürerek geçirir. Site Recovery daha önce bu dönüştürme VM'ler için yalnızca desteklenen. Windows Server 2012 çalıştıran fiziksel sunucular için veya üzeri destek sunulur.

#### <a name="azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | Desteklenen Oracle Linux 6,8 ve 6.9/7.0; eklendi ve UEK5 çekirdek için.
**Linux BRTFS dosya sistemi** | Azure sanal makineler için desteklenir.
**Kullanılabilirlik alanları, Azure Vm'leri** | Başka bir bölgeye çoğaltma kullanılabilirlik alanında dağıtılan Azure VM'ler için etkinleştirebilirsiniz. Artık, bir Azure VM için çoğaltmayı etkinleştirin ve yük devretme hedefi tek bir VM örneği, bir sanal makine bir kullanılabilirlik kümesinde veya bir kullanılabilirlik alanında VM ayarlayın. Ayar çoğaltmasını etkilemez. [Okuma](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) duyuruyu.
**Güvenlik Duvarı etkin depolama (portal/PowerShell)** | İçin eklenen Destek [Güvenlik Duvarı etkin depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Yönetilmeyen diskler başka bir Azure bölgesine olağanüstü durum kurtarma için Güvenlik Duvarı etkin depolama hesapları ile Azure Vm'lerini çoğaltabilirsiniz.<br/><br/> Yönetilmeyen diskler için hedef depolama hesapları, Güvenlik Duvarı etkin depolama hesapları kullanabilirsiniz.<br/><br/> Desteklenen portal ve PowerShell kullanarak.

## <a name="updates-december-2018"></a>Güncelleştirmeler (aralık 2018)

### <a name="automatic-updates-for-the-mobility-service-azure-vms"></a>Mobility hizmetini (Azure VM) için Otomatik Güncelleştirmeler

Site Recovery Mobility hizmeti uzantısı için otomatik güncelleştirmeler için bir seçenek eklendi. Mobility hizmeti uzantısı, Azure Site Recovery tarafından çoğaltılmış her VM'de yüklenir. Çoğaltmayı etkinleştirdiğinizde Site Recovery, uzantı için güncelleştirmeleri yönetmek izin verilip verilmeyeceğini seçin.

Güncelleştirmeleri VM yeniden başlatma gerektirmez ve çoğaltma etkilemez. [Daha fazla bilgi edinin](azure-to-azure-autoupdate.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma için fiyatlandırma hesaplayıcı

Olağanüstü durum kurtarma Azure VM, sanal makine lisanslama maliyetleri ve ağ ve depolama maliyetleri doğurur. Azure sağlayan bir [fiyatlandırma hesaplayıcısını](https://aka.ms/a2a-cost-estimator) yardımcı olmak için bu maliyetleri şekil. Site Recovery artık sağlar bir [tahmin fiyatlandırma örneği](https://aka.ms/a2a-cost-estimator) altı VM'den 12 standart HDD diskleri ve 6 Premium SSD diskleri ile kullanarak bir üç katmanlı uygulama göre bir örnek dağıtım fiyatlar.

- Örnek, bir gün için standart ve premium için 20 GB / 10 GB veri değişikliği varsayılır.
- Belirli dağıtımınız için maliyetlerini tahmin etmek istediğiniz değişkenleri değiştirebilirsiniz.
- Beklenen toplam veri değişim oranı VM'ler arasında beklenen ve VM'lerin ve yönetilen disk sayısını belirtebilirsiniz.
- Buna ek olarak, bant genişliği maliyetlerini tahmin etmek için bir sıkıştırma faktörü uygulayabilirsiniz.

[Okuma](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) duyuruyu.


## <a name="updates-october-2018"></a>Güncelleştirmeler (Ekim 2018)

### <a name="update-rollup-30"></a>Güncelleştirme paketi 30 

[Güncelleştirme paketi 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.

#### <a name="azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Bölge desteği** | Avustralya Orta 1 ve Avustralya Orta 2 için eklenen Destek sitesi.
**Disk şifrelemesi desteği** | Azure AD uygulaması ile Azure Disk şifrelemesi (ADE) ile şifrelenmiş Azure Vm'leri olağanüstü durum kurtarma için eklenen destek. [Daha fazla bilgi edinin](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Disk dışlama** | Başlatılmamış bir disk artık Azure VM çoğaltma sırasında otomatik olarak dışlanır.
**Güvenlik Duvarı etkin depolama (PowerShell)** | İçin eklenen Destek [Güvenlik Duvarı etkin depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-network-security).<br/><br/> Yönetilmeyen diskler başka bir Azure bölgesine olağanüstü durum kurtarma için Güvenlik Duvarı etkin depolama hesapları ile Azure Vm'lerini çoğaltabilirsiniz.<br/><br/> Yönetilmeyen diskler için hedef depolama hesapları, Güvenlik Duvarı etkin depolama hesapları kullanabilirsiniz.<br/><br/> Yalnızca PowerShell kullanılarak desteklenir.


### <a name="update-rollup-29"></a>Güncelleştirme paketi 29 

[Güncelleştirme paketi 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.


## <a name="updates-august-2018"></a>Güncelleştirmeler (Ağustos 2018)

### <a name="update-rollup-28"></a>Güncelleştirme paketi 28 

[Güncelleştirme paketi 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.

#### <a name="azure-vms-disaster-recovery"></a>Azure Vm'leri olağanüstü durum kurtarma 
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | RedHat Enterprise Linux 6.10 için desteklenen eklendi; CentOS 6.10.<br/><br/>
**Bulut desteği** | Almanya Azure Vm'leri için olağanüstü durum kurtarma desteklenir.
**Çapraz abonelik olağanüstü durum kurtarma** | Başka bir bölgede aynı Azure Active Directory kiracısı içinde farklı bir abonelikte bir bölgedeki Azure Vm'lerini çoğaltma desteği. [Daha fazla bilgi edinin](https://aka.ms/cross-sub-blog).

#### <a name="vmware-vmphysical-server-disaster-recovery"></a>VMware VM'LERİNİ/fiziksel sunucuları olağanüstü durum kurtarma 
Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | CentOS 6.10 RedHat Enterprise Linux 6.10 için eklenen destek.<br/><br/> Linux tabanlı VM'ler GUID bölümleme tablosu (GPT) bölüm stilini eski BIOS uyumluluk modunda kullanmak, artık desteklenmektedir. Gözden geçirme [Azure VM ile ilgili SSS](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks) daha fazla bilgi için. 
**Geçişten sonra Vm'leri için olağanüstü durum kurtarma** | Destek bir şirket içi VMware VM için olağanüstü durum kurtarma, ikincil bir bölgeye etkinleştirme, Azure'a geçiş için Mobility kaldırmak zorunda kalmadan hizmeti VM üzerinde çoğaltmayı etkinleştirmeden önce.
**Windows Server 2008** | Geçiş desteği, çalışan Windows Server 2008 R2/2008 64 bit ve 32-bit makineleri.<br/><br/> Geçiş yalnızca (çoğaltma ve yük devretme). Yeniden çalışma desteklenmez.

## <a name="updates-july-2018"></a>Güncelleştirmeler (Temmuz 2018)

### <a name="update-rollup-27-july-2018"></a>Güncelleştirme Paketi 27 (Temmuz 2018)

[Güncelleştirme Paketi 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) aşağıdaki güncelleştirmeler sağlar.

**Güncelleştirme** | **Ayrıntılar**
--- | ---
**Sağlayıcılar ve aracılar** | Bir güncelleştirme Site Recovery aracıları ve sağlayıcıları (toplama'nda açıklandığı şekilde).
**Sorun düzeltmeleri/iyileştirmeleri** | Düzeltmeleri ve geliştirmeleri (toplama'nda açıklandığı şekilde) sayısı.

#### <a name="azure-vms-disaster-recovery"></a>Azure Vm'leri olağanüstü durum kurtarma 

Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | Red Hat Enterprise Linux 7.5 için eklenen destek.

#### <a name="vmware-vmphysical-server-disaster-recovery"></a>VMware VM'LERİNİ/fiziksel sunucuları olağanüstü durum kurtarma 

Güncelleştirmede eklenen yeni özellikler.

**Özellik** | **Ayrıntılar**
--- | ---
**Linux desteği** | Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12 eklenen desteği.



## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirmeler hakkında güncel [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=site-recovery) sayfası.
