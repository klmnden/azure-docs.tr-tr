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
ms.openlocfilehash: c8b5cf840b8cb5eec2a993cfe35c8a8a7ac904fb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60791396"
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
   - **Kaynak konumu**: Sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu çizim için 'Doğu Asya' kaynak konumdur
   - **Dağıtım modeli**: Kaynak makineler Azure dağıtım modeli.
   - **Kaynak abonelik**: Kaynak sanal makinelerinize ait olduğu abonelik. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
   - **Kaynak grubu**: Kaynak sanal makinelerinize ait olduğu kaynak grubu. Sonraki adımda koruma için seçilen kaynak grubu altındaki tüm sanal makineler listelenmektedir.

     ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. İçinde **sanal makineler > sanal makineleri**tıklayın ve çoğaltmak istediğiniz her bir sanal Makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. ' A tıklayarak **Tamam**.
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. İçinde **ayarları**, isteğe bağlı olarak hedef site ayarları yapılandırabilirsiniz:

   - **Hedef konum**: Kaynak sanal makine verilerinizi burada çoğaltılır konumu. Seçili makineler konumuna bağlı olarak Site Recovery, uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasası konumu olarak aynı hedef konum tutmanızı öneririz.
   - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef aboneliği. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur.
   - **Hedef kaynak grubu**: Kaynak grubu için tüm çoğaltılan sanal makinelerinize ait. Varsayılan olarak Azure Site Recovery "asr" sonekine sahip adı ile hedef bölgede yeni bir kaynak grubu oluşturur. Azure Site Recovery tarafından zaten oluşturulan kaynak grubunun mevcut olması durumunda, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmek seçebilirsiniz. Hedef kaynak grubu konumunu, kaynak sanal makineleri barındırdığı bölge dışında tüm Azure bölgelerine olabilir.
   - **Hedef sanal ağ**: Varsayılan olarak, Site Recovery "asr" sonekine sahip adı ile hedef bölgede yeni bir sanal ağ oluşturur. Bu kaynak ağa eşlenen ve gelecekteki tüm koruma için kullanılır. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
   - **Depolama hesapları (kaynak makine yönetilen diskleri kullanmıyorsa) hedef**: Varsayılan olarak, Site Recovery, kaynak VM depolama yapılandırması yakından taklit eden yeni bir hedef depolama hesabı oluşturur. Depolama hesabı zaten mevcut olması durumunda, yeniden kullanılır.
   - **Yönetilen çoğaltma diskleri (kaynak sanal makine yönetilen diskleri kullanıyorsa)**: Site Recovery kaynak sanal makinenin yönetilen disklerle aynı depolama türüne (standart veya premium) kaynağın VM'ye yönetilen disk olarak yansıtmak için hedef bölgede yeni yönetilen çoğaltma diskleri oluşturur.
   - **Önbellek depolama hesapları**: Site Recovery kaynak bölgede önbellek depolama adlı ek bir depolama hesabı gerekir. Kaynak VM üzerinde'olmuyor tüm değişiklikleri izlenir ve bu hedef konuma çoğaltılmadan önce önbellek depolama hesabına gönderilir.
   - **Hedef kullanılabilirlik kümeleri**: Varsayılan olarak, Azure Site Recovery, yeni bir kullanılabilirlik Vm'leri bir kullanılabilirlik kümesi kaynak bölgede bir parçası için "asr" sonekine sahip adı ile hedef bölgede kümesi oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut durumda yeniden kullanılır.
   - **Hedef kullanılabilirlik**: Kullanılabilirlik alanları hedef bölge destekliyorsa, varsayılan olarak, Site Recovery hedef bölge kaynak bölgede aynı bölge sayısına atar.

     Hedef bölge kullanılabilirlik bölgelerini desteklemez, hedef Vm'leri varsayılan olarak tek örnekleri olarak yapılandırılır. Gerekirse, tür VM'ler 'Özelleştir' tıklayarak kullanılabilirlik kümeleri hedef bölgede bir parçası olarak yapılandırabilirsiniz.

     >[!NOTE]
     >Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türü - tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik alanı değiştiremezsiniz. Devre dışı bırakın ve çoğaltma kullanılabilirlik türünü değiştirmek etkinleştirmeniz gerekir.
     >
    
   - **Çoğaltma İlkesi**: Kurtarma noktası bekletme geçmişine ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery, ' 24 saatliğine kurtarma noktası bekletme ' 60 dakika için uygulamayla tutarlı anlık görüntü sıklığını ve varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

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
1. Tıklayın **Özelleştir:** çoğaltma ayarlarını değiştirmek için.
   - İçinde **çoklu VM tutarlılığını**, birlikte çoğaltmak istediğiniz Vm'leri seçin 
   - Bir çoğaltma grubundaki tüm makineler, yük devredildiğinde paylaşılan kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olur. Çoklu VM tutarlılığını etkinleştirmek etkileyebilir iş yükü performansı (CPU kullanımı yoğun olduğu gibi) ve yalnızca makineler aynı iş yükünü çalıştıran ve birden fazla makine arasında tutarlılık ihtiyacınız varsa kullanılmalıdır. Uygulamanın 2 sql sanal makineleri ve 2 web sunucuları varsa, örneğin, daha sonra yalnızca sql sanal makineleri çoğaltma grubunun bir parçası eklemeniz gerekir.
   - Çoğaltma grubunda en fazla 16 sanal makineler, sahip olmayı seçebilirsiniz.
   - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun. Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.
![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
2. Tıklayın **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**.
3. VM'ler için çoğaltma etkinleştirildikten sonra durumu VM sistem durumu ' kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma sırasında durum, ilerleme durumu yenilemek için biraz zaman alabilir. Tıklayın **Yenile** düğme, son durumu almak için.
>

# <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
