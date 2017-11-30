---
title: "Bir ikincil bölge'Azure Site Recovery ile Azure Vm'lerini çoğaltma | Microsoft Docs"
description: "Bu makalede Azure Site Recovery hizmetini kullanarak Azure başka bir bölge için bir Azure bölgesinde çalışan Vm'lerini çoğaltma açıklar."
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
ms.date: 11/29/2017
ms.author: asgang
ms.openlocfilehash: 114390712f5e5bf62ec515db62469e09361b8f85
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Başka bir Azure bölgesine çoğaltma Azure sanal makineler


Bu makalede Azure Site Recovery hizmetini kullanarak bir ikincil bir Azure bölgesine, Azure sanal makinelerini (VM'ler) bir Azure bölgesindeki, çoğaltma açıklar.

>[!NOTE]
>
> Site Recovery ile Azure VM çoğaltma şu anda önizlemede değil.

## <a name="prerequisites"></a>Ön koşullar

* Kurtarma Hizmetleri kasası yerinde olmalıdır. Vm'leriniz çoğaltmak istediğiniz hedef bölgede kasası oluşturmanız önerilir.
* Azure vm'lerinde giden internet bağlantısı erişimi denetlemek için ağ güvenlik grupları (NSG) kuralları veya bir güvenlik duvarı proxy kullanıyorsanız, gerekli URL'ler veya IP'leri izin emin olun. [Daha fazla bilgi edinin](./site-recovery-azure-to-azure-networking-guidance.md).
* Bir ExpressRoute veya şirket içi ve Azure, kaynak konumu arasında bir VPN bağlantısı varsa [nasıl oluşturacağınızı öğrenin](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration).
* Azure kullanıcı hesabınızın gereken [özel izinler](../site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines), bir Azure VM çoğaltmasını etkinleştirmek için.
Bir olağanüstü durum kurtarma bölge kullanmak istediğiniz hedef konumda VM'ler oluşturmak için Azure aboneliğinizin etkinleştirilmesi gerekir. Gerekli Kotayı etkinleştirmek için desteğe başvurun.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu yordamda, Doğu Asya kaynak konumu ve Güneydoğu Asya hedef olarak kullanılır.

1. Tıklatın **+ Çoğalt** sanal makineler için çoğaltmayı etkinleştirmek için kasada.
2. Doğrulayın **kaynak:** ayarlanır **Azure**.
3. Ayarlama **kaynak konumu** Doğu Asya için.
4. İçinde **dağıtım modeli**seçin **Klasik** veya **Resource Manager**.
5. İçinde **kaynak grubu**, kaynak Vm'leriniz ait olduğu grubu seçin. Seçilen kaynak grubundaki tüm sanal makineleri listelenir.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

6. İçinde **sanal makineleri > sanal makine Seç**tıklayın ve çoğaltmak istediğiniz her bir VM seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)
    
7. Altında **ayarları** > **hedef konumu**, burada VM veri kaynağı çoğaltılır belirtin. Site Recovery, seçili VM'ler bölge bağlı olarak uygun hedef bölgelerin bir listesi sağlar.
8. Site kurtarma hedef ayarları varsayılan ayarlar. Bunlar değiştirilebilir gerektiği gibi.

    - **Hedef kaynak grubu**. Varsayılan olarak, Site Recovery "asr" soneki ile hedef bölgede yeni bir kaynak grubu oluşturur. Oluşturulan kaynak grubu zaten varsa, yeniden kullanılır.
    - **Hedef sanal ağ**. Varsayılan olarak, Site Recovery hedef bölgesindeki "asr" son ekini içeren yeni bir sanal ağ oluşturur. Bu ağ kaynağı ağınıza eşlenir. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
    - **Hedef depolama hesapları**. Varsayılan olarak, Site Recovery kaynak VM depolama yapılandırmasıyla eşleşen yeni bir hedef depolama hesabı oluşturur. Oluşturulan hesap zaten varsa, yeniden kullanılır.
    - **Önbellek depolama hesapları**. Azure Site Recovery bir ek önbellek depolama hesabı, Kaynak bölgesi oluşturur. Kaynak VM'ler tüm değişiklikleri izlenen ve önbellek depolama hesabı çoğaltma hedef konumuna önce gönderilir.
    - **Kullanılabilirlik kümesi**. Varsayılan olarak, Site Recovery hedef bölgede bir "asr" soneki ile yeni bir kullanılabilirlik oluşturur. Oluşturulan kümesi zaten varsa, yeniden kullanılır.
    - **Çoğaltma İlkesi**. Site Recovery kurtarma noktası bekletme geçmişini ve uygulamayla tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Site Recovery kurtarma noktası bekletme için 24 saat ve uygulamayla tutarlı anlık görüntü sıklığı için 60 dakika, varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
9. Tıklatın **çoğaltmayı etkinleştirme** sanal makineleri korumaya başlamak için.

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

1. Bu hedef varsayılan düzenleyin:

    - **Hedef kaynak grubu**. Hedef konumda abonelik içindeki tüm kaynak gruplarının listesini herhangi bir kaynak grubu seçin.
    - **Hedef sanal ağ**. Hedef konumda tüm sanal ağları listesinden seçin.
    - **Kullanılabilirlik kümesi** kaynak bölge kümesinde bulunan VM'ler için kullanılabilirlik kümeleri ayarlarını yalnızca ekleyebilirsiniz.
    - **Hedef depolama hesapları**: kullanılabilir herhangi bir hesabı ekleyin.

        ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Tıklayın **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**. İlk çoğaltma sırasında VM durumu yenilemek için biraz zaman alabilir. Tıklatın **yenileme** en son durumunu almak için.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)
    
3. VM durumu VM'ler korunduktan sonra iade **öğeleri çoğaltılan**.



## <a name="next-steps"></a>Sonraki adımlar
[Bilgi](../azure-to-azure-tutorial-dr-drill.md) yük devretme testi çalıştırma.

