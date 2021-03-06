---
title: PowerShell ile Azure uygulaması için kimlik oluşturma | Microsoft Docs
description: Azure PowerShell kullanarak Azure Active Directory uygulamasıyla hizmet sorumlusu oluşturma ve rol tabanlı erişim denetimi aracılığıyla bu uygulamaya kaynaklar için erişim verme işlemleri açıklanır. Bir sertifikayla uygulamanın kimliğinin nasıl doğrulandığı gösterilir.
services: active-directory
documentationcenter: na
author: rwike77
manager: CelesteDG
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/24/2018
ms.author: ryanwi
ms.reviewer: tomfitz
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6d6de5186b1906d56b5a43317d9c36ad1cc6aad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540408"
---
# <a name="how-to-use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Nasıl yapılır: Azure PowerShell kullanarak sertifikayla bir hizmet sorumlusu oluşturma

Kaynaklara erişmesi gereken bir uygulamanız veya betiğiniz olduğunda, uygulama için bir kimlik ayarlayabilir ve uygulamanın kimliğini kendi kimlik bilgileriyle doğrulayabilirsiniz. Bu kimlik, hizmet sorumlusu olarak bilinir. Bu yaklaşım şunları yapmanızı sağlar:

* Uygulama kimliğine kendi izinlerinizden farklı izinler atayabilirsiniz. Normalde, bu izinler tam olarak uygulamaya gereken izinlerle sınırlı olur.
* Katılımsız bir betik yürütürken kimlik doğrulaması için sertifika kullanabilirsiniz.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure kaynakları için yönetilen kimliklerle göz önünde bulundurun. Kodunuzu Yönetilen kimlikleri ve Azure Active Directory (Azure AD) kimlik doğrulamasını destekleyen erişimleri kaynak destekleyen bir hizmeti çalıştıran yönetilen kimlikleri sizin için daha iyi bir seçenek vardır. Hangi şu anda, Destek Hizmetleri dahil olmak üzere, Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](../managed-identities-azure-resources/overview.md).

Bu makalede, sertifikayla kimlik doğrulaması yapan bir hizmet sorumlusunun nasıl oluşturulduğu gösterilir. Parolası olan bir hizmet sorumlusu ayarlamak için bkz. [Azure PowerShell ile Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps).

Bu makale için PowerShell'in [en son sürümünü](/powershell/azure/install-az-ps) kullanıyor olmalısınız.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="required-permissions"></a>Gerekli izinler

Bu makaleyi tamamlamak için yeterli izinleri hem Azure AD'NİZDE olmalıdır ve Azure aboneliği. Özellikle, Azure AD'de bir uygulama oluşturun ve hizmet sorumlusu, rol atama mümkün olması gerekir.

