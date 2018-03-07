---
title: "Azure Site Recovery ile şirket içi makineleri Azure’a geçirme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery kullanarak şirket içi makineleri Azure’a geçirme işlemi açıklanmaktadır."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 656ba02401d9ba610d0ebe33a683164af0b871f0
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
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
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya Hyper-V sunucularını hazırlayın.

Başlamadan önce olağanüstü durum kurtarma için [VMware](vmware-azure-architecture.md) veya [Hyper-V](hyper-v-azure-architecture.md) mimarilerini gözden geçirmeniz yararlı olabilir.


## <a name="prerequisites"></a>Ön koşullar

Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup ve Site Recovery** öğesine tıklayın.
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
    - **VMware**: **Azure’a** > **Evet, VMWare vSphere Hiper Yöneticisi ile** öğelerini seçin.
    - **Fiziksel makine**: **Azure’a** > **Sanallaştırılmamış/Diğer** öğelerini seçin.
    - **Hyper-V**: **Azure’a** > **Evet, Hyper-V ile** öğelerini seçin. Hyper-V VM’leri VMM tarafından yönetiliyorsa, **Evet**’i seçin.


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
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı dener. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
5. Azure VM’nin Azure’da beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.
6. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Geçişi Tamamla**’ya tıklayın. Böylece geçiş işlemi tamamlanır, VM için çoğaltma durdurulur ve sanal makine için Site Recovery faturalaması durdurulur.

    ![Geçişi tamamlama](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekebilir. Fiziksel sunucularda, VMware Linux makinelerinde, DHCP hizmeti etkinleştirilmemiş VMware VM’lerinde ve storvsc, vmbus, storflt, intelide, atapi önyükleme sürücülerine sahip olmayan VMware VM’lerinde uzun yük devretme sınama süreleriyle karşılaşabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şirket içi VM’leri Azure VM’lerine taşıdınız. Şimdi, Azure VM’leri için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> Bir şirket içi siteden geçiş yaptıktan sonra, Azure VM’leri için [olağanüstü durum kurtarmayı ayarlayın](azure-to-azure-replicate-after-migration.md).
