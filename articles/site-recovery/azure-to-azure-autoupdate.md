---
title: Azure'dan Azure'a olağanüstü durum kurtarma mobilite hizmetinin otomatik güncelleştirmesi | Microsoft Docs
description: Azure Site Recovery kullanarak Azure Vm'lerine çoğaltırken Mobility hizmeti, otomatik güncelleştirme genel bir bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 3f0f28ca22321b537ab7e8911c5cbb513a1ade81
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55818933"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Azure'dan Azure'a çoğaltma Mobility hizmetini otomatik güncelleştirme

Azure Site Recovery, burada mevcut özelliklere yönelik iyileştirmeler veya yenilerini eklenir ve bilinen sorunlar varsa sabit bir aylık bir yayın temposudur sahiptir. Bu hizmetle geçerli kalmasını sağlamak için aylık bu düzeltme eklerinin dağıtımını için planlama gerektiğini anlamına gelir. Yükseltmeye ilişkili üzerinden baş önlemek için kullanıcıların bunun yerine güncelleştirmeleri bileşenlerini yönetmek Site Recovery izin vermeyi seçebilirsiniz. Ayrıntılarıyla açıklandığı gibi [mimari başvurusu](azure-to-azure-architecture.md) için çoğaltma etkin olan bir Azure sanal makineler çoğaltılırken tüm Azure sanal makinelerde Azure'dan Azure'a olağanüstü durum kurtarma için Mobility hizmeti yüklü başka bir bölge. Mobility hizmeti uzantısı her yeni sürümle birlikte, otomatik güncelleştirme etkinleştirdikten sonra güncelleştirilir. Bu belge aşağıdaki ayrıntıları:

- Otomatik Güncelleştirme nasıl çalışır?
- Otomatik güncelleştirmeleri etkinleştir
- Sık karşılaşılan sorunlar ve sorun giderme
 
## <a name="how-does-automatic-update-work"></a>Otomatik Güncelleştirme nasıl çalışır

Güncelleştirmelerini yönetmek Site Recovery izin sonra (Azure Hizmetleri tarafından kullanılır) bir genel runbook kasa ile aynı abonelikte oluşturulan bir Otomasyon hesabı aracılığıyla dağıtılır. Bir Otomasyon hesabı, belirli bir kasa için kullanılır. Runbook, her VM için otomatik güncelleştirmeleri üzerinde etkin bir kasada denetler ve varsa yeni bir sürüme yükseltme Mobility hizmeti uzantısı başlatır. Runbook'un varsayılan zamanlama günlük olarak saat 12: 00'da çoğaltılan sanal makinenin coğrafi saat dilimine göre yinelenir. Runbook zamanlama ayrıca Otomasyon hesabı kullanıcı tarafından gerekirse değiştirilebilir. 

> [!NOTE]
> Otomatik güncelleştirmeleri etkinleştirme, Azure sanal makinelerinizin yeniden başlatılmasını gerektirmez ve devam eden çoğaltma etkilemez.

> [!NOTE]
> Otomasyon hesabı tarafından kullanılan işleri için iş çalıştırma zamanı dahil edilen ücretsiz birimler için bir Otomasyon hesabı olarak ay ve varsayılan olarak 500 dakika kullanılan dakika sayısını göre faturalandırılır. İşin günlük tutarlardan yürütülmesi bir **hakkında bir dakika için birkaç saniye** ve **ücretsiz krediler kapsamdaki**.

Ücretsiz BİRİMLER (aylık) dahil ** fiyat iş çalıştırma zamanı 500 dakika ₹0.14 / dakika

## <a name="enable-automatic-updates"></a>Otomatik güncelleştirmeleri etkinleştir

Site Recovery, aşağıdaki yollarla güncelleştirmeleri yönetmek izin vermeyi seçebilirsiniz:-

