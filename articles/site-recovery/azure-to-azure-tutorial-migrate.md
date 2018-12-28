---
title: Azure Iaas Vm'leri Azure Site Recovery hizmetini kullanarak başka bir Azure bölgesine Taşı | Microsoft Docs
description: Azure Iaas Vm'leri bir Azure bölgesinden diğerine taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: dc27e49cc67a902bb45b1d889bb61b1f4b3aab83
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53788778"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Kullanmanın yanı sıra [Azure Site Recovery](site-recovery-overview.md) yönetmek ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) amacıyla şirket içi makinelerin ve Azure Vm'lerinde olağanüstü durum kurtarma düzenlemek için hizmet sitesini de kullanabilirsiniz Kurtarma yönetmek için Azure Vm'leri için ikincil bir bölgeye taşıyın. Azure sanal makineleri taşımak için makinelere yönelik çoğaltmayı etkinleştirir ve bunları birincil bölgeden seçtiğiniz ikincil bölgeye yük devretmek.

Bu öğreticide Azure Vm'leri, başka bir bölgeye taşımak gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kurtarma hizmetleri kasası oluşturma
> * VM için çoğaltmayı etkinleştirme
> * Sanal Makineyi taşımak için yük devretme çalıştırma

Bu öğreticide önceden bir Azure aboneliğiniz olduğu varsayılır. Yoksa, başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.





## <a name="prerequisites"></a>Önkoşullar

- Azure sanal makineleri taşımak istediğiniz Azure bölgesinde olduğundan emin olun.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.



## <a name="before-you-start"></a>Başlamadan önce

Çoğaltma ayarlamadan önce bu adımları tamamlayın.


### <a name="verify-target-resources"></a>Hedef kaynaklarını doğrulama

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM’ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Site Recovery, hedef sanal makine için aynı boyutu veya mümkün olan en yakın boyutu seçer.


### <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunlara sahip olmalısınız:

1. Azure kaynaklarında sanal makine oluşturma izinleri. 'Sanal Makine Katılımcısı' yerleşik rolü, aşağıdakileri kapsayan şu izinlere sahiptir:
    - Seçilen kaynak grubunda sanal makine oluşturma izni
    - Seçilen sanal ağda sanal makine oluşturma izni
    - Seçilen depolama hesabına yazma izni

2. Azure Site Recovery işlemlerini yönetme izni de gerekir. 'Site Recovery Katkıda Bulunanı' rolü, Kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.


### <a name="verify-vm-outbound-access"></a>Sanal makine giden erişimini doğrulama

1. Taşımak istediğiniz sanal makineleri için ağ bağlantısını denetlemek için bir kimlik doğrulaması Ara sunucusu kullanmadığınızdan emin olun. 
2. Bu öğreticinin amaçları doğrultusunda, taşımak istediğiniz sanal makinelerin İnternet'e erişebilir ve giden erişimi denetlemek için bir güvenlik duvarı proxy kullanmıyorsanız varsayıyoruz. Öyleyse [buradan](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity) gereksinimleri denetleyin.

### <a name="verify-vm-certificates"></a>Sanal makine sertifikalarını doğrulama

En son kök sertifikalar Azure Vm'lerinde, taşımak istediğiniz mevcut olduğundan emin olun. En son kök sertifikalar mevcut değilse güvenlik kısıtlamaları nedeniyle sanal makine, Site Recovery’ye kaydedilemez.

- Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
- Linux VM’ler için, sanal makinedeki en son güvenilir kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.



## <a name="create-a-vault"></a>Kasa oluşturma

Kasayı, kaynak bölgesi dışında herhangi bir bölgede oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup ve Site Recovery** öğesine tıklayın.
3. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Panodan kasaya hızlıca erişmek için önce **Panoya sabitle** seçeneğine ve sonra **Oluştur**’a tıklayın.

   ![Yeni kasa](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.






## <a name="select-the-source"></a>Kaynağı seçme

1. Kurtarma Hizmetleri kasalarında **ConsotoVMVault** > **+Çoğalt** seçeneğine tıklayın.
2. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
3. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
4. Kaynak Yöneticisi dağıtım modelini seçin. Ardından **Çıkış kaynağı grubu**’nu seçin.
5. Ayarları kaydetmek için **Tamam**’a tıklayın.


## <a name="enable-replication-for-azure-vms"></a>Azure VM’leri için çoğaltmayı etkinleştirme

Site Recovery, abonelik ve kaynak grubu ile ilişkili VM’lerin listesini alır.


1. Azure portalında **Sanal makineler**’e tıklayın.
2. Taşımak istediğiniz VM'yi seçin. Daha sonra, **Tamam**'a tıklayın.
3. **Ayarlar** menüsünde **Olağanüstü durum kurtarma** seçeneğine tıklayın.
4. **Olağanüstü durumdan kurtarma yapılandırma** > **Hedef bölge** bölümünde, çoğaltma yapacağınız hedef bölgeyi seçin.
5. Bu öğretici için diğer varsayılan ayarları kabul edin.
6. **Çoğaltmayı etkinleştir**’e tıklayın. Bu, sanal makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde makineye tıklayın ve ardından **Yük devretme** seçeneğine tıklayın.
2. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Azure VM’nin Azure’da beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.
5. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Yürüt**’e tıklayın. Bu, geçiş işlemini sona erdirir.
6. Yürütme sona erdikten sonra **Çoğaltmayı Devre Dışı Bırak**’a tıklayın.  Bu, sanal makine için çoğaltma işlemini durdurur.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure sanal makinesi farklı bir Azure bölgesine taşındı. Şimdi taşınan sanal makine için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)

