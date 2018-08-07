---
title: Azure Otomasyonu DSC ile çalışmaya başlama
description: Açıklama ve en yaygın görevleri, Azure Otomasyonu Desired State Configuration (DSC) örnekleri
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 08/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9f2312064e9fb7676d5609ee077d5ed7e02e8f30
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524473"
---
# <a name="getting-started-with-azure-automation-dsc"></a>Azure Otomasyonu DSC ile çalışmaya başlama

Bu makalede ile Azure Otomasyonu Desired State Configuration (oluşturma, alma ve yapılandırmaları, makineleri yönetmek için hazırlama derleme ve raporları görüntüleme gibi DSC), en yaygın görevlerin nasıl yapılacağını açıklar. Hangi Azure Automation DSC genel bir bakış için bkz. [Azure Automation DSC genel bakış](automation-dsc-overview.md). DSC belgeleri için bkz. [Windows PowerShell Desired State Configuration genel](/powershell/dsc/overview).

Bu makalede, Azure Automation DSC kullanarak adım adım yönergeler sağlar. Bu makalede açıklanan adımları izleyerek olmadan ayarlamış bir örnek ortamı istiyorsanız, aşağıdaki Resource Manager şablonu kullanabilirsiniz: Bu şablon, Azure VM gibi bir tamamlanmış Azure Automation DSC ortamını ayarlama ayarlar Azure Automation DSC tarafından yönetilir.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örnekleri tamamlamak için aşağıdakiler gereklidir:

* Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
* Azure Resource Manager VM (Klasik değil) Windows Server 2008 R2 çalıştıran veya üzeri. VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>DSC yapılandırması oluşturma

Basit bir [DSC Yapılandırması](/powershell/dsc/configurations) varlığı veya yokluğu sağlar **Web-Server** Windows özelliği (düğümleri nasıl atadığınız bağlı olarak IIS).

1. Windows PowerShell ISE'yi (veya herhangi bir metin düzenleyicisinde) başlatın.
1. Aşağıdaki metni yazın:

    ```powershell
    configuration TestConfig
    {
        Node IsWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true

            }
        }

        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'

            }
        }
    }
    ```
1. Dosyayı Farklı Kaydet `TestConfig.ps1`.

Bu yapılandırma, her düğüm bloğunda bir kaynak çağırır [WindowsFeature kaynağı](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), varlığı veya yokluğu sağlar **Web-Server** özelliği.

## <a name="importing-a-configuration-into-azure-automation"></a>Bir yapılandırma Azure Automation'a aktarma

Ardından, yapılandırmayı Otomasyon hesabına aktarın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **DSC yapılandırmaları** altında **yapılandırma yönetimi**.
1. Üzerinde **DSC yapılandırmaları** sayfasında **+ yapılandırma Ekle**.
1. Üzerinde **alma Yapılandırması** sayfasında, Gözat `TestConfig.ps1` bilgisayarınızda dosyanın bulunduğu.

    ![Ekran görüntüsü ** ** içeri aktarma yapılandırma sayfası](./media/automation-dsc-getting-started/AddConfig.png)
1. **Tamam** düğmesine tıklayın.

## <a name="viewing-a-configuration-in-azure-automation"></a>Azure Otomasyonu'nda bir yapılandırmayı görüntüleme

Yapılandırma içeri aktardıktan sonra Azure portalında görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **DSC yapılandırmaları** altında **yapılandırma yönetimi**.
1. Üzerinde **DSC yapılandırmaları** sayfasında **TestConfig** (Bu önceki yordamda aktardığınız yapılandırmayı adıdır).
1. Üzerinde **TestConfig yapılandırma** sayfasında **yapılandırma kaynağını görüntüle**.

    ![TestConfig yapılandırma sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/ViewConfigSource.png)

    A **TestConfig yapılandırma kaynağı** sayfası açılır, yapılandırma PowerShell kodunu görüntüleme.

## <a name="compiling-a-configuration-in-azure-automation"></a>Azure automation'da bir yapılandırma derleme

