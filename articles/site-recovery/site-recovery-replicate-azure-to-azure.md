---
title: "Uygulamaları (Azure için Azure) çoğaltmak | Microsoft Docs"
description: "Bu makalede, azure'da başka bir bölge için bir Azure bölgesinde çalışan sanal makinelerin çoğaltmasını ayarlama açıklar."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: f9f97cf840b722c8cfee169dd1640e0682f287ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Başka bir Azure bölgesine çoğaltma Azure sanal makineler



>[!NOTE]
>
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.

Bu makalede, başka bir Azure bölgesine bir Azure bölgesinde çalışan sanal makinelerin çoğaltmasını ayarlama açıklar.

## <a name="prerequisites"></a>Ön koşullar

* Makaleyi zaten Site kurtarma ve kurtarma Hizmetleri kasası hakkında bildiğinizi varsayar. Oluşturulan bir 'Kurtarma Hizmetleri Kasası' öncesi olması gerekir.

    >[!NOTE]
    >
    > Vm'leriniz çoğaltmak istediğiniz konumu 'Kurtarma Hizmetleri Kasası' oluşturmak önerilir. Örneğin, hedef konumu 'Orta ABD' ise 'Orta ABD' kasası oluşturun.

* Azure vm'lerinde giden internet bağlantısı erişimi denetlemek için ağ güvenlik grupları (NSG) kuralları veya güvenlik duvarı proxy kullanıyorsanız, bu, beyaz liste gerekli URL'leri veya IP'leri. Başvurmak [Ağ Kılavuzu belge](./site-recovery-azure-to-azure-networking-guidance.md) daha fazla ayrıntı için.

* Bir ExpressRoute veya şirket içi ve Azure kaynak konumu arasında bir VPN bağlantısı varsa, izleyin [şirket içi ExpressRoute için Azure Site kurtarma değerlendirmeleri / VPN Yapılandırması](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) belge.

* Azure kullanıcı hesabınızın belirli sahip olması [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) bir Azure sanal makinesi çoğaltmayı etkinleştirmek için.

* DR bölge kullanmak istediğiniz hedef konumda VM'ler oluşturmak için Azure aboneliğinizin etkinleştirilmesi gerekir. Gerekli Kotayı etkinleştirmek için desteğine başvurabilirsiniz.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Azure Site Recovery kasası çoğaltmayı etkinleştirin
Bu çizim için biz 'Doğu Asya' çalışan sanal makineler çoğaltılacak ' Güneydoğu Asya ' konumuna Azure konumu. Adımlar aşağıdaki gibidir:

 Tıklatın **+ Çoğalt** sanal makineler için çoğaltmayı etkinleştirmek için kasada.

1. **Kaynak:** , bu durumda olan kaynak makinelerin noktasına başvurduğu **Azure**.

2. **Kaynak Konum:** sanal makinelerinizi korumak istediğiniz Azure bölgesidir. Bu çizim için kaynak konumu 'Doğu Asya' olacaktır

3. **Dağıtım modeli:** kaynak makinelerden Azure dağıtım modeline başvuruyor. Her iki Klasik seçebilir veya bir sonraki adımda koruma için kaynak yöneticisi ve belirli modele ait makineler listelenir.

      >[!NOTE]
      >
      > Yalnızca klasik bir sanal makine çoğaltabilir ve klasik sanal makine olarak kurtarın. Resource Manager sanal makine olarak kurtaramazsınız.

4. **Kaynak grubu:** kaynak sanal makinelerinizi ait olduğu kaynak grubu değil. Seçilen kaynak grubundaki tüm sanal makineleri, sonraki adımda koruma için listelenir.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

İçinde **sanal makineleri > sanal makine Seç**tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra Tamam'a tıklayın.
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


Ayarları bölümü altında hedef site özelliklerini yapılandırabilirsiniz.

