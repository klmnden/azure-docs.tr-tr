---
title: Azure Site Recovery, Azure Vm'leri için çoğaltma yapılandırma | Microsoft Docs
description: Bu makalede, bir Azure bölgesinden diğerine Site Recovery kullanarak Azure Vm'leri için çoğaltmayı yapılandırmak açıklar.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: asgang
ms.openlocfilehash: 86bd41d518006b0601a5c9d18e5429f76d5a4fc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64926648"
---
# <a name="replicate-azure-vms-to-another-azure-region"></a>Azure Vm'lerini başka bir Azure bölgesine çoğaltma


Bu makalede, Azure vm'leri, bir Azure bölgesinden diğerine çoğaltmayı etkinleştirmek açıklar.

## <a name="before-you-start"></a>Başlamadan önce

Bu makalede Site Recovery dağıtımı için hazırladığınıza açıklandığı varsayılır [Azure'dan Azure'a olağanüstü durum kurtarma öğretici](azure-to-azure-tutorial-enable-replication.md).

Önkoşulların yerinde olmalıdır ve bir kurtarma Hizmetleri kasası oluşturmuş olmalıdır.


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

3. İçinde **sanal makineler > sanal makineleri**tıklayın ve çoğaltmak istediğiniz her bir sanal Makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. İçinde **ayarları**, isteğe bağlı olarak hedef site ayarları yapılandırabilirsiniz:

   - **Hedef konum**: Kaynak sanal makine verilerinizi burada çoğaltılır konumu. Seçili makineler konumuna bağlı olarak Site Recovery, uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasası konumu olarak aynı hedef konum tutmanızı öneririz.
   - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef aboneliği. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur.
   - **Hedef kaynak grubu**: Kaynak grubu için tüm çoğaltılan sanal makinelerinize ait.
       - Site Recovery, varsayılan olarak hedef bölgede adında bir "asr" sonekine sahip yeni bir kaynak grubu oluşturur.
       - Site Recovery tarafından zaten oluşturulan kaynak grubu varsa, yeniden kullanılır.
       - Kaynak grubu ayarlarını özelleştirebilirsiniz.
       - Hedef kaynak grubu konumunu, kaynak VM'lerin barındırılan bölgesi dışında herhangi bir Azure bölgesine olabilir.
   - **Hedef sanal ağ**: Varsayılan olarak, Site Recovery, hedef bölgede adında bir "asr" sonekine sahip yeni bir sanal ağ oluşturur. Bu kaynak ağa eşlenen ve gelecekteki tüm koruma için kullanılır. [Daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) ağ eşlemesi hakkında.
   - **Hedef depolama hesapları (kaynak sanal makine yönetilen diskleri kullanmaz)** : Varsayılan olarak, Site Recovery, kaynak VM depolama yapılandırması yakından taklit eden yeni bir hedef depolama hesabı oluşturur. Depolama hesabı zaten mevcut olması durumunda, yeniden kullanılır.
   - **Yönetilen çoğaltma diskleri (kaynak VM, yönetilen diskler kullanır)** : Site Recovery kaynak sanal makinenin yönetilen disklerle aynı depolama türüne (standart veya premium) kaynağın VM'ye yönetilen disk olarak yansıtmak için hedef bölgede yeni yönetilen çoğaltma diskleri oluşturur.
   - **Önbellek depolama hesapları**: Site Recovery kaynak bölgede önbellek depolama adlı ek bir depolama hesabı gerekir. Kaynak VM üzerinde'olmuyor tüm değişiklikleri izlenir ve bu hedef konuma çoğaltılmadan önce önbellek depolama hesabına gönderilir.
   - **Hedef kullanılabilirlik kümeleri**: Varsayılan olarak, Site Recovery, yeni bir kullanılabilirlik hedef bölgede "asr" sonekine bir kullanılabilirlik kümesi kaynak bölgede bir parçası olan VM'ler için ad ile kümesi oluşturur. Site Recovery tarafından önceden oluşturulmuş kullanılabilirlik kümesi varsa, yeniden kullanılır.
   - **Hedef kullanılabilirlik**: Kullanılabilirlik alanları hedef bölge destekliyorsa, varsayılan olarak, Site Recovery hedef bölge kaynak bölgede aynı bölge sayısına atar.

     Hedef bölge kullanılabilirlik bölgelerini desteklemez, hedef Vm'leri varsayılan olarak tek örnekleri olarak yapılandırılır. Gerekirse, tür VM'ler 'Özelleştir' tıklayarak kullanılabilirlik kümeleri hedef bölgede bir parçası olarak yapılandırabilirsiniz.

     >[!NOTE]
     >Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türü - tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik alanı değiştiremezsiniz. Devre dışı bırakın ve çoğaltma kullanılabilirlik türünü değiştirmek etkinleştirmeniz gerekir.
     >
    
   - **Çoğaltma İlkesi**: Kurtarma noktası bekletme geçmişine ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Recovery, ' 24 saatliğine kurtarma noktası bekletme ' 60 dakika için uygulamayla tutarlı anlık görüntü sıklığını ve varsayılan ayarlarla yeni bir çoğaltma ilkesi oluşturur.

     ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