Bir düğüm için istenen durumu uygulayabilmeniz için önce bu durum tanımlayan bir DSC yapılandırması gerekir bir veya daha fazla düğüm yapılandırmaları (MOF belgesi) derlenmiş ve Automation DSC çekme sunucusuna yerleştirilen. Azure Otomasyonu DSC yapılandırmaları derleme daha ayrıntılı açıklaması için bkz [Azure Automation DSC yapılandırmaları derleme](automation-dsc-compile.md). Derleme yapılandırmaları hakkında daha fazla bilgi için bkz. [DSC yapılandırmaları](/powershell/dsc/configurations).

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **DSC yapılandırmaları** altında **yapılandırma yönetimi**.
1. Üzerinde **DSC yapılandırmaları** sayfasında **TestConfig** (daha önce içeri aktarılan yapılandırma adı).
1. Üzerinde **TestConfig yapılandırma** sayfasında **derleme**ve ardından **Evet**. Bu, bir derleme işi başlatır.

    ![Derleme düğmesi vurgulama TestConfig yapılandırma sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Bir Azure Otomasyonu yapılandırmasında derlediğinizde, çekme sunucusuna otomatik olarak oluşturulan düğümü yapılandırmaların MOF'lar dağıtır.

## <a name="viewing-a-compilation-job"></a>Derleme işi görüntüleme

Bir derleme başlattıktan sonra içinde görüntüleyebilirsiniz **derleme işleri** kutucuğu **yapılandırma** sayfası. **Derleme işleri** kutucuk şu anda çalışan gösterir, tamamlanmış ve başarısız olan işler. Bir derleme işi sayfasını açtığınızda, herhangi bir hata veya uyarıyla karşılaştı dahil olmak üzere bu iş hakkındaki bilgileri gösterir, giriş parametrelerini yapılandırma ve derleme günlükleri kullanılmadı.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **DSC yapılandırmaları** altında **yapılandırma yönetimi**.
1. Üzerinde **DSC yapılandırmaları** sayfasında **TestConfig** (daha önce içeri aktarılan yapılandırma adı).
1. Altında **derleme işleri**, görüntülemek istediğiniz derleme işi seçin. A **derleme işi** sayfası açılır, derleme işi başlatıldığı tarihten etiketli.

    ![Derleme işi sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/CompilationJob.png)
1. Herhangi bir kutucuğa tıklayın **derleme işi** görmek için sayfayı daha fazla iş hakkında ayrıntılar.

## <a name="viewing-node-configurations"></a>Düğüm yapılandırmaları görüntüleme

Bir derleme işinin başarıyla tamamlandığında, bir veya daha fazla yeni düğüm yapılandırması oluşturur. Düğüm yapılandırması çekme sunucusuna ve çekilir ve bir veya daha fazla düğüm tarafından uygulanan hazır dağıtılan bir MOF belgedir. Otomasyon hesabınızda düğüm yapılandırmaları görüntüleyebilirsiniz **DSC düğüm yapılandırmaları** sayfası. Düğüm yapılandırması form olan bir ada sahip *ConfigurationName*. *NodeName*.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Hub menüsünde **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)**, ardından **yapılandırmaları**.

    ![DSC düğüm yapılandırmaları sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Azure Otomasyonu DSC ile yönetim için bir Azure VM ekleme

Azure Automation DSC, Azure Vm'leri (Klasik ve Resource Manager), şirket içi Vm'leri, Linux makineler, AWS VM ve şirket içi fiziksel makineleri yönetmek için kullanabilirsiniz. Bu makalede, şunların nasıl yerleşik yalnızca Azure Resource Manager Vm'lerinde. Ekleme hakkında bilgi için bkz: diğer türleri, makinelerin [makineleri Azure Automation DSC tarafından Yönetim için hazırlama](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>Eklemek için bir Azure Resource Manager VM için Azure Automation DSC Yönetimi

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** altında **yapılandırma yönetimi** seçip **düğümleri**.
1. İçinde **düğümleri** sayfasında **+ Ekle**.

    ![Azure VM Ekle düğmesi vurgulama DSC düğümleri sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/OnboardVM.png)

1. Sanal makineler sayfasındaki VM'nizi seçin. **Azure Vm'leri ekleme** sayfasında **eklenecek sanal makineleri seçin**.
1. **Bağlan**'a tıklayın.

   > [!IMPORTANT]
   > Bu Windows Server 2008 R2 çalıştıran bir Azure Resource Manager sanal makinesi olmalıdır veya üzeri.

1. İçinde **kayıt** sayfasında, sanal makineye uygulamak istediğiniz düğüm yapılandırmasının adı girin **düğüm yapılandırması adı** kutusu. Bu, Otomasyon hesabında bir düğüm yapılandırmasının adı tam olarak eşleşmelidir. Bu noktada bir ad sağlayarak, isteğe bağlıdır. Düğüm ekleme sonra atanan düğüm yapılandırmasını değiştirebilirsiniz.
   Denetleme **gerekirse düğümü yeniden başlatma**ve ardından **Tamam**.

    ![Kayıt sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/RegisterVM.png)

    Belirtilen aralıklarda tarafından belirtilen VM uygulanır düğüm yapılandırmasının **yapılandırma modu sıklığı**, ve VM düğüm yapılandırması tarafından belirtilen aralıklarla güncelleştirmeleri denetler **Yenile Sıklık**. Bu değerleri nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [yerel Configuration Manager Yapılandırma](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
1. İçinde **Azure Vm'leri ekleme** sayfasında **Oluştur**.

Azure VM ekleme işlemini başlatır. Tamamlandığında, VM görünür **düğümleri** sekmesinde **durum yapılandırması (DSC)** Otomasyon hesabı sayfasında.

## <a name="viewing-the-list-of-dsc-nodes"></a>DSC düğümleri listesini görüntüleme

Eklenen Otomasyon hesabınızda yönetimine yönelik olan tüm makinelerin listesini görüntüleyebileceğiniz **düğümleri** sekmesinde **durum yapılandırması (DSC)** sayfası.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
3. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)**, ardından **düğümleri** sekmesi.

## <a name="viewing-reports-for-dsc-nodes"></a>DSC düğüm raporları görüntüleme

Her zaman Azure Automation DSC, yönetilen bir düğüm üzerindeki bir tutarlılık denetimi gerçekleştirir. düğüm durum raporu çekme sunucusuna geri gönderir. Bu düğüm için sayfadaki bu raporları görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
3. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** ve ardından **düğümleri** sekmesi.
4. İçinde **düğümleri** listesinde, görüntülemek istediğiniz düğümü seçin.
5. Üzerinde **düğüm** sayfasında, altında görüntülemek istediğiniz raporu tıklatın **raporları**.

    ![Rapor sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/NodeReport.png)

Her bir rapor sayfasında, aşağıdaki durum bilgileri için karşılık gelen bir tutarlılık denetimi görebilirsiniz:

* Rapor durumu — "Uyumlu", "Başarısız" yapılandırma düğümüdür ya da "Uyumlu" bir düğümdür (düğüm olduğunda **applyandmonitor** modu ve makine istenilen durumda değil).
* Tutarlılık denetimi için başlangıç saati.
* Tutarlılık denetimi için toplam çalışma zamanı.
* Tutarlılık denetimi türü.
* Hata kodu ve hata iletisi dahil olmak üzere tüm hatalar.
* Yapılandırma ve her bir kaynak (düğüm bu kaynak için istenen durumda olup) durumunu kullanılan tüm DSC kaynakları — bu kaynak için daha ayrıntılı bilgi almak için her kaynak tıklayabilirsiniz.
* Adı, IP adresi ve düğümün yapılandırma modu.

Ayrıca **ham raporu görüntüle** düğümün sunucuya gönderdiği gerçek verileri görmek için. Bu verileri kullanma hakkında daha fazla bilgi için bkz. [DSC rapor sunucusu kullanarak](https://msdn.microsoft.com/powershell/dsc/reportserver).

Bu raporun ilk kullanılabilir olmadan önce bir düğüm eklendikten sonra biraz zaman alabilir. İlk rapor için en fazla 30 dakika sonra yerleşik bir düğüm beklemeniz gerekebilir.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Farklı bir düğüm yapılandırması düğüme yeniden atama

Başlangıçta atanan olandan farklı bir düğüm yapılandırması kullanmak için bir düğüm atayabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
3. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** ve ardından **düğümleri** sekmesi.
4. Üzerinde **düğümleri** sekmesinde, atamak istediğiniz düğümün adına tıklayın.
5. Bu düğüm için sayfasında tıklayın **düğüm yapılandırması Ata**.

    ![Ata düğüm düğmesini vurgulama düğüm sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/AssignNode.png)
6. Üzerinde **düğüm yapılandırması Ata** sayfasında, düğüm yapılandırması düğüme atamak ve ardından istediğiniz **Tamam**.

    ![Düğüm yapılandırması Ata sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Bir düğümün kaydı

Azure Automation DSC tarafından yönetilecek bir düğüm artık istemiyorsanız kaydını kaldırabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
3. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** ve ardından **düğümleri** tab.git 
4. Üzerinde **düğümleri** sekmesinde, silmek istediğiniz düğümün adına tıklayın.
5. Bu düğüm için sayfasında tıklayın **Unregister**.

    ![Kaydı Kaldır düğmesine vurgulama düğüm sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>İlgili makaleler

* [Azure Automation DSC genel bakış](automation-dsc-overview.md)
* [Makineleri Azure Automation DSC tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
* [Windows PowerShell Desired State Configuration ' ne genel bakış](https://msdn.microsoft.com/powershell/dsc/overview)
* [Azure Otomasyonu DSC cmdlet'leri](/powershell/module/azurerm.automation/#automation)
* [Azure Otomasyonu DSC fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
