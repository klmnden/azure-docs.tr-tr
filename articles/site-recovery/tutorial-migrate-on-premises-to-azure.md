---
title: "Azure Site Recovery ile azure'a şirket içi makineler geçirme | Microsoft Docs"
description: "Bu makalede, Azure Site Kurtarma'yı kullanarak Azure için şirket içi makineleri geçirmek açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: cfd44f7f06faa7d1d00efa9427dbf5d1d0a89ef1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-on-premises-machines-to-azure"></a>Şirket içi makineleri Azure’a geçirme

[Azure Site Recovery](site-recovery-overview.md) hizmet yöneten ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetir.

Bu öğretici, Azure Site Recovery ile şirket içi sanal makineleri ve fiziksel sunucuları geçirmek nasıl gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Dağıtım önkoşulları ayarlama
> * Site kurtarma için bir kurtarma Hizmetleri kasası oluşturma
> * Şirket içi yönetim sunucuları dağıtma
> * Bir çoğaltma ilkesini ayarlayın ve çoğaltmayı etkinleştirme
> * Çalışma her şeyi emin olmak için bir olağanüstü durum kurtarma ayrıntıya çalışma
> * Azure için tek seferlik bir yük devretmeyi çalıştırma

## <a name="overview"></a>Genel Bakış

Çoğaltmayı etkinleştirme ve Azure'a devrederek tarafından bir makineyi geçirin.


## <a name="prerequisites"></a>Ön koşullar

İşte bu öğretici için yapmanız gerekenler.

- [Hazırlama](tutorial-prepare-azure.md) bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gibi Azure kaynakları.
- [Hazırlama](tutorial-prepare-on-premises-vmware.md) VMware şirket içi VMware sunucularını ve Vm'leri.
- Paravirtualized sürücüleri tarafından dışarı aktarılan cihazlar desteklenmeyen unutmayın.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak istediğinizi ve bunları nereye çoğaltacağınızı seçin.
1. Tıklatın **kurtarma Hizmetleri kasaları** > kasası.
2. Kaynak menüye tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
3. İçinde **koruma hedefi**seçin:
    - **VMware**: seçin **için Azure** > **Evet, VMWare vSphere hiper yönetici ile**.
    - **Fiziksel makine**: seçin **için Azure** > **değil sanallaştırılmış/diğer**.
    - **Hyper-V**: seçin **Azure'a** > **Evet, Hyper-V ile**.


## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

- [Ayarlanan](tutorial-vmware-to-azure.md#set-up-the-source-environment) VMware VM'ler için kaynak ortamı.
- [Ayarlanan](tutorial-physical-to-azure.md#set-up-the-source-environment) fiziksel sunucuları için kaynak ortamı.
- [Ayarlanan](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) Hyper-V sanal makineleri için kaynak ortamı.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

- [Bir çoğaltma ilkesini ayarlayın](tutorial-vmware-to-azure.md#create-a-replication-policy) VMware VM'ler için.


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

- [Çoğaltmayı etkinleştirme](tutorial-vmware-to-azure.md#enable-replication) VMware VM'ler için.


## <a name="run-a-test-migration"></a>Bir test geçişi çalıştırma

Çalıştıran bir [yük devretme sınamasını](tutorial-dr-drill-azure.md) Azure'a emin olmak için her şeyin beklendiği gibi çalıştığını.


## <a name="migrate-to-azure"></a>Azure’a geçiş

Geçirmek istediğiniz makineler için yük devretme gerçekleştirme.

1. İçinde **ayarları** > **öğeleri çoğaltılan** makineye tıklayın > **yük devretme**.
2. İçinde **yük devretme** seçin bir **kurtarma noktası** devretmesini. En son kurtarma noktası seçin.
3. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
4. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** Site Kurtarma'nın bir kapanma kaynak sanal makinelerin yük devretme tetiklemeden yapmaya istiyorsanız. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
5. Azure VM gibi Azure'da görünüp görünmediğini kontrol edin.
6. İçinde **öğeleri çoğaltılan**, VM'ye sağ tıklayın > **tam geçiş**. Bu geçiş işlemini tamamlar, sanal makine için çoğaltmayı durdurur ve VM için Site Recovery Faturalaması durdurulur.

    ![Geçişi tamamlama](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: yük devretme başlatılmadan önce VM çoğaltma durduruldu. Bir yük devretme devam ediyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz

Bazı senaryolarda, yük devretme tamamlamak için yaklaşık sekiz ile on dakika sürer ek işleme gerektirir. Artık fark edebilirsiniz test fiziksel sunucuları, VMware Linux makineler, DHCP hizmetini etkinleştirir yok VMware Vm'lerini ve aşağıdaki önyükleme sürücüleri yok VMware Vm'leri için yük devretme süreleri: storvsc, vmbus, storflt, Intelide, ATAPI.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure VM'ler, Azure geçişten sonra başka bir bölgeye çoğaltılıyor.](site-recovery-azure-to-azure-after-migration.md)
