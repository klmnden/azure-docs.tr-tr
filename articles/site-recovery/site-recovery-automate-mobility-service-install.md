---
title: "Azure Otomasyonu DSC Site Recovery Mobility hizmetiyle dağıtma | Microsoft Docs"
description: "VMware sanal ve fiziksel sunucu çoğaltma Azure için Azure Site Recovery Mobility hizmeti ve Azure Aracısı otomatik olarak dağıtmak için Azure Otomasyonu DSC kullanmayı açıklar"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: bcc5f11afbecac8fe63935f3401dd3e2d767e8aa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>Azure Otomasyonu DSC Mobility hizmetiyle VM çoğaltması için dağıtma
Operations Management Suite biz kapsamlı yedekleme ve iş sürekliliği planınızın bir parçası olarak kullanabileceğiniz olağanüstü durum kurtarma çözümü sağlar.

Biz bu gezisine Hyper-V ile birlikte Hyper-V çoğaltma kullanılarak başlatıldı. Ancak biz müşteriler kendi bulut birden fazla hiper ve platformlar olduğundan heterojen Kurulum desteklemek için genişletilen.

VMware iş yükleri ve/veya fiziksel sunucuları bugün çalıştırıyorsanız, yönetim sunucusu tüm Azure Site Recovery bileşenlerini Azure Hedefinizi olduğunda, Azure ile iletişim ve veri çoğaltmayı düzenlemek için ortamınızda çalıştırır.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Automation DSC kullanarak Site Recovery Mobility hizmeti dağıtma
Bu yönetim sunucusu yaptığı bir hızlı dökümünü yaparak başlayalım.

Yönetim sunucusu, çeşitli sunucu rolleri çalıştırır. Bu roller, biri *yapılandırma*, hangi iletişimi düzenler ve veri çoğaltma ve kurtarma işlemlerini yönetir.

Ayrıca, *işlem* rol çoğaltma ağ geçidi olarak davranır. Bu rolü korumalı kaynak makinelerden çoğaltma verilerini alıp, önbelleğe alma, sıkıştırma ve şifreleme iyileştirir ve Azure depolama hesabı için gönderir. İşlev işlem rolü için ayrıca Mobility hizmetinin korunan makinelere göndermeli ve VMware vm'lerinin otomatik bulma gerçekleştirmek için biridir.

Varsa, adlı bir azure'dan *ana hedef* rolü, bu işlemin bir parçası olarak çoğaltma verileri işleyecek.

Korumalı makineler için biz Bel *Mobility hizmeti*. Bu bileşen, Azure'a çoğaltmak istediğiniz her makineye (VMware VM veya fiziksel sunucusu) dağıtılır. Makinede veri Yazar yakalar ve bunları (işlem rolü) yönetim sunucusuna iletir.

İş sürekliliği ile ilgilenirken iş yüklerinizi, altyapınızı ve ilgili bileşenleri anlamak önemlidir. Ardından Kurtarma süresi hedefi (RTO) ve kurtarma noktası hedefi (RPO) gereksinimleri karşılayabilir. Bu bağlamda Mobility hizmeti beklediğiniz şekilde iş yüklerinizi korunmasını sağlamak için anahtardır.

Bu nedenle nasıl, iyileştirilmiş bir şekilde, bazı Operations Management Suite bileşenleri Yardım ile güvenilir bir korumalı kurulum olmasını sağlayabilirsiniz?

Bu makalede, emin olmak için Azure Otomasyonu istenen durum yapılandırması (DSC), Site Recovery ile birlikte nasıl kullanabileceğinize bir örnek sağlar:

* Mobility hizmeti ve Azure VM aracısını korumak istediğiniz Windows makineler dağıtılır.
* Çoğaltma hedefi Azure olduğunda Mobility hizmeti ve Azure VM Aracısı her zaman çalışır.

## <a name="prerequisites"></a>Ön koşullar
* Gerekli Kurulum depolamak için bir depo
* Yönetim sunucusu ile kaydetmek için gerekli parolayı depolamak için bir depo

  > [!NOTE]
  > Her yönetim sunucusu için benzersiz bir parola oluşturulur. Birden çok yönetim sunucusu dağıtmak için kullanacaksanız, doğru parolayı passphrase.txt dosyasında depolanır olmanız gerekir.
  >
  >
