---
title: PowerShell ile Azure uygulama kimliği oluşturma | Microsoft Docs
description: Bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak ve rol tabanlı erişim denetimi aracılığıyla kaynaklara erişim izni için Azure PowerShell kullanmayı açıklar. Uygulama bir sertifika ile kimlik doğrulaması yapmayı gösterir.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/12/2018
ms.author: tomfitz
ms.openlocfilehash: 70255ead4a556204689e9918b9c89e396f8122c0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Bir sertifika ile bir hizmet sorumlusu oluşturmak için Azure PowerShell'i kullanma

Bir uygulama ya da kaynaklara erişmek için gereken komut dosyası varsa, uygulamanın kendi kimlik bilgileriyle kimlik doğrulamasını ve uygulama için bir kimlik ayarlayın. Bu kimlik, bir hizmet sorumlusu bilinir. Bu yaklaşım sağlar:

* Kendi izinlerinizi farklı uygulama kimliği için izinleri atayın. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.
* Sertifika kimlik doğrulaması için Katılımsız betik yürütülürken kullanın.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure AD yönetilen hizmet kimliği kullanmayı düşünün. Azure AD MSI kod için bir kimlik oluşturma basitleştirir Azure Active Directory genel Önizleme özelliğidir. Kodunuzu Azure AD MSI destekleyen ve Azure Active Directory kimlik doğrulamasını destekleyen kaynaklarına erişen bir hizmette çalıştırıyorsa, Azure AD MSI sizin için daha iyi bir seçenektir. Hangi Hizmetleri şu anda, destek dahil olmak üzere Azure AD MSI hakkında daha fazla bilgi için bkz: [Azure kaynakları için Yönetilen hizmet kimliği](../active-directory/managed-service-identity/overview.md).

Bu makalede bir sertifikayla kimliğini doğrulayan bir hizmet sorumlusu oluşturulacağını gösterir. Bir hizmet sorumlusu parolayla ayarlamak için bkz: [Azure Azure PowerShell ile hizmet sorumlusu oluşturmak](/powershell/azure/create-azure-service-principal-azureps).

## <a name="required-permissions"></a>Gerekli izinler

Bu makalede tamamlamak için Azure Active Directory ve Azure abonelik yeterli izniniz olması gerekir. Özellikle, Azure Active Directory'de bir uygulama oluşturun ve hizmet sorumlusu rol atama mümkün olması gerekir.

