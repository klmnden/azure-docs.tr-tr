---
title: PowerShell ile Azure uygulaması için kimlik oluşturma | Microsoft Docs
description: Azure PowerShell kullanarak Azure Active Directory uygulamasıyla hizmet sorumlusu oluşturma ve rol tabanlı erişim denetimi aracılığıyla bu uygulamaya kaynaklar için erişim verme işlemleri açıklanır. Bir sertifikayla uygulamanın kimliğinin nasıl doğrulandığı gösterilir.
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
ms.date: 05/10/2018
ms.author: tomfitz
ms.openlocfilehash: 6ad1fd0ab51a93dbab5651ee80f6b6792dd331b9
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Azure PowerShell kullanarak sertifikayla bir hizmet sorumlusu oluşturma

Kaynaklara erişmesi gereken bir uygulamanız veya betiğiniz olduğunda, uygulama için bir kimlik ayarlayabilir ve uygulamanın kimliğini kendi kimlik bilgileriyle doğrulayabilirsiniz. Bu kimlik, hizmet sorumlusu olarak bilinir. Bu yaklaşım şunları yapmanızı sağlar:

* Uygulama kimliğine kendi izinlerinizden farklı izinler atayabilirsiniz. Normalde, bu izinler tam olarak uygulamaya gereken izinlerle sınırlı olur.
* Katılımsız bir betik yürütürken kimlik doğrulaması için sertifika kullanabilirsiniz.

> [!IMPORTANT]
> Hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure AD Yönetilen Hizmet Kimliği kullanmayı göz önünde bulundurun. Azure AD MSI, Azure Active Directory'nin kod için kimlik oluşturmayı basitleştiren ve genel önizleme aşamasında olan bir özelliğidir. Kodunuz Azure AD MSI desteği olan bir hizmette çalıştırılıyorsa ve Azure Active Directory kimlik doğrulamasını destekleyen kaynaklara erişiyorsa, Azure AD MSI sizin için daha iyi bir seçenektir. Azure AD MSI hakkında daha fazla bilgi edinmek ve şu anda bunu hangi hizmetlerin desteklediğini öğrenmek için bkz. [Azure kaynakları için Yönetilen Hizmet Kimliği](../active-directory/managed-service-identity/overview.md).

Bu makalede, sertifikayla kimlik doğrulaması yapan bir hizmet sorumlusunun nasıl oluşturulduğu gösterilir. Parolası olan bir hizmet sorumlusu ayarlamak için bkz. [Azure PowerShell ile Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps).

Bu makale için PowerShell'in [en son sürümünü](/powershell/azure/get-started-azureps) kullanıyor olmalısınız.

## <a name="required-permissions"></a>Gerekli izinler

Bu makaleyi tamamlamak için, hem Azure Active Directory’nizde hem de Azure aboneliğinizde yeterli izinlere sahip olmanız gerekir. Özellikle Azure Active Directory’de bir uygulama oluşturabilmeli ve hizmet sorumlusunu bir role atayabilmelisiniz.