* Windows Management Framework (WMF) koruma (Automation DSC gereksinimini) için etkinleştirmek istediğiniz makinelere yüklenen 5.0

  > [!NOTE]
  > WMF 4.0 yüklü olan Windows için DSC makineler kullanmak istiyorsanız, bölümüne bakın. [kullanmak DSC bağlantısı kesilmiş ortamlarda](## Use DSC in disconnected environments).
  

Mobility hizmetinin komut satırından yüklenebilir ve birkaç bağımsız değişken kabul eder. İşte bu nedenle ikili dosyaları (bunları, kurulumdan ayıkladıktan sonra) sahip ve bunları burada, alabilir bunları DSC Yapılandırması kullanılarak bir yerde depolamak gerekir.

## <a name="step-1-extract-binaries"></a>1. adım: Extract ikili dosyalar
1. Bu kurulum için gereken dosyaları ayıklamak için yönetim sunucunuzda aşağıdaki dizinine göz atın:

    **\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    Bu klasörde, adlandırılmış bir MSI dosyası görmeniz gerekir:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Yükleyici ayıklamak için aşağıdaki komutu kullanın:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Tüm dosyaları seçin ve bir sıkıştırılmış (ZIP) klasöre gönderin.

Automation DSC kullanarak kurulum mobilite hizmetinin otomatik hale getirmek için gereken ikili dosyalar artık sahipsiniz.

### <a name="passphrase"></a>Parola
Ardından, bu sıkıştırılmış klasörü eklemek istediğiniz yeri belirlemeniz gerekir. Kurulum için gereken parolayı depolamak için sonraki gösterildiği gibi bir Azure depolama hesabı kullanabilirsiniz. Aracı, sonra Yönetim sunucusu işleminin bir parçası olarak kaydeder.

Yönetim sunucusu dağıtıldığında aldığınız parolayı bir metin dosyasına passphrase.txt kaydedilebilir.

Daraltılmış klasörü ve parola ayrılmış bir Azure depolama hesabı kapsayıcısında yerleştirin.

![Klasör konumu](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Bu dosyalar, ağınızdaki bir paylaşımında tutmak tercih ederseniz, bunu yapabilirsiniz. Daha sonra kullanacaksınız DSC kaynağı erişimi vardır ve Kurulum ve parola alabilirsiniz emin olmak yeterlidir.

## <a name="step-2-create-the-dsc-configuration"></a>2. adım: DSC yapılandırması oluşturma
Kurulum WMF 5.0 bağlıdır. Makine başarıyla Otomasyonu DSC aracılığıyla yapılandırmayı uygulamak WMF 5.0 bulunması gerekir.

Aşağıdaki örnek DSC yapılandırma ortamı kullanır:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Yapılandırma aşağıdakileri yapın:

* Değişkenleri yapılandırma Mobility hizmeti ve Azure VM Aracısı ikili dosyalarının alınacağı söyleyecektir parola alınacağı ve çıktı depolanacağı konumu.
* Böylece kullanabileceğiniz yapılandırma xPSDesiredStateConfiguration DSC kaynağı alacak `xRemoteFile` depodan dosyalarını indirmek için.
* Yapılandırma İkili dosyalarını depolamak istediğiniz bir dizin oluşturur.
* Arşiv kaynak dosyaları sıkıştırılmış klasörden ayıklayın.
* ' % S'paketinin yükleme kaynak Mobility hizmeti UNIFIEDAGENT yükler. EXE yükleyici belirli bağımsız değişkenlere sahip. (Bağımsız değişkenler oluşturmak değişkenleri ortamınız yansıtacak şekilde değiştirilmesi gerekir.)
* Paket AzureAgent kaynak Azure'da çalışan her VM üzerinde önerilen Azure VM aracısı yükler. Azure VM aracısı ayrıca, yük devretme sonrasında VM uzantıları eklemek mümkün kılar.
* Hizmet kaynağı veya kaynakları ilgili Mobility hizmetlerinin ve Azure Hizmetleri zaman çalıştığını güvence altına alır.

Yapılandırma olarak Kaydet **ASRMobilityService**.

> [!NOTE]
> Böylece aracı doğru bağlanır ve doğru parolayı kullanacağı gerçek yönetim sunucusu yansıtacak şekilde CSIP yapılandırmanızda değiştirmek unutmayın.
>
>

## <a name="step-3-upload-to-automation-dsc"></a>3. adım: Automation DSC karşıya yükle
Yaptığınız DSC yapılandırması gerekli bir DSC kaynakları Modülü (xPSDesiredStateConfiguration) alacağı için DSC yapılandırması karşıya yüklemeden önce otomasyon modülü içeri aktarmanız gerekir.

Otomasyon hesabınızda oturum açın, Gözat **varlıklar** > **modülleri**, tıklatıp **Gözat galeri**.

Modül için burada arama ve hesabınıza aktarın.

![İçeri aktarma modülü](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Bu bitirdikten sonra makinenize yüklü Azure Resource Manager modüllerini sahip olduğu gidin ve yeni oluşturulan DSC yapılandırması içeri aktarmak için devam edin.

### <a name="import-cmdlets"></a>İçeri aktarma cmdlet'leri
PowerShell'de, Azure aboneliğinizde oturum açın. Automation hesabı bilgilerinizi bir değişkende yakalamak ve ortamınızı yansıtmak üzere cmdlet'leri değiştirin:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Yapılandırma, aşağıdaki cmdlet'i kullanarak Automation DSC karşıya yükle:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Automation DSC yapılandırmasını derleme
Ardından, onu düğümlerine kaydetmek başlayabilmeniz Automation DSC yapılandırmasını derleme gerekir. Aşağıdaki cmdlet'i çalıştırarak elde:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Barındırılan DSC çekme hizmet yapılandırmasını temel olarak dağıtıyorsunuz için bu işlem birkaç dakika sürebilir.

Yapılandırma derledikten sonra PowerShell'i (Get-AzureRmAutomationDscCompilationJob) kullanarak veya kullanarak iş bilgilerini alabilir [Azure portal](https://portal.azure.com/).

![İş alma](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Şimdi başarıyla yayımlandı ve Automation DSC, DSC yapılandırması karşıya.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>4. adım: Automation DSC yerleşik makinelere
> [!NOTE]
> Bu senaryo Tamamlanıyor önkoşulları Windows makinelerinizi WMF en son sürümüyle güncelleştirilir biridir. Karşıdan yükleyip, platformlar için doğru sürümünü yüklemek [Yükleme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=50395).
>
>

Düğümleriniz için uygulayacağınız DSC için şimdi bir metaconfig oluşturacaksınız. Bu başarılı olması için uç nokta URL'sini ve azure'da seçili Automation hesabınız için birincil anahtarınızı almak gerekir. Bu değerleri altında bulabilirsiniz **anahtarları** üzerinde **tüm ayarları** Otomasyon hesabı dikey penceresinde.

![Anahtar değerleri](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

Bu örnekte, Site Recovery kullanarak korumak istediğiniz bir Windows Server 2012 R2 fiziksel sunucu vardır.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Kayıt defterindeki dosya yeniden adlandırma işlemleri beklemedeki denetle
Automation DSC noktayla sunucu ilişkilendirilecek başlamadan önce tüm bekleyen kayıt defteri dosyasını yeniden adlandırma işlemleri için denetlemenizi öneririz. Bekleyen bir yeniden başlatma nedeniyle tamamlama Kurulum yasaklayabilir.

Hiçbir yeniden başlatma bekleniyor sunucuda olduğunu doğrulamak için aşağıdaki cmdlet'i çalıştırın:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Bu boş gösterir, devam etmek için Tamam demektir. Aksi durumda, bu sunucunun bir bakım penceresi sırasında yeniden tarafından bulundurmalıdır.

Sunucuda yapılandırmayı uygulamak için PowerShell Tümleşik komut dosyası ortamı (ISE) başlatın ve aşağıdaki komut dosyasını çalıştırın. Bu, Automation DSC hizmete kaydolmak ve özel yapılandırma (ASRMobilityService.localhost) almak için yerel Configuration Manager altyapısı yönlendiren aslında bir DSC yerel yapılandırmadır.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Bu yapılandırma, kendisini Otomasyonu DSC'ye kaydetmek yerel Configuration Manager altyapısı neden olur. Ayrıca, altyapı nasıl çalışacağını, yapılandırma değişikliklerini (ApplyAndAutoCorrect) ise ne yapmanız gerektiğini ve yeniden başlatma gerekli olduğunda nasıl, yapılandırmaya devam etmek belirler.

Automation DSC ile kaydetmek, sonra bu komut dosyasını çalıştırın, düğüm başlamanız gerekir.

![Düğüm kayıt işlemi sürüyor](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Azure portalına geri dönün, yeni kayıtlı düğümü Portalı'nda artık göründükten görebilirsiniz.

![Portalı'nda kayıtlı düğümü](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Sunucuda, düğüm doğru şekilde kaydedildiğini doğrulamak için aşağıdaki PowerShell cmdlet'ini çalıştırabilirsiniz:

```powershell
Get-DscLocalConfigurationManager
```

Yapılandırma çekilen ve sunucuya uyguladıktan sonra aşağıdaki cmdlet'i çalıştırarak doğrulayabilirsiniz:

```powershell
Get-DscConfigurationStatus
```

Sunucu yapılandırmasını başarıyla çekilen çıkış gösterir:

![Çıktı](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Ayrıca, Mobility hizmeti Kurulumu konumunda bulunan kendi günlük sahip *SystemDrive*\ProgramData\ASRSetupLogs.

İşte bu kadar. Şimdi başarıyla dağıtılan ve Site Recovery kullanarak korumak istediğiniz makinede Mobility hizmeti kayıtlı. DSC gerekli hizmetler her zaman çalışır durumda olduğundan emin olun.

![Başarılı dağıtım](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Yönetim sunucusu başarılı dağıtım algılandıktan sonra korumayı yapılandırın ve Site Recovery kullanarak makinesinde çoğaltmayı etkinleştirin.

## <a name="use-dsc-in-disconnected-environments"></a>Bağlantısı kesilmiş ortamlarda DSC kullanın
Makinelerinizi Internet'e bağlı değilseniz hala dağıtmak ve korumak istediğiniz iş yükünü Mobility hizmeti yapılandırmak için DSC üzerinde güvenebilirsiniz.

Temelde Automation DSC'den alma aynı yetenekleri sağlamak için ortamınızdaki kendi DSC istek sunucusuyla örneği. Diğer bir deyişle, istemcilerin (kaydedildikten sonra) yapılandırma DSC uç çeker. Ancak, yerel olarak veya uzaktan DSC yapılandırması makinelerinize, el ile göndermek için başka bir seçenek değildir.

Bu örnekte, bilgisayar adı için ek bir parametre yoktur. Uzak dosyalar, artık korumak istediğiniz makinelere tarafından erişilebilir olması gereken bir uzak paylaşım bulunur. Komut dosyasının sonuna yapılandırma enacts ve DSC yapılandırması hedef bilgisayara uygulanan başlar.

### <a name="prerequisites"></a>Ön koşullar
XPSDesiredStateConfiguration PowerShell Modülü yüklü olduğundan emin olun. WMF 5.0 yüklü olduğu Windows makineler için aşağıdaki cmdlet'i hedef makinelerde çalıştırarak xPSDesiredStateConfiguration modülü yükleyebilirsiniz:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Ayrıca, indirin ve WMF 4.0 yüklü Windows makineler dağıtmanız gerektiğinde modülü kaydedin. Bu cmdlet, PowerShellGet (WMF 5.0) mevcut olduğu bir makinede çalıştırın:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Ayrıca WMF 4.0 için emin [Windows 8.1 güncelleştirmesi KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) makinelerde yüklü.

Aşağıdaki yapılandırma WMF 5.0 ve WMF 4.0 sahip Windows makinelerine iletilmesini:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Şirket ağınıza Automation DSC'den alabilirsiniz yetenekleri taklit etmek üzere kendi DSC çekme sunucusuna örneği oluşturmak istiyorsanız, bkz: [DSC web çekme sunucusu kurma](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>İsteğe bağlı: DSC yapılandırması bir Azure Resource Manager şablonu kullanarak dağıtın
Bu makalede nasıl otomatik olarak Mobility hizmeti ve Azure VM Aracısı--dağıtmak ve sağlamak için kendi DSC yapılandırması korumak istediğiniz makinelerde çalıştırılan oluşturabileceğiniz üzerinde odaklı. Biz de bu DSC yapılandırması için yeni veya var olan bir Azure Otomasyonu hesabı dağıtacağınız bir Azure Resource Manager şablonu vardır. Şablon değişkenleri ortamınız için içerecek Otomasyon varlıkları oluşturmak için giriş parametreleri kullanır.

Şablon dağıttıktan sonra yalnızca 4. adım giriş için bu kılavuzdaki makinelerinizi başvurabilirsiniz.

Şablon aşağıdakileri yapın:

1. Var olan Otomasyon hesabını kullanabilir veya yeni bir tane oluşturun
2. Giriş parametreleri için alın:
   * ASRRemoteFile--Mobility hizmeti Kurulumu depoladığınız konumu
   * ASRPassphrase--passphrase.txt dosyasını depoladığınız konumu
   * ASRCSEndpoint--yönetim sunucunuzun IP adresi
3. XPSDesiredStateConfiguration PowerShell modülünü içeri aktarın
4. Oluşturma ve DSC yapılandırmasını derleme

Makinelerinizi koruması için ekleme başlayabilmeniz için yukarıdaki adımların tümünü doğru sırada gerçekleşir.

Şablon dağıtımı için yönergeler bulunan [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

PowerShell kullanarak şablonu dağıtma:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar
Mobility hizmeti aracıları dağıttıktan sonra şunları yapabilirsiniz [çoğaltmayı etkinleştirme](site-recovery-vmware-to-azure.md) sanal makineler için.
