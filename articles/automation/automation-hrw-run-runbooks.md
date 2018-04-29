---
title: Azure Otomasyon karma Runbook çalışanı üzerinde çalışan runbook
description: Bu makalede, yerel veri merkezinde veya Bulut sağlayıcısı karma Runbook çalışan rolü ile makinelerde çalışan runbook'ları hakkında bilgi sağlar.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/05/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6fc89469e1a9b2d7142e38f7aea5596fc9adb4e4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Bir karma Runbook çalışanı üzerinde çalışan runbook'ları

Azure Automation ve karma Runbook çalışanı üzerinde çalışacak çalışan runbook'ları yapısını fark yoktur. Her ile kullandığınız runbook'ları kaynakları yerel bilgisayarda veya dağıtıldığı, runbook'ları sırasında yerel ortamında kaynaklara karşı bir karma Runbook çalışanı genellikle hedefleyen runbook'ları yönetmek beri büyük olasılıkla ancak önemli ölçüde farklı Azure Otomasyonu, Azure bulut kaynaklarında genellikle yönetin.

Runbook'ları karma Runbook çalışanı üzerinde çalışacak şekilde yazarken, düzenlemek ve karma çalışanı barındıran makine içindeki runbook'ları test gerekir. Konak makine tüm PowerShell modülleri ve ağ erişimini yönetmek ve yerel kaynaklara erişmek için ihtiyacınız vardır. Bir runbook düzenlenmesi ve karma çalışanı makinede test sonra ardından karma çalışanı çalıştırmak kullanılabilir olduğu bir Azure Otomasyonu ortama karşıya yükleyebilirsiniz. İşlerini küçük farklılıklar bu dikkate alınması gereken bir karma Runbook çalışanı için runbook'ları yazma olduğunda ortaya çıkarabilir ve yerel sistem hesabı altında çalışacak bilmeniz önemlidir.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Karma Runbook çalışanını runbook başlatma

[Azure Otomasyonu Runbook başlatma](automation-starting-a-runbook.md) bir runbook'u başlatmak için farklı yöntemleri açıklar. Karma Runbook çalışanı ekler bir **RunOn** bir karma Runbook çalışan grubu adını belirtebileceğiniz seçeneği. Bir grubu belirtilirse, ardından runbook alınır ve o gruptaki çalışanlar biri tarafından çalıştırın. Bu seçenek belirtilmezse, ardından onu Azure Otomasyonu'nda normal olarak çalıştırılır.

Azure portalında bir runbook'u başlattığınızda ile sunulan bir **çalıştıracağınız** seçebileceğiniz seçeneği **Azure** veya **karma çalışanı**. Seçerseniz **karma çalışanı**, sonra da aşağı açılır listeden grup seçebilirsiniz.

Kullanım **RunOn** parametresi. Windows PowerShell kullanarak MyHybridGroup adlı bir karma Runbook çalışan grubu üzerinde Test-Runbook adlı bir runbook'u başlatmak için aşağıdaki komutu kullanabilirsiniz.

```powershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> **RunOn** parametresi eklendiği **başlangıç AzureAutomationRunbook** 0.9.1 sürümünde Microsoft Azure PowerShell cmdlet'i. Yapmanız gerekenler [en son sürümü karşıdan](https://azure.microsoft.com/downloads/) yüklü daha önceki bir varsa. Yalnızca bir iş istasyonunda burada Windows Powershell'den runbook başlatılıyor bu sürümü yüklemeniz gerekir. Bu bilgisayardan runbook'ları başlatmak düşünmüyorsanız çalışan bilgisayara yüklemeniz gerekmez. Bu Otomasyon hesabınızda yüklenecek Azure Powershell en son sürümünü gerektirir olduğundan başka bir runbook'tan bir karma Runbook çalışanı üzerinde şu anda bir runbook başlatılamıyor. En son sürümü Azure Otomasyonu'nda otomatik olarak güncelleştirilir ve otomatik olarak çalışanlarına en kısa sürede gönderilen.

## <a name="runbook-permissions"></a>Runbook izinleri

Bir karma Runbook çalışanı üzerinde çalışan runbook'ları, genellikle Azure dışında kaynaklara erişmesi beri runbook'ları Azure kaynakları için kimlik doğrulaması için kullanılan yöntemin kullanamazsınız. Runbook yerel kaynakları için kendi kimlik doğrulaması ya da sağlayabilir veya tüm runbook'lar için bir kullanıcı bağlamı sağlamak üzere bir farklı çalıştır hesabı belirtebilirsiniz.

### <a name="runbook-authentication"></a>Runbook kimlik doğrulaması

Eriştiklerinde kaynakları için kendi kimlik doğrulaması sağlamanız gerekir varsayılan olarak, runbook'lar şirket içi bilgisayarda yerel sistem hesabı bağlamında çalışır.

Kullanabileceğiniz [kimlik bilgisi](automation-credentials.md) ve [sertifika](automation-certificates.md) varlıklar runbook'unuzda cmdlet'leri farklı kaynaklarda kimlik doğrulaması için kimlik bilgilerini belirtmenizi sağlar. Aşağıdaki örnek, bir bilgisayarı yeniden başlatır bir runbook bir kısmı gösterir. Kimlik bilgilerini bir kimlik bilgisi varlığı ve değişken varlığı bilgisayardan adını alır ve ardından bilgisayarı yeniden başlat cmdlet'iyle bu değerleri kullanır.

```powershell-interactive
$Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
$Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Ayrıca yararlanabilirsiniz [Inlinescript](automation-powershell-workflow.md#inlinescript), kod bloklarını tarafından belirtilen kimlik bilgilerine sahip başka bir bilgisayarda çalıştırmanıza olanak sağlayan [PSCredential ortak parametresi](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>RunAs hesabı

Belirleyebileceğiniz yerel kaynakları için kendi kimlik doğrulaması runbook'ları sahip olmak yerine bir **RunAs** hesap için bir karma çalışanı grubu. Belirttiğiniz bir [kimlik bilgisi varlığı](automation-credentials.md) yerel kaynaklara erişimi olan ve gruptaki bir karma Runbook çalışanı üzerinde çalışan tüm runbook'ları bu kimlik bilgileri altında çalıştırın.

Kullanıcı adı kimlik bilgisi için aşağıdaki biçimlerden birinde olmalıdır:

* etki alanı\kullanıcı adı
* username@domain
* Kullanıcı adı (için hesapları şirket içi bilgisayarın yerel)

Karma çalışanı grubu için bir RunAs hesabı belirtmek için aşağıdaki yordamı kullanın:

1. Oluşturma bir [kimlik bilgisi varlığı](automation-credentials.md) yerel kaynaklara erişim.
2. Azure Portal'da Automation hesabını açın.
3. Seçin **karma çalışan grupları** döşeme ve grubu seçin.
4. Seçin **tüm ayarları** ve ardından **karma çalışanı grubu ayarları**.
5. Değişiklik **Farklı Çalıştır** gelen **varsayılan** için **özel**.
6. Kimlik bilgisi seçin ve tıklatın **kaydetmek**.

### <a name="automation-run-as-account"></a>Automation farklı çalıştır hesabı

Azure'daki kaynakların dağıtımı için otomatik yapı işleminizin bir parçası olarak, şirket içi bir görev veya grup adımı, dağıtım sırası desteklemek üzere sistemler için erişim gerektirebilir. Azure farklı çalıştır hesabını kullanarak kimlik doğrulamasını desteklemek için farklı çalıştır hesabı sertifikası yüklemeniz gerekir.

Aşağıdaki PowerShell runbook *verme RunAsCertificateToHybridWorker*, Azure Automation hesabınızdan farklı çalıştır sertifika verir ve indirir ve bunu bir karma yerel makine sertifika deposuna aktarır çalışan aynı hesaba bağlı. Bu adım tamamlandıktan sonra çalışan farklı çalıştır hesabını kullanarak Azure kimlik doğrulamasını başarıyla doğrular.

```powershell-interactive
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

Kaydet *verme RunAsCertificateToHybridWorker* runbook bilgisayarınıza bir `.ps1` uzantısı. Otomasyon hesabınızda içeri aktarın ve değişkenin değerini değiştirerek bu runbook'u düzenlemek `$Password` kendi parolanızı ile. Yayımlama ve çalıştıran ve runbook'ları farklı çalıştır hesabını kullanarak kimlik doğrulaması karma çalışanı grubu hedefleme runbook çalıştırın. İş akışı yerel makine deposuna sertifika alma girişimi raporları ve kaç tane Automation hesapları aboneliğinizde tanımlanır ve kimlik doğrulaması başarılı olup olmadığını bağlı olarak birden fazla satır ile izler.

## <a name="job-behavior"></a>İş davranışı

İşlerini biraz farklı karma Runbook çalışanları üzerinde Azure sanal çalıştırdığınızda olduklarından işlenir. Bir anahtar karma Runbook çalışanları üzerinde iş süreye göre bir sınır yoktur farktır. Uzun süre çalışan runbook'unuz varsa, örneğin, karma çalışanı barındıran makine yeniden başlatıldıktan için olası yeniden, esnek sağlamak istiyorsunuz. Karma çalışanı ana makine yeniden başlatılırsa, sonra çalışan bir runbook işi en başından veya PowerShell iş akışı runbook'ları için son denetim noktasından yeniden başlatır. Bir runbook işi birden fazla 3 kez yeniden başlatılırsa, ardından askıya alınmış.

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Karma Runbook çalışanı runbook'larda sorun giderme

Günlükleri yerel olarak her karma çalışanı C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes konumunda depolanır. Karma çalışanları de kayıt hatalarını ve olaylarını Windows olay günlüğünde altında **uygulama ve Hizmetleri Logs\Microsoft-SMA\Operational**. Çalışan üzerinde yürütülen runbook'ları ilgili olayları için yazılır **uygulama ve Hizmetleri Logs\Microsoft-Automation\Operational**. **Microsoft SMA** günlük çalışan ve runbook işleme gönderilen runbook işi ilgili pek çok daha fazla olay içerir. Sırada **Microsoft Otomasyon** olay günlüğü yok birçok olay runbook yürütülmesi gidermeye yardımcı ayrıntılarla, runbook işi sonuçlarını içeren.

[Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md) Azure Otomasyon karma gönderilen bulutta çalışan runbook işleri gibi çalıştırın. Bu gibi durumlarda, ayrıntılı ve ilerleme akışları ayrıca diğer runbook'lar için olduğu gibi etkinleştirebilirsiniz.

Runbook'larınızın başarıyla tamamlanamamasının ve Özet iş durumunu gösterir **askıya**, sorun giderme makalesi gözden [karma Runbook çalışanı: bir runbook işi askıya alındıdurumuilesonaerer](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook'u başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Otomasyonu Runbook başlatma](automation-starting-a-runbook.md).
* PowerShell ve PowerShell iş akışı runbook'ları metin düzenleyicisini kullanarak Azure Automation ile çalışmak için farklı yordamlar anlamak için bkz: [Azure Otomasyon Runbook'u düzenleme](automation-edit-textual-runbook.md)
