---
title: Azure Otomasyon karma Runbook çalışanı üzerinde runbook çalıştırma
description: Bu makalede, yerel veri merkezi veya karma Runbook çalışanı rolü ile bulut sağlayıcısı makinelerde runbook'ları çalıştırma hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 07/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8f21457a63470b88e93ead97454f996cea38073a
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43103777"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Bir karma Runbook çalışanı üzerinde runbook'ları çalıştırma

Azure Otomasyonu ve karma Runbook çalışanı üzerinde çalıştıranlar çalışan runbook'ları yapısı içinde bir fark yoktur. Kullandığınız her runbook'ları genellikle bir karma Runbook çalışanı hedefleyen runbook yerel bilgisayarda veya dağıtıldığı, runbook'ları sırasında yerel ortamda kaynaklarda kaynakları yönetme beri büyük olasılıkla ancak önemli ölçüde farklı Azure Otomasyonu, genellikle Azure bulut kaynaklarını yönetin.

Bir karma Runbook çalışanı üzerinde çalıştırmak için runbook'ları geliştirdiğinizde, düzenleyin ve karma çalışanı barındıran makine içine bir runbook'ları sınamanızı gerekir. Konak makinenin tüm PowerShell modülleri ve ağ erişimi ve yerel kaynaklara erişimi yönetmesine gerek vardır. Bir runbook düzenlenebilir ve karma çalışan makinede test sonra sonra bu karma çalışanı çalıştırılmak için uygun olduğu bir Azure Otomasyonu ortama yükleyebilirsiniz. İşleri windows için yerel sistem hesabını veya özel bir kullanıcı hesabı altında Çalıştır bilmek önemlidir **nxautomation** Linux üzerinde hangi tanıtmak farklar olmalıdır bir karma Runbook çalışanının runbook'ları yazma dikkate.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Karma Runbook çalışanı üzerinde runbook başlatma

