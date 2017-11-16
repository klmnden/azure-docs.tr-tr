---
title: "Site Recovery kasası bir Azure kurtarma Hizmetleri Kasası'na yükseltme"
description: "Bir kurtarma Hizmetleri Kasası'na bir Azure Site Recovery kasası yükseltmeyi öğrenin"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/15/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: aa4b82fdf50eca382c4c84610a5f4857106c6d6d
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a>Site Recovery kasası bir Azure Resource Manager tabanlı kurtarma Hizmetleri Kasası'na yükseltme

Bu makalede, Azure Site Recovery kasalarını Azure Resource Manager tabanlı kurtarma hizmeti kasalarını devam eden çoğaltma üzerinde hiçbir etkisi olmadan yükseltme açıklanır. Azure Kaynak Yöneticisi özellikleri ve avantajları hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Giriş
Kurtarma Hizmetleri kasası, yedekleme ve olağanüstü durum kurtarma bulut yerel olarak yönetmek için bir Azure Resource Manager kaynaktır. Yeni Azure portalında kullanabilirsiniz birleşik bir kasa olduğu ve klasik yedekleme değiştirir ve Site Recovery kasaları.

Kurtarma Hizmetleri kasaları dahil olmak üzere özellikleri, bir dizi sunar:

* Azure Resource Manager desteği: korumak ve sanal makineleri ve fiziksel makineler üzerinden bir Azure Resource Manager yığına başarısız.

* Diski dışarıda: geçici dosyalar veya tüm bant kullanmak istemiyorsanız yüksek bir karmaşıklık veri varsa, birimleri çoğaltma dışında bırakabilirsiniz. Bu özellik şu anda etkin *Azure VMware* ve *Hyper-V Azure* ve diğer senaryolar için genişletilir.

* Premium ve yerel olarak yedekli depolama için destek: artık sunucuları koruyabilir premium depolama alanına uygulamalarla yüksek korumak müşteriler izin hesapları giriş/saniye başına işlemi (IOPS) çıkış. Bu özellik şu anda etkin *Azure VMware*.

* Hızlandırıldı başlama deneyimi: Gelişmiş başlama deneyimi olağanüstü durum kurtarma Kurulumu kolaylaştırmak için tasarlanmıştır.

* Yedekleme ve aynı kasaya Site Recovery yönetimden: şimdi olağanüstü durum kurtarma için sunucular koruyabilir veya yedekleme, yönetim yükünü önemli ölçüde azaltabilir aynı kasadan gerçekleştirin.

