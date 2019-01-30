---
title: Azure Site Recovery'de yenilikler | Microsoft Docs
description: Azure Site Recovery'de sunulan yeni özellikleri bir özetini sunar
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: raynew
ms.openlocfilehash: 61e66a19b625141c69a9473373d3d5d808e18fde
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211126"
---
# <a name="whats-new-in-site-recovery"></a>Site Recovery'deki yenilikler

[Azure Site Recovery](site-recovery-overview.md) hizmeti güncelleştirildi ve sürekli olarak Gelişmiş. Yeniliklerden haberdar olun yardımcı olmak için bu makalede, en son sürümlerde, yeni özellikler ve yeni içerik hakkında bilgi sağlar. Bu sayfayı düzenli olarak güncelleştirilir.

Site Recovery özellikleri için önerileriniz varsa, öğrenmek isteriz [Geri bildirimlerinizi](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>S1 2019

### <a name="linux-support"></a>Linux desteği

[Güncelleştirme paketi 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) Site Recovery aracıları ve sağlayıcıları için bir güncelleştirme sağlar. Güncelleştirmeleri Linux desteği şu şekilde ekler:

- **Azure Vm'leri olağanüstü durumdan kurtarma**: RedHat iş istasyonu 7/6; Ubuntu, Debian ve SUSE yeni çekirdek sürümleri.
- **VMware Vm'lerini/fiziksel sunucuları azure'a olağanüstü durum kurtarma**: Red Hat Enterprise Linux 7.6; RedHat iş istasyonu 7/6; Oracle Linux 6.10/7.6 yeni çekirdek sürümleri Ubuntu, Debian ve SUSE.



## <a name="q4-2018"></a>S4 2018

## <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Azure VM'LERİNDE olağanüstü durum kurtarma için fiyatlandırma hesaplayıcı

Azure Vm'leri için olağanüstü durum kurtarma sanal makine lisanslama maliyetleri ve ağ ve depolama maliyetleri doğurur. Azure sağlayan bir [fiyatlandırma hesaplayıcısını](https://aka.ms/a2a-cost-estimator) yardımcı olmak için bu maliyetleri şekil. Site Recovery artık sağlar bir [tahmin fiyatlandırma örneği](https://aka.ms/a2a-cost-estimator) altı VM'den 12 standart HDD diskleri ve 6 Premium SSD diskleri ile kullanarak bir üç katmanlı uygulama göre bir örnek dağıtım fiyatlar. Örnek, bir gün için standart ve premium için 20 GB / 10 GB veri değişikliği varsayar. Belirli dağıtımınız için maliyetlerini tahmin etmek istediğiniz değişkenleri değiştirebilirsiniz. Beklenen toplam veri değişim oranı VM'ler arasında beklenen ve VM'lerin ve yönetilen disk sayısını belirtebilirsiniz. Buna ek olarak, bant genişliği maliyetlerini tahmin etmek için bir sıkıştırma faktörü uygulayabilirsiniz. [Okuma](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) duyuruyu.

### <a name="support-for-azure-vms-in-zones"></a>Bölgelerdeki Azure Vm'leri için destek

Azure kullanılabilirlik alanları, bir Azure bölgesi içinde benzersiz fiziksel konumları şunlardır. Her bölge, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Artık, bir Azure VM için çoğaltmayı etkinleştirin ve yük devretme hedefi tek bir VM örneği, bir sanal makine bir kullanılabilirlik kümesinde veya bir kullanılabilirlik alanında VM ayarlayın. Ayar çoğaltmasını etkilemez. [Okuma](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) duyuruyu.
 
### <a name="disaster-recovery-for-encrypted-vms"></a>Şifrelenmiş Vm'leri için olağanüstü durum kurtarma

Site Recovery, Azure ile Azure AD uygulamasını Azure Disk şifrelemesi (ADE) ile şifrelenmiş VM'ler destekler. [Daha fazla bilgi edinin](azure-to-azure-how-to-enable-replication-ade-vms.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Olağanüstü Durum Kurtarma kullanarak sanal makineler için accelerated networking

Hızlandırılmış ağ, ağ performansını iyileştirme, bir sanal makineye tek kökte g/ç sanallaştırma (SR-IOV) etkinleştir. Bir Azure sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery, hızlandırılmış ağ etkin olup olmadığını algılar. Site Recovery otomatik olarak yapılandırır yük devretme hedef çoğaltma Azure VM, her ikisi için accelerated networking sonra bu doğruysa [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) ve [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Daha fazla bilgi edinin](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Mobility hizmeti uzantısının otomatik güncelleştirmeler

Site Recovery Mobility hizmeti uzantısı için otomatik güncelleştirmeler için bir seçenek eklendi. Mobility hizmeti uzantısı, Azure Site Recovery tarafından çoğaltılmış her VM'de yüklenir. Çoğaltmayı etkinleştirdiğinizde Site Recovery, uzantı için güncelleştirmeleri yönetmek izin verilip verilmeyeceğini seçin. Güncelleştirmeleri VM yeniden başlatma gerektirmez ve çoğaltma etkilemez. [Daha fazla bilgi edinin](azure-to-azure-autoupdate.md).

### <a name="support-for-standard-ssd-disks"></a>Standart SSD disk desteği

Kullanıma sunulan azure [standart katı hal sürücüleri (SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd) tutarlı bir performans gerekir, ancak yüksek yoksa web sunucularının IOPS disk gibi uygulamalar için ekonomik depolama çözümü sağlamak amacıyla disklerin. Bunlar, premium SSD ve HDD standart diskler öğeleri birleştirin. Site Recovery, standart SSD disk kullanarak, Azure Vm'leri için olağanüstü durum kurtarma sağlar. Varsayılan olarak, disk türü, hedef bölgeye yük devretme sonrasında korunur.

### <a name="support-for-azure-storage-firewall"></a>Azure depolama güvenlik duvarı desteği

Hesabı için güvenlik duvarı kuralları açarak ağları belirli bir dizi Azure depolama hesaplarına güvenli hale getirebilirsiniz. Varsayılan olarak iç ağlara ve internet trafiği reddetmeye yönelik depolama hesaplarını yapılandırın ve ardından erişim akışına ait özel sanal ağlar. Site kurtarma, ikincil bir bölgeye Güvenlik Duvarı etkin depolama hesaplarındaki yönetilmeyen disklere sahip VM'ler için çoğaltmayı destekler. Yönetilmeyen diskler için hedef bölgede depolama hesapları ile Güvenlik Duvarı etkin seçebilirsiniz. Kaynak VM'lerin olduğu bulunduğu ağ için ağ erişimini kısıtlayarak önbellek depolama hesabına erişimi de kısıtlayabilirsiniz. Şunları yapmanız gerektiğini Not [erişime izin ver](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) güvenilen Microsoft Hizmetleri için.

## <a name="q3-2018"></a>2018 Ç3 

### <a name="linux-support"></a>Linux desteği

#### <a name="update-rollup-28"></a>Güncelleştirme paketi 28

[Güncelleştirme paketi 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) Site Recovery aracıları ve sağlayıcıları için bir güncelleştirme sağlar. Güncelleştirmeleri Linux desteği şu şekilde ekler:

- **Azure Vm'leri olağanüstü durumdan kurtarma**: Red Hat Enterprise Linux 6.10; CentOS 6.10; Linux tabanlı VM'ler GUID bölümleme tablosu (GPT) bölüm stilini eski BIOS uyumluluk modunda kullanmak, artık desteklenmektedir.
- **VMware Vm'lerini/fiziksel sunucuları azure'a olağanüstü durum kurtarma**: Red Hat Enterprise Linux 6.10; CentOS 6.10; Linux tabanlı VM'ler GUID bölümleme tablosu (GPT) bölüm stilini eski BIOS uyumluluk modunda kullanmak, artık desteklenmektedir.

#### <a name="update-rollup-27"></a>Güncelleştirme Paketi 27

[Güncelleştirme paketi 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) Site Recovery aracıları ve sağlayıcıları için bir güncelleştirme sağlar. Güncelleştirmeleri Linux desteği şu şekilde ekler:

- **Azure Vm'leri olağanüstü durumdan kurtarma**: Red Hat Enterprise Linux 7.5
- **VMware Vm'lerini/fiziksel sunucuları azure'a olağanüstü durum kurtarma**: SUSE Linux Enterprise Server 12, Red Hat Enterprise Linux 7.5

### <a name="support-for-azure-vms-running-on-windows-server-2016"></a>Azure Windows Server 2016 çalıştıran VM'ler için destek

Windows Server 2016 çalıştıran azure sanal makineler, Azure Site Recovery ile Azure bölgeleri arasında çoğaltılabilir.

### <a name="support-for-azure-vms-running-storage-spaces-direct"></a>Azure depolama alanları doğrudan'ı çalıştıran VM'ler için destek

[Depolama alanları doğrudan](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) (Windows Server 2016'dan başlayarak kullanılabilir) sürücüleri bir depolama havuzunda bir araya getirir ve sonra depolama alanları oluşturmak için havuzdan kapasite kullanır. Depolama alanları kullanılabilir tek başına bir VM üzerinde bir [Azure VM Konuk kümesi](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) her küme düğümünde yerel depolama veya paylaşılan depolama kümesi kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirmeler hakkında güncel [Azure güncelleştirmeleri](https://azure.microsoft.com/updates/?product=site-recovery) sayfası.


