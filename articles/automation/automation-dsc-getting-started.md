---
title: "Azure Otomasyonu DSC ile çalışmaya başlama | Microsoft Docs"
description: "Açıklama ve en yaygın görevleri de Azure Otomasyonu istenen durum yapılandırması (DSC) örnekleri"
services: automation
documentationcenter: na
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;gwallace
ms.openlocfilehash: e8b7d0d38f59589cbe6f82798b4e725af7b20e23
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a>Azure Otomasyonu DSC ile çalışmaya başlama
Bu makale ile Azure Otomasyonu istenen durum yapılandırması (oluşturma, alma ve yapılandırmaları, onboarding makineleri yönetmek için derleme ve raporları görüntüleme gibi DSC), en yaygın görevlerin nasıl yapılacağını açıklar. Hangi Azure Otomasyonu DSC genel bir bakış için bkz: [Azure Automation DSC genel bakış](automation-dsc-overview.md). DSC belgeler için bkz: [Windows PowerShell istenen durum yapılandırması genel bakış](https://msdn.microsoft.com/PowerShell/dsc/overview).

Bu makale Azure Otomasyonu DSC kullanarak adım adım yönergeler sağlar. Zaten bu makalede açıklanan adımları izleyerek olmadan ayarlanmış bir örnek ortamı isterseniz, aşağıdakileri kullanabilirsiniz [Resource Manager şablonu](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup). Bu şablon tamamlanmış bir Azure Otomasyonu DSC ortam, Azure Otomasyonu DSC tarafından yönetilen bir Azure VM dahil olmak üzere ayarlar.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki örneklerde tamamlamak için aşağıdakiler gereklidir:

* Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
* Bir Azure Kaynak Yöneticisi'ni VM (Klasik değil) Windows Server 2008 R2 çalıştıran veya sonraki bir sürümü. VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>DSC yapılandırması oluşturma
Basit bir [DSC Yapılandırması](https://msdn.microsoft.com/powershell/dsc/configurations) varlığının veya yokluğunun sağlar **Web sunucusu** Windows özelliği (düğümler nasıl atadığınız bağlı olarak IIS).

1. Windows PowerShell ISE (veya herhangi bir metin düzenleyicisi) başlatın.
2. Aşağıdaki metni yazın:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
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
3. Dosyayı Farklı Kaydet `TestConfig.ps1`.

Bu yapılandırma bir kaynak her düğümü blok çağırır [WindowsFeature kaynak](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), varlığı veya yokluğuna göre sağlar **Web sunucusu** özelliği.

## <a name="importing-a-configuration-into-azure-automation"></a>Bir yapılandırma Azure Automation'a içeri aktarma
Ardından, yapılandırmayı Otomasyon dikkate alın.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC yapılandırmaları**.
4. Üzerinde **DSC yapılandırmaları** dikey penceresinde tıklatın **bir yapılandırma eklemek**.
5. Üzerinde **alma Yapılandırması** dikey penceresinde, Gözat ' `TestConfig.ps1` dosyayı bilgisayarınıza.
   
    ![Ekran görüntüsü ** alma yapılandırma ** dikey penceresi](./media/automation-dsc-getting-started/AddConfig.png)
6. **Tamam** düğmesine tıklayın.

## <a name="viewing-a-configuration-in-azure-automation"></a>Azure Otomasyonu'nda bir yapılandırmayı görüntüleme
Bir yapılandırma içe aktardıktan sonra Azure Portalı'nda görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC yapılandırmaları**
4. Üzerinde **DSC yapılandırmaları** dikey penceresinde tıklatın **TestConfig** (Bu önceki yordamda içeri aktardığınız yapılandırma adıdır).
5. Üzerinde **TestConfig yapılandırma** dikey penceresinde tıklatın **yapılandırma kaynağı görüntüle**.
   
    ![TestConfig yapılandırma dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    A **TestConfig yapılandırma kaynağı** dikey penceresi açılır ve yapılandırması için PowerShell kodu görüntüleme.

## <a name="compiling-a-configuration-in-azure-automation"></a>Bir Azure Otomasyonu yapılandırmasında derleme
Bir düğüme istenilen durumu uygulayabilmeniz için önce bu duruma tanımlama DSC yapılandırması bir veya daha fazla düğüm yapılandırmaları (MOF belge) derlenmiş ve gerekir Automation DSC çekme sunucusuna yerleştirilen. Azure Otomasyonu DSC yapılandırmalarında derleme daha ayrıntılı açıklaması için bkz: [Azure Otomasyonu DSC yapılandırmalarında derleme](automation-dsc-compile.md). Derleme yapılandırmaları hakkında daha fazla bilgi için bkz: [DSC yapılandırmaları](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC yapılandırmaları**
4. Üzerinde **DSC yapılandırmaları** dikey penceresinde tıklatın **TestConfig** (daha önce içe aktarılan yapılandırma adı).
5. Üzerinde **TestConfig yapılandırma** dikey penceresinde tıklatın **derleme**ve ardından **Evet**. Bu derleme işi başlatır.
   
    ![Derleme düğmesi vurgulama TestConfig yapılandırma dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Bir Azure Otomasyonu yapılandırmasında derlerken çekme sunucusuna otomatik olarak tüm oluşturulan düğüm yapılandırması MOF dosyalarından dağıtır.
> 
> 

## <a name="viewing-a-compilation-job"></a>Derleme işi görüntüleme
Bir derleme başlattıktan sonra içinde görüntüleyebilirsiniz **derleme işleri** parçasında **yapılandırma** dikey. **Derleme işleri** döşeme şu anda çalışan gösterir, tamamlandı ve işleri başarısız oldu. Bir derleme iş dikey penceresi açtığınızda, herhangi bir hata veya uyarı karşılaştı dahil olmak üzere bu iş hakkındaki bilgileri gösterir, kullanılan yapılandırma ve derleme günlükleri giriş parametreleri.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC yapılandırmaları**.
4. Üzerinde **DSC yapılandırmaları** dikey penceresinde tıklatın **TestConfig** (daha önce içe aktarılan yapılandırma adı).
5. Üzerinde **derleme işleri** , döşeme **TestConfig yapılandırma** dikey penceresinde, listelenen işleri birini tıklatın. A **derleme işi** derleme işi başlatıldı tarihle etiketli dikey penceresi açılır.
   
    ![Derleme işi dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/CompilationJob.png)
6. Herhangi bir kutucuğu tıklayın **derleme işi** dikey penceresini görmek için daha fazla iş hakkında ayrıntılar.

## <a name="viewing-node-configurations"></a>Düğüm yapılandırmaları görüntüleme
Derleme işi başarılı şekilde tamamlandığını bir veya daha fazla yeni düğüm yapılandırmaları oluşturur. Düğüm yapılandırması çekme sunucusuna ve çekilen ve bir veya daha fazla düğüm tarafından uygulanan hazır dağıtılan bir MOF belgedir. Otomasyon hesabınızda düğüm yapılandırmaları görüntüleyebileceği **DSC düğüm yapılandırmaları** dikey. Düğüm yapılandırması form ile bir ada sahip *ConfigurationName*. *NodeName*.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC düğüm yapılandırmaları**.
   
    ![DSC düğüm yapılandırmaları dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Azure Otomasyonu DSC ile bir Azure VM yönetimi için hazırlanma
Azure Otomasyonu DSC, Azure Vm'leri (Klasik ve Resource Manager), şirket içi sanal makineleri, Linux makineler, AWS VM'ler ve şirket içi fiziksel makineleri yönetmek için kullanabilirsiniz. Bu makalede, bilgi yerleşik yalnızca Azure Kaynak Yöneticisi Vm'leri nasıl. Ekleme hakkında bilgi için bkz: diğer türleri makine [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>Onboarding için Azure Otomasyonu DSC tarafından Yönetim için bir Azure Kaynak Yöneticisi'ni VM
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC düğümleri**.
4. İçinde **DSC düğümleri** dikey penceresinde tıklatın **eklemek Azure VM**.
   
    ![Azure VM Ekle düğmesi vurgulama DSC düğümleri dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/OnboardVM.png)
5. İçinde **eklemek Azure Vm'leri** dikey penceresinde tıklatın **giriş için sanal makine Seç**.
6. İçinde **seçin VM'ler** dikey penceresinde, istediğiniz yerleşik VM seçin ve tıklatın **Tamam**.
   
   > [!IMPORTANT]
   > Bu Windows Server 2008 R2 çalıştıran bir Azure Kaynak Yöneticisi'ni VM olmalıdır veya sonraki bir sürümü.
   > 
   > 
7. İçinde **eklemek Azure Vm'leri** dikey penceresinde tıklatın **kayıt verileri yapılandırma**.
8. İçinde **kayıt** dikey penceresinde, VM için uygulamak istediğiniz düğüm yapılandırmasının adı girin **düğüm yapılandırma adı** kutusu. Bu Otomasyon hesabında bir düğüm yapılandırmasının adı tam olarak eşleşmelidir. Bu noktada sağlayan bir adı isteğe bağlıdır. Onboarding düğüm sonra atanan düğüm yapılandırmasını değiştirebilirsiniz.
   Denetleme **gerekirse düğümü yeniden**ve ardından **Tamam**.
   
    ![Kayıt dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/RegisterVM.png)
   
    Belirtilen uygulanır VM tarafından belirlenen aralıklarla düğüm yapılandırması **yapılandırma modu sıklığı**, ve VM tarafından belirlenen aralıklarla düğüm yapılandırması için güncelleştirmeleri denetler **Yenile Sıklık**. Bu değerleri nasıl kullanıldığı konusunda daha fazla bilgi için bkz: [yerel Configuration Manager Yapılandırma](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. İçinde **eklemek Azure Vm'leri** dikey penceresinde tıklatın **oluşturma**.

Azure VM ekleme işlemini başlatır. Tamamlandığında, VM görünür **DSC düğümleri** dikey penceresinde Otomasyon hesabı.

## <a name="viewing-the-list-of-dsc-nodes"></a>DSC düğümleri listesini görüntüleme
Otomasyon hesabınızda Yönetimi edildi kaldırılmış tüm makinelerin listesini görüntüleyebileceğiniz **DSC düğümleri** dikey.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC düğümleri**.

## <a name="viewing-reports-for-dsc-nodes"></a>DSC düğüm raporları görüntüleme
Azure Otomasyonu DSC yönetilen bir düğüm üzerinde tutarlılık denetimi gerçekleştirir her zaman düğüm bir durum raporu çekme sunucusuna geri gönderir. Bu düğüm için dikey penceresinde, bu raporları görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC düğümleri**.
4. Üzerinde **raporları** kutucuğu, listedeki raporların birini tıklatın.
   
    ![Rapor dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/NodeReport.png)

Dikey penceresinde bir tek tek raporu, karşılık gelen tutarlılık denetimi için aşağıdaki durum bilgileri görebilirsiniz:

* Rapor durumu — "Uyumlu", "Başarısız" yapılandırma düğümdür ya da "Uyumlu" bir düğümdür (düğüm olduğunda **applyandmonitor** modu ve makine istenilen durumda değil).
* Tutarlılık denetimi için başlangıç saati.
* Tutarlılık denetimi için toplam çalışma zamanı.
* Tutarlılık denetimi türü.
* Hata iletisi ve hata kodu da dahil olmak üzere tüm hatalar. 
* Yapılandırma ve durum (düğüm bu kaynak için istenen durumunda olup) her bir kaynağın kullanılan DSC kaynakları; kaynağı için daha ayrıntılı bilgi almak için her bir kaynağın tıklatabilirsiniz.
* Adı, IP adresi ve düğümün yapılandırma modu.

Tıklatarak **görüntülemek ham rapor** düğümü sunucuya gönderdiği gerçek verileri görmek için. Bu verileri kullanma hakkında daha fazla bilgi için bkz: [DSC rapor sunucusu kullanarak](https://msdn.microsoft.com/powershell/dsc/reportserver).

İlk rapor kullanılabilir olmadan önce bir düğümü edildi sonra biraz zaman alabilir. İlk rapor için 30 dakika sonra yerleşik bir düğüm beklemeniz gerekebilir.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Farklı düğüm yapılandırması düğüme yeniden atama
Başlangıçta atanan olandan farklı bir düğüme yapılandırmasını kullanmak için bir düğüm atayabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC düğümleri**.
4. Üzerinde **DSC düğümleri** dikey penceresinde atamak istediğiniz düğümün adını tıklatın.
5. Bu düğüm için dikey penceresinde **Ata düğümü**.
   
    ![Ata düğüm düğmesini vurgulama düğümü dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/AssignNode.png)
6. Üzerinde **atamak düğüm yapılandırması** dikey penceresinde, düğüm atayın ve ardından istediğiniz düğüm yapılandırması **Tamam**.
   
    ![Düğüm yapılandırması atayın dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Bir düğümün kaydı silinirken
Azure Otomasyonu DSC tarafından yönetilmek üzere bir düğümü artık istemiyorsanız kaydını kaldırabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde **tüm kaynakları** ve ardından Otomasyon hesabınızın adını.
3. Üzerinde **Otomasyon hesabı** dikey penceresinde tıklatın **DSC düğümleri**.
4. Üzerinde **DSC düğümleri** dikey penceresinde, kaydını kaldırmak istediğiniz düğümün adını tıklatın.
5. Bu düğüm için dikey penceresinde **Unregister**.
   
    ![Unregister düğmesi vurgulama düğümü dikey penceresinin ekran görüntüsü](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>İlgili makaleler
* [Azure Otomasyonu DSC genel bakış](automation-dsc-overview.md)
* [Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler](automation-dsc-onboarding.md)
* [Windows PowerShell istenen durum yapılandırması genel bakış](https://msdn.microsoft.com/powershell/dsc/overview)
* [Azure Otomasyonu DSC cmdlet'leri](/powershell/module/azurerm.automation/#automation)
* [Azure Otomasyonu DSC fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

