---
title: Azure Site Recovery ile şirket içi makineleri Azure’a geçirme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak şirket içi makineleri Azure’a geçirme işlemi açıklanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 31d08c0dac63662568bf55a021e85ec414c61e52
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58360376"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’a geçirme

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) amacıyla şirket içi makinelere ve sanal makinelere yönelik olağanüstü durum kurtarmayı yönetmek ve düzenlemek için [Azure Site Recovery](site-recovery-overview.md) hizmetini kullanmanın yanı sıra, şirket içi makinelerin Azure’a geçişini yönetmek için de Site Recovery’yi kullanabilirsiniz.


Bu öğreticide, şirket içi VM’ler ve fiziksel sunucuları Azure’a geçirme gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma hedefi seçme
> * Kaynak ve hedef ortamı ayarlama
> * Çoğaltma ilkesi ayarlama
> * Çoğaltmayı etkinleştirme
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir geçiş testi çalıştırma
> * Azure’a bir defalık yük devretme çalıştırma

Bu, serideki üçüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) sunucularını hazırlayın.

Başlamadan önce olağanüstü durum kurtarma için [VMware](vmware-azure-architecture.md) veya [Hyper-V](hyper-v-azure-architecture.md) mimarilerini gözden geçirmeniz yararlı olabilir.