- [Etkinleştirme çoğaltma adımının bir parçası](#as-part-of-the-enable-replication-step)
- [İki durumlu uzantısı kasa içinde ayarlarını güncelleştirme](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Etkinleştirme çoğaltma adımının bir parçası:

Etkinleştirdiğinizde çoğaltma bir sanal makine için başlangıç ya da [sanal makine görünümünden](azure-to-azure-quickstart.md), veya [kurtarma Hizmetleri kasasından](azure-to-azure-how-to-enable-replication.md), Site Recovery ya da izin vermeyi seçtiğiniz seçeneği alırsınız Site Recovery uzantısı için veya aynı el ile yönetmek için güncelleştirmeleri yönetme.

![Çoğaltma otomatik güncelleştirme etkinleştir](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>İki durumlu uzantısı kasa içinde ayarlarını güncelleştirme

1. Kasa içinde gidin **Yönet**-> **Site Recovery altyapısı**
2. Altında **Azure sanal makineleri**-> **uzantı güncelleştirme ayarları**, izin vermek isteyip istemediğinizi seçmek için Değiştir'i tıklatın *güncelleştirmeleri yönetmek için ASR* veya *el ile yönetmeniz*. **Kaydet**’e tıklayın.

![Kasa geçiş otomatik güncelleştirme](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Seçeneğini belirlediğinizde *ASR yönetmek için izin*, tüm sanal makinelere karşılık gelen kasayı ayarı uygulanır.


> [!Note] 
> Her iki seçenek, güncelleştirmeleri yönetmek için kullanılan Otomasyon hesabının bildirir. İlk kez bir kasada bu özelliği etkinleştirmek, yeni bir Otomasyon hesabı oluşturulur. Tüm sonraki etkinleştir çoğaltmalar aynı kasaya daha önce oluşturulmuş bir kullanır.

**Bir özel Otomasyon hesabı kullanmak istiyorsanız, Lütfen kullanmak aşağıdaki betiği:-**

```azurepowershell
param(
    [Parameter(Mandatory=$true)]
    [String] $VaultResourceId,

    [Parameter(Mandatory=$true)]
    [ValidateSet("Enabled",'Disabled')]
    [Alias("Enabled or Disabled")]
    [String] $AutoUpdateAction,

    [Parameter(Mandatory=$false)]
    [String] $AutomationAccountArmId
)

$SiteRecoveryRunbookName = "Modify-AutoUpdateForVaultForPatner"
$TaskId = [guid]::NewGuid().ToString()
$SubscriptionId = "00000000-0000-0000-0000-000000000000"
$AsrApiVersion = "2018-01-10"
$RunAsConnectionName = "AzureRunAsConnection"
$ArmEndPoint = "https://management.azure.com"
$AadAuthority = "https://login.windows.net/"
$AadAudience = "https://management.core.windows.net/"
$AzureEnvironment = "AzureCloud"
$Timeout = "160"

function Throw-TerminatingErrorMessage
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    throw ("Message: {0}, TaskId: {1}.") -f $Message, $TaskId
}

function Write-Tracing
{
    Param
    (
        [Parameter(Mandatory=$true)]      
        [ValidateSet("Informational", "Warning", "ErrorLevel", "Succeeded", IgnoreCase = $true)]
        [String]
        $Level,

        [Parameter(Mandatory=$true)]
        [String]
        $Message,

        [Switch]
        $DisplayMessageToUser
    )

    Write-Output $Message

}

function Write-InformationTracing
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    Write-Tracing -Message $Message -Level Informational -DisplayMessageToUser
}

function ValidateInput()
{
    try
    {
        if(!$VaultResourceId.StartsWith("/subscriptions", [System.StringComparison]::OrdinalIgnoreCase))
        {
            $ErrorMessage = "The vault resource id should start with /subscriptions."
            throw $ErrorMessage
        }

        $Tokens = $VaultResourceId.SubString(1).Split("/")
        if(!($Tokens.Count % 2 -eq 0))
        {
            $ErrorMessage = ("Odd Number of tokens: {0}." -f $Tokens.Count)
            throw $ErrorMessage
        }

        if(!($Tokens.Count/2 -eq 4))
        {
            $ErrorMessage = ("Invalid number of resource in vault ARM id expected:4, actual:{0}." -f ($Tokens.Count/2))
            throw $ErrorMessage
        }

        if($AutoUpdateAction -ieq "Enabled" -and [string]::IsNullOrEmpty($AutomationAccountArmId))
        {
            $ErrorMessage = ("The automation account ARM id should not be null or empty when AutoUpdateAction is enabled.")
            throw $ErrorMessage
        }
    }
    catch
    {
        $ErrorMessage = ("ValidateInput failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Initialize-SubscriptionId()
{
    try
    {
        $Tokens = $VaultResourceId.SubString(1).Split("/")

        $Count = 0
        $ArmResources = @{}
        while($Count -lt $Tokens.Count)
        {
            $ArmResources[$Tokens[$Count]] = $Tokens[$Count+1]
            $Count = $Count + 2
        }
        
        return $ArmResources["subscriptions"]
    }
    catch
    {
        Write-Tracing -Level ErrorLevel -Message ("Initialize-SubscriptionId: failed with [Exception: {0}]." -f $_.Exception) -DisplayMessageToUser
        throw
    }
}

function Invoke-InternalRestMethod($Uri, $Headers, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-RestMethod -Uri $Uri -Headers $Headers    
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Invoke-InternalWebRequest($Uri, $Headers, $Method, $Body, $ContentType, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-WebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -UseBasicParsing
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Get-Header([ref]$Header, $AadAudience, $AadAuthority, $RunAsConnectionName){
    try 
    {
        $RunAsConnection = Get-AutomationConnection -Name $RunAsConnectionName
        $TenantId = $RunAsConnection.TenantId
        $ApplicationId = $RunAsConnection.ApplicationId
        $CertificateThumbprint = $RunAsConnection.CertificateThumbprint
        $Path = "cert:\CurrentUser\My\{0}" -f $CertificateThumbprint
        $Secret = Get-ChildItem -Path $Path
        $ClientCredential = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.ClientAssertionCertificate(
                $ApplicationId,
                $Secret)

        # Trim the forward slash from the AadAuthority if it exist.
        $AadAuthority = $AadAuthority.TrimEnd("/")
        $AuthContext = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(
            "{0}/{1}" -f $AadAuthority, $TenantId )
        $AuthenticationResult = $authContext.AcquireToken($AadAudience, $Clientcredential)
        $Header.Value['Content-Type'] = 'application\json'
        $Header.Value['Authorization'] = $AuthenticationResult.CreateAuthorizationHeader()
        $Header.Value["x-ms-client-request-id"] = $TaskId + "/" + (New-Guid).ToString() + "-" + (Get-Date).ToString("u")
    }
    catch
    {
        $ErrorMessage = ("Get-BearerToken: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Get-ProtectionContainerToBeModified([ref] $ContainerMappingList)
{
    try 
    {
        Write-InformationTracing ("Get protection container mappings : {0}." -f $VaultResourceId)
        $ContainerMappingListUrl = $ArmEndPoint + $VaultResourceId + "/replicationProtectionContainerMappings" + "?api-version=" + $AsrApiVersion
        
        Write-InformationTracing ("Getting the bearer token and the header.")
        Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
        
        $Result = @()
        Invoke-InternalRestMethod -Uri $ContainerMappingListUrl -Headers $header -Result ([ref]$Result)
        $ContainerMappings = $Result[0]

        Write-InformationTracing ("Total retrieved container mappings: {0}." -f $ContainerMappings.Value.Count)
        foreach($Mapping in $ContainerMappings.Value)
        {
            if(($Mapping.properties.providerSpecificDetails -eq $null) -or ($Mapping.properties.providerSpecificDetails.instanceType -ine "A2A"))
            {
                Write-InformationTracing ("Mapping properties: {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the provider does not match." -f ($Mapping.Id))
                continue;
            }

            if($Mapping.Properties.State -ine "Paired")
            {
                Write-InformationTracing ("Ignoring container mapping: {0} as the the state is not paired." -f ($Mapping.Id))
                continue;
            }

            Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties.providerSpecificDetails))
            $MappingAutoUpdateStatus = $Mapping.properties.providerSpecificDetails.agentAutoUpdateStatus
            $MappingAutomationAccountArmId = $Mapping.properties.providerSpecificDetails.automationAccountArmId
            $MappingHealthErrorCount = $Mapping.properties.HealthErrorDetails.Count

            if($AutoUpdateAction -ieq "Enabled" -and
                ($MappingAutoUpdateStatus -ieq "Enabled") -and
                ($MappingAutomationAccountArmId -ieq $AutomationAccountArmId) -and
                ($MappingHealthErrorCount -eq 0))
            {
                Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the auto update is already enabled and is healthy." -f ($Mapping.Id))
                continue;
            }

            ($ContainerMappingList.Value).Add($Mapping.id)
        }
    }
    catch
    {
        $ErrorMessage = ("Get-ProtectionContainerToBeModified: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

$OperationStartTime = Get-Date
$ContainerMappingList = New-Object System.Collections.Generic.List[System.String]
$JobsInProgressList = @()
$JobsCompletedSuccessList = @()
$JobsCompletedFailedList = @()
$JobsFailedToStart = 0
$JobsTimedOut = 0
$Header = @{}

$AzureRMProfile = Get-Module -ListAvailable -Name AzureRM.Profile | Select Name, Version, Path
$AzureRmProfileModulePath = Split-Path -Parent $AzureRMProfile.Path
Add-Type -Path (Join-Path $AzureRmProfileModulePath "Microsoft.IdentityModel.Clients.ActiveDirectory.dll")

$Inputs = ("Tracing inputs VaultResourceId: {0}, Timeout: {1}, AutoUpdateAction: {2}, AutomationAccountArmId: {3}." -f $VaultResourceId, $Timeout, $AutoUpdateAction, $AutomationAccountArmId)
Write-Tracing -Message $Inputs -Level Informational -DisplayMessageToUser
$CloudConfig = ("Tracing cloud configuration ArmEndPoint: {0}, AadAuthority: {1}, AadAudience: {2}." -f $ArmEndPoint, $AadAuthority, $AadAudience)
Write-Tracing -Message $CloudConfig -Level Informational -DisplayMessageToUser
$AutomationConfig = ("Tracing automation configuration RunAsConnectionName: {0}." -f $RunAsConnectionName)
Write-Tracing -Message $AutomationConfig -Level Informational -DisplayMessageToUser

ValidateInput
$SubscriptionId = Initialize-SubscriptionId
Get-ProtectionContainerToBeModified ([ref]$ContainerMappingList)

$Input = @{
  "properties"= @{
    "providerSpecificInput"= @{
        "instanceType" = "A2A"
        "agentAutoUpdateStatus" = $AutoUpdateAction
        "automationAccountArmId" = $AutomationAccountArmId
    }
  }
}
$InputJson = $Input |  ConvertTo-Json

if ($ContainerMappingList.Count -eq 0)
{
    Write-Tracing -Level Succeeded -Message ("Exiting as there are no container mappings to be modified.") -DisplayMessageToUser
    exit
}

Write-InformationTracing ("Container mappings to be updated has been retrieved with count: {0}." -f $ContainerMappingList.Count)

try
{
    Write-InformationTracing ("Start the modify container mapping jobs.")
    ForEach($Mapping in $ContainerMappingList)
    {
    try {
            $UpdateUrl = $ArmEndPoint + $Mapping + "?api-version=" + $AsrApiVersion
            Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
            
            $Result = @()
            Invoke-InternalWebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -Result ([ref]$Result)
            $Result = $Result[0]

            $JobAsyncUrl = $Result.Headers['Azure-AsyncOperation']
            Write-InformationTracing ("The modify container mapping job invoked with async url: {0}." -f $JobAsyncUrl)
            $JobsInProgressList += $JobAsyncUrl;

            # Rate controlling the set calls to maximum 60 calls per minute.
            # ASR throttling for set calls is 200 in 1 minute.
            Start-Sleep -Milliseconds 1000
        }
        catch{
            Write-InformationTracing ("The modify container mappings job creation failed for: {0}." -f $Ru)
            Write-InformationTracing $_
            $JobsFailedToStart++
        }
    }

    Write-InformationTracing ("Total modify container mappings has been initiated: {0}." -f $JobsInProgressList.Count)
}
catch
{
    $ErrorMessage = ("Modify container mapping jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

try
{
    while($JobsInProgressList.Count -ne 0)
    {
        Sleep -Seconds 30
        $JobsInProgressListInternal = @()
        ForEach($JobAsyncUrl in $JobsInProgressList)
        {
            try
            {
                Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
                $Result = Invoke-RestMethod -Uri $JobAsyncUrl -Headers $header
                $JobState = $Result.Status
                if($JobState -ieq "InProgress")
                {
                    $JobsInProgressListInternal += $JobAsyncUrl
                }
                elseif($JobState -ieq "Succeeded" -or `
                    $JobState -ieq "PartiallySucceeded" -or `
                    $JobState -ieq "CompletedWithInformation")
                {
                    Write-InformationTracing ("Jobs succeeded with state: {0}." -f $JobState)
                    $JobsCompletedSuccessList += $JobAsyncUrl
                }
                else
                {
                    Write-InformationTracing ("Jobs failed with state: {0}." -f $JobState)
                    $JobsCompletedFailedList += $JobAsyncUrl
                }
            }
            catch
            {
                Write-InformationTracing ("The get job failed with: {0}. Ignoring the exception and retrying the next job." -f $_.Exception)

                # The job on which the tracking failed, will be considered in progress and tried again later.
                $JobsInProgressListInternal += $JobAsyncUrl
            }

            # Rate controlling the get calls to maximum 120 calls per minute.
            # ASR throttling for get calls is 10000 in 60 minutes.
            Start-Sleep -Milliseconds 500
        }

        Write-InformationTracing ("Jobs remaining {0}." -f $JobsInProgressListInternal.Count)

        $CurrentTime = Get-Date
        if($CurrentTime -gt $OperationStartTime.AddMinutes($Timeout))
        {
            Write-InformationTracing ("Tracing modify cloud pairing jobs has timed out.")
            $JobsTimedOut = $JobsInProgressListInternal.Count
            $JobsInProgressListInternal = @()
        }

        $JobsInProgressList = $JobsInProgressListInternal
    }
}
catch
{
    $ErrorMessage = ("Tracking modify cloud pairing jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage  -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage 
}

Write-InformationTracing ("Tracking modify cloud pairing jobs completed.")
Write-InformationTracing ("Modify cloud pairing jobs success: {0}." -f $JobsCompletedSuccessList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed: {0}." -f $JobsCompletedFailedList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed to start: {0}." -f $JobsFailedToStart)
Write-InformationTracing ("Modify cloud pairing jobs timedout: {0}." -f $JobsTimedOut)

if($JobsTimedOut -gt  0)
{
    $ErrorMessage = "One or more modify cloud pairing jobs has timedout."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)   
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}
elseif($JobsCompletedSuccessList.Count -ne $ContainerMappingList.Count)
{
    $ErrorMessage = "One or more modify cloud pairing jobs failed."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

Write-Tracing -Level Succeeded -Message ("Modify cloud pairing completed.") -DisplayMessageToUser
```

### <a name="manage-manually"></a>El ile yönet

1. Azure sanal makinelerinizde yüklü mobilite hizmeti için kullanılabilir yeni güncelleştirmeler varsa, "Yeni Site recovery çoğaltma aracısı güncelleştirmesi kullanılabilir. okuyan bir bildirim görür Yüklemek için tıklayın."

     ![Çoğaltılan öğeler penceresi](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)
3. Sanal makine seçimi sayfasını açmak için bildirimi seçin.
4. İstediğiniz üzerinde mobility hizmetini ve sanal makineleri seçin **Tamam**.

     ![Çoğaltılan öğeler VM listesi](./media/vmware-azure-install-mobility-service/update-okpng.png)

Her bir seçili sanal makine için Mobility hizmetini güncelleştirme işlemi başlatır.


## <a name="common-issues--troubleshooting"></a>Sık karşılaşılan sorunlar ve sorun giderme

Otomatik Güncelleştirmeler ile'ilgili bir sorun varsa, aynı 'Yapılandırma sorunlarını' kasa panosunda altında bildirilir. 

Otomatik güncelleştirmeleri etkinleştir çalışıldı ve başarısız durumunda, aşağıdaki sorun giderme için bkz.

**Hata**: Azure Farklı Çalıştır hesabı (hizmet sorumlusu) oluşturma ve hizmet sorumlusuna Katkıda Bulunan rolü verme izniniz yok. 
- Önerilen eylem: Oturum açılan hesaba 'Katkıda bulunan' atandığından emin olun ve işlemi yeniden deneyin. Başvurmak [bu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) belge doğru izinleri atama hakkında daha fazla bilgi için.
 
Otomatik Güncelleştirmeler üzerinde etkin bir kez sorunların çoğu Site Recovery hizmeti tarafından taşınarak ve tıklayarak gerektirir '**onarım**' düğmesi.

![Onarma düğmesi](./media/azure-to-azure-autoupdate/repair.png)

Onarma düğmesi kullanılamaz durumda uzantı ayarları bölmesi altında görüntülenen hata iletisini inceleyin.

 - **Hata**: Farklı Çalıştır hesabının kurtarma Hizmetleri kaynağına erişim izni yok.

    **Önerilen eylem**: Silin ve ardından [farklı çalıştır hesabını yeniden oluşturma](https://docs.microsoft.com/azure/automation/automation-create-runas-account) veya Otomasyon farklı çalıştır hesabı Azure Active Directory uygulamasının kurtarma Hizmetleri kaynağına erişimi olduğundan emin olun.

- **Hata**: Farklı Çalıştır hesabı bulunamadı. Ya da bunlardan biri silinmiş veya oluşturulmamış: Azure Active Directory uygulaması, hizmet sorumlusu, rol, Otomasyon sertifikası varlığı, Otomasyon bağlantısı varlığı; ya da parmak izi sertifika ve bağlantı arasındaki aynı değil. 

    **Önerilen eylem**: Silme ve [sonra farklı çalıştır hesabını yeniden oluşturun](https://docs.microsoft.com/azure/automation/automation-create-runas-account).