Hesabınızın yeterli izinlere sahip olup olmadığını denetlemenin en kolay yolu portalı kullanmaktır. Bkz. [Gerekli izinleri denetleme](resource-group-create-service-principal-portal.md#required-permissions).

## <a name="create-service-principal-with-self-signed-certificate"></a>Otomatik olarak imzalanan bir sertifikayla hizmet sorumlusu oluşturma

Aşağıdaki örnekte basit bir senaryo ele alınmıştır. [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) kullanarak otomatik olarak imzalanan bir sertifikayla hizmet sorumlusu oluşturur ve [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) kullanarak hizmet sorumlusuna [Contributor](../role-based-access-control/built-in-roles.md#contributor) (Katkıda Bulunan) rolünü atar. Rol atamasının kapsamı şu anda seçili olan Azure aboneliğinizdir. Farklı bir abonelik seçmek için [Set-AzureRmContext](/powershell/module/azurerm.profile/set-azurermcontext) komutunu kullanın.

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

Örnek, yeni hizmet sorumlusunun tüm Azure Active Directory'ye yayılmasına zaman vermek için 20 saniyeliğine uyku moduna geçer. Betiğiniz yeteri kadar uzun beklemiyorsa, "{DIR-ID} dizininde {ID} sorumlusu yok" hatasını görürsünüz. Bu hatayı gidermek için biraz bekleyin ve sonra **New-AzureRmRoleAssignment** komutunu yeniden çalıştırın.

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

Hizmet sorumlusu olarak her oturum açtığınızda, AD uygulamanız için dizinin kiracı kimliğini sağlamanız gerekir. Kiracı, bir Azure Active Directory örneğidir.

```powershell
$TenantId = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzureRmADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzureRmAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Sertifika Yetkilisinin sertifikasıyla hizmet sorumlusu oluşturma

Aşağıdaki örnekte, hizmet sorumlusu oluşturmak için bir Sertifika Yetkilisinin verdiği sertifika kullanılır. Atamanın kapsamı belirtilen Azure aboneliğidir. Hizmet sorumlusuna [Contributor](../role-based-access-control/built-in-roles.md#contributor) rolünü atar. Rol ataması sırasında hata oluştursa, atamayı yeniden dener.

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

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell betiği aracılığıyla sertifika sağlama
Hizmet sorumlusu olarak her oturum açtığınızda, AD uygulamanız için dizinin kiracı kimliğini sağlamanız gerekir. Kiracı, bir Azure Active Directory örneğidir.

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

Uygulama kimliği ve kiracı kimliği büyük/küçük harfe duyarlı değildir, dolayısıyla bunları doğrudan betiğinize ekleyebilirsiniz. Kiracı kimliğini almanız gerekiyorsa şunu kullanın:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Uygulama kimliğini almanız gerekiyorsa şunu kullanın:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Kimlik bilgilerini değiştirme

Güvenliğin bozulması veya kimlik bilgilerinin süresinin dolması nedeniyle AD uygulamasının kimlik bilgilerini değiştirmek için, [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) ve [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlet'lerini kullanın.

Uygulamanın tüm kimlik bilgilerini kaldırmak için şunu kullanın:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | Remove-AzureRmADAppCredential
```

Sertifika değeri eklemek için, bu makalede gösterildiği gibi otomatik olarak imzalanan bir sertifika oluşturun. Ardından şunu kullanın:

```powershell
Get-AzureRmADApplication -DisplayName exampleapp | New-AzureRmADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Hata ayıklama

Hizmet sorumlusu oluştururken şu hataları alabilirsiniz:

* **"Authentication_Unauthorized"** veya **"Bağlamda hiç abonelik bulunamadı."** - Hesabınızın uygulamayı kaydetmek için Azure Active Directory üzerinde [gerekli izinleri](#required-permissions) olmadığında bu hatayı görürsünüz. Normalde, Azure Active Directory'nizde yalnızca yönetici kullanıcılar uygulama kaydedebildiği durumlarda ve sizin hesabınız bir yönetici hesabı olmadığında bu hata gösterilir. Yöneticinizden size yönetici rolü atamasını veya kullanıcıların uygulama kaydetmesini etkinleştirmesini isteyin.

* Hesabınızın **"'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}' eylemini gerçekleştirme yetkisi yok."** - Hesabınızın bir kimliğe rol atamak için yeterli izinleri olmadığında bu hatayı görürsünüz. Abonelik yöneticinizden sizi Kullanıcı Erişimi Yöneticisi rolüne atamasını isteyin.

## <a name="next-steps"></a>Sonraki adımlar
* Parolası olan bir hizmet sorumlusu ayarlamak için bkz. [Azure PowerShell ile Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps).
* Kaynakları yönetmek üzere bir uygulamayı Azure'la tümleştirme işleminin ayrıntılı adımları için bkz. [Azure Resource Manager API'siyle yetkilendirme için geliştirici kılavuzu](resource-manager-api-authentication.md).
* Uygulamaların ve hizmet sorumlularının daha ayrıntılı açıklaması için bkz. [Uygulama Nesneleri ve Hizmet Sorumlusu Nesneleri](../active-directory/active-directory-application-objects.md). 
* Azure Active Directory kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure AD için Kimlik Doğrulama Senaryoları](../active-directory/active-directory-authentication-scenarios.md).
