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
ms.openlocfilehash: 7619b8831d75ce639c6f6c773c7c7d491abc93e7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62116218"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Mevcut Azure Iaas sanal makinelerinizi (VM) taşımak istediğiniz çeşitli senaryolar vardır bir bölgeden diğerine. Örneğin, güvenilirlik ve yönetilebilirlik iyileştirmek veya idare nedenlerle taşımak için mevcut Vm'lerinizin kullanılabilirliğini artırmak isteyebilirsiniz. Daha fazla bilgi için [Azure VM'yi taşıma genel bakış](azure-to-azure-move-overview.md). 

Kullanabileceğiniz [Azure Site Recovery](site-recovery-overview.md) iş sürekliliği ve olağanüstü durum kurtarma (BCDR) için şirket içi makinelerin ve Azure Vm'lerinde olağanüstü durum kurtarmayı yönetmek ve düzenlemek için hizmet. Site Recovery, Azure sanal makinelerinin ikincil bir bölgeye taşıma yönetmek için de kullanabilirsiniz.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> 
> * Taşıma için önkoşulları doğrulayın
> * Kaynak VM'lerin ve hedef bölge hazırlama
> * Verileri kopyalayın ve çoğaltmayı etkinleştirin
> * Taşımayı gerçekleştirmek ve test yapılandırması
> * Kaynak bölgedeki kaynakları silme
> 
> [!NOTE]
> Bu öğreticide, Azure Vm'leri bir bölgeden diğerine olduğundan taşıma işlemini göstermektedir. Bir kullanılabilirlik kümesine sanal makinelerin taşınmasında kullanılabilirliği geliştirmek ihtiyacınız varsa, bölgeye sabitlenmiş farklı bir bölgedeki Vm'leri, bkz: [kullanılabilirlik öğretici Azure Vm'leri taşıma](move-azure-vms-avset-azone.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure sanal makineleri taşımak istediğiniz Azure bölgesinde olduğundan emin olun.
- Doğrulayın tercih ettiğiniz [kaynak bölge - hedef bölge birleşimi desteklenir](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)ve hedef bölge hakkında konusunda bilinçli bir karar.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini doğrulayın. Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin Yöneticisi olursunuz. Abonelik Yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Bir sanal Makineye yönelik çoğaltmayı etkinleştirmek ve temelde Azure Site Recovery kullanarak veri kopyalama için şunlara sahip olmalısınız:

    * Azure kaynaklarında sanal makine oluşturma izinleri. Sanal makine Katılımcısı yerleşik rolü kapsayan şu izinlere sahiptir:
        - Seçilen kaynak grubunda sanal makine oluşturma izni
        - Seçilen sanal ağda sanal makine oluşturma izni
        - Seçilen depolama hesabına yazma izni

    * Azure Site Recovery işlemlerini yönetmek için izinler. Site Recovery katkıda bulunanı rolü, Kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="prepare-the-source-vms"></a>Kaynak Vm'leri hazırlama

1. En son kök sertifikalar taşımak istediğiniz Azure Vm'leri üzerinde olduğundan emin olun. Sanal makinede en son kök sertifikalar mevcut değilse güvenlik kısıtlamaları veri kopyalama için hedef bölgede engeller.

    - Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
    - Linux VM'ler için sanal makinede en son güvenilir kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.
1. Taşımak istediğiniz sanal makineler için ağ bağlantısını denetlemek için bir kimlik doğrulaması Ara sunucusu kullanmadığınızdan emin olun.
1. Taşımaya çalışıyorsunuz VM internet erişimi yok ya da giden erişimi denetlemek için bir güvenlik duvarı proxy kullandığı [gereksinimleri kontrol](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).
1. Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirleyin. Bu içerir, ancak yük Dengeleyiciler, ağ güvenlik grupları (Nsg'ler) için sınırlı değildir ve genel IP'ler.

## <a name="prepare-the-target-region"></a>Hedef bölge hazırlama

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturmanıza olanak verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

1. Aboneliğinizin kaynak VM'lerin eşleşen boyutları ile Vm'leri desteklemek için yeterli kaynaklara sahip olduğunu doğrulayın. Site Recovery, hedef veri kopyalamak için Site Recovery kullanıyorsanız, aynı boyutta veya hedef sanal makine için olası en yakın boyutu seçer.

1. Kaynak ağ düzeninde tanımlanan her bileşeni için bir hedef kaynak oluşturduğunuzdan emin olun. Bu adım, sanal makineleriniz kaynak bölgede olan hedef bölgedeki tüm işlevleri ve özellikleri sahip olmasını sağlamak önemlidir.

    > [!NOTE]
    > Azure Site Recovery otomatik olarak bulur ve kaynak VM için çoğaltmayı etkinleştirdiğinizde, bir sanal ağ oluşturur. Ayrıca, bir ağı önceden oluşturun ve VM için çoğaltmayı etkinleştirme kullanıcı akışında atayın. Daha sonra belirtildiği gibi diğer tüm kaynakları hedef bölgede el ile oluşturmanız gerekir.

     Kaynak VM yapılandırmasına bağlı olarak, ilgili ağ kaynakları en yaygın olarak oluşturmak için kullanılan, aşağıdaki belgelere bakın:

   - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
   - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
   - [Genel IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
     Tüm diğer ağ bileşenleri için bkz: [belgeleri ağ](https://docs.microsoft.com/azure/#pivot=products&panel=network).

1. El ile [üretim dışı ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) istediğinizde son taşımak için hedef bölgede gerçekleştirmeden önce test yapılandırması hedef bölgede. Üretim ağ ile en az bir girişim sağlar çünkü bu adımı öneririz.

## <a name="copy-data-to-the-target-region"></a>Hedef bölge için veri kopyalama
Aşağıdaki adımlar hedef bölgeye veri kopyalamak için Azure Site Recovery kullanmayı gösterir.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Kaynak bölgesi dışında herhangi bir bölgede kasa oluşturun

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
1. Seçin **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
1. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
1. Kaynak grubunu oluşturma **ContosoRG**.
1. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için coğrafi kullanılabilirlik kısmına bakın [Azure Site kurtarma fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
1. İçinde **kurtarma Hizmetleri kasaları**seçin **genel bakış** > **ContosoVMVault** > **+ Çoğalt**.
1. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
1. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
1. Kaynak Yöneticisi dağıtım modelini seçin. Ardından **kaynak abonelik** ve **kaynak kaynak grubu**.
1. Seçin **Tamam** ayarları kaydetmek için.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Azure Vm'leri için çoğaltmayı etkinleştirin ve veri kopyalamaya başla

Site Recovery abonelik ve kaynak grubu ile ilişkili VM'lerin listesini alır.

1. Sonraki adımda, taşıma ve ardından istediğiniz VM'yi seçin **Tamam**.
1. İçinde **ayarları**seçin **olağanüstü durum kurtarma**.
1. İçinde **olağanüstü durumdan kurtarma yapılandırma** > **hedef bölge**, kendisine, çoğaltma yapacağınız hedef bölgeyi seçin.
1. Bu öğretici için diğer varsayılan ayarları kabul edin.
1. Seçin **çoğaltmayı etkinleştir**. Bu adım, sanal Makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![Çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

## <a name="test-the-configuration"></a>Test yapılandırması

1. Kasası'na gidin. İçinde **ayarları** > **çoğaltılan öğeler**, taşımak için hedef bölgeyi ve ardından istediğiniz VM'yi seçin **+ yük devretme testi**.
1. İçinde **yük devretme testi**, yük devretme için kullanılacak bir kurtarma noktası seçin:

   - **En son işlenen**: VM'nin Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle bir düşük kurtarma süresi hedefi (RTO) sağlanılır veri işlemeye zaman harcanmadığından.
   - **Uygulamayla tutarlı olan sonuncu**: Bu seçenek tüm sanal makineler en son uygulamayla tutarlı kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktasını seçin.

1. ' % S'hedef Azure seçin yapılandırmasını test etmek için Azure Vm'lerini taşımak istediğiniz sanal ağ.
    > [!IMPORTANT]
    > Test yük devretmesi için ayrı bir Azure VM ağını kullanmanızı öneririz. Çoğaltma ve sonunda Vm'lerinizi içine taşımak istediğiniz etkinleştirildiğinde ayarlandığı üretim ağı kullanmayın.
1. Taşıma testi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
1. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM'nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
1. Silmek istediğiniz VM tha taşıma testin bir parçası olarak oluşturulmuş, tıklayın **yük devretme testini Temizle** çoğaltılan öğe üzerinde. İçinde **notları**testiyle ilişkili gözlemlerinizi kaydetmek ve kaydedin.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Hedef bölgeye taşımayı gerçekleştirmek ve onaylayın

1. Kasası'na gidin. İçinde **ayarları** > **çoğaltılan öğeler**VM'yi seçin ve ardından **yük devretme**.
1. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin.
1. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
1. İş tamamlandıktan sonra sanal Makinenin hedef Azure bölgeniz beklendiği gibi görüntülenip görüntülenmediğini denetleyin.
1. İçinde **çoğaltılan öğeler**, VM'yi sağ tıklayarak seçin > **işleme**. Bu adım, hedef bölge için taşıma işlemi tamamlanır. İşleme işi tamamlanana kadar bekleyin.

## <a name="delete-the-resource-in-the-source-region"></a>Kaynak bölgedeki kaynak silme

Sanal Makineye gidin. Seçin **çoğaltma devre dışı bırakma**. Bu adım verileri kopyalama sanal makine için gelen işlemi durdurur.

> [!IMPORTANT]
> Azure Site Recovery çoğaltması için ücretlendirildiğiniz önlemek için bu adımı gerçekleştirmek önemlidir.

Kaynak kaynaklardan herhangi birini yeniden kullanmak için herhangi bir plan varsa, bu adımları izleyin:

1. 4 adımda bir parçası olarak kullanıma listelenen kaynak bölgedeki tüm ilgili ağ kaynakları silmek [kaynak Vm'leri hazırlama](#prepare-the-source-vms).
1. Karşılık gelen bir depolama hesabı kaynak bölgede silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure VM'deki farklı bir Azure bölgesine taşındı. Şimdi, taşınan sanal makine için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)