Hesabınızın yeterli izinlere sahip olup olmadığını denetlemenin en kolay yolu portalı kullanmaktır. Bkz: [gerekli izni denetleyin](resource-group-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Hizmet sorumlusu ile otomatik olarak imzalanan sertifika oluşturma

Aşağıdaki örnekte basit bir senaryoyu ele alınmaktadır. Kullandığı [yeni AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) otomatik olarak imzalanan bir sertifika ve kullandığı bir hizmet sorumlusu oluşturmak için [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) atamak için [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor)hizmet sorumlusu rolüne. Rol ataması şu anda seçili Azure aboneliğinize kapsamlıdır. Farklı bir abonelik seçmek için kullanın [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Örneğin yeni hizmet için bir süre Azure Active Directory yaymak için asıl izin vermek 20 saniye için uyku moduna geçer. Kodunuzu yeterince uzun değil beklerseniz, belirten bir hata görürsünüz: "{ID} asıl yok dizin {DIR-ID}." Bu hatayı gidermek için ardından çalıştırın bekleyin **New-AzureRmRoleAssignment** yeniden komutu.

Sonraki örnek daha karmaşık olduğundan, geçerli Azure aboneliğinden farklı rol ataması kapsam ayarlamanıza olanak tanır. Yalnızca bir kaynak grubu için rol ataması kapsamını sınırlamak istediğinizde ResourceGroup parametresini belirtin. Rol atama sırasında bir hata meydana gelirse, atama yeniden dener. Windows 10 veya Windows Server 2016 Azure PowerShell 2.0 yüklü olmalıdır.

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Connect-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -Subscription $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Varsa, **Windows 10 veya Windows Server 2016 gerekmez**, karşıdan yüklemek gereken [otomatik olarak imzalanan sertifika Oluşturucu](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center gelen. İçeriğini ayıklayın ve ihtiyacınız cmdlet'i içeri aktarın.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

Komut dosyasında, sertifikayı oluşturmak için aşağıdaki iki satırı değiştirin.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -StoreName My `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell komut dosyası aracılığıyla sertifikası sağlayın

Bir hizmet sorumlusu oturum olduğunda, Kiracı kimliği dizininin AD uygulamanız için sağlamanız gerekir. Bir kiracı, Azure Active Directory örneğidir.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

Doğrudan komut dosyanıza katıştırmak için uygulama kimliği ve Kiracı kimliği hassas, değil. Kiracı Kimliği almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Uygulama Kimliğini almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Sertifika yetkilisinden sertifika ile hizmet sorumlusu oluşturma

Aşağıdaki örnek, hizmet sorumlusu oluşturmak için bir sertifika yetkilisi tarafından verilen bir sertifika kullanıyor. Atama belirtilen Azure aboneliği kapsamlıdır. Hizmet sorumlusu ekler [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rol. Rol atama sırasında bir hata meydana gelirse, atama yeniden dener.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell komut dosyası aracılığıyla sertifikası sağlayın
Bir hizmet sorumlusu oturum olduğunda, Kiracı kimliği dizininin AD uygulamanız için sağlamanız gerekir. Bir kiracı, Azure Active Directory örneğidir.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

Doğrudan komut dosyanıza katıştırmak için uygulama kimliği ve Kiracı kimliği hassas, değil. Kiracı Kimliği almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Uygulama Kimliğini almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Kimlik bilgilerini değiştirme

Ya da güvenliğinin aşılması veya bir kimlik bilgisi sona erme nedeniyle, bir AD uygulaması için kimlik bilgilerini değiştirmek için kullanın [Kaldır AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) ve [yeni AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlet'leri.

Bir uygulama için tüm kimlik bilgilerini kaldırmak için kullanın:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

Bir sertifika değer eklemek için bu makaledeki gösterildiği gibi otomatik olarak imzalanan bir sertifika oluşturun. Ardından, kullanın:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Hata ayıklama

Bir hizmet sorumlusu oluştururken aşağıdaki hataları alabilirsiniz:

* **"Authentication_Unauthorized"** veya **"abonelik bağlamda bulunamadı."** Hesabınızı olmadığı zaman-bu hatayı görmek [gerekli izinleri](#required-permissions) uygulama kaydetmek için Azure Active Directory üzerinde. Yalnızca yönetici kullanıcıların Azure Active Directory'de uygulamaları kaydedebilirsiniz ve hesabınızın bir yönetici değil, bu hata genellikle, bakın Ya da bir yönetici rolü atayın veya kullanıcıların uygulamaları kaydetmek yöneticinize başvurun.

* Hesabınızı **"kapsamı '/ subscriptions / {GUID}' üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi yok."**  -Hesabınız için bir kimlik rol atamak için yeterli izinlere sahip olmadığında bu hataya bakın. Kullanıcı erişimi Yöneticisi rolüne eklemek için abonelik yöneticinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar
* Bir hizmet sorumlusu parolayla ayarlamak için bkz: [Azure Azure PowerShell ile hizmet sorumlusu oluşturmak](/powershell/azure/create-azure-service-principal-azureps).
* Kaynakları yönetmek için Azure'da bir uygulamayı tümleştirme ayrıntılı adımlar için bkz: [Geliştirici Kılavuzu'na yetkilendirme Azure Kaynak Yöneticisi API'si ile](resource-manager-api-authentication.md).
* Uygulamalar ve hizmet asıl adı daha ayrıntılı bir açıklaması için bkz: [uygulama ve hizmet sorumlusu nesneleri](../active-directory/active-directory-application-objects.md). 
* Azure Active Directory kimlik doğrulaması hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](../active-directory/active-directory-authentication-scenarios.md).
