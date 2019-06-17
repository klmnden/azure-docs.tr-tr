---
title: Azure Otomasyonu durumu yapılandırmasında yapılandırmaları derleme
description: Bu makalede, Azure otomasyonu için Desired State Configuration ' nı (DSC) yapılandırmaları derleme açıklar.
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 09/10/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 847c928681451b4fef93198e2f2272d5bb04b1b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64919795"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>Azure Otomasyonu durumu yapılandırması DSC yapılandırmaları derleme

Azure Otomasyonu durum yapılandırması ile Desired State Configuration ' nı (DSC) yapılandırma iki yolla derleyebilirsiniz: Azure Portalı'nda ve Windows PowerShell ile. Aşağıdaki tabloda, ne zaman her biri özelliklere bağlı olarak hangi yöntemin kullanılacağını belirlemenize yardımcı olur:

**Azure portal**

- En basit yöntem etkileşimli kullanıcı arabirimi
- Formu basit parametre değerlerini sağlamak için
- İş durumu kolayca izleyin
- Azure oturum açma ile kimliği doğrulanmış erişim

**Windows PowerShell**

- Windows PowerShell cmdlet'lerle komut satırından çağırma
- Birden çok adım ile otomatik çözüm eklenebilir
- Basit ve karmaşık bir parametre değerlerini sağlayın
- İş durumu izleme
- PowerShell cmdlet'leri desteklemek için gereken istemci
- ConfigurationData geçirin
- Kimlik bilgilerini kullanan yapılandırmaları derleme

Bir derleme metodunda verdikten sonra derleme başlatmak için aşağıdaki yordamları kullanın.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Azure portalı ile bir DSC yapılandırması derlenirken

1. Otomasyon hesabınızdan tıklayın **durum yapılandırması (DSC)** .
1. Tıklayarak **yapılandırmaları** sekmesine ve ardından derlemek için yapılandırma adına tıklayın.
1. Tıklayın **derleme**.
1. Yapılandırma parametresi yok, derlemeniz isteyip istemediğinizi onaylamanız istenir. Yapılandırma parametreleri varsa **derleme Yapılandırması** parametre değerleri sağlamadan şekilde dikey penceresi açılır. Şu [ **temel parametreleri** ](#basic-parameters) bölümü parametreleri hakkında daha ayrıntılı bilgi için.
1. **Derleme işi** derleme işin durumunu ve Azure Otomasyonu durumu yapılandırma çekme sunucusunda yerleştirilmesine neden düğüm yapılandırmaları (MOF yapılandırma belgelerini) takip edebilmeniz, sayfası açılır.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Windows PowerShell ile bir DSC yapılandırması derlenirken

Kullanabileceğiniz [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) Windows PowerShell ile derleme başlatılamadı. Aşağıdaki örnek kod derleme olarak adlandırılan bir DSC yapılandırması başlar **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

`Start-AzureRmAutomationDscCompilationJob` durumunu izlemek için kullanabileceğiniz bir derleme iş nesnesi döndürür. Daha sonra bu derleme iş nesnesi ile kullanabilirsiniz [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob)
derleme işi durumunu belirlemek için ve [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput)
kendi akışlarını (çıktı) görüntülemek için. Aşağıdaki örnek kod derlemesini başlatır **SampleConfig** yapılandırması tamamlandı ve daha sonra kendi akışlarını görüntüler kadar bekler.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Temel parametreleri

Parametre bildiriminde parametre türleri ve özellikleri de dahil olmak üzere, DSC yapılandırmaları Azure Automation runbook'ları olduğu gibi aynı şekilde çalışır. Bkz: [Azure Automation'da bir runbook başlatma](automation-starting-a-runbook.md) runbook parametreleri hakkında daha fazla bilgi edinmek için.

Aşağıdaki örnekte adlı iki parametre kullanır **FeatureName** ve **olmasına**özelliklerinde değerlerini belirlemek için **ParametersExample.sample** düğümü yapılandırma, derleme sırasında oluşturuldu.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

Azure Otomasyonu durum yapılandırması portalında veya Azure PowerShell ile temel parametreleri kullanan bir DSC yapılandırmaları derleme:

### <a name="portal"></a>Portal

Portalda tıklandıktan sonra parametre değerleri girebilirsiniz **derleme**.

