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
ms.date: 12/08/2017
ms.author: asgang
<<<<<<< HEAD
ms.openlocfilehash: dc7dff33aa2c3e844c6a91024fcfc98148416f7e
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
=======
ms.openlocfilehash: 209ec47388ee7291f8107df022e0c2bb202ba6b5
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Başka bir Azure bölgesine çoğaltma Azure sanal makineler



>[!NOTE]
>
> Azure sanal makineler için Site Recovery çoğaltma şu anda önizlemede değil.

Bu makalede, başka bir Azure bölgesine bir Azure bölgesinde çalışan sanal makinelerin çoğaltmasını ayarlama açıklar.

## <a name="prerequisites"></a>Ön koşullar

* Makaleyi zaten Site kurtarma ve kurtarma Hizmetleri kasası hakkında bildiğinizi varsayar. 'Önceden oluşturulmuş bir kurtarma Hizmetleri Kasası' olması gerekir.

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

2. **Kaynak Konum:** sanal makinelerinizi korumak istediğiniz Azure bölgesidir. Bu çizim için kaynak konum 'Doğu Asya' değil.

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

2. **Hedef kaynak grubu:** çoğaltılmış sanal makineleriniz için tüm ait kaynak grubu değil. Varsayılan olarak Azure Site Recovery "asr" sonekine sahip adla hedef bölgede yeni bir kaynak grubu oluşturur. Azure Site Recovery tarafından önceden oluşturulmuş kaynak grubu mevcut olmaması durumunda, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmek seçebilirsiniz.    
3. **Hedef sanal ağ:** varsayılan olarak, Azure Site Recovery yeni bir sanal ağ hedef bölgede "asr" sonekine sahip adla oluşturur. Bu kaynak ağınıza eşlenecek ve gelecekteki tüm koruma için kullanılır.

    > [!NOTE]
    > [Ağ ayrıntıları denetlemek](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında daha fazla bilgi için.

4. **Depolama hesapları hedef:** varsayılan olarak, Azure Site Recovery kaynak VM depolama yapılandırmanızı mimicking yeni bir hedef depolama hesabı oluşturur. Azure Site Recovery tarafından önceden oluşturulmuş depolama hesabı mevcut olmaması durumunda, yeniden kullanılır.

5. **Depolama hesapları önbelleğe:** Azure Site Recovery önbellek depolama kaynağı bölgede adlı ek depolama alanı hesabı gerekiyor. Kaynak sanal makinelerin gerçekleştiği tüm değişiklikleri izlenen ve bu hedef konumuna çoğaltma önce önbellek depolama hesabına gönderilir.

6. **Kullanılabilirlik kümesi:** varsayılan olarak, Azure Site Recovery hedef bölgede kümesi "asr" sonekine sahip adı ile yeni bir kullanılabilirlik oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut olmaması durumunda, yeniden kullanılır.

7.  **Çoğaltma İlkesi:** kurtarma noktası bekletme geçmişi ve uygulama tutarlılığı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery ' 24 saattir kurtarma noktası bekletme ve ' 60 dakika uygulama tutarlılığı anlık görüntü sıklığı için varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Azure Site Recovery tarafından kullanılan varsayılan değerleri değiştirmek istemeniz durumunda, gereksinimlerinize göre ayarlarını değiştirebilirsiniz.

1. **Özelleştir:** Azure Site Recovery tarafından kullanılan varsayılan değerleri değiştirmek için tıklatın.

2. **Hedef kaynak grubu:** abonelik içindeki hedef konumda var olan tüm kaynak gruplarının listesini kaynak grubunu seçebilirsiniz.

3. **Hedef sanal ağ:** hedef konumda bulunan tüm sanal ağ listesini bulabilirsiniz.

4. **Kullanılabilirlik kümesi:** kullanılabilirlik kaynak bölgede bir parçası olan sanal makineler için kullanılabilirlik kümeleri ayarlarını yalnızca ekleyebilirsiniz.

5. **Hedef depolama hesapları:**

![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG) tıklayın **hedef kaynak oluşturma** ve çoğaltmayı etkinleştirme


Korunan sanal makine sonra altında VM'ler sağlık durumunu kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma süre boyunca olabilir durumunu yenilemek için süren olasılığı ve biraz zaman ilerleme görmüyorsanız. En son durumunu almak için dikey pencerenin üst kısmında Yenile düğmesini tıklatabilirsiniz.
>

![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
- [Daha fazla bilgi edinin](site-recovery-failover.md) farklı türdeki yük devretmeler ve bunları çalıştırma hakkında.
- Daha fazla bilgi edinmek [kurtarma planlarına kullanarak](site-recovery-create-recovery-plans.md) RTO azaltmak için.
- Daha fazla bilgi edinmek [Azure sanal makineleri yeniden korumayı](site-recovery-how-to-reprotect.md) yük devretme sonrasında.
