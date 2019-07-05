---
title: Azure Otomasyon karma Runbook çalışanı üzerinde runbook çalıştırma
description: Bu makalede, yerel veri merkezi veya karma Runbook çalışanı rolü ile bulut sağlayıcısı makinelerde runbook'ları çalıştırma hakkında bilgi sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 01/29/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 88e3c0514861da0bd11acffd26cced54717e4418
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478497"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Bir karma Runbook çalışanı üzerinde runbook'ları çalıştırma

Bir karma Runbook çalışanı üzerinde çalışan runbook'ları ile Azure Otomasyonu'nda çalışan runbook'ları ve yapısı içinde bir fark yoktur. Kullandığınız her runbook'ları büyük olasılıkla önemli ölçüde farklılık gösterir. Bu fark, yerel bilgisayarda veya yerel ortamda, dağıtıldığı kaynaklara karşı kaynak genellikle bir karma Runbook çalışanı hedefleyen bir runbook'ları yönetme olmasıdır. Azure Otomasyonu runbook'ları genellikle Azure bulutunda kaynakları yönetin.

Bir karma Runbook çalışanı üzerinde çalıştırmak için runbook'ları geliştirdiğinizde, düzenleyin ve karma çalışanı barındıran makine içine bir runbook'ları sınamanızı gerekir. Konak makinenin tüm PowerShell modülleri ve ağ erişimi ve yerel kaynaklara erişimi yönetmesine gerek vardır. Karma çalışanı makinede bir runbook test sonra sonra bu karma çalışanı çalıştırılmak için uygun olduğu bir Azure Otomasyonu ortama yükleyebilirsiniz. Bilmek önemlidir Windows ya da özel bir kullanıcı hesabı için yerel sistem hesabı altında çalıştıran işlerin `nxautomation` Linux üzerinde. Bu davranış, runbook'ları için bir karma Runbook çalışanı yazarken farklar ortaya çıkarabilir. Runbook'larınızı yazarken bu değişikliklerin gözden geçirilmesi gerekir.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Karma Runbook çalışanı üzerinde runbook başlatma