![Yapılandırma derleme parametreleri](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell gerektirir parametrelerinde bir [hashtable](/powershell/module/microsoft.powershell.core/about/about_hash_tables) burada anahtarının parametre adıyla eşleştiği ve değerin parametre değeri eşittir.

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

PSCredentials parametrelere geçirme hakkında daha fazla bilgi için bkz: [kimlik bilgisi varlıkları](#credential-assets) aşağıda.

## <a name="composite-resources"></a>Bileşik kaynaklar

**Bileşik kaynaklar** DSC yapılandırmaları bir yapılandırması içinde iç içe geçmiş kaynaklar olarak kullanmanıza izin verir. Bu, tek bir kaynak için birden fazla yapılandırması uygulamanıza imkan sağlar. Bkz: [bileşik kaynaklar: Bir kaynak olarak bir DSC Yapılandırması kullanılarak](/powershell/dsc/authoringresourcecomposite) hakkında daha fazla bilgi edinmek için **bileşik kaynakları**.

> [!NOTE]
> Sırayla **bileşik kaynakları** doğru şekilde derlenmesi için ilk bileşik dayanan tüm DSC kaynakları, ilk Azure Automation hesabı modülleri depoda yüklü değil veya düzgün şekilde içe aktarmaz sağlamalısınız.

Bir DSC eklemek için **bileşik kaynak**, arşive kaynak modülünü eklemelisiniz (* .zip). Modüller depoya Azure Otomasyon hesabınıza gidin. Ardından 'Modül Ekle' düğmesine tıklayın.

![Modül Ekle](./media/automation-dsc-compile/add_module.png)

Arşiv bulunduğu dizine gidin. Arşiv dosyasını seçin ve Tamam'a tıklayın.

![Modülü seçin](./media/automation-dsc-compile/select_dscresource.png)

Burada izleyebilirsiniz durumunu modülleri dizinine geri alınır, **bileşik kaynak** ayıklar ve Azure Otomasyonu ile kaydeder.

![Bileşik kaynak Al](./media/automation-dsc-compile/register_composite_resource.png)

Modül kaydedildikten sonra doğrulamak için tıklayabilirsiniz **bileşik kaynakları** bir yapılandırmada kullanılacak kullanıma sunulmuştur.

![Bileşik kaynak doğrula](./media/automation-dsc-compile/validate_composite_resource.png)

Çağırabilirsiniz sonra **bileşik kaynak** yapılandırmanızı içine şu şekilde:

```powershell
Node ($AllNodes.Where{$_.Role -eq 'WebServer'}).NodeName
{
    DomainConfig myCompositeConfig
    {
        DomainName = $DomainName
        Admincreds = $Admincreds
    }

    PSWAWebServer InstallPSWAWebServer
    {
        DependsOn = '[DomainConfig]myCompositeConfig'
    }
}
```

## <a name="configurationdata"></a>ConfigurationData

**ConfigurationData** , PowerShell DSC kullanırken herhangi bir ortama özgü yapılandırma yapısal yapılandırmasından ayırmanıza olanak sağlar. Bkz: ["Ne" PowerShell DSC "nerede" ayırma](https://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) hakkında daha fazla bilgi edinmek için **ConfigurationData**.

> [!NOTE]
> Kullanabileceğiniz **ConfigurationData** Azure PowerShell kullanarak Azure Otomasyon durum yapılandırması, ancak Azure Portalı'nda derlenirken.

Aşağıdaki örnek DSC yapılandırması kullanan **ConfigurationData** aracılığıyla **$ConfigurationData** ve **$AllNodes** anahtar sözcükleri. Ayrıca gerekir [ **xWebAdministration** Modülü](https://www.powershellgallery.com/packages/xWebAdministration/) bu örneğin:

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

PowerShell ile önceki DSC yapılandırması derleyebilirsiniz. Aşağıdaki PowerShell iki düğüm yapılandırmaları Azure Automation durum yapılandırma çekme sunucusuna ekler: **ConfigurationDataSample.MyVM1** ve **ConfigurationDataSample.MyVM3**:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

## <a name="assets"></a>Varlıklar

Varlık başvuruları Azure Otomasyonu durum yapılandırması ve runbook'ları için de aynıdır. Daha fazla bilgi için aşağıdakilere bakın:

- [Sertifikalar](automation-certificates.md)
- [Bağlantılar](automation-connections.md)
- [Kimlik Bilgileri](automation-credentials.md)
- [Değişkenler](automation-variables.md)

### <a name="credential-assets"></a>Kimlik bilgisi varlıkları

DSC yapılandırmaları Azure Automation Otomasyon kimlik bilgisi varlıkları kullanarak başvurabilir `Get-AutomationPSCredential` cmdlet'i. Bir yapılandırması olan bir parametreye sahipse bir **PSCredential** yazın, sonra da kullanabilirsiniz `Get-AutomationPSCredential` cmdlet'i cmdlet'e kimlik bilgisi almak için bir Azure Otomasyonu kimlik bilgisi varlığı dize adını geçirerek. Parametre gerektiren için daha sonra söz konusu nesne kullanabilirsiniz **PSCredential** nesne. Arka planda bu ada sahip bir Azure Otomasyonu kimlik bilgisi varlığı alınır ve yapılandırmaya geçirildi. Aşağıdaki örnekte, bu eylemi gösterilmektedir.

Kimlik bilgilerini tutma düğüm yapılandırmaları (MOF yapılandırma belge), güvenli düğümü yapılandırma MOF dosyasının kimlik bilgilerini şifrelenmesini gerektirir. Ancak, şu anda PowerShell DSC PowerShell DSC kastettiğinizi bilemez Azure Otomasyonu tüm MOF dosyasını sonra neslini şifreleme, çünkü kimlik bilgilerini düz metin olarak düğüm yapılandırma MOF oluşturma sırasında verilecek sorunsuz bildirmeniz gerekir derleme işi.

Kimlik bilgilerini düz metin olarak oluşturulan düğüm yapılandırmasının MOF'lar verilecek mudur PowerShell DSC söyleyebilirsiniz kullanarak [ **ConfigurationData**](#configurationdata). Geçirmeniz `PSDscAllowPlainTextPassword = $true` aracılığıyla **ConfigurationData** DSC yapılandırma görünür ve kimlik bilgilerini kullanan her düğüm bloğun adı.

Aşağıdaki örnek, bir Otomasyon kimlik bilgisi varlığı kullanan bir DSC yapılandırması gösterir.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

PowerShell ile önceki DSC yapılandırması derleyebilirsiniz. Aşağıdaki PowerShell iki düğüm yapılandırmaları Azure Automation durum yapılandırma çekme sunucusuna ekler: **CredentialSample.MyVM1** ve **CredentialSample.MyVM2**.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

> [!NOTE]
> Derleme tamamlandığında belirten bir hata alabilirsiniz: **'Microsoft.PowerShell.Management' eklentisi zaten içeri aktarıldığından 'Microsoft.PowerShell.Management' modülü içeri aktarılamadı.** Bu uyarıyı güvenle yoksayılabilir.

## <a name="partial-configuration"></a>Kısmi yapılandırma

Azure Otomasyonu durum yapılandırması kullanımını destekleyen [kısmi yapılandırmalar](https://docs.microsoft.com/powershell/dsc/pull-server/partialconfigs).
Bu senaryoda, DSC yapılandırmalarını birden çok bağımsız olarak yönetmek üzere yapılandırılmış ve Azure Otomasyonu gelen retreieved her yapılandırmadır.
Ancak, yalnızca bir yapılandırma bir düğüm başına Otomasyon hesabı atanabilir.
Başka bir deyişle, bir düğümü için iki yapılandırması kullanıyorsanız iki Otomasyon hesabı gerektirir.
Takımlar nasıl çalışabileceğini hakkında daha fazla bilgi için işbirliği içinde sunucuları yönetmek için birlikte yapılandırmayı kod olarak kullanarak bkz [anlama DSC'ın bir CI/CD işlem hattı rolünde](https://docs.microsoft.com/powershell/dsc/overview/authoringadvanced).

## <a name="importing-node-configurations"></a>Düğüm yapılandırmaları alma

Ayrıca, Azure dışında derlenmiş düğüm yapılandırmaları (MOF'lar) içe aktarabilirsiniz. Düğüm yapılandırmaları imzalanabilir bunun avantajlarından biri. İmzalı düğüm yapılandırması düğüme uygulanan yapılandırma yetkili bir kaynaktan geldiğini sağlama DSC aracı tarafından yönetilen bir düğümde yerel olarak doğrulanır.

> [!NOTE]
> İçeri aktarma kullanabilirsiniz yapılandırmaları Azure Automation hesabınızda oturum açmış ancak Azure Otomasyonu desteklememektedir imzalı yapılandırmaları derleme.

> [!NOTE]
> Düğüm yapılandırma dosyası, Azure Automation'a içeri aktarılacak izin vermek için 1 MB'tan büyük olmalıdır.

Düğüm yapılandırmaları imzalama hakkında daha fazla bilgi için bkz. [WMF 5.1 - yenilikleri nasıl oturum yapılandırmasını ve modül](/powershell/wmf/5.1/dsc-improvements#dsc-module-and-configuration-signing-validations).

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Azure portalında bir düğüm yapılandırmasını alma

1. Otomasyon hesabınızdan tıklayın **durum yapılandırması (DSC)** altında **yapılandırma yönetimi**.
1. İçinde **durum yapılandırması (DSC)** sayfasında, şirket **yapılandırmaları** sekmesine ve ardından tıklayın **+ Ekle**.
1. İçinde **alma** yanındaki klasör simgesine tıklayın, sayfa **düğüm yapılandırma dosyası** göz atmak için yerel bilgisayarınızda bir düğüm yapılandırma dosyası (MOF) metin.

   ![Yerel bir dosya için Gözat](./media/automation-dsc-compile/import-browse.png)

1. Bir ad girin **yapılandırma adı** metin. Bu ad, düğüm yapılandırmasının derlenmiş yapılandırmasının adı eşleşmelidir.
1. **Tamam**'ı tıklatın.

### <a name="importing-a-node-configuration-with-powershell"></a>PowerShell ile bir düğüm yapılandırmasını alma

Kullanabileceğiniz [alma AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet'ini düğüm yapılandırması, Otomasyon hesabına aktarın.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)
