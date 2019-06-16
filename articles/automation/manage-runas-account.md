---
title: Azure Otomasyonu farklı çalıştır hesaplarını yönetme
description: Bu makalede, farklı çalıştır hesapları PowerShell ile veya portalından yönetmek açıklar.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/24/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 140b1263047849e13a44441c368e6357078574d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240803"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Azure Otomasyonu farklı çalıştır hesaplarını yönetme

Farklı Çalıştır hesapları Azure Otomasyonu'nda Azure cmdlet'lerini ile Azure kaynaklarını yönetmek için kimlik doğrulaması sağlamak için kullanılır.

Bir farklı çalıştır hesabı oluşturduğunuzda, Azure Active Directory'de yeni bir hizmet sorumlusu kullanıcısı oluşturur ve bu kullanıcıya abonelik düzeyinde katılımcı rolü atar. Karma Runbook çalışanları Azure sanal makinelerde kullanmak, runbook'lar için kullanabileceğiniz [kimliklerini Azure kaynakları için yönetilen](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) Azure kaynaklarınıza kimlik doğrulaması için farklı çalıştır hesapları yerine.

Farklı Çalıştır hesapları iki tür vardır:

* **Azure farklı çalıştır hesabı** -bu hesabı yönetmek için kullanılan [Resource Manager dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md) kaynakları.
  * Otomatik olarak imzalanan bir sertifika ile Azure AD uygulaması oluşturur, Azure AD’de bu uygulama için bir hizmet sorumlusu hesabı oluşturur ve geçerli aboneliğinizde hesap için Katkıda Bulunan rolünü atar. Bu ayarı Sahip veya başka bir rolle değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md).
  * Belirtilen Otomasyon hesabında *AzureRunAsCertificate* adlı bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlıkları, Azure AD uygulaması tarafından kullanılan sertifika özel anahtarını içerir.
  * Belirtilen Otomasyon hesabında *AzureRunAsConnection* adlı bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı applicationId, tenantId, subscriptionId ve sertifika parmak izini içerir.