1. **Hedef konumu:** bu burada kaynak sanal makine verilerinizi çoğaltılabilir konumdur. Seçili makineler konumuna bağlı olarak, Site Recovery, uygun hedef bölgelerin listesini sağlar.

    > [!TIP]
    > Hedef konumu tutmanız önerilir aynı itibariyle, Kurtarma Hizmetleri kasası.

2. **Hedef kaynak grubu:** hangi tüm çoğaltılmış sanal makinelere ait olur kaynak grubu değil. Varsayılan olarak ASR yeni bir kaynak grubu hedef bölgede "asr" sonekine sahip adıyla oluşturur. ASR tarafından önceden oluşturulmuş kaynak grubu mevcut olmaması durumunda, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmek seçebilirsiniz.    
3. **Hedef sanal ağ:** varsayılan olarak, ASR yeni bir sanal ağ hedef bölgede "asr" sonekine sahip adıyla oluşturur. Bu kaynak ağınıza eşlenecek ve gelecekteki tüm koruma için kullanılır.

    > [!NOTE]
    > [Ağ ayrıntıları denetlemek](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında daha fazla bilgi için.

4. **Depolama hesapları hedef:** varsayılan olarak, ASR kaynak VM depolama yapılandırmanızı mimicking yeni hedef depolama hesabı oluşturacak. ASR tarafından önceden oluşturulmuş depolama hesabı mevcut olmaması durumunda, yeniden kullanılır.

5. **Depolama hesapları önbelleğe:** ASR önbellek depolama kaynağı bölgede adlı ek depolama alanı hesabı gerekiyor. Kaynak sanal makinelerin gerçekleştiği tüm değişiklikleri izlenen ve bu hedef konumuna çoğaltma önce önbellek depolama hesabına gönderilir.

6. **Kullanılabilirlik kümesi:** varsayılan olarak, hedef bölgede kümesi "asr" sonekine sahip adı ile yeni bir kullanılabilirlik ASR oluşturur. Kullanılabilirlik kümesi zaten ASR tarafından oluşturulan mevcut olmaması durumunda, yeniden kullanılır.

7.  **Çoğaltma İlkesi:** kurtarma noktası bekletme geçmişi ve uygulama tutarlılığı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, ASR ' 24 saattir kurtarma noktası bekletme ve ' 60 dakika uygulama tutarlılığı anlık görüntü sıklığı için varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

ASR tarafından kullanılan varsayılan değerleri değiştirmek istemeniz durumunda, gereksinimlerinize göre ayarlarını değiştirebilirsiniz.

1. **Özelleştir:** ASR tarafından kullanılan varsayılan değerleri değiştirmek için tıklatın.

2. **Hedef kaynak grubu:** abonelik içindeki hedef konumda var olan tüm kaynak gruplarının listesini kaynak grubunu seçebilirsiniz.

3. **Hedef sanal ağ:** hedef konumda bulunan tüm sanal ağ listesini bulabilirsiniz.

4. **Kullanılabilirlik kümesi:** kullanılabilirlik kaynak bölgede bir parçası olan sanal makineler için kullanılabilirlik kümeleri ayarlarını yalnızca ekleyebilirsiniz.

5. **Hedef depolama hesapları:**

![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG) tıklayın **hedef kaynak oluşturma** ve çoğaltmayı etkinleştirme


Korunan sanal makine sonra altında VM'ler sağlık durumunu kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma süre boyunca durum yenilemesinin zaman almasıdır ve biraz zaman ilerleme görmüyorsanız olasılığı verebilir. En son durumunu almak için dikey pencerenin üst kısmında Yenile düğmesini tıklatabilirsiniz.
>

![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
- [Daha fazla bilgi edinin](site-recovery-failover.md) farklı türdeki yük devretmeler ve bunları çalıştırma hakkında.
- Daha fazla bilgi edinmek [kurtarma planlarına kullanarak](site-recovery-create-recovery-plans.md) RTO azaltmak için.
- Daha fazla bilgi edinmek [Azure sanal makineleri yeniden korumayı](site-recovery-how-to-reprotect.md) yük devretme sonrasında.