[Azure Automation'da bir Runbook başlatma](automation-starting-a-runbook.md) bir runbook başlatmak için farklı yöntemler açıklanır. Karma Runbook çalışanı ekler bir **RunOn** bir karma Runbook çalışanı grubuna adını belirtebileceğiniz seçeneği. Bir grup belirtilmişse, ardından runbook alındığında ve o gruptaki çalışanlar birini çalıştırın. Bu seçenek belirtilmezse, Azure Automation'da ardından normal şekilde çalışır.

Azure portalında bir runbook'u başlattığınızda ile sunulan bir **çalıştıracağınız** seçebileceğiniz seçeneği **Azure** veya **karma çalışanı**. Seçerseniz **karma çalışanı**, grubun açılan listeden seçin.

Kullanım **RunOn** parametresi. Windows PowerShell kullanarak MyHybridGroup adlı bir karma Runbook çalışan grubu üzerinde Test-Runbook adlı bir runbook başlatmak için aşağıdaki komutu kullanabilirsiniz.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> **RunOn** parametresi eklenmiş **başlangıç AzureAutomationRunbook** 0.9.1 sürümünde, Microsoft Azure PowerShell cmdlet'i. Yapmanız gerekenler [son sürümünü indirin](https://azure.microsoft.com/downloads/) yüklü daha önceki bir varsa. Yalnızca bir iş istasyonunda burada Powershell'den runbook başlatma bu sürümü yüklemeniz gerekir. Bu bilgisayardan runbook'ları başlatmak istediğiniz sürece çalışan bilgisayarda yüklemeniz gerekmez"

## <a name="runbook-permissions"></a>Runbook izinleri

Bir karma Runbook çalışanı üzerinde çalışan runbook'ları genellikle kaynakların Azure dışında eriştikleri beri runbook'ları Azure kaynakları için kimlik doğrulaması için kullanılan yöntemin aynısını kullanamaz. Runbook'un yerel kaynakları için kendi kimlik doğrulamasını ya da sağlayabilir veya tüm runbook'ları için bir kullanıcı bağlam sağlamak için bir farklı çalıştır hesabı belirtebilirsiniz.

### <a name="runbook-authentication"></a>Runbook kimlik doğrulaması

Varsayılan olarak, Windows ve özel bir kullanıcı hesabı için yerel sistem hesabı bağlamında runbook'ları çalıştırmak **nxautomation** şirket içi bilgisayarda Linux için bu nedenle gerekir sağlarlar, erişimleri olan kaynaklar için kendi kimlik doğrulama .

Kullanabileceğiniz [kimlik bilgisi](automation-credentials.md) ve [sertifika](automation-certificates.md) varlıklar runbook'unuzda cmdlet'leriyle farklı kaynaklarda kimlik doğrulaması için kimlik bilgilerini belirtmenizi sağlar. Aşağıdaki örnek, bir bilgisayarı yeniden başlatan bir runbook bir bölümü gösterilmektedir. Bu kimlik bilgilerini bir kimlik bilgisi varlığı ve değişken varlığı bilgisayar adını alır ve ardından bu değerleri Restart-Computer cmdlet'ini kullanır.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Ayrıca yararlanabilir [Inlinescript](automation-powershell-workflow.md#inlinescript), kod bloklarını tarafından belirtilen kimlik bilgilerine sahip başka bir bilgisayarda çalıştırmak olanak tanıyan [PSCredential ortak parametresi](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Farklı Çalıştır hesabı

Varsayılan olarak karma Runbook çalışanı için yerel sistem Windows ve özel bir kullanıcı hesabı kullanan **nxautomation** runbook'ları çalıştırmak Linux için. Belirtebileceğiniz runbook'lar yerel kaynakları için kendi kimlik doğrulamalarını sağlamak yerine, bir **RunAs** hesap için bir karma çalışanı grubu. Belirttiğiniz bir [kimlik bilgisi varlığı](automation-credentials.md) yerel kaynaklara erişimi olan ve gruptaki bir karma Runbook çalışanı üzerinde çalışan tüm runbook'ları bu kimlik bilgileri altında çalıştırın.

Kullanıcı adı kimlik bilgisi için aşağıdaki biçimlerden birinde olmalıdır:

* etki alanı\kullanıcı adı
* username@domain
* Kullanıcı adı (için hesapları şirket içi bilgisayarın yerel)

Karma çalışanı grubu için bir farklı çalıştır hesabı belirtmek için aşağıdaki yordamı kullanın:

1. Oluşturma bir [kimlik bilgisi varlığı](automation-credentials.md) yerel kaynaklara erişim.
2. Azure portalında Otomasyon hesabını açın.
3. Seçin **karma çalışan grupları** kutucuğuna ve sonra da grubunu seçin.
4. Seçin **tüm ayarlar** ardından **karma çalışanı grubu ayarları**.
5. Değişiklik **Çalıştır** gelen **varsayılan** için **özel**.
6. Kimlik bilgisi seçin ve tıklayın **Kaydet**.

### <a name="automation-run-as-account"></a>Otomasyon farklı çalıştır hesabı

Kaynakları azure'da dağıtmak için otomatik yapı işleminizin bir parçası olarak, şirket içi sistemlere dağıtım sıranızda bir görev veya adımları kümesini desteklemek için erişimi gerektirebilir. Azure farklı çalıştır hesabını kullanarak kimlik doğrulamasını desteklemek için farklı çalıştır hesabı sertifikası yüklemeniz gerekir.

Aşağıdaki PowerShell runbook *dışarı aktarma RunAsCertificateToHybridWorker*, Azure Otomasyonu hesabınızdan Çalıştır sertifikası dışarı aktarır ve indirir ve karma yerel makine sertifika deposuna içeri aktarır çalışan hesabın aynısını bağlı. Bu adım tamamlandıktan sonra çalışan farklı çalıştır hesabı kullanarak Azure'a başarıyla kimlik doğrular.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
Run this runbook in the hybrid worker where you want the certificate installed.
This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
AUTHOR: Azure Automation Team
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test that authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

> [!IMPORTANT]
> **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

Kaydet *dışarı aktarma RunAsCertificateToHybridWorker* runbook ile bilgisayarınızda bir `.ps1` uzantısı. Otomasyon hesabınızda içeri aktarılması ve değişkenin değerini değiştirerek bu runbook'u düzenlemek `$Password` kendi parolanızı ile. Yayımlayın ve ardından çalıştıran ve farklı çalıştır hesabıyla runbook kimlik doğrulaması karma çalışanı grubu hedefleme runbook'u çalıştırın. İş akışı sertifikasını yerel makine deposuna içeri girişimi raporları ve kaç Otomasyon hesapları aboneliğinizdeki tanımlanır ve kimlik doğrulaması başarılı olursa bağlı olarak birden çok satır izler.

## <a name="job-behavior"></a>İş davranışı

İşleri biraz farklı karma Runbook çalışanları üzerinde Azure sanal çalıştırdıklarında olduklarından işlenir. Önemli bir fark karma Runbook çalışanları şirket iş süresi sınırı yoktur. Runbook'ları, Azure'da çalıştırdığınız sanal son 3 saat sınırlı [adil paylaşımı](automation-runbook-execution.md#fair-share). Uzun süre çalışan runbook'iniz varsa, örneğin karma çalışanı barındıran makine yeniden başlatılır, olası yeniden başlatma için esnek olmasını sağlamak istiyorsunuz. Karma çalışanı ana makinenin yeniden başlatılırsa, daha sonra çalışan bir runbook işi baştan ya da PowerShell iş akışı runbook'ları için en son kontrol noktasından yeniden başlatır. Ardından bir runbook işi birden fazla 3 kez yeniden başlatılması durumunda bekletilir.

## <a name="run-only-signed-runbooks"></a>Yalnızca imzalı runbook'ları çalıştırma

Karma Runbook çalışanları, bazı yapılandırma ile yalnızca imzalı runbook'ları çalıştırmak için yapılandırılabilir. Aşağıdaki bölümde, imzalı runbook'ları çalıştırmak için karma Runbook çalışanları ayarlama ve runbook'larınızı oturum açma açıklanmaktadır.

> [!NOTE]
> Yalnızca imzalı runbook'ları çalıştırmak için bir karma Runbook çalışanı yapılandırdıktan sonra runbook'ları olan **değil** edilmiş imzalı çalışan üzerinde yürütülmesi başarısız.

### <a name="create-signing-certificate"></a>İmzalama sertifikası oluşturma

Aşağıdaki örnek runbook'ları imzalamak için kullanılabilecek kendinden imzalı bir sertifika oluşturur. Örnek, bir sertifika oluşturur ve bunu aktarır. Sertifika, karma Runbook çalışanlarını daha sonra içeri aktarılır. Parmak izini de bunu daha sonra sertifikayı başvurmak için kullanılan döndürülür.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="configure-the-hybrid-runbook-workers"></a>Karma Runbook çalışanlarını yapılandırma

Bir gruptaki her bir karma Runbook çalışanı için oluşturulan sertifikayı kopyalamanız. Sertifikayı içeri aktarmak ve imza doğrulaması runbook'ları kullanmak için karma çalışanı yapılandırmak için aşağıdaki betiği çalıştırın.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Runbook'ları sertifikayı kullanarak oturum açın

Karma Runbook çalışanları kullanacak şekilde yapılandırılmış runbook'ları yalnızca imzalanmış, karma Runbook çalışanı üzerinde kullanılacak olan runbook'ları oturum açmanız gerekir. Aşağıdaki örnek PowerShell runbook'larınızı imzalamak için kullanın.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Runbook imzalandığında Otomasyon hesabınızda içeri aktarıldı ve gerekir imza bloğunu ile yayımlanan. Runbook'ları orchestrator'a öğrenmek için bkz. [bir runbook'u dosyadan Azure Automation'a içeri](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).

## <a name="troubleshoot"></a>Sorun giderme

Runbook'larınızı başarıyla tamamlanamamasının, sorun giderme kılavuzunu gözden [runbook yürütme hataları](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Automation'da bir Runbook başlatma](automation-starting-a-runbook.md).
* Azure automation'da metin düzenleyicisini kullanarak PowerShell ve PowerShell iş akışı runbook'ları ile çalışmak için farklı yordamlar anlamak için bkz: [Azure Otomasyonu Runbook'u düzenleme](automation-edit-textual-runbook.md)
