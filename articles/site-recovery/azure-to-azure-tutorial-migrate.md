---
title: "Azure Site Kurtarma'yı kullanarak Azure bölgeler arasında Azure Vm'leri geçirme | Microsoft Docs"
description: "Azure Iaas Vm'leri bir Azure bölgesinden diğerine geçirmek için Azure Site Recovery kullanın."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: e7b925d2daed11ee4e070cda6bcbd4a3511d9c17
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye geçirme

Kullanmanın yanı sıra [Azure Site Recovery](site-recovery-overview.md) yönetmek ve şirket içi makineler ve Azure Vm'leri olağanüstü durum kurtarma iş devamlılığı ve olağanüstü durum kurtarma (BCDR) amaçları doğrultusunda düzenlemek için hizmet sitesini de kullanabilirsiniz Azure VM'ler geçiş ikincil bir bölgeye yönetmek için kurtarma. Azure sanal makineleri geçirmek için bunlar için çoğaltmayı etkinleştirmek ve bunları birincil bölgesinden tercih ettiğiniz ikincil bölgeye yük devri.

Bu öğretici Azure Vm'leri için başka bir bölge geçirme gösterilmiştir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir kurtarma Hizmetleri kasası oluşturma
> * Bir sanal makine için çoğaltmayı etkinleştirme
> * VM geçirmek için bir yük devretmeyi çalıştırma

Bu öğretici bir Azure aboneliğiniz zaten varsa varsayar. Oluşturursanız değilse, bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) başlamadan önce.

>[!NOTE]
>
> Azure VM'ler için Site Recovery çoğaltma şu anda önizlemede değil.



## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için Azure sanal makineleri geçirmek istediğiniz bir Azure bölgesindeki gerekir. Ayrıca, çeşitli başlamadan önce doğrulamanız ayarlar vardır.


### <a name="verify-target-resources"></a>Hedef kaynakları doğrulayın

1. Azure aboneliğiniz olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturmak izin verdiğinden emin olun. Gerekli Kotayı etkinleştirmek için desteğe başvurun.

2. Aboneliğinizi VM'ler kaynağınız VM'ler eşleşen boyutlarıyla desteklemek için yeterli kaynak bulunduğundan emin olun. Site Recovery aynı boyutta veya en yakın olası boyutu hedef VM için seçer.


### <a name="verify-account-permissions"></a>Hesap izinlerini doğrulayın

Ücretsiz Azure hesabınız yalnızca oluşturduysanız, aboneliğinizi yönetici demektir. Abonelik Yöneticisi değilse, gereksinim duyduğunuz izinleri atamak için yöneticinizle birlikte çalışarak. Yeni bir sanal makine için çoğaltmayı etkinleştirmek için şunlara sahip olmalısınız:

1. Azure kaynaklarında bir VM oluşturmak için izinler. 'Sanal makine Katılımcısı' yerleşik rolü dahil bu izinlere sahiptir:
    - Seçilen kaynak grubunda bir VM oluşturma izni
    - Seçilen sanal ağ içinde bir VM oluşturma izni
    - Seçilen depolama hesabına yazma izni

2. Azure Site Recovery işlemlerini yönetme izni de gerekir. 'Site kurtarma katılımcı' rolü bir kurtarma Hizmetleri kasası Site Recovery işlemlerini yönetmek için gerekli tüm izinlere sahiptir.


### <a name="verify-vm-outbound-access"></a>VM giden erişim doğrulayın

1. Geçirmek istediğiniz VM'ler için ağ bağlantısını denetlemek için bir kimlik doğrulama proxy'si kullanmadığınızı emin olun. 
2. Geçirmek istediğiniz sanal makineleri varsayıyoruz Bu öğreticinin amaçları Internet'e erişebilir ve kullanmadığınız bir giden erişimi denetlemek için bir güvenlik duvarı proxy. Varsa, gereksinimlerini denetleyin [burada](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>VM sertifikaların doğrulama

Tüm son kök sertifikaları geçirmek istediğiniz Azure Vm'lerinde mevcut olduğundan emin olun. VM son kök sertifikaları değilseniz, Site kurtarma, güvenlik kısıtlamaları nedeniyle kaydedilemedi.

- Tüm güvenilen kök sertifikalar makinede; böylece Windows VM'ler için VM, en son Windows güncelleştirmeleri yükleyin. Bağlantısı kesilmiş bir ortamda standart Windows Update ve kuruluşunuz için sertifika güncelleştirme işlemlerini izleyin.
- Linux VM'ler için VM son güvenilen kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtımcı tarafından sağlanan yönergeleri izleyin.



## <a name="create-a-vault"></a>Bir kasa oluşturun

Kaynak bölgesi dışında herhangi bir bölgede kasası oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Yeni** > **İzleme ve Yönetim** > **Backup ve Site Recovery** seçeneğine tıklayın.
3. İçinde **adı**, kolay ad belirtin **ContosoVMVault**. Birden fazla aboneliğiniz varsa, uygun olanı seçin.
4. Bir kaynak grubu oluşturmak **ContosoRG**.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Kasa panodan hızlı bir şekilde erişmek için tıklatın **panoya Sabitle** ve ardından **oluşturma**.

   ![Yeni kasa](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

Yeni kasa eklenen **Pano** altında **tüm kaynakları**ve ana **kurtarma Hizmetleri kasaları** sayfası.






## <a name="select-the-source"></a>Bir kaynak seçin

1. Kurtarma Hizmetleri kasalarının içinde tıklatın **ConsotoVMVault** > **+ Çoğalt**.
2. İçinde **kaynak**seçin **Azure - Önizleme**.
3. İçinde **kaynak konumu**, çalışmakta her yere Vm'lerinizi Azure bölgesinde bir kaynak seçin.
4. Resource Manager dağıtım modeli seçin. Ardından **kaynak kaynak grubu**.
5. Ayarları kaydetmek için **Tamam**’a tıklayın.


## <a name="enable-replication-for-azure-vms"></a>Azure VM'ler için çoğaltmayı etkinleştirme

Site Recovery abonelik ve kaynak grubu ile ilişkili sanal makinelerin listesini alır.


1. Azure portalında tıklatın **sanal makineleri**.
2. Geçirmek istediğiniz VM seçin. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **ayarları**, tıklatın **olağanüstü durum kurtarma (Önizleme)**.
4. İçinde **olağanüstü durum kurtarma yapılandırma** > **hedef bölgesi** , çoğaltmak hedef bölgeyi seçin.
5. Bu öğretici için diğer varsayılan ayarları kabul edin.
6. Tıklatın **çoğaltmasını etkinleştir**. Bu sanal makine için çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![Çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

>[!NOTE]
  >
  > Şu anda, Azure sanal makinelerini çoğaltma yönetilen diskleri desteklenmez. 

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1. İçinde **ayarları** > **öğeleri çoğaltılan**makineye tıklayın ve ardından **yük devretme**.
2. İçinde **yük devretme**seçin **son**. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın**. Site Recovery yük devretme tetiklemeden önce kaynak VM kapatmaya çalışır. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
4. Azure VM gibi Azure'da görünüp görünmediğini kontrol edin.
5. İçinde **öğeleri çoğaltılan**, VM'ye sağ tıklayın > **tam geçiş**. Bu geçiş işlemi tamamlandıktan ve sanal makine için çoğaltmayı durdurur.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure VM farklı bir Azure bölgesine geçirildi. Şimdi, olağanüstü durum kurtarma için geçirilen VM yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma geçişten sonra ayarlayın](azure-to-azure-quickstart.md)

