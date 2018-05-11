---
title: Azure Automation farklı çalıştır hesapları oluşturun
description: Bu makalede, Otomasyon hesabınızı güncelleştirme ve PowerShell ile ya da portaldan Farklı Çalıştır hesapları oluşturma işlemi açıklanmaktadır.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 03a5cf6fd049e63616155a87bf112636b74be0a1
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="update-your-automation-account-authentication-with-run-as-accounts"></a>Farklı Çalıştır hesaplarıyla Otomasyon hesabı kimlik doğrulamasını güncelleştirme 
Mevcut Otomasyon hesabınızı Azure portalından güncelleştirebilir veya aşağıdaki durumlarda PowerShell kullanabilirsiniz:

* Bir Otomasyon hesabı oluşturdunuz, ancak Farklı Çalıştır hesabı oluşturmayı reddettiniz.
* Resource Manager kaynaklarını yönetmek için bir Otomasyon hesabı zaten kullanıyorsunuz ve runbook kimlik doğrulaması için Farklı Çalıştır hesabını içerecek şekilde güncelleştirmek istiyorsunuz.
* Klasik kaynakları yönetmek için bir Otomasyon hesabı zaten kullanıyorsunuz ve yeni bir hesap oluşturup runbook’larınızı ve varlıklarınızı ona geçirmek yerine Klasik Farklı Çalıştır hesabını kullanacak şekilde güncelleştirmek istiyorsunuz.   

İşlem, Otomasyon hesabınızda aşağıdaki öğeleri oluşturur.

**Farklı Çalıştır hesapları için:**

* Otomatik olarak imzalanan bir sertifika ile Azure AD uygulaması oluşturur, Azure AD’de bu uygulama için bir hizmet sorumlusu hesabı oluşturur ve geçerli aboneliğinizde hesap için Katkıda Bulunan rolünü atar. Bu ayarı Sahip veya başka bir rolle değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md).
* Belirtilen Otomasyon hesabında *AzureRunAsCertificate* adlı bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlıkları, Azure AD uygulaması tarafından kullanılan sertifika özel anahtarını içerir.
* Belirtilen Otomasyon hesabında *AzureRunAsConnection* adlı bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı applicationId, tenantId, subscriptionId ve sertifika parmak izini içerir.

**Klasik Farklı Çalıştır hesapları için:**

* Belirtilen Otomasyon hesabında *AzureClassicRunAsCertificate* adlı bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlığı, yönetim sertifikası tarafından kullanılan sertifika özel anahtarını içerir.
* Belirtilen Otomasyon hesabında *AzureClassicRunAsConnection* adlı bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı; abonelik adı, subscriptionId ve sertifika varlık adını içerir.

## <a name="prerequisites"></a>Önkoşullar
[PowerShell kullanarak Farklı Çalıştır hesapları oluşturmayı](#create-run-as-account-using-powershell) seçerseniz bu işlem için şunlar gerekir:

* Azure Resource Manager 3.4.1 veya sonrasındaki modülleri içeren Windows 10 ve Windows Server 2016. PowerShell betiği önceki Windows sürümlerini desteklemez.
* Azure PowerShell 1.0 ve üzeri. PowerShell 1.0 sürümü hakkında bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).
* *–AutomationAccountName* ve *-ApplicationDisplayName* parametrelerinin değeri olarak başvurulan bir Otomasyon hesabı.

Betik parametreleri için gerekli olan *SubscriptionID*, *ResourceGroup* ve *AutomationAccountName* değerlerini almak için aşağıdakileri yapın:

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Otomasyon** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Automation Hesapları**’nı seçin.
2. Otomasyon hesap sayfasında Otomasyon hesabınızı seçin ve ardından **Hesap Ayarları** altında **Özellikler**’i seçin.  
3. **Özellikler** sayfasındaki değerleri not alın.<br><br> ![Otomasyon hesabı "Özellikler" dikey penceresi](media/automation-create-runas-account/automation-account-properties.png)  

### <a name="required-permissions-to-update-your-automation-account"></a>Otomasyon hesabınızı güncelleştirmek için gereken izinler
Otomasyon hesabını güncelleştirmek isterseniz bu konuyu tamamlamak için gereken aşağıdaki özel ayrıcalıklara ve izinlere sahip olmanız gerekir.   
 
