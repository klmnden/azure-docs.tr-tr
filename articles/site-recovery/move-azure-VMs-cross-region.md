---
title: Azure Site Recovery hizmetini kullanarak Azure Iaas Vm'leri için başka bir Azure bölgesine Taşı | Microsoft Docs
description: Azure Iaas Vm'leri bir Azure bölgesinden diğerine taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 10966a7e658e02f04137b594fc12ec09cb676cf8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793734"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Hizmet (Iaas) sanal makineler Azure altyapı bir bölgeden diğerine güvenilirlik, kullanılabilirlik, yönetim veya idare geliştirmek için taşımak isteyebilirsiniz. Bu öğreticide Azure Site RECOVERY'yi kullanarak VM'lerin başka bir bölgeye taşıma gösterilmektedir. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları doğrulama
> * Kaynak Vm'leri hazırlama
> * Hedef bölge hazırlama
> * Hedef bölge için veri kopyalama
> * Test yapılandırması
> * Taşımayı gerçekleştirmek
> * Kaynak bölgesi kaynaklardan AT


> [!IMPORTANT]
> Bu makalede, Azure Vm'leri bir bölgeden diğerine taşımak açıklanır *olduğundan*. Amacınız, VM'ler için kullanılabilirlik alanları taşıyarak altyapınızı kullanılabilirliğini artırmak için bkz: [taşıma Azure Vm'leri için kullanılabilirlik alanları](move-azure-vms-avset-azone.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure Vm'leri kaynak taşımak istediğiniz Azure bölgesi içinde olduğundan emin olun *gelen*.
- Doğrulayın tercih ettiğiniz [kaynak bölge hedef bölge birleşimi desteklenir](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)ve dikkatli bir şekilde hedef bölgeyi seçin.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini doğrulayın. Hemen ücretsiz Azure hesabınızı oluşturduysanız *,* aboneliğinizin Yöneticisi olursunuz. Yönetici değilseniz, ihtiyaç duyduğunuz izinleri almak için yöneticiyle birlikte çalışın:
  -  Site Recovery kullanarak hedef VM ve kopya veri yönelik çoğaltmayı etkinleştirmek için Azure kaynaklarınızın bir VM oluşturmak için izinleri olmalıdır. Sanal Makine Katılımcısı yerleşik rolü bu izinlere sahiptir. İzinleri ile şunları yapabilirsiniz:
        - Seçilen kaynak grubunda sanal makine oluşturma.
        - Seçilen sanal ağda sanal makine oluşturma.
        - Seçilen depolama hesabına yazma.

  - Site Recovery işlemlerini yönetme izni de gerekir. Site Recovery katkıda bulunanı rolü, bir Azure kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="prepare-the-source-vms"></a>Kaynak Vm'leri hazırlama

1. Azure Vm'leri taşımayı planlıyorsanız, en son kök sertifikalar olup olmadığını denetleyin. Yoksa hedef bölgeye veri kopyalama güvenlik kısıtlamaları nedeniyle etkinleştirilemiyor.

    - Böylece tüm güvenilen kök sertifikaların tamamı makinede mevcut Windows Vm'leri için en son Windows güncelleştirmelerini yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
    - Linux VM'ler için en son güvenilir kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtıcınız yönergeleri izleyin.
2. Taşımayı planlıyorsanız VM'ler için ağ bağlantısını denetlemek için bir kimlik doğrulaması Ara sunucusu kullanmadığınızdan emin olun.
3. Taşımak istediğiniz bir VM internet erişimi yok ve bir güvenlik duvarı proxy'si giden erişimi denetlemek, onay kullanılıyorsa [gereksinimleri](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms).
4. Kaynak ağ düzeni ve şu anda, (ancak bunlarla sınırlı olmamak üzere) kullandığınız tüm kaynakları belge yük Dengeleyiciler, ağ güvenlik grupları ve genel IP adresleri için doğrulama.

## <a name="prepare-the-target-region"></a>Hedef bölge hazırlama

1. Azure aboneliğinizde olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturabilirsiniz doğrulayın. Gerekiyorsa, gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğiniz, kaynak Vm'leri desteklemek için yeterli kaynakları olduğundan emin olun. Hedef veri kopyalamak için Site Recovery kullanıyorsanız, aynı veya sanal makineleri hedeflemek için kullanılabilir en yakın boyutu seçer.

3. Kaynak ağ düzende tanımladığınız her bileşen için bir hedef kaynak oluşturduğunuzdan emin olun. Bu, sanal makinelerinizin tüm işlevleri ve özellikleri kaynak bölgede sahip oldukları hedef bölgede olacağını sağlar.

   Azure Site Recovery otomatik olarak bulur ve kaynak VM için çoğaltmayı etkinleştirdiğinizde, bir sanal ağ ve depolama hesabı oluşturur. Ayrıca, önceden bu kaynakları oluşturmak ve bunları VM için çoğaltmayı etkinleştirme adımının bir parçası atayabilirsiniz. Ancak, diğer tüm kaynakları hedef bölgede el ile oluşturmanız gerekir. Dayalı en yaygın olarak kullanılan ağ kaynakları oluşturmak için aşağıdaki belgelere bakın, kaynak VM yapılandırması:

   - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
   - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
   - [Genel IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Tüm diğer ağ bileşenleri için bkz: [Azure ağ belgeleri](https://docs.microsoft.com/azure/#pivot=products&panel=network). 

4. Taşıma işlemi, el ile gerçekleştirmeden önce test yapılandırması için [üretim dışı ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) hedef bölgede. Kurulum test üretim ortamıyla çok az girişim oluşturur ve kullanmanızı öneririz.
    
## <a name="copy-data-to-the-target-region"></a>Hedef bölge için veri kopyalama
Aşağıdaki adımları, hedef bölge için veri kopyalamak için Azure Site RECOVERY'yi kullanın.

### <a name="create-the-vault-in-any-region-except-the-source"></a>Kaynak dışında herhangi bir bölgede kasa oluşturun

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. Seçin **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
3. İçin **adı**, kolay ad belirtin **ContosoVMVault**. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için bkz: [Azure Site Recovery fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kurtarma Hizmetleri kasaları için seçin **genel bakış** > **ConsotoVMVault** > **+ Çoğalt**.
7. İçin **kaynak**seçin **Azure**.
8. İçin **kaynak konumu**, kaynak sanal makinelerinizi şu anda çalıştığı Azure bölgesi seçin.
9. Azure Resource Manager dağıtım modelini seçin. Ardından, **kaynak abonelik** ve **kaynak kaynak grubu**.
10. Seçin **Tamam** ayarları kaydetmek için.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Azure Vm'leri için çoğaltmayı etkinleştirin ve veri kopyalamaya başla

Site Recovery abonelik ve kaynak grubu ile ilişkili VM'lerin listesini alır.

1. Taşıyın ve ardından istediğiniz VM'yi seçin **Tamam**.
2. İçin **ayarları**seçin **olağanüstü durum kurtarma**.
3. İçin **olağanüstü durumdan kurtarma yapılandırma** > **hedef bölge**, çoğaltma yapıyorsanız hedef bölgeyi seçin.
4. Varsayılan hedef kaynaklar veya önceden oluşturduğunuz bu kullanmayı seçin.
5. Seçin **çoğaltmayı etkinleştir** işi başlatmak için.

   ![Çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>Test yapılandırması


1. Kasası'na gidin. İçinde **ayarları** > **çoğaltılan öğeler**, hedef bölge için taşımak istediğiniz sanal makineyi seçin. Ardından, **yük devretme testi**.
2. İçinde **yük devretme testi**, yük devretme için kullanılacak bir kurtarma noktası seçin:

   - **En son işlenen**: VM'nin Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenek, düşük kurtarma süresi hedefi (RTO) sağlar. böylece veri işlemeye zaman harcanmadığından.
   - **Uygulamayla tutarlı olan sonuncu**: Tüm sanal makineler en son uygulamayla tutarlı kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktasını seçin.

3. ' % S'hedef Azure seçin yapılandırmasını test etmek için Azure Vm'lerini taşımak istediğiniz sanal ağ.

   > [!IMPORTANT]
   > Üretim ağdaki hedef bölgede yük devretme testi için ayrı bir Azure VM ağını kullanmanızı öneririz.

4. Taşıma Sınamayı başlatmak için seçin **Tamam**. İlerleme durumunu izlemek üzere açmak için VM seçin, **özellikleri.** Ya da seçin **yük devretme testi** kasadaki işi. Ardından, **ayarları** > **işleri** > **Site Recovery işleri**.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Test etmek için oluşturulan bir VM'yi silmek için işaretleyin **yük devretme testini Temizle** çoğaltılan öğe üzerinde. Gelen **notları**test ilişkili gözlemlerinizi kaydetmek ve kaydedin.

## <a name="perform-the-move-and-confirm"></a>Taşımayı gerçekleştirmek ve onaylayın

1. Kasaya Git **ayarları** > **çoğaltılan öğeler**sanal makineyi seçin ve ardından **yük devretme**.
1. İçin **yük devretme**seçin **son**. 
2. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Ancak kapatma başarısız olsa bile yük devretme devam ediyor. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
3. İşi tamamlandığında, sanal Makinenin hedef Azure bölgeniz beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.
4. İçinde **çoğaltılan öğeler**, VM'ye sağ tıklayın ve seçin **işleme**. Bu geçiş tamamlanır. İşleme işi tamamlanana kadar bekleyin.

## <a name="discard-the-resources-from-the-source-region"></a>Kaynak bölgesi kaynaklardan AT

- Seçme ve VM Git **çoğaltmayı devre dışı bırak**. Bu VM için veri kopyalama işlemini durdurur.

  > [!IMPORTANT]
  > Taşıdıktan sonra Site Recovery çoğaltması için ücretlendirildiğiniz önlemek için bu adımı tamamlayın.

Kaynak kaynaklardan herhangi birini kullanmayı planlamıyorsanız, aşağıdaki adımları izleyin:

1. 4. adımda listelenen kaynak bölgedeki tüm ilgili ağ kaynakları silmek [kaynak Vm'leri hazırlama](#prepare-the-source-vms).
2. Karşılık gelen bir depolama hesabı kaynak bölgede silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Vm'leri için farklı bir Azure bölgesinde taşımak öğrendiniz. Artık bu sanal makineler için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)