Hesabınızın yeterli izinlere sahip olup olmadığını denetlemenin en kolay yolu portalı kullanmaktır. Bkz. [Gerekli izinleri denetleme](howto-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Otomatik olarak imzalanan bir sertifikayla hizmet sorumlusu oluşturma

Aşağıdaki örnekte basit bir senaryo ele alınmıştır. Kullandığı [yeni AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal) otomatik olarak imzalanan bir sertifika ile kullanan bir hizmet sorumlusu oluşturmak için [New-AzureRmRoleAssignment](/powershell/module/az.resources/new-azroleassignment) atamak [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) rolü için hizmet sorumlusu. Rol atamasının kapsamı şu anda seçili olan Azure aboneliğinizdir. Farklı bir aboneliği seçmek için kullanın [kümesi AzContext](/powershell/module/Az.Accounts/Set-AzContext).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Örnek süre yeni hizmet sorumlusu Azure AD yaymak için izin vermek 20 saniye için uyku moduna geçer. Betiğinizi yeterince beklemez, belirten bir hata görürsünüz: "{ID} sorumlusu {DIR-kimliği} dizininde yok." Bu hatayı gidermek için ardından kısa bir süre bekleyin. **yeni AzRoleAssignment** yeniden komutu.

**ResourceGroupName** parametresini kullanıp rol atamasının kapsamı olarak belirli bir kaynak grubunu belirtebilirsiniz. Ayrıca kapsam olarak belirli bir kaynağı belirtmek için **ResourceType** ve **ResourceName** parametrelerini kullanabilirsiniz. 

**Windows 10'unuz veya Windows Server 2016'nız yoksa**, Microsoft Betik Merkezi'nden [Otomatik olarak imzalanan sertifika oluşturucusunu](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) indirmeniz gerekir. İçindekileri ayıklayın ve ihtiyacınız olan cmdlet'i içeri aktarın.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

Sertifikayı oluşturmak için betikte aşağıdaki iki satırı değiştirin.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell betiği aracılığıyla sertifika sağlama

Hizmet sorumlusu olarak her oturum açtığınızda, AD uygulamanız için dizinin kiracı kimliğini sağlamanız gerekir. Bir kiracı, Azure AD örneğidir.

```powershell
$TenantId = (Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -eq "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Sertifika Yetkilisinin sertifikasıyla hizmet sorumlusu oluşturma

Aşağıdaki örnekte, hizmet sorumlusu oluşturmak için bir Sertifika Yetkilisinin verdiği sertifika kullanılır. Atamanın kapsamı belirtilen Azure aboneliğidir. Hizmet sorumlusuna [Contributor](../../role-based-access-control/built-in-roles.md#contributor) rolünü atar. Rol ataması sırasında hata oluştursa, atamayı yeniden dener.

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

 Connect-AzAccount
 Import-Module Az.Resources
 Set-AzContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell betiği aracılığıyla sertifika sağlama
Hizmet sorumlusu olarak her oturum açtığınızda, AD uygulamanız için dizinin kiracı kimliğini sağlamanız gerekir. Bir kiracı, Azure AD örneğidir.

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

 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

Uygulama kimliği ve kiracı kimliği büyük/küçük harfe duyarlı değildir, dolayısıyla bunları doğrudan betiğinize ekleyebilirsiniz. Kiracı kimliğini almanız gerekiyorsa şunu kullanın:

```powershell
(Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
```

Uygulama kimliğini almanız gerekiyorsa şunu kullanın:

```powershell
(Get-AzADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Kimlik bilgilerini değiştirme

Güvenliğin tehlikeye girmesi veya bir kimlik bilgisi süre sonu nedeniyle, bir AD uygulaması için kimlik bilgilerini değiştirmek için [Remove-AzADAppCredential](/powershell/module/az.resources/remove-azadappcredential) ve [yeni AzADAppCredential](/powershell/module/az.resources/new-azadappcredential) cmdlet'leri.

Uygulamanın tüm kimlik bilgilerini kaldırmak için şunu kullanın:

```powershell
Get-AzADApplication -DisplayName exampleapp | Remove-AzADAppCredential
```

Sertifika değeri eklemek için, bu makalede gösterildiği gibi otomatik olarak imzalanan bir sertifika oluşturun. Ardından şunu kullanın:

```powershell
Get-AzADApplication -DisplayName exampleapp | New-AzADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Hata ayıklama

Hizmet sorumlusu oluştururken şu hataları alabilirsiniz:

* **"Authentication_Unauthorized"** veya **"Bağlamda hiç abonelik bulunamadı."** Hesabınız yoksa,-bu hatayı görmeye [gerekli izinler](#required-permissions) Azure AD'de bir uygulamayı kaydedin. Yalnızca Azure Active Directory'de yönetici kullanıcı uygulama kaydedebilir ve hesabınızda bir yönetici değil. Bu hata genellikle bakın Yöneticinizden size yönetici rolü atamasını veya kullanıcıların uygulama kaydetmesini etkinleştirmesini isteyin.

* Hesabınızı **"'/ subscriptions / {guid}' kapsamı üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi yok."**  -Hesabınız için bir kimlik bir rol atamak için yeterli izinlere sahip olmadığında bu hatayı görürsünüz. Abonelik yöneticinizden sizi Kullanıcı Erişimi Yöneticisi rolüne atamasını isteyin.

## <a name="next-steps"></a>Sonraki adımlar

* Parolası olan bir hizmet sorumlusu ayarlamak için bkz. [Azure PowerShell ile Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps).
* Kaynakları yönetmek üzere bir uygulamayı Azure'la tümleştirme işleminin ayrıntılı adımları için bkz. [Azure Resource Manager API'siyle yetkilendirme için geliştirici kılavuzu](../../azure-resource-manager/resource-manager-api-authentication.md).
* Uygulamaların ve hizmet sorumlularının daha ayrıntılı açıklaması için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](app-objects-and-service-principals.md).
* Azure AD kimlik doğrulaması hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](authentication-scenarios.md).
