---
title: Azure Iaas Vm'leri olarak bölgeye sabitlenmiş Vm'leri Azure Site Recovery hizmetini kullanarak başka bir Azure bölgesine Taşı | Microsoft Docs
description: Azure Iaas Vm'leri, başka bir Azure bölgesine bölgeye sabitlenmiş Vm'leri olarak taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5597f3c017ccf2dbb58b7b6b046720c8f49803c5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312302"
---
# <a name="move-azure-vms-into-availability-zones"></a>Kullanılabilirlik alanına Azure sanal makineleri taşıma
Azure kullanılabilirlik alanları, uygulamalarınızın ve verilerinizin veri merkezi arızasına karşı korur. Her kullanılabilirlik alanları, soğutma ve ağ bağımsız güç ile donatılmış bir veya daha fazla veri merkezlerinden oluşur. Dayanıklılık sağlamak için üç ayrı bölge etkinleştirilmiş tüm bölgelerde en az yoktur. Bir bölge içinde kullanılabilirlik alanlarının fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi arızasına karşı korur. Kullanılabilirlik alanları ile Azure, sektördeki en iyi % 99,99 VM çalışma SLA'sı sunar. Kullanılabilirlik alanı, bölgelerde desteklenir, belirtildiği gibi [burada](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones). 

Burada belirli bir bölge içinde 'tek örnek' olarak dağıttığınız sanal makinelerinizi ve bunları kullanılabilirlik alanı içine taşıyarak, kullanılabilirliği artırmak istiyorsanız bir senaryoda, Azure Site RECOVERY'yi kullanarak bunu yapabilirsiniz. Bu ek halinde kategorilere ayrılabilir:

- Tek Örnekli VM'ler hedef bölgede kullanılabilirlik alanları içine taşı
- Vm'leri bir kullanılabilirlik kümesindeki kullanılabilirlik hedef bölgede içine taşı

> [!IMPORTANT]
> Şu anda Azure Site Recovery Vm'lerden bölgesinde diğerine taşınmasını destekler ve bir bölge içinde taşıma desteklemiyor. 

## <a name="verify-prerequisites"></a>Önkoşulları doğrulama

- Hedef bölge olup olmadığını doğrulayın [desteklemek için kullanılabilirlik alanı](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones)-doğrulayın tercih ettiğiniz [kaynak bölge - hedef bölge birleşimi desteklenir](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) ve hedef bölge konusunda bilinçli bir karar.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini doğrulayın: Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Bir sanal makine için çoğaltmayı etkinleştirmek ve sonunda Azure Site Recovery kullanarak hedefe veri kopyalamak için şunlara sahip olmalısınız:

    1. Azure kaynaklarında sanal makine oluşturma izinleri. 'Sanal Makine Katılımcısı' yerleşik rolü, aşağıdakileri kapsayan şu izinlere sahiptir:
        - Seçilen kaynak grubunda sanal makine oluşturma izni
        - Seçilen sanal ağda sanal makine oluşturma izni
        - Seçilen depolama hesabına yazma izni

    2. Azure Site Recovery işlemlerini yönetme izni de gerekir. 'Site Recovery Katkıda Bulunanı' rolü, Kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="prepare-the-source-vms"></a>Kaynak Vm'leri hazırlama

1. Site Recovery kullanarak kullanılabilirlik alanı'na taşımak isterseniz sanal makinelerinizi yönetilen diskleri kullanıyor olması gerekir. Mevcut Windows yönetilmeyen diskler kullanan sanal makineleri (VM'ler) dönüştürebilir, belirtilen adımları izleyerek tarafından yönetilen diskleri kullanma Vm'leri dönüştürebilirsiniz [burada](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks). Bunun için kullanılabilirlik kümesi olarak yapılandırılmış olduğundan emin olun. ' yönetilebilir. 
2. En son kök sertifikalar Azure Vm'lerinde taşımak istediğiniz mevcut olduğundan emin olun. En son kök sertifikalar mevcut değilse güvenlik kısıtlamaları nedeniyle veri kopyalama hedef bölge için etkinleştirilemez.

3. Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.

4. Linux VM’ler için, sanal makinedeki en son güvenilir kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.
5. Taşımak istediğiniz sanal makineleri için ağ bağlantısını denetlemek için bir kimlik doğrulaması Ara sunucusu kullanmadığınızdan emin olun.

6. Taşıma çalıştığınız VM internet erişimi yok ve giden erişimi denetlemek için bir güvenlik duvarı Ara sunucusu kullanarak, gereksinimleri gözden geçirin [burada](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

7. Kaynak ağ düzeni ve şu anda - yük Dengeleyiciler, Nsg'ler, doğrulamanızı için genel IP vb. dahil olmak üzere kullandığınız tüm kaynakları belirleyin.

## <a name="prepare-the-target-region"></a>Hedef bölge hazırlama

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM’ler oluşturmanıza izin verdiğinden emin olun. Gerekirse gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Hedef veri kopyalamak için Site Recovery kullanıyorsanız, aynı boyutta veya hedef sanal makine için olası en yakın boyutu seçer.

3. Kaynak ağ düzeninde tanımlanan her bileşeni için bir hedef kaynak oluşturduğundan emin olun. Bu olduğundan emin olun, hedef bölge için kesme sonrası önemlidir, İşlevler ve kaynak olan özellikler sanal makinelerinizin vardır.

    > [!NOTE]
    > Azure Site Recovery otomatik olarak bulur ve sanal ağ ve depolama hesabı, kaynak VM için çoğaltmayı etkinleştirin veya Ayrıca bu kaynakların önceden oluşturma ve etkinleştirme çoğaltma adımının bir parçası sanal Makineye atamak oluşturur. Ancak diğer kaynaklar için aşağıda belirtildiği gibi bunları hedef bölgede el ile oluşturmanız gerekir.

     Kaynak VM yapılandırmasına bağlı olarak, ilgili en yaygın olarak kullanılan ağ kaynakları oluşturmak için aşağıdaki belgelere bakın.

    - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Yük Dengeleyiciler] (https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials
        
     - [Genel IP ](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Tüm diğer ağ bileşenleri için ağ ile başvurmak [belgeleri.](https://docs.microsoft.com/azure/#pivot=products&panel=network) 

> [!IMPORTANT]
> Hedef bölge yedekli yük dengeleyici kullanmak için emin olun. Daha fazla bilgi edinebilirsiniz [burada](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).

4. El ile [üretim dışı ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) son kesme üzerinden hedef bölgeye gerçekleştirmeden önce yapılandırmayı test etmek isterseniz, hedef bölgede. Bu, üretim ortamıyla çok az girişim oluşturacak ve önerilir.


> [!NOTE]
> Aşağıdaki adımları tek bir VM için kurtarma Hizmetleri kasası ve çoğaltma + tıklayıp ilgili sanal makineleri birlikte seçerek için giderek aynı birden çok VM için genişletebilirsiniz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme
Aşağıdaki adımlar hedef bölgeye veri çoğaltma işleminin sonunda kullanılabilirlik alanına taşımadan önce etkinleştirmek için Azure Site Recovery kullanma size yol gösterir.

1. Azure portalında **sanal makineler**ve kullanılabilirlik alanına taşımak istediğiniz VM'yi seçin.
2. **İşlemler** menüsünde **Olağanüstü durum kurtarma** seçeneğine tıklayın.
3. **Olağanüstü durumdan kurtarma yapılandırma** > **Hedef bölge** bölümünde, çoğaltma yapacağınız hedef bölgeyi seçin. Bu bölge sağlamak [destekler](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) kullanılabilirlik alanları.
![etkinleştir-rep-1. PNG](media/azure-vms-to-zones/enable-rep-1.PNG)

1. Seçin **sonraki: Gelişmiş ayarlar**
2. Hedef subscriptiom, hedef VM kaynak grubu ve sanal ağ için appropraite değerleri seçin.
3. İçinde **kullanılabilirlik** bölümünde, istediğiniz sanal Makineyi taşımak kullanılabilirlik bölgesi seçin. 
> [!NOTE]
> Kullanılabilirlik kümesi veya kullanılabilirlik bölgesi seçeneğini görmüyorsanız, lütfen emin [önkoşulları](#prepare-the-source-vms) karşılandığından ve [hazırlık](#prepare-the-source-vms) kaynağını Vm'leri getirildiğinden.

   ![etkinleştir-rep-2. PNG](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Çoğaltma Etkinleştir'i tıklatın. Bu, sanal makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

## <a name="verify-settings"></a>Ayarları doğrulama

Çoğaltma işlemi bittikten sonra, çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı test edebilirsiniz.

1. VM menüsünde, **Olağanüstü durum kurtarma** seçeneğine tıklayın.
2. Çoğaltma durumunu, oluşturulan kurtarma noktalarını ve haritadaki kaynak ve hedef bölgelerini doğrulayabilirsiniz.

   ![Çoğaltma durumu](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>Test yapılandırması

1. Sanal makine üzerinde menüden **olağanüstü durum kurtarma**.
2. Tıklayın **yük devretme testi** simgesi.
3. **Yük Devretme Testi** bölümünde, yük devretmede kullanılması için bir kurtarma noktası seçin:

   - **En son işlenen**: VM'nin Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük RTO sağlanılır (Kurtarma Süresi Hedefi)
   - **Uygulamayla tutarlı olan sonuncu**: Bu seçenek tüm sanal makineler en son uygulamayla tutarlı kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktasını seçin.

3. Test yapılandırması için Azure Vm'lerini taşımak istediğiniz Azure sanal ağı seçin test hedefleyin. 

> [!IMPORTANT]
> Test hataları ve üretim ağdaki Vm'lerinizi sonunda taşımak istediğiniz hedef bölgede için ayrı bir Azure VM ağını kullanmanızı öneririz.

4. Taşıma testi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Silmek istediğiniz taşıma testin bir parçası olarak oluşturulan VM, tıklayın **yük devretme testini Temizle** çoğaltılan öğe üzerinde. İçinde **notları**testiyle ilişkili gözlemlerinizi kaydetmek ve kaydedin.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Taşımak için hedef bölgede gerçekleştirir ve onaylayın.

1.  Sanal makine üzerinde menüden **olağanüstü durum kurtarma**.
2. Tıklayarak **yük devretme** simgesi.
3. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin. 
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz. 
5. İş tamamlandıktan sonra sanal Makinenin hedef Azure bölgeniz beklendiği gibi görüntülenip görüntülenmediğini denetleyin.
6. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Yürüt**’e tıklayın. Bu, hedef bölge için taşıma işlemi tamamlanır. İşleme iş tamamlanana kadar bekleyin.

## <a name="discard-the-resource-in-the-source-region"></a>Kaynak bölgedeki kaynak atma 

1. Sanal Makineye gidin.  Tıklayarak **çoğaltma devre dışı bırakma**.  Bu VM için veri kopyalama işlemini durdurur.  

> [!IMPORTANT]
> Site Recovery çoğaltma sonrası için taşıma ücret önlemek için yukarıdaki adımı gerçekleştirmek önemlidir. Kaynak çoğaltma ayarları otomatik olarak temizlenir. Çoğaltma bir parçası yüklenen Site Recovery uzantısı kaldırılmaz ve el ile kaldırılması gerekiyor lütfen unutmayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure VM kullanılabilirliği, bir kullanılabilirlik kümesi veya kullanılabilirlik alanı taşıyarak artırdık. Şimdi taşınan sanal makine için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)