* AD kullanıcı hesabınızın Microsoft.Automation kaynakları için katılımcı rolü eşdeğer izinlere sahip bir rol makalesinde ana hatlarıyla eklenmeli [Azure automation'da rol tabanlı erişim denetimi](automation-role-based-access-control.md#contributor).  
* Azure AD kiracınızdaki yönetici olmayan kullanıcıların [AD uygulamalarını kaydedebilmesi için](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) Azure AD kiracısının **Kullanıcı ayarları** sayfasındaki **Kullanıcılar uygulamaları kaydedebilir** seçeneği **Evet** olarak ayarlanmış olmalıdır. Uygulama kayıtları ayarı **Hayır** olarak ayarlanırsa bu işlemi gerçekleştiren kullanıcının, Azure AD’de genel yönetici olması gerekir.

Aboneliğin genel yönetici/ortak yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğine üye değilseniz Active Directory’ye konuk olarak eklenirsiniz. Bu durumda, “Oluşturma izniniz yok…” iletisini alırsınız. uyarısını **Otomasyon Hesabı Ekle** dikey penceresinde görürsünüz. İlk olarak genel yönetici/ortak yönetici rolüne eklenen kullanıcılar aboneliğin Active Directory örneğinden kaldırılabilir ve tekrar eklenerek Active Directory’de tam bir Kullanıcı haline getirilebilir. Bu durumu doğrulamak için Azure portalındaki **Azure Active Directory** bölmesinde **Kullanıcılar ve gruplar**’ı, **Tüm kullanıcılar**’ı seçin ve belirli bir kullanıcıyı seçtikten sonra **Profil**’i seçin. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin **Konuk** olmaması gerekir.

## <a name="create-run-as-account-from-the-portal"></a>Portaldan Farklı Çalıştır hesabı oluşturma
Bu bölümde, Azure portalında Azure Otomasyonu hesabınızı güncelleştirmek için aşağıdaki adımları uygulayın. Farklı Çalıştır ve Klasik Farklı Çalıştır hesaplarını ayrı ayrı oluşturabilirsiniz. Klasik kaynak oluşturmanıza gerek yoksa yalnızca Azure Farklı Çalıştır hesabını oluşturabilirsiniz.  

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Otomasyon** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Automation Hesapları**’nı seçin.
3. **Otomasyon Hesapları** sayfasındaki Otomasyon hesapları listesinden Otomasyon hesabınızı seçin.
4. Sol bölmedeki **Hesap Ayarları** bölümünde **Farklı Çalıştır Hesapları**'nı seçin.  
5. Gereken hesaba bağlı olarak **Azure Farklı Çalıştır Hesabı**’nı veya **Azure Klasik Farklı Çalıştır Hesabı**’nı seçin. Seçim sonrasında **Azure Farklı Çalıştır Ekle** veya **Azure Klasik Farklı Çalıştır Hesabı Ekle** bölmesi görüntülenir ve genel bakış bilgilerini gözden geçirdikten sonra Farklı Çalıştır hesabı oluşturma işlemine devam etmek için **Oluştur**’a tıklamanız gerekir.  
6. Azure Farklı Çalıştır hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz. Hesabın oluşturulduğunu belirten bir başlık da gösterilir. Bu işlemin tamamlanması birkaç dakika sürebilir.  

## <a name="create-run-as-account-using-powershell-script"></a>PowerShell betiği ile Farklı Çalıştır hesabı oluşturma
Bu PowerShell betiği aşağıdaki yapılandırmalar için destek içerir:

* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturun.
* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.
* Kuruluş sertifika yetkiliniz (CA) tarafından verilen bir sertifika kullanarak bir Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı oluşturun.
* Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.

>[!NOTE]
> Klasik Farklı Çalıştır hesabı oluşturma seçeneğini belirlerseniz, betik yürütüldükten sonra Otomasyon hesabının oluşturulduğu abonelik için yönetim deposuna ortak sertifikayı (.cer dosya adı uzantısı) yükleyin.
> 

1. Aşağıdaki betiği bilgisayarınıza kaydedin. Bu örnekte *New-RunAsAccount.ps1* dosya adıyla kaydedin.

    ```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,
        
        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,
        
        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,
        
        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,
        
        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,
        
        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,
        
        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForRunAsAccount,
        
        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,
        
        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,
        
        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,
        
        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",
        
        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )
        
    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256
        
        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }
        
    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid
        
        $startDate = Get-Date
        $endDate = (Get-Date $PfxCert.GetExpirationDateString()).AddDays(-1)
        
        #Create an Azure AD application, AD App Credential, AD ServicePrincipal
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId) 
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $startDate -EndDate $endDate 
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
        
        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }
        
    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }
        
    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }
        
    Import-Module AzureRM.Profile
    Import-Module AzureRM.Resources
        
    $AzureRMProfileVersion = (Get-Module AzureRM.Profile).Version
    if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 4) -or ($AzureRMProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }
        
    Connect-AzureRmAccount -Environment $EnvironmentName 
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        
    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"
        
    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }
        
    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName
        
    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true
        
    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}
        
    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues
        
    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"
        
        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }
        
        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false
        
        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}
        
        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues
        
        Write-Host -ForegroundColor red       $UploadMessage
    }
    ```

2. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
3. Yükseltilmiş komut satırı kabuğundan, 1. adımda oluşturduğunuz komut dosyasını içeren klasöre gidin.  
4. İstediğiniz yapılandırmanın parametre değerlerini kullanarak betiği yürütün.

    **Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Kurumsal sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Betik yürütüldükten sonra Azure kimlik doğrulamasını yapmanız istenecektir. Aboneliğin yöneticiler rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
    >
    >

Betik başarıyla yürütüldükten sonra aşağıdakilere dikkat edin:
* Otomatik olarak imzalanan bir ortak sertifika (.cer dosyası) ile Klasik Farklı Çalıştır hesabı oluşturduysanız, betik bu hesabı oluşturup bilgisayarınızdaki geçici dosya klasörüne, PowerShell oturumunu yürütmek için kullandığınız *%USERPROFILE%\AppData\Local\Temp* kullanıcı profili altında kaydeder.
* Kurumsal ortak sertifika (.cer file) ile bir Klasik Farklı Çalıştır hesabı oluşturduysanız bu sertifikayı kullanın. Yönergeleri izleyin [yönetim API sertifikası Azure portalına](../azure-api-management-certs.md)ve ardından kullanarak Klasik dağıtım kaynaklarla kimlik bilgisi yapılandırmasını doğrulamak [örnek kimlik doğrulaması için kod Azure Klasik dağıtım kaynaklarla](automation-verify-runas-authentication.md#classic-run-as-authentication). 
* Klasik Farklı Çalıştır hesabı *oluşturmadıysanız*, Resource Manager kaynakları kimlik doğrulaması yapmak ve kimlik bilgisi yapılandırmasını doğrulamak için [Service Management kaynakları ile kimlik doğrulamaya yönelik örnek kodu](automation-verify-runas-authentication.md#automation-run-as-authentication) kullanın.

## <a name="limiting-run-as-account-permissions"></a>Farklı Çalıştır hesabı izinleri sınırlama

Azure Automation kaynaklarına karşı Otomasyon hedefleme denetlemek için farklı çalıştır hesabı varsayılan olarak abonelikte katılımcı hakları verilir. RunAs hizmet sorumlusu neler yapabileceğinizi kısıtlamak gerekiyorsa, hesabı için abonelik katkıda bulunan rolünden kaldırmak ve belirtmek istediğiniz kaynak grupları Katılımcısı olarak ekleyin.

Azure portalında seçin **abonelikleri** ve Otomasyon hesabınızda abonelik seçin. Seçin **erişim denetimi (IAM)** ve Automation hesabınız için hizmet sorumlusu arayın (gibi görünüyor \<AutomationAccountName\>_unique tanımlayıcısı). Hesabını seçin ve tıklatın **kaldırmak** abonelikten kaldırmak için.

![Abonelik Katkıda Bulunanlar](media/automation-create-runas-account/automation-account-remove-subscription.png)

Hizmet sorumlusu bir kaynak grubuna eklemek için kaynak grubu seçin ve portal Azure'da seçin **erişim denetimi (IAM)**. Seçin **Ekle**, bu açılır **izinleri eklemek** sayfası. İçin **rol**seçin **katkıda bulunan**. İçinde **seçin** metin kutusunda, farklı çalıştır hesabı için hizmet asıl adını yazın ve listeden seçin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın. Bu, Azure Automation farklı çalıştır hizmet asıl erişim vermek istediğiniz kaynakları grupları için gerçekleştirin.

## <a name="next-steps"></a>Sonraki adımlar
* Hizmet sorumluları hakkında daha fazla bilgi için bkz: [uygulama ve hizmet sorumlusu nesneleri](../active-directory/active-directory-application-objects.md).
* Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için bkz: [Azure Cloud Services sertifikalarına genel bakış](../cloud-services/cloud-services-certs-create.md).
