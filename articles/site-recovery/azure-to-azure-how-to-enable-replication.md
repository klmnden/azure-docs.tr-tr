---
title: Azure Site Recovery, Azure Vm'leri için çoğaltma yapılandırma | Microsoft Docs
description: Bu makalede, bir Azure bölgesinden diğerine Site Recovery kullanarak Azure Vm'leri için çoğaltmayı yapılandırmak açıklar.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 797cef2d3a0e2cfeb28411c30a7da8a5b6f2d953
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834528"
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Azure sanal makinelerini başka bir Azure bölgesine çoğaltma



Bu makalede, Azure vm'leri, bir Azure bölgesinden diğerine çoğaltmayı etkinleştirmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bu senaryo için Site RECOVERY'yi önceden ayarladığınız açıklandığı varsayılır [Azure-Azure Öğreticisi](azure-to-azure-tutorial-enable-replication.md). Önkoşulları hazır ve kurtarma Hizmetleri kasası oluşturdunuz, emin olun.



## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Çoğaltmayı etkinleştirin. Bu yordam, Doğu Asya Birincil Azure bölgesi olduğunu ve ikincil bölgeye Güney Doğu Asya olduğunu varsayar.

1. Kasaya tıklayın **+ Çoğalt**.
2. Aşağıdaki alanları dikkat edin:
    - **Kaynak**: Bu durumda sanal makinelerin başlangıç noktasını **Azure**.
    - **Kaynak konumu**: sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu çizim için 'Doğu Asya' kaynak konumdur
    - **Dağıtım modeli**: kaynak makineleri Azure dağıtım modeli.
    - **Kaynak abonelik**: kaynak sanal makinelerinize ait olduğu abonelik. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
    - **Kaynak grubu**: kaynak sanal makinelerinize ait olduğu kaynak grubu. Sonraki adımda koruma için seçilen kaynak grubu altındaki tüm sanal makineler listelenmektedir.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. İçinde **sanal makineler > sanal makineleri**tıklayın ve çoğaltmak istediğiniz her bir sanal Makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. ' A tıklayarak **Tamam**.
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. İçinde **ayarları**, isteğe bağlı olarak hedef site ayarları yapılandırabilirsiniz:

    - **Hedef konum**: konumun burada kaynak sanal makine verilerinizi çoğaltılabilir. Seçili makineler konumuna bağlı olarak Site Recovery, uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasası konumu olarak aynı hedef konum tutmanızı öneririz.
    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur.
    - **Hedef kaynak grubu**: çoğaltılmış sanal makineleriniz için tüm ait kaynak grubu. Varsayılan olarak Azure Site Recovery "asr" sonekine sahip adı ile hedef bölgede yeni bir kaynak grubu oluşturur. Azure Site Recovery tarafından zaten oluşturulan kaynak grubunun mevcut olması durumunda, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmek seçebilirsiniz. Hedef kaynak grubu konumunu, kaynak sanal makineleri barındırdığı bölge dışında tüm Azure bölgelerine olabilir.
    - **Hedef sanal ağ**: varsayılan olarak, Site Recovery yeni bir sanal ağ hedef bölgede "asr" sonekine sahip adla oluşturur. Bu kaynak ağa eşlenen ve gelecekteki tüm koruma için kullanılır. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
    - **Depolama hesapları (kaynak makine yönetilen diskleri kullanmıyorsa) hedef**: varsayılan olarak, Site Recovery, kaynak VM depolama yapılandırması yakından taklit eden yeni bir hedef depolama hesabı oluşturur. Depolama hesabı zaten mevcut olması durumunda, yeniden kullanılır.
    - **Yönetilen çoğaltma diskleri (kaynak sanal makine yönetilen diskleri kullanıyorsa)**: Site Recovery, kaynak sanal makinenin yönetilen disklerle aynı depolama türüne (standart veya premium) kaynağın VM'ye yönetilen disk olarak yansıtmak için hedef bölgede yeni yönetilen çoğaltma diskleri oluşturur.
    - **Önbellek depolama hesapları**: Site Recovery kaynak bölgede önbellek depolama adlı ek bir depolama hesabı gerekir. Kaynak VM üzerinde'olmuyor tüm değişiklikleri izlenir ve bu hedef konuma çoğaltılmadan önce önbellek depolama hesabına gönderilir.
    - **Hedef kullanılabilirlik kümeleri**: varsayılan olarak, Azure Site Recovery, yeni bir kullanılabilirlik Vm'leri bir kullanılabilirlik kümesi kaynak bölgede bir parçası için "asr" sonekine sahip adı ile hedef bölgede kümesi oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut durumda yeniden kullanılır.
    - **Hedef kullanılabilirlik**: hedef bölge kullanılabilirlik destekliyorsa, varsayılan olarak, Site Recovery hedef bölge kaynak bölgede aynı bölge sayısına atar.

    Hedef bölge kullanılabilirlik bölgelerini desteklemez, hedef Vm'leri varsayılan olarak tek örnekleri olarak yapılandırılır. Gerekirse, tür VM'ler 'Özelleştir' tıklayarak kullanılabilirlik kümeleri hedef bölgede bir parçası olarak yapılandırabilirsiniz.

    >[!NOTE]
    >Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türü - tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik alanı değiştiremezsiniz. Devre dışı bırakın ve çoğaltma kullanılabilirlik türünü değiştirmek etkinleştirmeniz gerekir.
    >
    
    - **Çoğaltma İlkesi**: kurtarma noktası bekletme geçmişine ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery, ' 24 saatliğine kurtarma noktası bekletme ' 60 dakika için uygulamayla tutarlı anlık görüntü sıklığını ve varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery tarafından kullanılan varsayılan hedef ayarlarını değiştirebilirsiniz.

1. Tıklayın **Özelleştir:** yanındaki 'Hedef aboneliği' varsayılan hedef aboneliği değiştirmek için. Aynı Azure Active Directory (AAD) kiracısında mevcut tüm abonelikleri listeden aboneliği seçin.

2. Tıklayın **Özelleştir:** varsayılan ayarlarını değiştirmek için:
    - İçinde **hedef kaynak grubu**, aboneliğin hedef konumdaki tüm kaynak grupları listesinden kaynak grubunu seçin.
    - İçinde **hedef sanal ağ**, ağ sanal ağ içinde hedef konum listesinden seçin.
    - İçinde **kullanılabilirlik kümesi**, bir kullanılabilirlik kümesi kaynak bölgede parçası iseler VM kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - İçinde **hedef depolama hesapları**, kullanmak istediğiniz hesabı seçin.

        ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Tıklayın **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**.
3. VM'ler için çoğaltma etkinleştirildikten sonra durumu VM sistem durumu ' kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma sırasında durum, ilerleme durumu yenilemek için biraz zaman alabilir. Tıklayın **Yenile** düğme, son durumu almak için.
>

# <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