Yükseltilen deneyimi ve özellikleri hakkında daha fazla bilgi için bkz: [depolama, yedekleme ve kurtarma blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Dikkat çekici özellikleri

* Devam eden çoğaltma üzerindeki etkisi: devam eden çoğaltma sırasında herhangi bir kesintiye uğratmadan devam etmek ve yükseltme sonrası.

* Ek ücret ödemeden: güncelleştirilmiş özellikleri kümesinin tamamını hiçbir ek ücret ödemeden.

* Veri kaybı: Bu işlem, yükseltme ve bir geçiş olduğundan, var olan çoğaltma kurtarma noktalarının ve ayarları sırasında ve yükseltmeden sonra değişmeden kalır.


## <a name="what-happens-during-the-vault-upgrade"></a>Kasa yükseltme sırasında ne olur?

Yükseltme sırasında yeni bir sunucu kaydetme veya bir sanal makine (VM) için çoğaltma etkinleştirme gibi işlemleri gerçekleştiremezsiniz. Verileri okuma veya devam eden çoğaltma kasasına korumalı öğelerin gibi kasaya veri yazma işlemlerini kesintiye uğramadan devam edin.

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a>Otomasyon ve araçları yükseltme sonrasında yapılan değişiklikler
Kasa türü, Klasik dağıtım modelinden Resource Manager dağıtım modeline yükseltme gibi yükseltmeden sonra çalışmaya devam ettiğinden emin olmak için araç ve var olan Otomasyon güncelleştirin.

### <a name="prepare-your-environment-for-the-upgrade"></a>Yükseltme için ortamınızı hazırlama

* [PowerShell yükleyin veya 5 veya üzeri sürüme yükseltin](https://www.microsoft.com/download/details.aspx?id=50395)
* [Azure PowerShell MSI en son sürümünü yükleyin](https://github.com/Azure/azure-powershell/releases)
* [Kurtarma Hizmetleri kasası yükseltme betiğini indir](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Ön koşullar
Azure Resource Manager tabanlı kurtarma hizmeti kasalarını Site Recovery kasası yükseltmek için aşağıdaki gereksinimleri karşılamalıdır:

* En düşük aracı sürümü: sunucunuzda yüklü Azure Site Recovery sağlayıcısı sürümü 5.1.1700.0 olmalı veya sonraki bir sürümü.

* Desteklenen yapılandırma: depolama alanı ağı (SAN) veya SQL Server AlwaysOn Kullanılabilirlik grupları ile kasanızı yapılandıramazsınız. Tüm diğer yapılandırmalar desteklenmez.

    >[!NOTE]
    >Yükseltmeden sonra depolama eşlemesi yalnızca PowerShell aracılığıyla yönetebilirsiniz.

* Desteklenen dağıtım senaryosu: kasanızı olmamalıdır *Azure VMware* eski dağıtım modeli. Devam etmeden önce ilk gelişmiş dağıtım modeline taşıma.

* Yönetimi ile ilgili hiç etkin kullanıcı tarafından başlatılan iş düzlemine işlemleri: yükseltme sırasında yönetim düzeyi erişim Yasak olduğundan yükseltme tetiklemeden önce tüm yönetim düzlemi eylemleri yapın. Bu işlem, devam eden çoğaltmayı içermez.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Bu yükseltme my devam eden çoğaltma etkiliyor mu?**

Hayır. Devam eden çoğaltmayı kesintiye uğramadan sırasında ve yükseltme sonrasında devam eder.

**Siteden siteye VPN ve IP ayarlarını gibi ağ ayarları ne olur?**

Yükseltme ağ ayarlarını etkilemez. Tüm Azure-için-şirket içi bağlantılar sürdürülür.

**Yakın gelecekte yükseltmeyi planladığınız yok my kasalarını ne olur?**

Eski Azure portalında Site Recovery kasası desteği Eylül 2017 başlayarak kullanım dışı kalacaktır. Yeni portalına taşımak için yükseltme özelliğini kullanırsanız öneririz.

**Bu geçiş planı için varolan my araç anlamı nedir?**  

Resource Manager dağıtım modeli için araç güncelleştirmek yükseltme planlarınızı için hesap gereken en önemli değişikliklerden biri kullanılır. Kurtarma Hizmetleri kasalarının Resource Manager dağıtım modeline dayanır. 

**Yönetim düzeyi kapalı kalma süresinin ne kadar süreyle mu son?**

Yükseltmenin normalde yaklaşık 15 ila 30 dakika sürer ve maksimum bir saat kadar sürebilir.

**I yükselttikten sonra geri alabilirsiniz?**

Hayır. Kaynakları başarılı bir şekilde yükselttikten sonra geri alma desteklenmiyor.

**My abonelik ya da bunlar yükseltilebilir olup olmadığını görmek için kaynakları doğrulamak için?**

Evet. Platform tarafından desteklenen yükseltme, yükseltme ilk adımda kaynakları yükseltme kapasitesi olduğunu doğrulamak için seçenektir. Doğrulama başarısız olursa, ilgili hata iletisi veya uyarı alırsınız.

**Yükseltme ile ilgili bir sorunu nasıl bildirebilirim?**

Yükseltme sırasında hatalar karşılaşırsanız, hatayı listelenen işlem kimliği unutmayın. Microsoft Support sorunu çözümlemek için proaktif olarak çalışır. Ayrıca abonelik kimliği, kasa adını ve işlem kimliği ile birlikte Destek ekibine başvurun Destek, mümkün olan en kısa sürede sorunu çözmek için çalışır. Açıkça Bunu yapmak için Microsoft tarafından belirtilmedikçe işlemi yeniden denemeyin.

## <a name="run-the-script"></a>Komut dosyasını çalıştır

PowerShell'de aşağıdaki komutu çalıştırın:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* Subscriptionıd: yükseltme yapıyorsanız kasası ile ilişkili abonelik kimliği.

* VaultName: Yükseltme yapıyorsanız kasa adı.

* Konumu: Yükseltme yapıyorsanız kasası konumu.

* ResourceType: Site Recovery için HyperVRecoveryManagerVault kasaları.

* TargetResourceGroupName: kaynak grubu yerleştirilecek yükseltilmiş kasası istiyor. Azure Resource Manager veya yeni bir tane var olan bir kaynak grubunda TargetResourceGroupName olabilir. Sağlanan TargetResourceGroupName mevcut değilse, kasa ile aynı konumda yükseltmesinin bir parçası olarak oluşturulur. Daha fazla bilgi için "Kaynak grupları" bölümüne bakın [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Kaynak grubu adlandırma bazı kısıtlamalara tabi olur. Kasa yükseltme hatayı önlemek için adlandırma kuralını dikkatlice inceleyin emin olun.
    >
    >Örneğin:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - Subscriptionıd 1234-54123-354354-56416-8645 - VaultName gen2dr-konum "Kuzey Avrupa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc

Alternatif olarak, aşağıdaki komut dosyasını çalıştırabilirsiniz. Gerekli Parametreler için değer girin.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. PowerShell Betiği kimlik bilgilerinizi girmenizi ister. Bunları, iki kez Klasik dağıtım modeli hesabı için bir kez ve Azure Resource Manager hesabı için bir kez girin.

2. Kimlik bilgilerinizi girdikten sonra bir altyapı Kurulumu yukarıda açıklanan gereksinimlerini karşılayıp karşılamadığını belirlemek için bir onay komut dosyasını çalıştırır.

3. Önkoşulları işaretli ve onaylandıktan sonra kasa yükseltme işlemine devam istenir. Yükseltme işlemi kasanızı yükseltmeyi başlatır. Tüm yükseltme tamamlanması 15 ila 30 dakika sürebilir.

4. Yükseltme başarıyla tamamlandıktan sonra yükseltilen kasaya yeni Azure portalına erişebilir.

## <a name="post-upgrade-vault-management"></a>Yükseltme sonrası kasa yönetimi

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasına Azure Site Recovery kullanarak çoğaltma

* Artık Azure Vm'leriniz bir bölgesinden diğerine koruyabilirsiniz. Daha fazla bilgi için bkz: [Azure Site Recovery ile bölgeler arasında Azure sanal makineleri çoğaltmak](site-recovery-azure-to-azure.md).

* VMware Vm'lerini Azure'a çoğaltma hakkında daha fazla bilgi için bkz: [çoğaltmak VMware sanal makinelerini Site Recovery ile azure'a](vmware-walkthrough-overview.md).

* Hyper-V Vm'lerini (VMM olmadan) Azure'a çoğaltma hakkında daha fazla bilgi için bkz: [azure'a çoğaltmak Hyper-V sanal makineleri (VMM olmadan)](hyper-v-site-walkthrough-overview.md).

* Hyper-V Vm'lerini (VMM ile) Azure'a çoğaltma hakkında daha fazla bilgi için bkz: [Azure portalında Site Recovery kullanarak VMM bulutlarındaki Hyper-V çoğaltma sanal makineleri](vmm-to-azure-walkthrough-overview.md).

* Hyper-Vm'lerini (VMM ile) ikincil bir siteye çoğaltma hakkında daha fazla bilgi için bkz: [Azure portalını kullanarak bir ikincil VMM sitesi için VMM bulutlarındaki Hyper-V çoğaltma sanal makinelerini](site-recovery-vmm-to-vmm.md).

* VMware Vm'lerini ikincil bir siteye çoğaltma hakkında daha fazla bilgi için bkz: [Replicate şirket içi VMware sanal makineleri veya fiziksel sunucuları ikincil bir siteye Klasik Azure portalında](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>Çoğaltılmış öğelerinizi görüntüleme

Aşağıdaki resimde, kasa için anahtar varlıkları görüntüler kurtarma Hizmetleri kasası Pano sayfası gösterilir. Kasaya korumalı varlıkların listesini görüntülemek için seçin **Site Recovery** > **öğeleri çoğaltılan**.


![Çoğaltılan öğe](./media/upgrade-site-recovery-vaults/replicateditems.png)

Aşağıdaki resimde, çoğaltılmış öğeleri görüntülemek için iş akışı gösterilmiştir ve **yük devretme** bir yük devretmeyi başlatmadan için komutu.

![Çoğaltılan öğe](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Çoğaltma ayarlarınızı görüntüleyin

Site kurtarma kasasına her koruma grubu kopyalama sıklığı, kurtarma noktası bekletme, uygulama ile tutarlı anlık görüntü sıklığı ve diğer çoğaltma ayarları ile yapılandırılır. Kurtarma Hizmetleri kasasına bu ayarları bir çoğaltma ilkesi yapılandırılır. İlke adı koruma grubunun adıdır veya *primarycloud_Policy*.

Çoğaltma İlkesi hakkında daha fazla bilgi için bkz: [Azure VMware için çoğaltma İlkesi'ni yönetmek](site-recovery-setup-replication-settings-vmware.md).