[Azure Automation'da bir Runbook başlatma](automation-starting-a-runbook.md) bir runbook başlatmak için farklı yöntemler açıklanır. Karma Runbook çalışanı ekler bir **RunOn** bir karma Runbook çalışanı grubuna adını belirtebileceğiniz seçeneği. Bir gruba belirtildiği zaman, ardından runbook alındığında ve o gruptaki çalışanlar birini çalıştırın. Bu seçenek belirtilmezse, Azure Automation'da ardından normal şekilde çalışır.

Azure portalında bir runbook'u başlattığınızda eklemediğiniz bir **çalıştıracağınız** seçebileceğiniz seçeneği **Azure** veya **karma çalışanı**. Seçerseniz **karma çalışanı**, grubun açılan listeden seçin.

Kullanım **RunOn** parametresi. Windows PowerShell kullanarak MyHybridGroup adlı bir karma Runbook çalışan grubu üzerinde Test-Runbook adlı bir runbook başlatmak için aşağıdaki komutu kullanabilirsiniz.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> **RunOn** parametresi eklenmiş **başlangıç AzureAutomationRunbook** 0.9.1 sürümünde, Microsoft Azure PowerShell cmdlet'i. Yapmanız gerekenler [son sürümünü indirin](https://azure.microsoft.com/downloads/) yüklü daha önceki bir varsa. Yalnızca bir iş istasyonunda burada Powershell'den runbook başlatma bu sürümü yüklemeniz gerekir. Bu bilgisayardan runbook'ları başlatmak istediğiniz sürece çalışan bilgisayarda yüklemeniz gerekmez"

## <a name="runbook-permissions"></a>Runbook izinleri

Bir karma Runbook çalışanı üzerinde çalışan runbook'ları genellikle bu yana değil azure'daki kaynaklara Erişmekte olduğunuz runbook'ları Azure kaynakları için kimlik doğrulaması için kullanılan yöntemin aynısını kullanamaz. Runbook'un yerel kaynakları için kendi kimlik doğrulamasını ya da sağlayabilir veya kimlik doğrulaması kullanarak yapılandırabilirsiniz [kimliklerini Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager
). Tüm runbook'ları için bir kullanıcı bağlam sağlamak için bir farklı çalıştır hesabı belirtebilirsiniz.

### <a name="runbook-authentication"></a>Runbook kimlik doğrulaması

Varsayılan olarak, Windows ve özel bir kullanıcı hesabı için yerel sistem hesabı bağlamında runbook'ları çalıştırmak `nxautomation` şirket içi bilgisayarda Linux için bu nedenle gerekir sağlarlar, erişimleri olan kaynaklar için kendi kimlik doğrulama.

Kullanabileceğiniz [kimlik bilgisi](automation-credentials.md) ve [sertifika](automation-certificates.md) varlıklar runbook'unuzda cmdlet'leriyle farklı kaynaklarda kimlik doğrulaması için kimlik bilgilerini belirtmenizi sağlar. Aşağıdaki örnek, bir bilgisayarı yeniden başlatan bir runbook bir bölümü gösterilmektedir. Bu kimlik bilgilerini bir kimlik bilgisi varlığı ve değişken varlığı bilgisayar adını alır ve ardından bu değerleri Restart-Computer cmdlet'ini kullanır.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Ayrıca [Inlinescript](automation-powershell-workflow.md#inlinescript), tarafından belirtilen kimlik bilgilerine sahip başka bir bilgisayara kod bloklarını çalıştırmanıza olanak tanıyan [PSCredential ortak parametresi](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Farklı Çalıştır hesabı

Varsayılan olarak karma Runbook çalışanı için yerel sistem Windows ve özel bir kullanıcı hesabı kullanan `nxautomation` runbook'ları çalıştırmak Linux için. Belirtebileceğiniz runbook'lar yerel kaynakları için kendi kimlik doğrulamalarını sağlamak yerine, bir **RunAs** hesap için bir karma çalışanı grubu. Belirttiğiniz bir [kimlik bilgisi varlığı](automation-credentials.md) yerel kaynaklara erişimi olan ve gruptaki bir karma Runbook çalışanı üzerinde çalışan tüm runbook'ları bu kimlik bilgileri altında çalıştırın.

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

### <a name="managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri

Karma Runbook çalışanları Azure sanal makineler üzerinde çalışan Azure kaynakları için yönetilen kimlikleri, Azure kaynaklarında kimlik doğrulaması için kullanabilirsiniz. Farklı Çalıştır hesapları kullanarak Azure kaynakları için yönetilen kimlikleri için birçok faydası vardır.

* Run As sertifikasını dışarı aktarma ve karma Runbook çalışanı olarak içeri aktarma gerekmez
* Farklı Çalıştır hesabı tarafından kullanılan sertifikayı yenilemek gerekmez.
* Runbook kodunuzda Çalıştır bağlantı nesnesi tanıtıcı gerekmez.

Bir karma Runbook çalışanında Azure kaynakları için yönetilen bir kimlik kullanmak için aşağıdaki adımları tamamlamanız gerekir:

1. Bir Azure VM oluşturma
2. [Sanal makinenizde Azure kaynakları için yönetilen kimlik Yapılandır](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm)
3. [Bir kaynak grubu Kaynak Yöneticisi'nde, VM erişim](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)
4. [Sanal makinenin sistem tarafından atanan yönetilen kimlik kullanarak bir erişim belirteci alma](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager)
5. [Windows karma Runbook çalışanı yükleme](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker) sanal makinede.

Yukarıdaki adımlar tamamlandıktan sonra kullanabileceğiniz `Connect-AzureRmAccount -Identity` Azure kaynaklarında kimlik doğrulaması için runbook. Bu yapılandırma, farklı çalıştır hesabı kullanın ve farklı çalıştır hesabının sertifika süresi yönetme ihtiyacını azaltır.

```powershell
# Connect to Azure using the Managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
Connect-AzureRmAccount -Identity

# Get all VM names from the subscription
Get-AzureRmVm | Select Name
```

### <a name="runas-script"></a>Otomasyon farklı çalıştır hesabı

Kaynakları azure'da dağıtmak için otomatik yapı işleminizin bir parçası olarak, şirket içi sistemlere dağıtım sıranızda bir görev veya adımları kümesini desteklemek için erişimi gerektirebilir. Azure farklı çalıştır hesabını kullanarak kimlik doğrulamasını desteklemek için farklı çalıştır hesabı sertifikası yüklemeniz gerekir.

Aşağıdaki PowerShell runbook **dışarı aktarma RunAsCertificateToHybridWorker**, Azure Otomasyonu hesabınızdan Çalıştır sertifikası dışarı aktarır ve indirir ve karma yerel makine sertifika deposuna içeri aktarır çalışan hesabın aynısını bağlıdır. Bu adım tamamlandıktan sonra çalışan farklı çalıştır hesabı kullanarak Azure'a başarıyla kimlik doğrular.

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

Kaydet *dışarı aktarma RunAsCertificateToHybridWorker* runbook ile bilgisayarınızda bir `.ps1` uzantısı. Otomasyon hesabınızda içeri aktarılması ve değişkenin değerini değiştirerek bu runbook'u düzenlemek `$Password` kendi parolanızı ile. Yayımlayın ve ardından runbook'u çalıştırın. Çalıştıran ve farklı çalıştır hesabıyla runbook kimlik doğrulaması karma çalışanı grubu hedefi. İş akışı sertifikasını yerel makine deposuna içeri girişimi raporları ve birden fazla satır ile izler. Bu davranış, kaç Otomasyon hesapları aboneliğinizdeki tanımlama ve kimlik doğrulaması başarılı olursa bağlıdır.

## <a name="job-behavior"></a>İş davranışı

İşleri biraz farklı karma Runbook çalışanları üzerinde Azure sanal çalıştırdıklarında oldukları daha işlenir. Önemli bir fark karma Runbook çalışanları şirket iş süresi sınırı yoktur. Runbook'ları, Azure'da çalıştırdığınız sanal 3 saat nedeniyle sınırlı [adil paylaşımı](automation-runbook-execution.md#fair-share). Bir uzun süre çalışan runbook için olası yeniden başlatma için dayanıklı olduğundan emin olmanız gerekir. Örneğin makine barındıran karma çalışan ile yeniden başlatır. Karma çalışanı ana makinenin yeniden başlatılırsa, daha sonra çalışan bir runbook işi baştan ya da PowerShell iş akışı runbook'ları için en son kontrol noktasından yeniden başlatır. 3 katından daha sonra bir runbook işi yeniden başlatılır ve ardından askıya alındı.

## <a name="run-only-signed-runbooks"></a>Yalnızca imzalı runbook'ları çalıştırma

Karma Runbook çalışanları, bazı yapılandırma ile yalnızca imzalı runbook'ları çalıştırmak için yapılandırılabilir. Aşağıdaki bölümde, imzalı çalıştırmak için karma Runbook çalışanları ayarlama işlemi açıklanmaktadır [Windows karma Runbook çalışanı](#windows-hybrid-runbook-worker) ve [Linux karma Runbook çalışanı](#linux-hybrid-runbook-worker)

> [!NOTE]
> Yalnızca imzalı runbook'ları çalıştırmak için bir karma Runbook çalışanı yapılandırdıktan sonra runbook'ları olan **değil** edilmiş imzalı çalışan üzerinde yürütülmesi başarısız.

### <a name="windows-hybrid-runbook-worker"></a>Windows karma Runbook çalışanı

#### <a name="create-signing-certificate"></a>İmzalama sertifikası oluşturma

Aşağıdaki örnek runbook'ları imzalamak için kullanılabilecek kendinden imzalı bir sertifika oluşturur. Örnek, bir sertifika oluşturur ve bunu aktarır. Sertifika, karma Runbook çalışanlarını daha sonra içeri aktarılır. Parmak izi, bu değer daha sonra sertifikayı başvurmak için kullanılan da döndürülür.

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

#### <a name="configure-the-hybrid-runbook-workers"></a>Karma Runbook çalışanlarını yapılandırma

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

#### <a name="sign-your-runbooks-using-the-certificate"></a>Runbook'ları sertifikayı kullanarak oturum açın

Karma Runbook çalışanları kullanacak şekilde yapılandırılmış runbook'ları yalnızca imzalanmış, karma Runbook çalışanı üzerinde kullanılacak olan runbook'ları oturum açmanız gerekir. Aşağıdaki örnek PowerShell runbook'larınızı imzalamak için kullanın.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Runbook imzalandığında Otomasyon hesabınızda içeri aktarıldı ve gerekir imza bloğunu ile yayımlanan. Runbook'ları orchestrator'a öğrenmek için bkz. [bir runbook'u dosyadan Azure Automation'a içeri](manage-runbooks.md#import-a-runbook).

### <a name="linux-hybrid-runbook-worker"></a>Linux karma Runbook çalışanı

Bir Linux karma Runbook çalışanında runbook'ları imzalamak için sahip olması, karma Runbook çalışanı gerekir [GPG](https://gnupg.org/index.html) makinede çalıştırılabilir mevcut.

#### <a name="create-a-gpg-keyring-and-keypair"></a>GPG kimlik ve anahtar çifti oluşturma

Bir anahtar çifti karma Runbook çalışanı hesabı kullanmanız gerekir ve kimlik Anahtarlığı oluşturmak için `nxautomation`.

Kullanım `sudo` olarak oturum açmak için `nxautomation` hesabı.

```bash
sudo su – nxautomation
```

Bir kez kullanarak `nxautomation` hesap, gpg anahtar çifti oluşturun.

```bash
sudo gpg --generate-key
```

GPG anahtar çifti oluşturma adımlarında size yol gösterecektir. Makine oluşturulması gereken anahtarı için bir ad, bir e-posta adresi, sona erme saati, parola ve yeterince bekle sağlamaları gerekir.

Sahibi değiştirmem gerekecek GPG dizini sudo ile güvenle `nxautomation`. 

Sahibini değiştirmek için aşağıdaki komutu çalıştırın.

```bash
sudo chown -R nxautomation ~/.gnupg
```

#### <a name="make-the-keyring-available-the-hybrid-runbook-worker"></a>Karma Runbook çalışanı kimlik Anahtarlığı kullanılabilir yap

Kimlik Anahtarlığı oluşturulduktan sonra karma Runbook çalışanı için kullanılabilir hale getirmek gerekir. Ayarları dosyasında değişiklik `/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf` bölümünde aşağıdaki örnek eklemek için `[worker-optional]`

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

#### <a name="verify-signature-validation-is-on"></a>İmza doğrulaması üzerinde olduğundan emin olun

İmza doğrulaması makinede devre dışı bırakılırsa, açmak gerekir. İmza doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın. Değiştirme `<LogAnalyticsworkspaceId>` çalışma alanı kimliğiniz ile

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

#### <a name="sign-a-runbook"></a>Bir runbook oturum

İmza doğrulaması yapılandırıldıktan sonra bir runbook imzalamak için aşağıdaki komutu kullanabilirsiniz:

```bash
gpg –-clear-sign <runbook name>
```

İmzalı runbook adı olacaktır `<runbook name>.asc`.

İmzalı runbook, Azure otomasyonu için şimdi karşıya yüklenebilir ve normal bir runbook gibi yürütülebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Automation'da bir Runbook başlatma](automation-starting-a-runbook.md).
* Azure automation'da metin düzenleyicisini kullanarak PowerShell runbook'ları ile çalışmak için farklı yolları anlamak için bkz: [Azure Otomasyonu Runbook'u düzenleme](automation-edit-textual-runbook.md)
* Runbook'larınızı tamamlanmasıyla değil, sorun giderme kılavuzunu gözden [runbook yürütme hataları](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).