* **Azure Klasik farklı çalıştır hesabı** -bu hesabı yönetmek için kullanılan [Klasik dağıtım modelini](../azure-resource-manager/resource-manager-deployment-model.md) kaynakları.
  * Abonelikte bir yönetim sertifikası oluşturur
  * Belirtilen Otomasyon hesabında *AzureClassicRunAsCertificate* adlı bir Otomasyon sertifikası varlığı oluşturur. Sertifika varlığı, yönetim sertifikası tarafından kullanılan sertifika özel anahtarını içerir.
  * Belirtilen Otomasyon hesabında *AzureClassicRunAsConnection* adlı bir Otomasyon bağlantı varlığı oluşturur. Bağlantı varlığı; abonelik adı, subscriptionId ve sertifika varlık adını içerir.
  * Oluşturma veya yenilemek için aboneliğin ortak yönetici olmanız gerekir
  
  > [!NOTE]
  > Azure bulut çözümü sağlayıcısı (Azure CSP) abonelikleri yalnızca Azure Resource Manager modeline destek, Azure Resource Manager - program kullanılamıyor. CSP aboneliklerini kullanırken Azure Klasik farklı çalıştır hesabı oluşturulmamış. Azure farklı çalıştır hesabını yine de oluşturulur. CSP abonelikleri hakkında daha fazla bilgi edinmek için [CSP aboneliklerinde kullanılabilir hizmetler](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments).

  > [!NOTE]
  > Bir farklı çalıştır hesabı için hizmet sorumlusu, varsayılan olarak Azure Active Directory Okuma izinlerine sahip değil. Okumak veya Azure Active Directory'yi yönetmek için izinler eklemek istiyorsanız, hizmet sorumlusu altında bu izni gerekir **API izinleri**. Daha fazla bilgi için bkz. [web API'lerine erişim izni ekleyin](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="permissions"></a>Farklı Çalıştır hesaplarını yapılandırmak için izinler

Oluşturun veya bir farklı çalıştır hesabını güncelleştirmek için özel ayrıcalıklara ve izinlere olmalıdır. Azure Active Directory'de genel yönetici ve abonelikte sahip tüm görevleri tamamlayabilirsiniz. Görev ayrımı sahip olduğu bir durumda, görevleri, eşdeğer cmdlet ve gerekli izinlere listesini aşağıdaki tabloda gösterilmiştir:

|Görev|Cmdlet  |En düşük izinleri  |İzinleri ayarladığınız yerdir|
|---|---------|---------|---|
|Azure AD uygulaması oluşturun|[New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication)     | Uygulama geliştirici rolünü<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Giriş > Azure Active Directory > Uygulama kayıtları |
|Bir kimlik bilgisi uygulamaya ekleyin.|[New-AzureRmADAppCredential](/powershell/module/AzureRM.Resources/New-AzureRmADAppCredential)     | Uygulama Yöneticisi veya genel yönetici<sup>1</sup>         |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Giriş > Azure Active Directory > Uygulama kayıtları|
|Oluşturma ve bir Azure AD hizmet sorumlusu alma|[Yeni AzureRMADServicePrincipal](/powershell/module/AzureRM.Resources/New-AzureRmADServicePrincipal)</br>[Get-AzureRmADServicePrincipal](/powershell/module/AzureRM.Resources/Get-AzureRmADServicePrincipal)     | Uygulama Yöneticisi veya genel yönetici<sup>1</sup>        |[Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Giriş > Azure Active Directory > Uygulama kayıtları|
|Atayın veya RBAC rolü için belirtilen sorumluyu alma|[Yeni-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)</br>[Get-AzureRMRoleAssignment](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)      | Aşağıdaki izinlere sahip olmalıdır:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br>Veya y:</br></br>Kullanıcı erişimi Yöneticisi veya sahibi        | [Abonelik](../role-based-access-control/role-assignments-portal.md)</br>Giriş > abonelikler > \<abonelik adı\> -erişim denetimi (IAM)|
|Oluşturma veya bir Otomasyon sertifikası kaldırma|[New-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/New-AzureRmAutomationCertificate)</br>[Remove-AzureRmAutomationCertificate](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationCertificate)     | Kaynak grubu üzerinde katkıda bulunan         |Otomasyon hesabı kaynak grubu|
|Oluşturma veya bir Otomasyon bağlantı kaldırma|[New-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/New-AzureRmAutomationConnection)</br>[Remove-AzureRmAutomationConnection](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationConnection)|Kaynak grubu üzerinde katkıda bulunan |Otomasyon hesabı kaynak grubu|

<sup>1</sup> Azure AD kiracınızdaki yönetici olmayan kullanıcılar için [AD uygulamalarını kaydedebilir](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) varsa Azure AD kiracısının **kullanıcılar uygulamaları kaydedebilir** seçeneğini **kullanıcı ayarları**sayfası ayarlandığında **Evet**. Uygulama kayıtları ayarı ayarlanırsa **Hayır**, bu eylemi gerçekleştiren kullanıcı, önceki tabloda tanımlanan olması gerekir.

Eklemiş önce aboneliğin Active Directory örneğine üye değilseniz **genel yönetici** rolü, aboneliği, bir konuk olarak eklenir. Bu durumda, aldığınız bir `You do not have permissions to create…` üzerinde uyarı **Otomasyon hesabı Ekle** sayfası. İçin eklenen kullanıcılar **genel yönetici** rol ilk kullanılabilir aboneliğin Active Directory örneğinden kaldırılabilir ve tekrar eklenerek Active Directory'de tam bir kullanıcı olacak şekilde. Bu durumu doğrulamak için Azure portalındaki **Azure Active Directory** bölmesinde **Kullanıcılar ve gruplar**’ı, **Tüm kullanıcılar**’ı seçin ve belirli bir kullanıcıyı seçtikten sonra **Profil**’i seçin. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin **Konuk** olmaması gerekir.

## <a name="permissions-classic"></a>Klasik farklı çalıştır hesaplarını yapılandırmak için izinler

Yapılandırma veya Klasik farklı çalıştır hesapları yenilemek için olmalıdır **ortak yönetici** abonelik düzeyinde rolü. Klasik izinler hakkında daha fazla bilgi edinmek için [Azure Klasik abonelik yöneticileri](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="create-a-run-as-account-in-the-portal"></a>Portalda bir farklı çalıştır hesabı oluşturma

Bu bölümde, Azure portalında Azure Otomasyonu hesabınızı güncelleştirmek için aşağıdaki adımları uygulayın. Farklı Çalıştır ve Klasik Farklı Çalıştır hesaplarını ayrı ayrı oluşturabilirsiniz. Klasik kaynak oluşturmanıza gerek yoksa yalnızca Azure Farklı Çalıştır hesabını oluşturabilirsiniz.  

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Otomasyon** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Automation Hesapları**’nı seçin.
3. **Otomasyon Hesapları** sayfasındaki Otomasyon hesapları listesinden Otomasyon hesabınızı seçin.
4. Sol bölmedeki **Hesap Ayarları** bölümünde **Farklı Çalıştır Hesapları**'nı seçin.  
5. Gereken hesaba bağlı olarak **Azure Farklı Çalıştır Hesabı**’nı veya **Azure Klasik Farklı Çalıştır Hesabı**’nı seçin. Seçim sonrasında **Azure Farklı Çalıştır Ekle** veya **Azure Klasik Farklı Çalıştır Hesabı Ekle** bölmesi görüntülenir ve genel bakış bilgilerini gözden geçirdikten sonra Farklı Çalıştır hesabı oluşturma işlemine devam etmek için **Oluştur**’a tıklamanız gerekir.  
6. Azure Farklı Çalıştır hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz. Hesabın oluşturulduğunu belirten bir başlık da gösterilir. Bu işlemin tamamlanması birkaç dakika sürebilir.  

## <a name="create-run-as-account-using-powershell"></a>PowerShell kullanarak farklı çalıştır hesabı oluşturma

## <a name="prerequisites"></a>Önkoşullar

PowerShell'de bir farklı çalıştır hesabı oluşturma için gereksinimler aşağıdaki listede verilmiştir:

* Windows 10 veya Azure Resource Manager 3.4.1 veya sonrasındaki modülleri içeren Windows Server 2016 ve üzeri. PowerShell betiği önceki Windows sürümlerini desteklemez.
* Azure PowerShell 1.0 ve üzeri. PowerShell 1.0 sürümü hakkında bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).
* *–AutomationAccountName* ve *-ApplicationDisplayName* parametrelerinin değeri olarak başvurulan bir Otomasyon hesabı.
* Ne listelenen için eşdeğer izinlere [gerekli izinler farklı çalıştır hesaplarını yapılandırmak için](#permissions)

Değerlerini almak için *Subscriptionıd*, *ResourceGroup*, ve *AutomationAccountName*, parametreleri komut dosyası için gerekli olan, aşağıdaki adımları tamamlayın:

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Otomasyon** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Automation Hesapları**’nı seçin.
1. Otomasyon hesap sayfasında Otomasyon hesabınızı seçin ve ardından **Hesap Ayarları** altında **Özellikler**’i seçin.  
1. Not **abonelik kimliği**, **adı**, ve **kaynak grubu** üzerinde değerleri **özellikleri** sayfası.

   ![Otomasyon hesabı "Özellikler" sayfası](media/manage-runas-account/automation-account-properties.png)

Bu PowerShell betiği aşağıdaki yapılandırmalar için destek içerir:

* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturun.
* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.
* Kuruluş sertifika yetkiliniz (CA) tarafından verilen bir sertifika kullanarak bir Farklı Çalıştır ve Klasik Farklı Çalıştır hesabı oluşturun.
* Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.

>[!NOTE]
> Klasik Farklı Çalıştır hesabı oluşturma seçeneğini belirlerseniz, betik yürütüldükten sonra Otomasyon hesabının oluşturulduğu abonelik için yönetim deposuna ortak sertifikayı (.cer dosya adı uzantısı) yükleyin.

1. Aşağıdaki betiği bilgisayarınıza kaydedin. Bu örnekte *New-RunAsAccount.ps1* dosya adıyla kaydedin.

   Betik, kaynak oluşturmak için birden çok Azure Resource Manager cmdlet'lerini kullanır. Aşağıdaki tabloda, cmdlet'ler ve gerekli izinleri gösterilir.

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
        [string] $EnterpriseCertPathForRunAsAccount,

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

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId) 
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
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

    # To use the new Az modules to create your Run As accounts please uncomment the following lines and ensure you comment out the previous 8 lines that import the AzureRM modules to avoid any issues. To learn about about using Az modules in your Automation Account see https://docs.microsoft.com/azure/automation/az-modules

    # Import-Module Az.Automation
    # Enable-AzureRmAlias


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

    > [!IMPORTANT]
    > **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya [modüllerinizi güncelleştirme](automation-update-azure-modules.md) , Otomasyon Hesabı.

1. Bilgisayarınızda **Windows PowerShell**’i yükseltilmiş kullanıcı haklarına sahip **Başlat** ekranından başlatın.
1. Yükseltilmiş komut satırı kabuğundan, 1. adımda oluşturduğunuz komut dosyasını içeren klasöre gidin.  
1. İstediğiniz yapılandırmanın parametre değerlerini kullanarak betiği yürütün.

    **Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturma**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
    ```

    **Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
    ```

    **Kurumsal sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma**  

    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
    ```

    **Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma**
  
    ```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
    ```

    > [!NOTE]
    > Betik yürütüldükten sonra Azure kimlik doğrulamasını yapmanız istenecektir. Aboneliğin yöneticiler rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

Betik başarıyla yürütüldükten sonra aşağıdakilere dikkat edin:

* Otomatik olarak imzalanan bir ortak sertifika (.cer dosyası) ile Klasik Farklı Çalıştır hesabı oluşturduysanız, betik bu hesabı oluşturup bilgisayarınızdaki geçici dosya klasörüne, PowerShell oturumunu yürütmek için kullandığınız *%USERPROFILE%\AppData\Local\Temp* kullanıcı profili altında kaydeder.

* Kurumsal ortak sertifika (.cer file) ile bir Klasik Farklı Çalıştır hesabı oluşturduysanız bu sertifikayı kullanın. Yönergelerini izleyin [Azure portalında yönetim API sertifikayı yükleme](../azure-api-management-certs.md).

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını silme

Bu bölümde bir Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını silip yeniden oluşturma işlemi açıklamaktadır. Bu eylemi gerçekleştirdiğinizde Otomasyon hesabı korunur. Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını sildikten sonra Azure portalında yeniden oluşturabilirsiniz.

1. Azure portalında Otomasyon hesabınızı açın.

2. **Otomasyon hesabı** sayfasında **Farklı Çalıştır Hesapları**'nı seçin.

3. **Farklı Çalıştır Hesapları** özellikleri sayfasında silmek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin. Ardından, seçili hesabın **Özellikler** bölmesinde **Sil**'e tıklayın.

   ![Farklı Çalıştır hesabını silme](media/manage-runas-account/automation-account-delete-runas.png)

1. Hesap silinirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

1. Hesap silindikten sonra **Farklı Çalıştır Hesapları** özellikler sayfasında **Azure Farklı Çalıştır Hesabı** seçeneğini belirleyerek hesabı yeniden oluşturabilirsiniz.

   ![Otomasyon Farklı Çalıştır hesabını yeniden oluşturma](media/manage-runas-account/automation-account-create-runas.png)

## <a name="cert-renewal"></a>Otomatik olarak imzalanan sertifika yenileme

Belirli bir noktada Çalıştır hesabınızın süresi dolmadan önce sertifikayı yenilemeniz gerekir. Farklı Çalıştır hesabının tehlikede olduğunu düşünüyorsanız, hesabı silip yeniden oluşturabilirsiniz. Bu bölümde bu işlemlerin nasıl gerçekleştirileceği ele alınmaktadır.

Farklı Çalıştır hesabı için oluşturduğunuz otomatik olarak imzalanan sertifikanın süresi, oluşturulduktan bir yıl sonra dolar. Sertifikayı süresi dolmadan önce herhangi bir zamanda yenileyebilirsiniz. Yenilediğinizde, yukarı veya etkin olarak çalışan sıraya ve bu farklı çalıştır hesabıyla kimlik doğrulaması runbook'ların olumsuz etkilenmez emin olmak için geçerli sertifika saklanır. Sertifika, sona erme tarihine kadar geçerliliğini sürdürür.

> [!NOTE]
> Otomasyon Farklı Çalıştır hesabınızı, kuruluş sertifika yetkiliniz tarafından yayınlanan bir sertifika kullanmak üzere yapılandırdıysanız ve bu seçeneği kullanırsanız, kurumsal sertifika otomatik olarak imzalanan bir sertifikayla değiştirilir.

Sertifikayı yenilemek için aşağıdakileri yapın:

1. Azure portalında Otomasyon hesabınızı açın.

1. Seçin **farklı çalıştır hesapları** altında **hesap ayarları**.

    ![Otomasyon hesabı özellikleri bölmesi](media/manage-runas-account/automation-account-properties-pane.png)

1. **Farklı Çalıştır Hesapları** özellikleri sayfasında sertifikasını yenilemek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin.

1. Seçili hesabın **Özellikler** bölmesinde **Sertifikayı yenile**'ye tıklayın.

    ![Farklı Çalıştır hesabının sertifikasını yenileme](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Sertifika yenilenirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

## <a name="limiting-run-as-account-permissions"></a>Farklı Çalıştır hesabı izinleri sınırlama

Kaynaklarda Azure Otomasyonu'nda Otomasyon hedefleyen kontrol etmek için farklı çalıştır hesabı varsayılan olarak abonelikte katılımcı hakları verilir. RunAs hizmet sorumlusu yapabileceklerini sınırlamak gerekiyorsa, hesabı aboneliğe katkıda bulunan rolünden kaldırmak ve belirtmek istediğiniz kaynak grupları için katkıda bulunan olarak ekleyin.

Azure portalında **abonelikleri** ve Otomasyon hesabınızın aboneliği seçin. Seçin **erişim denetimi (IAM)** seçip **rol atamaları** sekmesi. Automation hesabınız için hizmet sorumlusu arayın (gibi görünüyor \<AutomationAccountName\>_unique tanımlayıcı). Hesabı seçin ve tıklayın **Kaldır** abonelikten kaldırmak için.

![Abonelik Katkıda Bulunanlar](media/manage-runas-account/automation-account-remove-subscription.png)

Hizmet sorumlusu bir kaynak grubuna eklemek için Azure portal ve select kaynak grubunu seçin **erişim denetimi (IAM)** . Seçin **rol ataması Ekle**, bu açılır **rol ataması Ekle** sayfası. İçin **rol**seçin **katkıda bulunan**. İçinde **seçin** metin kutusuna farklı çalıştır hesabı için hizmet sorumlusu adını yazın ve listeden seçin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın. Kaynak grupları, Azure Otomasyonu Garklı Çalıştır hizmet sorumlusu erişimi vermek istediğiniz için bu adımları tamamlayın.

## <a name="misconfiguration"></a>Yanlış yapılandırma

İlk kurulum sırasında, Farklı Çalıştır veya Klasik Farklı Çalıştır hesabının düzgün çalışması için gerekli olan bazıları silinmiş veya düzgün oluşturulmamış olabilir. Bu öğeler şunlardır:

* Sertifika varlığı
* Bağlantı varlığı
* Farklı Çalıştır hesabının katkıda bulunan rolünden kaldırılması
* Azure AD'de hizmet sorumlusu veya uygulama

Yanlış yapılandırmanın önceki ve diğer örneklerinde, Otomasyon hesabı değişiklikleri algılar ve hesabın **Farklı Çalıştır Hesapları** özellikleri sayfasında *Tamamlanmadı* durumunu gösterir.

![Tamamlanmamış Farklı Çalıştır hesabı yapılandırma durumu](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Farklı Çalıştır hesabını seçtiğinizde hesabın **Özellikler** bölmesinde aşağıdaki hata iletisi görüntülenir:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Hesabı silip yeniden oluşturarak Farklı Çalıştır hesabıyla ilgili bu sorunları hızlı bir şekilde çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Hizmet sorumluları hakkında daha fazla bilgi için bkz. [uygulama nesneleri ve hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md).
* Sertifikalar ve Azure hizmetleri hakkında daha fazla bilgi için bkz. [Azure Cloud Services sertifikalarına genel bakış](../cloud-services/cloud-services-certs-create.md).