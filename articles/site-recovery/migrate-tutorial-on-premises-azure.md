---
title: Azure Site Recovery ile şirket içi makineleri Azure’a geçirme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanarak şirket içi makineleri Azure’a geçirme işlemi açıklanmaktadır.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: df4f89bd1b2e3c0423f5d758cfa637e4da9e04d0
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66396535"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’a geçirme


Bu makalede şirket içi makineleri Azure'a geçirmek nasıl kullanarak [Azure Site Recovery](site-recovery-overview.md). Genellikle, Site Recovery, şirket içi makinelerin ve Azure Vm'lerinde olağanüstü durum kurtarmayı yönetmek ve düzenlemek için kullanılır. Ancak, bu da geçiş için kullanılabilir. Geçiş, bir özel durum ile olağanüstü durum kurtarma gibi aynı adımları kullanır. Geçiş işleminde, makineleri şirket içi sitenizden yük devretmeyi son adımdır. Olağanüstü durum kurtarma dön şirket içi bir geçiş senaryosunda çalışamaz.


Bu öğreticide, şirket içi VM’ler ve fiziksel sunucuları Azure’a geçirme gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Geçiş için kaynak ve hedef ortamı ayarlama
> * Çoğaltma ilkesi ayarlama
> * Çoğaltmayı etkinleştirme
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir geçiş testi çalıştırma
> * Azure’a bir defalık yük devretme çalıştırma


> [!TIP]
> Azure geçişi hizmeti artık yeni, aracısız bir deneyim için Önizleme geçirme VMware Vm'leri için Azure'da teklifidir. [Daha fazla bilgi edinin](https://aka.ms/migrateVMs-signup).


## <a name="before-you-start"></a>Başlamadan önce

Parasanallaştırılmış sürücüler tarafından dışarı aktarılan cihazlar desteklenmez unutmayın.


## <a name="prepare-azure-and-on-premises"></a>Azure ve şirket içi hazırlama

1. Açıklandığı gibi Azure'ı hazırlama [bu makalede](tutorial-prepare-azure.md). Bu makalede olağanüstü durum kurtarma için hazırlık adımları olsa da, adımları geçiş için de geçerlidir.
2. Şirket içi [VMware](vmware-azure-tutorial-prepare-on-premises.md) veya [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) sunucularını hazırlayın. Fiziksel makineleri geçiriyorsanız, herhangi bir şeyi hazırlamanız gerekmez. Yalnızca doğrulayın [destek matrisi](vmware-physical-azure-support-matrix.md).


## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.
1. **Kurtarma Hizmetleri kasaları** > kasa öğesine tıklayın.
2. Kaynak Menüsünde, **Site Recovery** > **Altyapıyı Hazırlama** > **Koruma hedefi** seçeneklerine tıklayın.
3. **Koruma hedefi**’nde, geçişini yapmak istediğiniz öğeyi seçin.
    - **VMware**: Seçin **Azure'a** > **Evet, VMWare vSphere Hypervisor ile**.
    - **Fiziksel makine**: Seçin **Azure'a** > **sanallaştırılmamış/diğer**.
    - **Hyper-V**: Seçin **Azure'a** > **Evet, Hyper-V ile**. Hyper-V VM’leri VMM tarafından yönetiliyorsa, **Evet**’i seçin.


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

**Senaryo** | **Ayrıntılar**
--- | --- 
VMware | Ayarlanan [kaynak ortamı](vmware-azure-set-up-source.md), ayarlayın ve [yapılandırma sunucusu](vmware-azure-deploy-configuration-server.md).
Fiziksel makine | [Ayarlanan](physical-azure-set-up-source.md) kaynak ortam ve yapılandırma sunucusu.
Hyper-V | Ayarlanan [kaynak ortam](hyper-v-azure-tutorial.md#set-up-the-source-environment)<br/><br/> Ayarlanan [kaynak ortamı](hyper-v-vmm-azure-tutorial.md#set-up-the-source-environment) dağıtılan System Center VMM ile Hyper-V için.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Kaynak Yöneticisi dağıtım modelini belirtin.
3. Site Recovery, Azure kaynaklarını denetler.
    - VMware Vm'lerini veya fiziksel sunucuları geçiriyorsanız, Site Recovery, yük devretme sonrasında oluşturulduğunda, Azure Vm'lerini yer alacağı bir Azure ağına sahip doğrular.
    - Site Recovery, Hyper-V sanal makineleri geçiriyorsanız, uyumlu Azure depolama hesabını ve ağı sahip doğrular.
4. System Center VMM tarafından yönetilen Hyper-V Vm'lerini geçiriyorsanız, ayarlanan [ağ eşlemesini](hyper-v-vmm-azure-tutorial.md#configure-network-mapping).

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

**Senaryo** | **Ayrıntılar**
--- | --- 
VMware | Ayarlanmış bir [Çoğaltma İlkesi](vmware-azure-set-up-replication.md) VMware Vm'leri için.
Fiziksel makine | Ayarlanmış bir [Çoğaltma İlkesi](physical-azure-disaster-recovery.md#create-a-replication-policy) fiziksel makineler için.
Hyper-V | Ayarlanmış bir [Çoğaltma İlkesi](hyper-v-azure-tutorial.md#set-up-a-replication-policy)<br/><br/> Ayarlanmış bir [Çoğaltma İlkesi](hyper-v-vmm-azure-tutorial.md#set-up-a-replication-policy) dağıtılan System Center VMM ile Hyper-V için.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

**Senaryo** | **Ayrıntılar**
--- | --- 
VMware | VMware VM’leri için [çoğaltmayı etkinleştirin](vmware-azure-enable-replication.md).
Fiziksel makine | [Çoğaltmayı etkinleştirme](physical-azure-disaster-recovery.md#enable-replication) fiziksel makineler için.
Hyper-V | [Çoğaltmayı etkinleştirme](hyper-v-azure-tutorial.md#enable-replication)<br/><br/> [Çoğaltmayı etkinleştirme](hyper-v-vmm-azure-tutorial.md#enable-replication) dağıtılan System Center VMM ile Hyper-V için.


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

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekir. Uzun yük devretme testi süreleriyle fiziksel sunucuları, VMware Linux makineler, DHCP hizmetinin etkinleştirilmiş sahip olmayan VMware Vm'leri ve önyükleme sürücülerine sahip olmayan VMware Vm'lerini: storvsc, vmbus, storflt, intelide, atapi.

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

Bu öğreticide şirket içi VM’leri Azure VM’lerine taşıdınız. Şimdi

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma ayarlama](azure-to-azure-replicate-after-migration.md) Azure Vm'leri için ikincil bir Azure bölgesine.

  
