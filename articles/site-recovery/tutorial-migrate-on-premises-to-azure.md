---
title: "Azure Site Recovery ile azure'a şirket içi makineler geçirme | Microsoft Docs"
description: "Bu makalede, Azure Site Kurtarma'yı kullanarak Azure için şirket içi makineleri geçirmek açıklar."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ee9397406cbca21d8bd53019d9daac5a037f508c
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’a geçirme

Kullanmanın yanı sıra [Azure Site Recovery](site-recovery-overview.md) yönetmek ve şirket içi makineler ve Azure Vm'leri olağanüstü durum kurtarma iş devamlılığı ve olağanüstü durum kurtarma (BCDR) amaçları doğrultusunda düzenlemek için hizmet sitesini de kullanabilirsiniz Şirket içi makineleri azure'a geçişini yönetmek için kurtarma.


Bu öğretici, şirket içi sanal makineleri ve fiziksel sunucuları Azure'a geçirmek nasıl gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma hedefi seçin
> * Kaynak ve hedef ortamını ayarlama
> * Bir çoğaltma ilkesini ayarlayın
> * Çoğaltmayı etkinleştirme
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırma
> * Azure için tek seferlik bir yük devretmeyi çalıştırma

Bir dizi üçüncü öğreticide budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. Şirket içi hazırlama [VMware](tutorial-prepare-on-premises-vmware.md) veya Hyper-V sunucuları.

Başlamadan önce gözden geçirmek yararlı [VMware](concepts-vmware-to-azure-architecture.md) veya [Hyper-V](concepts-hyper-v-to-azure-architecture.md) mimari olağanüstü durum kurtarma için.


## <a name="prerequisites"></a>Önkoşullar

Paravirtualized sürücüleri tarafından dışarı aktarılan cihazlar desteklenmez.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Yeni** > **İzleme ve Yönetim** > **Backup ve Site Recovery** seçeneğine tıklayın.
3. İçinde **adı**, kolay ad belirtin **ContosoVMVault**. Birden fazla aboneliğiniz varsa, uygun olanı seçin.
4. Bir kaynak grubu oluşturmak **ContosoRG**.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Kasa panodan hızlı bir şekilde erişmek için tıklatın **panoya Sabitle** ve ardından **oluşturma**.

   ![Yeni kasa](./media/tutorial-migrate-on-premises-to-azure/onprem-to-azure-vault.png)

Yeni kasa eklenen **Pano** altında **tüm kaynakları**ve ana **kurtarma Hizmetleri kasaları** sayfası.



## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçin

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.
1. Tıklatın **kurtarma Hizmetleri kasaları** > kasası.
2. Kaynak menüye tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
3. İçinde **koruma hedefi**, geçirmek istediğiniz seçin.
    - **VMware**: seçin **için Azure** > **Evet, VMWare vSphere hiper yönetici ile**.
    - **Fiziksel makine**: seçin **için Azure** > **değil sanallaştırılmış/diğer**.
    - **Hyper-V**: seçin **Azure'a** > **Evet, Hyper-V ile**. Hyper-V sanal makineleri VMM tarafından yönetiliyorsa seçin **Evet**.


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

- [Ayarlanan](tutorial-vmware-to-azure.md#set-up-the-source-environment) VMware VM'ler için kaynak ortamı.
- [Ayarlanan](tutorial-physical-to-azure.md#set-up-the-source-environment) fiziksel sunucuları için kaynak ortamı.
- [Ayarlanan](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) Hyper-V sanal makineleri için kaynak ortamı.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Resource Manager dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="set-up-a-replication-policy"></a>Bir çoğaltma ilkesini ayarlayın

- [Bir çoğaltma ilkesini ayarlayın](tutorial-vmware-to-azure.md#create-a-replication-policy) VMware VM'ler için.
- [Bir çoğaltma ilkesini ayarlayın](tutorial-physical-to-azure.md#create-a-replication-policy) fiziksel sunucuları için.
- [Bir çoğaltma ilkesini ayarlayın](tutorial-hyper-v-to-azure.md#set-up-a-replication-policy) Hyper-V sanal makineleri için.


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

- [Çoğaltmayı etkinleştirme](tutorial-vmware-to-azure.md#enable-replication) VMware VM'ler için.
- [Çoğaltmayı etkinleştirme](tutorial-physical-to-azure.md#enable-replication) fiziksel sunucuları için.
- [Çoğaltmayı etkinleştirme](tutorial-hyper-v-to-azure.md#enable-replication) Hyper-V sanal makineleri için.


## <a name="run-a-test-migration"></a>Bir test geçişi çalıştırma

Çalıştıran bir [yük devretme sınamasını](tutorial-dr-drill-azure.md) Azure'a emin olmak için her şeyin beklendiği gibi çalıştığını.


## <a name="migrate-to-azure"></a>Azure’a geçiş

Geçirmek istediğiniz makineler için yük devretme gerçekleştirme.

1. İçinde **ayarları** > **öğeleri çoğaltılan** makineye tıklayın > **yük devretme**.
2. İçinde **yük devretme** seçin bir **kurtarma noktası** devretmesini. En son kurtarma noktası seçin.
3. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
4. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın**. Site Recovery, bir kapanma kaynak sanal makinelerin yük devretme tetiklemeden önce yapın dener. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
5. Azure VM gibi Azure'da görünüp görünmediğini kontrol edin.
6. İçinde **öğeleri çoğaltılan**, VM'ye sağ tıklayın > **tam geçiş**. Bu geçiş işlemini tamamlar, sanal makine için çoğaltmayı durdurur ve VM için Site Recovery Faturalaması durdurulur.

    ![Geçişi tamamlama](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: VM çoğaltma yük devretme başlamadan önce durduruldu. Bir yük devretme devam ediyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz

Bazı senaryolarda, yük devretme tamamlamak için yaklaşık sekiz ile on dakika sürer ek işleme gerektirir. Artık fark edebilirsiniz test fiziksel sunucuları, VMware Linux makineler, DHCP hizmetini etkinleştirir yok VMware Vm'lerini ve aşağıdaki önyükleme sürücüleri yok VMware Vm'leri için yük devretme süreleri: storvsc, vmbus, storflt, Intelide, ATAPI.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure VM'ler için şirket içi sanal makineleri geçirildi. Artık Azure VM'ler için olağanüstü durum kurtarma yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma ayarlamak](site-recovery-azure-to-azure-after-migration.md) bir şirket içi siteden geçişten sonra Azure VM'ler için.
