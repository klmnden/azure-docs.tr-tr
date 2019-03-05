---
title: Azure Iaas Vm'leri Azure Site Recovery hizmetini kullanarak başka bir Azure bölgesine Taşı | Microsoft Docs
description: Azure Iaas Vm'leri bir Azure bölgesinden diğerine taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: ae1e65162ffab35e6d3ef3bf84c1eb6bbe75c65f
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314305"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Burada mevcut Azure Iaas sanal makinelerinizi bir bölgeden diğerine - güvenilirlik ve yönetilebilirlik geliştirmek için veya var olan sanal makinelerinizin kullanılabilirliğini artırmak için veya VS, ayrıntılı olarakidarenedenlerdendolayıtaşımakistiyorsunuzçeşitlisenaryolarvardır.[burada](azure-to-azure-move-overview.md). Kullanmanın yanı sıra [Azure Site Recovery](site-recovery-overview.md) yönetmek ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) amacıyla şirket içi makinelerin ve Azure Vm'lerinde olağanüstü durum kurtarma düzenlemek için hizmet sitesini de kullanabilirsiniz Kurtarma yönetmek için Azure Vm'leri için ikincil bir bölgeye taşıyın.       

Bu öğreticide Azure Vm'leri, Azure Site Recovery kullanarak başka bir bölgeye taşımak gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * [Önkoşulları doğrulama](#verify-prerequisites)
> * [Kaynak Vm'leri hazırlama](#prepare-the-source-vms)
> * [Hedef bölge hazırlama](#prepare-the-target-region)
> * [Hedef bölge için veri kopyalama](#copy-data-to-the-target-region)
> * [Test yapılandırması](#test-the-configuration) 
> * [Taşımayı gerçekleştirmek](#perform-the-move-to-the-target-region-and-confirm) 
> * [Kaynak bölgedeki kaynakları AT](#discard-the-resource-in-the-source-region) 

> [!IMPORTANT]
> Bu belge, Azure Vm'leri bir bölgeden diğerine olduğu gibi gereksinim, bir kullanılabilirlik bölgeye sabitlenmiş Vm'leri için farklı bir bölgede kümesindeki sanal makinelerin taşınmasında kullanılabilirliği geliştirmek için ise başvuru olarak Öğreticisine taşımak için size kılavuzluk eder [burada](move-azure-VMs-AVset-Azone.md).

## <a name="verify-prerequisites"></a>Önkoşulları doğrulama

- Azure sanal makineleri taşımak istediğiniz Azure bölgesinde olduğundan emin olun.
- Doğrulama tercih ettiğiniz [kaynak bölge - hedef bölge birleşimi desteklenir](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) ve hedef bölge konusunda bilinçli bir karar
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini doğrulayın: Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Bir sanal Makineye yönelik çoğaltmayı etkinleştirmek ve temelde Azure Site Recovery kullanarak veri kopyalama için şunlara sahip olmalısınız:

    1. Azure kaynaklarında sanal makine oluşturma izinleri. 'Sanal Makine Katılımcısı' yerleşik rolü, aşağıdakileri kapsayan şu izinlere sahiptir:
        - Seçilen kaynak grubunda sanal makine oluşturma izni
        - Seçilen sanal ağda sanal makine oluşturma izni
        - Seçilen depolama hesabına yazma izni

    2. Azure Site Recovery işlemlerini yönetme izni de gerekir. 'Site Recovery Katkıda Bulunanı' rolü, Kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="prepare-the-source-vms"></a>Kaynak Vm'leri hazırlama

1. En son kök sertifikalar Azure Vm'lerinde taşımak istediğiniz mevcut olduğundan emin olun. En son kök sertifikalar mevcut değilse güvenlik kısıtlamaları nedeniyle veri kopyalama hedef bölge için etkinleştirilemez.

    - Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
    - Linux VM’ler için, sanal makinedeki en son güvenilir kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.
2. Taşımak istediğiniz sanal makineleri için ağ bağlantısını denetlemek için bir kimlik doğrulaması Ara sunucusu kullanmadığınızdan emin olun.
3. Taşıma çalıştığınız VM erişimi yoksa, internet olan gereksinimleri gözden geçirin, giden erişimi denetlemek için bir güvenlik duvarı proxy kullanıyor [burada](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).
4. Kaynak ağ düzeni ve şu anda kullanma - dahil olmak üzere ancak bunlarla sınırlı olmamak üzere yük Dengeleyiciler, Nsg'ler, genel IP vb. tüm kaynakları belirleyin.

## <a name="prepare-the-target-region"></a>Hedef bölge hazırlama

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM’ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Hedef veri kopyalamak için Site Recovery kullanıyorsanız, aynı boyutta veya hedef sanal makine için olası en yakın boyutu seçer.

3. Kaynak ağ düzeninde tanımlanan her bileşeni için bir hedef kaynak oluşturduğundan emin olun. Bu olduğundan emin olun, hedef bölge için kesme sonrası önemlidir, İşlevler ve kaynak olan özellikler sanal makinelerinizin vardır.

    > [!NOTE]
    > Azure Site Recovery otomatik olarak bulur ve kaynak VM için çoğaltmayı etkinleştirin veya Ayrıca bir ağı önceden oluşturun ve VM için çoğaltmayı etkinleştirme kullanıcı akışında atama bir sanal ağ oluşturur. Ancak diğer kaynaklar için aşağıda belirtildiği gibi bunları hedef bölgede el ile oluşturmanız gerekir.

     En sık kullanılan ağ kaynakları, kaynak VM yapılandırmasına bağlı olarak ilgili oluşturmak için aşağıdaki belgelere bakın.

    - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Genel IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
    Tüm diğer ağ bileşenleri için ağ ile başvurmak [belgeleri.](https://docs.microsoft.com/azure/#pivot=products&panel=network) 

4. El ile [üretim dışı ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) son kesme üzerinden hedef bölgeye gerçekleştirmeden önce yapılandırmayı test etmek isterseniz, hedef bölgede. Bu, üretim ile en az bir girişim oluşturacak ve önerilir.
    
## <a name="copy-data-to-the-target-region"></a>Hedef bölge için veri kopyalama
Aşağıdaki adımların Azure Site Recovery hedef bölgeye veri kopyalamak için nasıl kullanılacağını size yol gösterir.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Kasayı, kaynak bölgesi dışında herhangi bir bölgede oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. Tıklayın **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
3. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Birden fazla aboneliğiniz varsa bir. Abonelik, uygun olanı seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Kurtarma Hizmetleri kasalarında tıklayın **genel bakış** > **ConsotoVMVault** > **+ Çoğalt**.
7. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
8. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
9. Kaynak Yöneticisi dağıtım modelini seçin. Ardından **kaynak abonelik** ve **kaynak kaynak grubu**.
10. Ayarları kaydetmek için **Tamam**’a tıklayın.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Azure Vm'leri için çoğaltmayı etkinleştirin ve veri kopyalamaya başlayın.

Site Recovery, abonelik ve kaynak grubu ile ilişkili VM’lerin listesini alır.

1. Sonraki adımda. Taşımak istediğiniz VM'yi seçin. Daha sonra, **Tamam**'a tıklayın.
3. **Ayarlar** menüsünde **Olağanüstü durum kurtarma** seçeneğine tıklayın.
4. **Olağanüstü durumdan kurtarma yapılandırma** > **Hedef bölge** bölümünde, çoğaltma yapacağınız hedef bölgeyi seçin.
5. Bu öğretici için diğer varsayılan ayarları kabul edin.
6. **Çoğaltmayı etkinleştir**’e tıklayın. Bu, sanal makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>Test yapılandırması


1. Kasası'na gidebilirsiniz **ayarları** > **çoğaltılan öğeler**, sanal makine için hedef bölgede taşımak için istediğinize tıklayın **+ yük devretme testi** simge.
2. **Yük Devretme Testi** bölümünde, yük devretmede kullanılması için bir kurtarma noktası seçin:

   - **En son işlenen**: VM'nin Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük RTO sağlanılır (Kurtarma Süresi Hedefi)
   - **Uygulamayla tutarlı olan sonuncu**: Bu seçenek tüm sanal makineler en son uygulamayla tutarlı kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktasını seçin.

3. ' % S'hedef Azure seçin yapılandırmasını test etmek için Azure Vm'lerini taşımak istediğiniz sanal ağ. 

> [!IMPORTANT]
> Test yük devretmesi ve değil çoğaltmayı etkinleştirdiğinizde ayarlanmış, Vm'leri sonunda taşımak istediğiniz üretim ağı için ayrı bir Azure VM ağını kullanmanızı öneririz.

4. Taşıma testi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Silmek istediğiniz taşıma testin bir parçası olarak oluşturulan VM, tıklayın **yük devretme testini Temizle** çoğaltılan öğe üzerinde. İçinde **notları**testiyle ilişkili gözlemlerinizi kaydetmek ve kaydedin.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Taşımak için hedef bölgede gerçekleştirir ve onaylayın.

1.  Kasası'na gidebilirsiniz **ayarları** > **çoğaltılan öğeler**, sanal makineye tıklayın ve ardından **yük devretme**.
2. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz. 
4. İş tamamlandıktan sonra sanal Makinenin hedef Azure bölgeniz beklendiği gibi görüntülenip görüntülenmediğini denetleyin.
5. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Yürüt**’e tıklayın. Bu, hedef bölge için taşıma işlemi tamamlanır. İşleme iş tamamlanana kadar bekleyin.

## <a name="discard-the-resource-in-the-source-region"></a>Kaynak bölgedeki kaynak atma 

1. Sanal Makineye gidin.  Tıklayarak **çoğaltma devre dışı bırakma**.  Bu VM için veri kopyalama işlemini durdurur.  

> [!IMPORTANT]
> ASR çoğaltma için ücret önlemek için bu adımı gerçekleştirmek önemlidir.

Lütfen kaynak kaynaklardan herhangi birini yeniden kullanmak için herhangi bir plan varsa sonraki adım kümesini ile devam edin.

1. Adım 4'te bir parçası olarak kullanıma listelenen kaynak bölgedeki tüm ilgili ağ kaynakları silmek için devam [kaynak Vm'leri hazırlama](#prepare-the-source-vms) 
2. Karşılık gelen bir depolama hesabı kaynak bölgede silin.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure sanal makinesi farklı bir Azure bölgesine taşındı. Şimdi taşınan sanal makine için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)