> [!TIP]
> Yeni aracısız deneyimimizi azure'a geçirme VMware Vm'leri için katılmak istiyorsunuz? [Daha fazla bilgi edinin](https://aka.ms/migrateVMs-signup).

## <a name="prerequisites"></a>Önkoşullar

Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. Tıklayın **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
3. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Panodan kasaya hızlıca erişmek için önce **Panoya sabitle** seçeneğine ve sonra **Oluştur**’a tıklayın.

   ![Yeni kasa](./media/migrate-tutorial-on-premises-azure/onprem-to-azure-vault.png)

Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.


## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.
1. **Kurtarma Hizmetleri kasaları** > kasa öğesine tıklayın.
2. Kaynak Menüsünde, **Site Recovery** > **Altyapıyı Hazırlama** > **Koruma hedefi** seçeneklerine tıklayın.
3. **Koruma hedefi**’nde, geçişini yapmak istediğiniz öğeyi seçin.
    - **VMware**: Seçin **Azure'a** > **Evet, VMWare vSphere Hypervisor ile**.
    - **Fiziksel makine**: Seçin **Azure'a** > **sanallaştırılmamış/diğer**.
    - **Hyper-V**: Seçin **Azure'a** > **Evet, Hyper-V ile**. Hyper-V VM’leri VMM tarafından yönetiliyorsa, **Evet**’i seçin.


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

- VMware VM’leri için kaynak ortamı [ayarlayın](vmware-azure-tutorial.md#set-up-the-source-environment).
- Fiziksel sunucular için kaynak ortamı [ayarlayın](physical-azure-disaster-recovery.md#set-up-the-source-environment).
- Hyper-V VM’leri için kaynak ortamı [ayarlayın](hyper-v-azure-tutorial.md#set-up-the-source-environment).

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Kaynak Yöneticisi dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

- VMware VM’leri için [çoğaltma ilkesi ayarlayın](vmware-azure-tutorial.md#create-a-replication-policy).
- Fiziksel VM’ler için [çoğaltma ilkesi ayarlayın](physical-azure-disaster-recovery.md#create-a-replication-policy).
- Hyper-V VM’leri için [çoğaltma ilkesi ayarlayın](hyper-v-azure-tutorial.md#set-up-a-replication-policy).


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

- VMware VM’leri için [çoğaltmayı etkinleştirin](vmware-azure-tutorial.md#enable-replication).
- Fiziksel sunucular için [çoğaltmayı etkinleştirin](physical-azure-disaster-recovery.md#enable-replication).
- Hyper-V VM’leri için VMM [ile](hyper-v-vmm-azure-tutorial.md#enable-replication) veya VMM [olmadan](hyper-v-azure-tutorial.md#enable-replication) çoğaltmayı etkinleştirin.


## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma

Her şeyin beklendiği gibi çalıştığından emin olmak için bir Azure’a [yük devretme testi](tutorial-dr-drill-azure.md) çalıştırın.


## <a name="migrate-to-azure"></a>Azure’a geçiş

Geçirmek istediğiniz makineler için yük devretmeyi çalıştırın.

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde makine > **Yük devretme**’ye tıklayın.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. En son kurtarma noktasını seçin.
3. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery yük devretmeyi tetiklemeden önce sanal makineleri kapatmayı dener. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
5. Azure VM’nin Azure’da beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.
6. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Geçişi Tamamla**’ya tıklayın. Bu, şunları yapar:

   - Geçiş işlemi tamamlanır, şirket içi VM için çoğaltma durdurulur ve sanal makine için Site Recovery Faturalaması durdurulur.
   - Bu adım, çoğaltma verilerini temizler. Bu, geçirilen sanal makinelerin silmez.

     ![Geçişi tamamlama](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Devam eden bir yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekebilir. Fiziksel sunucularda, VMware Linux makinelerinde, DHCP hizmeti etkinleştirilmemiş VMware VM’lerinde ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücülerine sahip olmayan VMware VM’lerinde uzun yük devretme sınama süreleriyle karşılaşabilirsiniz.

## <a name="after-migration"></a>Geçişten sonra

Makineler Azure’a geçirildikten sonra, tamamlamanız gereken birkaç adım vardır.

Bazı adımlar, [kurtarma planlarındaki](site-recovery-runbook-automation.md) yerleşik otomasyon betikleri kullanılarak geçiş işleminin parçası olarak otomatikleştirilebilir.   


### <a name="post-migration-steps-in-azure"></a>Azure’da geçiş sonrası adımlar

- Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin. 
- Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
- [Azure VM aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows), Azure Yapı Denetleyicisi ile sanal makine etkileşimini yönetir. Azure Backup, Site Recovery ve Azure Güvenliği gibi bazı Azure hizmetleri için gereklidir.
    - VMware makinelerini ve fiziksel sunucuları geçiriyorsanız Mobility Hizmeti yükleyicisi, Windows makinelere kullanılabilir Azure sanal makine aracısını yükler. Linux sanal makineleri üzerinde, yük devretmeden sonra aracıyı yüklemeniz önerilir.
    - Azure sanal makinelerini ikincil bölgeye geçiriyorsanız Azure sanal makine aracısı, geçişten önce sanal makinede sağlanmalıdır.
    - Hyper-V sanal makinelerini Azure’a geçiriyorsanız, geçişten sonra Azure sanal makinesine Azure sanal makine aracısını yükleyin.
- Sanal makineden Site Recovery sağlayıcısını/aracısını kendiniz kaldırın. VMware Vm'lerini veya fiziksel sunucuları geçirirseniz, Mobility hizmetini sanal makineden kaldırın.
- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin]( https://docs.microsoft.com/azure/backup/quick-backup-vm-portal).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](azure-to-azure-quickstart.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi [Anlık yönetim]( https://docs.microsoft.com/azure/security-center/security-center-just-in-time) özelliği ile gelen trafik erişimini kilitleme ve sınırlama
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama]( https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/ ) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/ )’ni ziyaret edin.
- İzleme ve yönetim için:
    - [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.

### <a name="post-migration-steps-on-premises"></a>Şirket içi geçiş sonrası adımlar

- Uygulama trafiğini, geçirilen Azure sanal makinesi örneğinde çalıştırılan uygulamaya taşıyın.
- Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
- Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
- Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şirket içi VM’leri Azure VM’lerine taşıdınız. Şimdi Azure sanal makineleri için ikincil bir Azure bölgesine [olağanüstü durum kurtarmayı ayarlayabilirsiniz](azure-to-azure-replicate-after-migration.md).

  
