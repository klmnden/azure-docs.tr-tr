---
title: Azure Otomasyonu durum yapılandırması ile çalışmaya başlama
description: Açıklama ve Azure Otomasyonu durum yapılandırması (DSC) en yaygın görevleri örnekleri
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 04/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 60cd2d21167739e824489e30ebd187a5fc0cc12d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61074585"
---
# <a name="getting-started-with-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması ile çalışmaya başlama

Bu makalede, Azure Otomasyonu durumu oluşturma, alma ve yapılandırmaları, makineleri yönetmek için hazırlama derleme ve raporları görüntüleme gibi yapılandırma ile en yaygın görevlerin nasıl yapılacağını açıklar. Azure Otomasyonu durum Yapılandırması'ne genel bakış için bkz. [Azure Otomasyon durum yapılandırmasına genel bakış](automation-dsc-overview.md). Desired State Configuration ' nı (DSC) belgeleri için bkz. [Windows PowerShell Desired State Configuration genel](/powershell/dsc/overview).

Bu makalede, Azure Otomasyonu durum yapılandırmasını kullanarak adım adım yönergeler sağlar. Bu makalede açıklanan adımları izleyerek olmadan ayarlamış bir örnek ortamı istiyorsanız, aşağıdaki Resource Manager şablonu kullanabilirsiniz: [Azure Otomasyonu tarafından yönetilen düğüm şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-automation-configuration). Bu şablon, Azure Otomasyonu durumu yapılandırması tarafından yönetilen bir Azure VM gibi tamamlanmış bir Azure Otomasyonu durum yapılandırması ortamı ayarlar.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örnekleri tamamlamak için aşağıdakiler gereklidir:

- Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
- Azure Resource Manager VM (Klasik değil) çalıştıran bir [işletim sistemi desteklenen](automation-dsc-overview.md#operating-system-requirements). VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>DSC yapılandırması oluşturma

Basit bir [DSC Yapılandırması](/powershell/dsc/configurations) varlığı veya yokluğu sağlar **Web-Server** Windows özelliği (düğümleri nasıl atadığınız bağlı olarak IIS).

1. Başlangıç [VSCode](https://code.visualstudio.com/docs) (veya herhangi bir metin düzenleyicisinde).
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

Bu yapılandırma, her düğüm bloğunda bir kaynak çağırır [WindowsFeature kaynağı](/powershell/dsc/windowsfeatureresource), varlığı veya yokluğu sağlar **Web-Server** özelliği.

## <a name="importing-a-configuration-into-azure-automation"></a>Bir yapılandırma Azure Automation'a aktarma

Ardından, yapılandırmayı Otomasyon hesabına aktarın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **yapılandırmaları** sekmesine ve ardından tıklayın **+ Ekle**.
1. Üzerinde **alma Yapılandırması** sayfasında, Gözat `TestConfig.ps1` bilgisayarınızda dosyanın bulunduğu.

   ![Ekran görüntüsü ** içeri aktarma yapılandırması ** dikey penceresi](./media/automation-dsc-getting-started/AddConfig.png)

1. **Tamam** düğmesine tıklayın.

## <a name="viewing-a-configuration-in-azure-automation"></a>Azure Otomasyonu'nda bir yapılandırmayı görüntüleme

Yapılandırma içeri aktardıktan sonra Azure portalında görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **yapılandırmaları** sekmesine ve ardından tıklayın **TestConfig** (Bu önceki aktardığınız yapılandırmayı adıdır yordam).
1. Üzerinde **TestConfig yapılandırma** sayfasında **yapılandırma kaynağını görüntüle**.

   ![TestConfig yapılandırma dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/ViewConfigSource.png)

   A **TestConfig yapılandırma kaynağı** sayfası açılır, yapılandırma PowerShell kodunu görüntüleme.

## <a name="compiling-a-configuration-in-azure-automation"></a>Azure automation'da bir yapılandırma derleme

Bir düğüm için istenen durumu uygulayabilmeniz için önce bu durum tanımlayan bir DSC yapılandırması gerekir bir veya daha fazla düğüm yapılandırmaları (MOF belgesi) derlenmiş ve Automation DSC çekme sunucusuna yerleştirilen. Derleme yapılandırmaları Azure Automation durum Yapılandırması'nda daha ayrıntılı açıklaması için bkz [yapılandırmaları Azure Automation durum yapılandırmasında derleme](automation-dsc-compile.md).
Derleme yapılandırmaları hakkında daha fazla bilgi için bkz. [DSC yapılandırmaları](/powershell/dsc/configurations).

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **yapılandırmaları** sekmesine ve ardından tıklayın **TestConfig** (daha önce içeri aktarılan yapılandırma adı).
1. Üzerinde **TestConfig yapılandırma** sayfasında **derleme**ve ardından **Evet**. Bu, bir derleme işi başlatır.

   ![Derleme düğmesi vurgulama TestConfig yapılandırma sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Bir Azure Otomasyonu yapılandırmasında derlediğinizde, çekme sunucusuna otomatik olarak oluşturulan düğümü yapılandırmaların MOF'lar dağıtır.

## <a name="viewing-a-compilation-job"></a>Derleme işi görüntüleme

Bir derleme başlattıktan sonra içinde görüntüleyebilirsiniz **derleme işleri** kutucuğu **yapılandırma** sayfası. **Derleme işleri** kutucuk şu anda çalışan gösterir, tamamlanmış ve başarısız olan işler. Bir derleme işi sayfasını açtığınızda, herhangi bir hata veya uyarıyla karşılaştı dahil olmak üzere bu iş hakkındaki bilgileri gösterir, giriş parametrelerini yapılandırma ve derleme günlükleri kullanılmadı.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** sayfasında **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **yapılandırmaları** sekmesine ve ardından tıklayın **TestConfig** (daha önce içeri aktarılan yapılandırma adı).
1. Altında **derleme işleri**, görüntülemek istediğiniz derleme işi seçin. A **derleme işi** sayfası derleme işi başlatıldığı tarihten etiketli açılır.

   ![Derleme işi sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/CompilationJob.png)

1. Herhangi bir kutucuğa tıklayın **derleme işi** görmek için sayfayı daha fazla iş hakkında ayrıntılar.

## <a name="viewing-node-configurations"></a>Düğüm yapılandırmaları görüntüleme

Bir derleme işinin başarıyla tamamlandığında, bir veya daha fazla yeni düğüm yapılandırması oluşturur. Düğüm yapılandırması çekme sunucusuna ve çekilir ve bir veya daha fazla düğüm tarafından uygulanan hazır dağıtılan bir MOF belgedir. Otomasyon hesabınızda düğüm yapılandırmaları görüntüleyebilirsiniz **durum yapılandırması (DSC)** sayfası. Düğüm yapılandırması form olan bir ada sahip *ConfigurationName*. *NodeName*.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **yapılandırmaları derlenmiş** sekmesi.

   ![Derlenmiş yapılandırmaları sekmesinin Ekran görüntüsü](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması ile yönetim için bir Azure VM ekleme

Azure Vm'leri (Klasik ve Resource Manager), şirket içi Vm'leri, Linux makineler, AWS VM ve şirket içi fiziksel makineleri yönetmek için Azure Otomasyon durum Yapılandırması'nı kullanabilirsiniz. Bu makalede, şunların nasıl yerleşik yalnızca Azure Resource Manager Vm'lerinde. Ekleme hakkında bilgi için bkz: diğer türleri, makinelerin [makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-state-configuration"></a>Eklemek için bir Azure Resource Manager VM için Azure Otomasyon durum yapılandırması yönetimi

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfası üzerinde çalışırken **düğümleri** sekmesinde **+ Ekle**.

   ![Azure VM Ekle düğmesi vurgulama DSC düğümleri sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/OnboardVM.png)

1. Üzerinde **sanal makineler** sayfasında, VM'nize seçin.
1. Üzerinde **sanal makine** ayrıntı sayfası, tıklayın **+ Connect**.

   > [!IMPORTANT]
   > Bu, çalışan bir Azure Resource Manager sanal makinesi olmalıdır bir [işletim sistemi desteklenen](automation-dsc-overview.md#operating-system-requirements).

2. İçinde **kayıt** sayfasında, sanal makineye uygulamak istediğiniz düğüm yapılandırmasının adı **düğüm yapılandırması adı** kutusu. Bu noktada bir ad sağlayarak, isteğe bağlıdır. Düğüm ekleme sonra atanan düğüm yapılandırmasını değiştirebilirsiniz.
   Denetleme **gerekirse düğümü yeniden başlatma**, ardından **Tamam**.

   ![Kayıt dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/RegisterVM.png)

   Belirtilen aralıklarda tarafından belirtilen VM uygulanır düğüm yapılandırmasının **yapılandırma modu sıklığı**, ve VM düğüm yapılandırması tarafından belirtilen aralıklarla güncelleştirmeleri denetler **Yenile Sıklık**. Bu değerleri nasıl kullanıldığı hakkında daha fazla bilgi için bkz. [yerel Configuration Manager Yapılandırma](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).

Azure VM ekleme işlemini başlatır. Tamamlandığında, VM görünür **düğümleri** sekmesinde **durum yapılandırması (DSC)** Otomasyon hesabı sayfasında.

## <a name="viewing-the-list-of-managed-nodes"></a>Yönetilen düğümler listesini görüntüleme

Eklenen Otomasyon hesabınızda yönetimine yönelik olan tüm makinelerin listesini görüntüleyebileceğiniz **düğümleri** sekmesinde **durum yapılandırması (DSC)** sayfası.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **düğümleri** sekmesi.

## <a name="viewing-reports-for-managed-nodes"></a>Yönetilen düğümler için raporlarını görüntüleme

Azure Otomasyonu durum yapılandırması, yönetilen bir düğüm üzerindeki bir tutarlılık denetimi gerçekleştirir her zaman düğüm bir durum raporu çekme sunucusuna geri gönderir. Bu düğüm için sayfadaki bu raporları görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **düğümleri** sekmesi. Burada, yapılandırma durumu ve ayrıntıları her düğüm için genel bakış görebilirsiniz.

   ![Ekran görüntüsü, düğümü sayfası](./media/automation-dsc-getting-started/NodesTab.png)

1. Üzerinde çalışırken **düğümleri** sekmesini ve ardından Raporlama'yı açmak için düğümü kaydı. Raporlama ek ayrıntıları görüntülemek istediğiniz raporu tıklatın.

   ![Raporu dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/NodeReport.png)

Her bir rapor dikey penceresinde, aşağıdaki durum bilgileri için karşılık gelen bir tutarlılık denetimi görebilirsiniz:

- Rapor durumu — "Uyumlu", "Başarısız" yapılandırma düğümüdür ya da "Uyumlu" bir düğümdür (düğüm olduğunda **ApplyandMonitor** modu ve makine istenilen durumda değil).
- Tutarlılık denetimi için başlangıç saati.
- Tutarlılık denetimi için toplam çalışma zamanı.
- Tutarlılık denetimi türü.
- Hata kodu ve hata iletisi dahil olmak üzere tüm hatalar.
- Yapılandırma ve her bir kaynak (düğüm bu kaynak için istenen durumda olup) durumunu kullanılan tüm DSC kaynakları — bu kaynak için daha ayrıntılı bilgi almak için her kaynak tıklayabilirsiniz.
- Adı, IP adresi ve düğümün yapılandırma modu.

Ayrıca **ham raporu görüntüle** düğümün sunucuya gönderdiği gerçek verileri görmek için.
Bu verileri kullanma hakkında daha fazla bilgi için bkz. [DSC rapor sunucusu kullanarak](/powershell/dsc/reportserver).

Bu raporun ilk kullanılabilir olmadan önce bir düğüm eklendikten sonra biraz zaman alabilir. İlk rapor için en fazla 30 dakika sonra yerleşik bir düğüm beklemeniz gerekebilir.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Farklı bir düğüm yapılandırması düğüme yeniden atama

Başlangıçta atanan olandan farklı bir düğüm yapılandırması kullanmak için bir düğüm atayabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **düğümleri** sekmesi.
1. Üzerinde **düğümleri** sekmesinde, atamak istediğiniz düğümün adına tıklayın.
1. Bu düğüm için sayfasında tıklayın **düğüm yapılandırması Ata**.

    ![Ata düğüm yapılandırma düğmesini vurgulama düğüm ayrıntıları sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/AssignNode.png)

1. Üzerinde **düğüm yapılandırması Ata** sayfasında, düğüm yapılandırması düğüme atamak ve ardından istediğiniz **Tamam**.

    ![Düğüm yapılandırması Ata sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Bir düğümün kaydı

Azure Automation DSC tarafından yönetilecek bir düğüm artık istemiyorsanız kaydını kaldırabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, tıklayın **tüm kaynakları** ve ardından bunları Otomasyon hesabınıza adı.
1. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. Üzerinde **durum yapılandırması (DSC)** sayfasında **düğümleri** sekmesi.
1. Üzerinde **düğümleri** sekmesinde, silmek istediğiniz düğümün adına tıklayın.
1. Bu düğüm için sayfasında tıklayın **Unregister**.

    ![Kaydı Kaldır düğmesine vurgulama düğüm ayrıntıları sayfasının ekran görüntüsü](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>İlgili makaleler

- [Azure Otomasyonu durum yapılandırmasına genel bakış](automation-dsc-overview.md)
- [Makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
- [Windows PowerShell Desired State Configuration ' ne genel bakış](/powershell/dsc/overview)
- [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- [Azure Otomasyonu durumu fiyatlandırma yapılandırması](https://azure.microsoft.com/pricing/details/automation/)
