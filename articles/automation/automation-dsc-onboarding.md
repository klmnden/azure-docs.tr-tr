---
title: Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler
description: Azure Otomasyonu DSC ile yönetim için makineleri ayarlama
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c1090751db4df54e36e5263c4036d447c95d7b50
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a>Azure Otomasyonu DSC tarafından Yönetim için hazırlama makineler

## <a name="why-manage-machines-with-azure-automation-dsc"></a>Neden Azure Otomasyonu DSC makinelerle yönetme?

Gibi [PowerShell istenen durum Yapılandırması](https://technet.microsoft.com/library/dn249912.aspx), tüm Bulut veya şirket içi veri merkezinde Azure Otomasyonu istenen durum yapılandırması olan DSC düğümleri (fiziksel ve sanal makineler) için basit ancak güçlü, yapılandırma yönetimi hizmeti. Bu ölçeklenebilirliği makineler binlerce arasında hızlı ve kolay bir şekilde bir merkezi, güvenli konumdan sağlar. Ata her makine bunları bildirim temelli yapılandırmaları ve gösteren raporları görüntüleme, belirtilen istenen duruma uyumluluk kullanıcının, kolayca yerleşik makineler olabilir. Azure Otomasyonu DSC yönetim DSC için PowerShell komut dosyası için Azure Otomasyonu yönetim katmanı nedir katmanıdır. Diğer bir deyişle, Azure Automation PowerShell komut dosyalarını yönetmenize yardımcı olan aynı şekilde, ayrıca DSC yapılandırmaları yönetmenize yardımcı olur. Azure Otomasyonu DSC kullanmanın avantajları hakkında daha fazla bilgi için bkz: [Azure Otomasyonu DSC genel bakış](automation-dsc-overview.md).

Azure Otomasyonu DSC, çeşitli makinelerde yönetmek için kullanılabilir:

* Azure sanal makineleri (Klasik)
* Azure sanal makineleri
* Amazon Web Hizmetleri (AWS) sanal makineler
* Windows fiziksel/sanal makineleri şirket içi veya bulutta Azure/AWS dışında
* Şirket içi, Azure veya Azure dışında bir bulut Linux fiziksel/sanal makineleri

Ayrıca, buluttan makine yapılandırmasını yönetmek hazır değilseniz Azure Otomasyonu DSC de yalnızca rapor uç noktası olarak kullanılabilir. Bu DSC şirket içi aracılığıyla (anında iletme) istenen yapılandırmayı ayarlamak ve düğüm uyumluluğunu belirtilen istenen duruma Azure Automation ile zengin raporlama ayrıntıları görüntülemenizi sağlar.

> [!NOTE]
> Yüklü sanal makine DSC uzantısı 2.70 büyükse, DSC ile Azure Vm'lerini yönetme ekstra ücret ödemeden dahil edilir. Başvurmak [ **fiyatlandırma sayfası Otomasyon** ](https://azure.microsoft.com/pricing/details/automation/) daha fazla ayrıntı için.


Aşağıdaki bölümlerde, her tür bir Azure Otomasyonu DSC makineye discovery'yi ekleyebilir nasıl verilmiştir.

## <a name="azure-virtual-machines-classic"></a>Azure sanal makineleri (Klasik)

Azure Otomasyonu DSC'ye kolayca yerleşik Azure sanal makineleri (Klasik) yapılandırma yönetimi için Azure portal veya PowerShell kullanarak yapabilirsiniz. Başlık altında ve yönetici VM içine uzak zorunda kalmadan Azure VM istenen durum yapılandırması uzantısı VM Azure Otomasyonu DSC'ye kaydeder. Azure VM istenen durum yapılandırması uzantısı zaman uyumsuz olarak çalışır olduğundan, ilerleme durumunu izlemek ve bu sorun giderme adımları aşağıda sağlanan [ **sorun giderme Azure sanal makine ekleme** ](#troubleshooting-azure-virtual-machine-onboarding) bölümü.

### <a name="azure-portal"></a>Azure portalına

İçinde [Azure portal](http://portal.azure.com/), tıklatın **Gözat** -> **sanal makineleri (Klasik)**. Eklemek istediğiniz Windows VM seçin. Sanal makinenin Pano dikey penceresinde **tüm ayarları** -> **uzantıları** -> **Ekle** -> **Azure Otomasyonu DSC** -> **oluşturma**. Girin [PowerShell DSC Local Configuration Manager değerleri](https://msdn.microsoft.com/powershell/dsc/metaconfig4) kullanım örneği, Otomasyon hesabınızın kayıt anahtarı ve kayıt URL'si ve bir düğüm yapılandırması için isteğe bağlı olarak VM atamak için gerekli.

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

Otomasyon hesabının giriş için aşağıdaki görmek için makine anahtarı ve kayıt URL'sini bulmak için [ **güvenli kayıt** ](#secure-registration) bölümü:

### <a name="powershell"></a>PowerShell

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Connect-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in the name of a Node Configuration in Azure Automation DSC, for this VM to conform to
# NOTE: DSC Node Configuration names are case sensitive in the portal.
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
    AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

> [!NOTE]
> DSC düğüm yapılandırması adları portalda büyük küçük harfe duyarlı. Eşleşmeyen durumda düğüm DSC düğümleri altında göstermez.

## <a name="azure-virtual-machines"></a>Azure sanal makineleri

Azure Otomasyonu DSC kolayca yerleşik Azure sanal makineleri yapılandırma yönetimi için Azure portalı, Azure Resource Manager şablonları veya PowerShell kullanarak olanak sağlar. Başlık altında ve yönetici VM içine uzak zorunda kalmadan Azure VM istenen durum yapılandırması uzantısı VM Azure Otomasyonu DSC'ye kaydeder. Azure VM istenen durum yapılandırması uzantısı zaman uyumsuz olarak çalışır olduğundan, ilerleme durumunu izlemek ve bu sorun giderme adımları aşağıda sağlanan [ **sorun giderme Azure sanal makine ekleme** ](#troubleshooting-azure-virtual-machine-onboarding) bölümü.

### <a name="azure-portal"></a>Azure portalına

İçinde [Azure portal](https://portal.azure.com/), yerleşik sanal makinelere istediğiniz Azure Automation hesabı gidin. Otomasyon hesabı Panoda tıklatın **DSC düğümleri** -> **+ Azure VM eklemek**.

Giriş için bir Azure sanal makinesini seçin.

Makine yoksa PowerShell istenen durum uzantısı yüklü ve güç durumu çalıştıran **Bağlan**.

Altında **kayıt**, girin [PowerShell DSC Local Configuration Manager değerleri](https://msdn.microsoft.com/powershell/dsc/metaconfig4) kullanım örneği ve bir düğüm yapılandırması için isteğe bağlı olarak VM atamak için gereklidir.

![](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure sanal makineler dağıtılabilir ve Azure Resource Manager şablonları aracılığıyla Azure Otomasyonu dsc'ye edildi. Bkz: [VM DSC uzantısı aracılığıyla ve Azure Otomasyonu DSC yapılandırma](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) bir örnek şablon için bu onboards var olan bir Azure Otomasyonu DSC VM. Kayıt anahtarı ve kayıt URL'si gerçekleştirilecek bulmak için bu şablonda giriş olarak aşağıdaki bakın [ **güvenli kayıt** ](#secure-registration) bölümü.

### <a name="powershell"></a>PowerShell

[Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet, PowerShell aracılığıyla Azure portalında yerleşik sanal makineler için kullanılabilir.

## <a name="amazon-web-services-aws-virtual-machines"></a>Amazon Web Hizmetleri (AWS) sanal makineler

Kolayca yerleşik Amazon Web Hizmetleri sanal makineler AWS DSC Araç Seti kullanarak Azure Otomasyonu DSC yapılandırma yönetimi için kullanabilirsiniz. Araç hakkında daha fazla bilgiyi [burada](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Windows fiziksel/sanal makineleri şirket içi veya bulutta Azure/AWS dışında

Birkaç basit adımda üzerinden internet'e giden erişime sahip oldukları sürece şirket içi Windows makinelerini ve Windows Azure olmayan Bulutları (örneğin, Amazon Web Hizmetleri) makinelerinizde da Azure Otomasyonu DSC, sayede olabilir:

1. En son sürümünü emin olun [WMF 5](http://aka.ms/wmf5latest) onboarding için Azure Automation DSC'ye eklemek istediğiniz makinelerde yüklü.
2. Aşağıdaki bölümde'ndaki yönergeleri izleyin [ **oluşturma DSC metaconfigurations** ](#generating-dsc-metaconfigurations) gerekli DSC metaconfigurations içeren bir klasör oluşturmak için.
3. Uzaktan PowerShell DSC meta yapılandırmasını eklemek istediğiniz makinelere uygulayın. **En son sürümünü bu komutu çalıştırmak makinenin olmalıdır [WMF 5](http://aka.ms/wmf5latest) yüklü**:

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. PowerShell DSC metaconfigurations uzaktan uygulanamıyor, 2. adımda her makine üzerine metaconfigurations klasörünü, giriş kopyalayın. ' I çağırın **kümesi DscLocalConfigurationManager** giriş için her bir makinede yerel olarak.
5. Azure portal veya cmdlet'ler kullanarak, yerleşik makinelerine artık Azure Otomasyon hesabınızda kayıtlı DSC düğümleri olarak gösterilmediğini kontrol edin.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Şirket içi, Azure veya Azure dışında bir bulut Linux fiziksel/sanal makineleri

Birkaç basit adımda üzerinden internet'e giden erişime sahip oldukları sürece şirket içi Linux makineler, azure'da Linux makineler ve Azure olmayan bulutlarındaki Linux makineler de Azure Otomasyonu DSC, sayede olabilir:

1. En son sürümünü emin olun [PowerShell istenen durum yapılandırması Linux için](https://github.com/Microsoft/PowerShell-DSC-for-Linux) onboarding için Azure Automation DSC'ye eklemek istediğiniz makinelerde yüklü.
2. Varsa [PowerShell DSC Local Configuration Manager varsayılanları](https://msdn.microsoft.com/powershell/dsc/metaconfig4) , kullanım büyük küçük harf duyarlı ve yerleşik makineleri gibi istediğiniz, bunlar **her ikisi de** isteneceğini ve raporlamak için Azure Otomasyonu DSC:

   + Azure Otomasyonu DSC giriş için her Linux makinesinde Register.py yerleşik için PowerShell DSC Local Configuration Manager varsayılan değerleri kullanarak kullanın:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + Kayıt anahtarı ve kayıt URL'si Automation hesabınız için bulmak için aşağıdakilere bakın [ **güvenli kayıt** ](#secure-registration) bölümü.

     PowerShell DSC Local Configuration Manager varsayılan olarak, **yapmak** **değil** , kullanım örneği veya istediğiniz onboarding için eşleşme makineleri sağlayacak şekilde bunlar yalnızca Azure Otomasyonu DSC için rapor ancak çekme yapılandırma veya PowerShell modülleri ondan yapmak için 3-6 adımlarını izleyin. Aksi takdirde, 6. adıma doğrudan devam edin.

3. Aşağıdaki yönergeleri izleyin [ **oluşturma DSC metaconfigurations** ](#generating-dsc-metaconfigurations) gerekli DSC metaconfigurations içeren bir klasör oluşturmak için bölüm.
4. Uzaktan PowerShell DSC meta yapılandırmasını eklemek istediğiniz makinelere uygulayın:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

En son sürümünü bu komutu çalıştırmak makinenin olmalıdır [WMF 5](http://aka.ms/wmf5latest) yüklü.

1. Adım 5 Linux makine üzerine klasöründeki bu makineye karşılık gelen meta yapılandırmasını, PowerShell DSC metaconfigurations uzaktan, dahili, her Linux makineye uygulayamazsınız kopyalayın. ' I çağırın `SetDscLocalConfigurationManager.py` yerel olarak her Linux makinesinde onboarding için Azure Automation DSC'ye eklemek istediğiniz:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. Azure portal veya cmdlet'ler kullanarak, yerleşik makinelerine artık Azure Otomasyon hesabınızda kayıtlı DSC düğümleri olarak gösterilmediğini kontrol edin.

## <a name="generating-dsc-metaconfigurations"></a>DSC metaconfigurations oluşturma

Genel olarak yerleşik herhangi bir Azure Otomasyonu DSC için makine bir [DSC meta yapılandırmasını](https://msdn.microsoft.com/powershell/dsc/metaconfig) olabilir makine isteneceğini ve/veya Azure Otomasyonu DSC raporu için DSC Aracısı bildirir, uygulandığında, oluşturulur. Azure Otomasyonu DSC DSC metaconfigurations PowerShell DSC yapılandırması veya Azure Automation PowerShell cmdlet'lerini kullanarak oluşturulabilir.

> [!NOTE]
> DSC metaconfigurations için yerleşik bir Otomasyon makineye yönetimi için hesap gizli kod dizeleri içerir. Oluşturduğunuz tüm DSC metaconfigurations düzgün bir şekilde korumak emin olun veya kullandıktan sonra silin.

### <a name="using-a-dsc-configuration"></a>DSC Yapılandırması'nı kullanarak

1. Yerel ortamınızdaki bir makinede PowerShell ISE'yi yönetici olarak açın. Makinede en son sürümü yüklü olmalıdır [WMF 5](http://aka.ms/wmf5latest) yüklü.
2. Aşağıdaki komut dosyasını yerel olarak kopyalayın. Bu komut dosyası metaconfigurations ve devre dışı meta yapılandırmasını oluşturma kazandırın için bir komut oluşturmak için PowerShell DSC yapılandırması içerir.

> [!NOTE]
> DSC düğüm yapılandırması adları portalda büyük küçük harfe duyarlı. Eşleşmeyen durumda düğüm DSC düğümleri altında göstermez.

    ```powershell
    # The DSC configuration that will generate metaconfigurations
    [DscLocalConfigurationManager()]
    Configuration DscMetaConfigs
    {

        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
    }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
    }

    # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. Kayıt anahtarı ve URL yerleşik makinelerine adlarını yanı sıra, Automation hesabınız için doldurun. Diğer tüm parametreler isteğe bağlıdır. Kayıt anahtarı ve kayıt URL'si Automation hesabınız için bulmak için aşağıdakilere bakın [ **güvenli kayıt** ](#secure-registration) bölümü.
4. Azure Otomasyonu DSC, ancak çekme yapılandırma veya PowerShell modülleri DSC durum bilgilerini bildirmek için makineler istiyorsanız ayarlayın **sadece rapor** parametresi true.
5. Betiği çalıştırın. Adlı bir klasör şimdi olmalıdır **DscMetaConfigs** çalışma dizininizi makineler için PowerShell DSC metaconfigurations içeren (Yönetici) olarak onboarding için:

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Azure Automation cmdlet'leri kullanma

PowerShell DSC Local Configuration Manager Varsayılanları, kullanım büyük küçük harf duyarlı ve bunlar hem isteneceğini ve Azure Otomasyonu DSC rapor şekilde yerleşik makinelerine istiyorsanız Azure Automation cmdlet'leri gerekli DSC metaconfigurations oluşturmanın basitleştirilmiş bir yöntem sağlar:

1. Yerel ortamınızdaki bir makinede PowerShell konsolunu veya PowerShell ISE'yi yönetici olarak açın.
2. Azure Kaynak Yöneticisi'ni kullanarak bağlanmak **Connect-AzureRmAccount**
3. Yerleşik düğümlerine istediğiniz Otomasyon hesabı eklemek istediğiniz makineler için PowerShell DSC metaconfigurations indirin:

    ```powershell
    # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # The name of the ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. Adlı bir klasör şimdi olmalıdır ***DscMetaConfigs***, makineler için PowerShell DSC metaconfigurations içeren (Yönetici) olarak onboarding için:
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Güvenli kayıt

Makineler güvenli bir şekilde bir Azure Otomasyonu hesabı (Azure Otomasyonu DSC dahil) bir PowerShell DSC çekme veya Raporlama sunucusuna kimlik doğrulaması için bir DSC düğüm verir WMF 5 DSC kaydı protokolü üzerinden gerçekleştirebilir. Düğüm sunucusuna kaydeder bir **kayıt URL'si**, kimlik doğrulama kullanarak bir **kayıt anahtarı**. Kayıt sırasında sunucu sonrası kaydı için kimlik doğrulaması için kullanmak üzere bu düğüm için benzersiz bir sertifika DSC çekme/raporlama sunucusu ve DSC düğümü anlaşmaları. Bu işlem, bir başka bir düğüm bir gibi tehlikeye kimliğine bürünmek ve kötü amaçlı olarak davranmakta edildi düğümleri engeller. Kayıt sonra kayıt anahtarını yeniden kimlik doğrulaması için kullanılmaz ve düğümden silinir.

DSC kaydı protokolünden için gerekli bilgileri almak **anahtarları** altında **hesap ayarlarını** Azure portalında. Anahtar simgesine tıklayarak bu dikey penceresini açmak **Essentials** Otomasyon hesabının paneli.

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* Kayıt, anahtarları Yönet dikey URL alanına URL'dir.
* Kayıt birincil erişim tuşunu veya ikincil erişim anahtarını anahtarları Yönet dikey penceresinde anahtardır. Her iki anahtar kullanılabilir.

Ek güvenlik için herhangi bir anda bir Otomasyon hesabı birincil ve ikincil erişim anahtarını üretilebilir (üzerinde **anahtarları Yönet** dikey) önceki anahtar kullanılarak gelecekteki düğümü kayıtlar önlemek için.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Sorun giderme Azure sanal makine ekleme

Azure Otomasyonu DSC, yapılandırma yönetimi için Azure Windows Vm'lerini kolayca eklemeyi olanak tanır. Başlık altında Azure VM istenen durum yapılandırması uzantısı VM Azure Otomasyonu DSC'ye kaydetmek için kullanılır. Azure VM istenen durum yapılandırması uzantısı zaman uyumsuz olarak çalışır olduğundan, ilerleme durumunu izleme ve sorun giderme yürütülmesinin önemli olabilir.

> [!NOTE]
> Onboarding Azure VM istenen durum yapılandırması uzantısını kullanan Azure Otomasyonu DSC bir Azure Windows VM herhangi bir yöntemini bir saate kadar Azure Otomasyonu'nda kayıtlı olarak göstermek için düğümünü doğru kadar sürebilir. Windows Management Framework 5.0 VM'de yerleşik için gerekli olan Azure VM DSC uzantısı yüklemesi nedeniyle Azure Otomasyonu DSC VM'ye budur.

Sorun giderme veya Azure VM istenen durum yapılandırması uzantısının durumunu görüntülemek için Azure portalında edildi olan VM gidin ve ardından **uzantıları** altında **ayarları**. Ardından **DSC** veya **DSCForLinux** işletim sisteminize bağlı olarak. Daha fazla ayrıntı için tıklattığınız **ayrıntılı durumunu görüntüleme**.

## <a name="certificate-expiration-and-reregistration"></a>Sertifika geçerlilik süresi ve saklanması

Bir Azure Otomasyonu DSC DSC düğüm olarak bir makine kaydolduktan sonra pek çok neden bu düğüme gelecekte yeniden kaydettirin gerekebilir vardır:

* Kaydolduktan sonra her düğüme benzersiz bir sertifikası bir yıl sonra süresi dolar kimlik doğrulaması için otomatik olarak görüşür. Şu anda, böylece düğümleri bir yılın süre sonra yeniden kaydettirin gerek süre sonu yaklaştığı zaman PowerShell DSC kaydı Protokolü otomatik olarak sertifikalar yenileyemezsiniz. Yeniden önce her düğüm Windows Management Framework 5.0 RTM çalıştığından emin olun. Bir düğümün kimlik doğrulama sertifikasının süresi dolmadan ve düğüm yeniden kaydetme değil, düğüm Azure Automation ile iletişim kuramıyor ve 'Unresponsive.' işaretli Saklanması gerçekleştirilen 90 gün veya daha az sertifika sona erme saati veya sertifika sona erme zamanı sonra herhangi bir noktada oluşturulan ve kullanılan yeni bir sertifika içinde neden olur.
* Değişiklik yapmak için [PowerShell DSC Local Configuration Manager değerleri](https://msdn.microsoft.com/powershell/dsc/metaconfig4) ConfigurationMode gibi bir düğümün başlangıç kayıt sırasında kümesi. Şu anda bu DSC Aracısı değerleri yalnızca yeniden kayıt işlemi değiştirilebilir. Düğüm yapılandırması düğüme atanan tek istisnası--bu doğrudan Azure Otomasyonu DSC değiştirilebilir.

Yeniden kayıt işlemi, düğüm başlangıçta, bu belgede açıklanan ekleme yöntemlerden birini kullanarak kayıtlı aynı şekilde gerçekleştirilebilir. Azure Otomasyonu DSC düğüm yeniden önce kaydı gerekmez.

## <a name="related-articles"></a>İlgili makaleler

* [Azure Otomasyonu DSC genel bakış](automation-dsc-overview.md)
* [Azure Otomasyonu DSC cmdlet'leri](/powershell/module/azurerm.automation/#automation)
* [Azure Otomasyonu DSC fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