### <a name="enable-replication-for-added-disks"></a>Eklenen diskler için çoğaltmayı etkinleştirme

Çoğaltma etkin olduğu bir Azure VM diskleri eklerseniz, aşağıdakiler gerçekleşir:
-   Sanal makine için çoğaltma durumu bir uyarı gösterir ve bunu bildiren bir not bildirir veya daha fazla disk koruması için kullanılabilir.
-   Eklenen diskleri için korumayı etkinleştirin, uyarı diskin ilk çoğaltmadan sonra kaybolur.
-   Diske ait çoğaltma etkinleştirmemeyi seçerseniz, uyarıyı Kapat seçeneğini belirleyebilirsiniz.

    
    ![Yeni disk eklendi](./media/azure-to-azure-how-to-enable-replication/newdisk.png)

Eklenen bir disk için çoğaltmayı etkinleştirmek için aşağıdakileri yapın:

1.  Kasadaki > **çoğaltılan öğeler**, disk eklediğiniz VM'ye tıklayın.
2.  Tıklayın **diskleri**ve ardından çoğaltmayı etkinleştirmek istediğiniz veri diski seçin (bu disklere sahip bir **korumalı** durumu).
3.  İçinde **Disk ayrıntıları**, tıklayın **çoğaltmayı etkinleştir**.

    ![Eklenen diski için çoğaltmayı etkinleştirme](./media/azure-to-azure-how-to-enable-replication/enabled-added.png)

Disk sorunu için çoğaltma sistem durumu uyarısı etkin çoğaltma işi çalıştırır ve ilk çoğaltma bittikten sonra kaldırılır.


  
## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery tarafından kullanılan varsayılan hedef ayarlarını değiştirebilirsiniz.

1. Tıklayın **Özelleştir:** yanındaki 'Hedef aboneliği' varsayılan hedef aboneliği değiştirmek için. Aynı Azure Active Directory (AAD) kiracısında mevcut tüm abonelikleri listeden aboneliği seçin.

2. Tıklayın **Özelleştir:** varsayılan ayarlarını değiştirmek için:
    - İçinde **hedef kaynak grubu**, aboneliğin hedef konumdaki tüm kaynak grupları listesinden kaynak grubunu seçin.
    - İçinde **hedef sanal ağ**, ağ sanal ağ içinde hedef konum listesinden seçin.
    - İçinde **kullanılabilirlik kümesi**, bir kullanılabilirlik kümesi kaynak bölgede parçası iseler VM kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - İçinde **hedef depolama hesapları**, kullanmak istediğiniz hesabı seçin.

        ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
3. Tıklayın **Özelleştir:** çoğaltma ayarlarını değiştirmek için.
4. İçinde **çoklu VM tutarlılığını**, birlikte çoğaltmak istediğiniz Vm'leri seçin.
    - Bir çoğaltma grubundaki tüm makineler, yük devredildiğinde paylaşılan kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olur.
    - (CPU kullanımı yoğun olduğu gibi) çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebilir. Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden fazla makine arasında tutarlılık ihtiyacınız varsa etkinleştirilmelidir.
    - Bir uygulamanın 2 SQL Server sanal makineleri ve iki web sunucusu varsa, örneğin, ardından, yalnızca SQL Server sanal makineleri çoğaltma grubuna eklemeniz gerekir.
    - Bir çoğaltma grubunda en fazla 16 sanal makinelerinin sahip olmayı seçebilirsiniz.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar.
    - Vm'leri 20004 numaralı bağlantı noktası arasındaki dahili iletişimi engelleyen bir güvenlik duvarı Gereci bulunduğundan emin olun.
    - Sanal makineleri çoğaltma grubunun parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktasında giden trafiği belirli Linux sürümü için yönergeler göre el ile açıldığında emin olun.
![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
5. Tıklayın **hedef kaynak oluşturma** > **çoğaltmayı etkinleştirme**.
6. VM'ler için çoğaltma etkinleştirildikten sonra durumu VM sistem durumu ' kontrol edebilirsiniz **çoğaltılan öğeler**

>[!NOTE]
>İlk çoğaltma sırasında durum, ilerleme durumu yenilemek için biraz zaman alabilir. Tıklayın **Yenile** düğme, son durumu almak için.
>

# <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
