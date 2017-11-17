---
title: "PowerShell ile Azure uygulama kimliği oluşturma | Microsoft Docs"
description: "Bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak ve rol tabanlı erişim denetimi aracılığıyla kaynaklara erişim izni için Azure PowerShell kullanmayı açıklar. Uygulama ile bir parola veya sertifika kimlik doğrulaması yapmayı gösterir."
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
ms.date: 08/28/2017
ms.author: tomfitz
ms.openlocfilehash: 57eec4277e584c3c2828e0fe029b9db10428934e
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure PowerShell kullanma

Bir uygulama ya da kaynaklara erişmek için gereken komut dosyası varsa, uygulamanın kendi kimlik bilgileriyle kimlik doğrulamasını ve uygulama için bir kimlik ayarlayın. Bu kimlik, bir hizmet sorumlusu bilinir. Bu yaklaşım sağlar:

* Kendi izinlerinizi farklı uygulama kimliği için izinleri atayın. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.
* Sertifika kimlik doğrulaması için Katılımsız betik yürütülürken kullanın.

Bu konuda nasıl kullanılacağını gösterir [Azure PowerShell](/powershell/azure/overview) kendi kimlik bilgilerini ve kimlik altında çalıştırmak bir uygulama için gereksinim duyduğunuz her şeyi ayarlamak için.

## <a name="required-permissions"></a>Gerekli izinler
Bu konuda tamamlamak için Azure Active Directory ve Azure aboneliğinize yeterli izniniz olması gerekir. Özellikle, Azure Active Directory'de bir uygulama oluşturun ve hizmet sorumlusu rol atama mümkün olması gerekir. 

Hesabınızın yeterli izinlere sahip olup olmadığını denetlemenin en kolay yolu portalı kullanmaktır. Bkz: [gerekli izni denetleyin](resource-group-create-service-principal-portal.md#required-permissions).

Şimdi, ile kimlik doğrulaması için bir bölüm için devam edin:

* [Parola](#create-service-principal-with-password)
* [otomatik olarak imzalanan sertifika](#create-service-principal-with-self-signed-certificate)
* [Sertifika yetkilisinden sertifika](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>PowerShell komutları

Bir hizmet sorumlusu ayarlamak için kullanın:

| Komut | Açıklama |
| ------- | ----------- | 
| [AzureRmADServicePrincipal yeni](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Bir Azure Active Directory Hizmet sorumlusu oluşturur |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Belirtilen kapsamda belirtilen asıl belirtilen RBAC rolü atar. |


## <a name="create-service-principal-with-password"></a>Parola ile hizmet sorumlusu oluşturma

Aboneliğiniz için katılımcı rolü ile bir hizmet sorumlusu oluşturmak için kullanın: 

```powershell
Login-AzureRmAccount
$password = convertto-securestring {provide-password} -asplaintext -force
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password $password
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Örneğin yeni hizmet için bir süre Azure Active Directory yaymak için asıl izin vermek 20 saniye için uyku moduna geçer. Kodunuzu yetecek kadar uzun süre beklemez belirten bir hata görürsünüz: "PrincipalNotFound: asıl {ID} dizininde yok."

Aşağıdaki komut dosyası varsayılan abonelik dışında bir kapsam belirtmenize olanak sağlar ve bir hata oluşursa rol atamasını yeniden deneme sayısı:

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
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

Komut dosyası hakkında dikkat edilecek bazı öğeler için:

* Varsayılan abonelik kimliği erişimi vermek için ResourceGroup veya Subscriptionıd parametreleri sağlamak gerekmez.
* Yalnızca bir kaynak grubu için rol ataması kapsamını sınırlamak istediğinizde ResourceGroup parametresini belirtin.
*  Bu örnekte, hizmet sorumlusu katkıda bulunan rolü ekleyin. Diğer roller için bkz: [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md).
* Komut dosyasını yeni hizmet için bir süre Azure Active Directory yaymak için asıl izin vermek 15 saniye için uyku moduna geçer. Kodunuzu yetecek kadar uzun süre beklemez belirten bir hata görürsünüz: "PrincipalNotFound: asıl {ID} dizininde yok."
* Daha fazla abonelik veya kaynak grupları için hizmet asıl erişimi vermeniz gerekiyorsa, çalıştırmak `New-AzureRMRoleAssignment` farklı kapsamlar yeniden cmdlet'iyle.


### <a name="provide-credentials-through-powershell"></a>PowerShell ile kimlik bilgileri sağlayın
Şimdi, işlemleri gerçekleştirmek için uygulama olarak oturum açmak gerekir. Kullanıcı adı için `ApplicationId` uygulama için oluşturulan. Parola için hesabı oluşturulurken belirtilen bir kullanın. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-ID}
```

Kiracı kimliği hassas, olmadığından doğrudan komut dosyanıza ekleme. Kiracı Kimliği almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Hizmet sorumlusu ile otomatik olarak imzalanan sertifika oluşturma

Kendinden imzalı bir sertifika ve aboneliğiniz için katılımcı rolü ile bir hizmet sorumlusu oluşturmak için kullanın: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Örneğin yeni hizmet için bir süre Azure Active Directory yaymak için asıl izin vermek 20 saniye için uyku moduna geçer. Kodunuzu yetecek kadar uzun süre beklemez belirten bir hata görürsünüz: "PrincipalNotFound: asıl {ID} dizininde yok."

Aşağıdaki komut dosyasında varsayılan abonelik dışında bir kapsam belirtmenize olanak sağlar ve bir hata oluşursa rol atamasını yeniden dener. Windows 10 veya Windows Server 2016 Azure PowerShell 2.0 yüklü olmalıdır.

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

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
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

Komut dosyası hakkında dikkat edilecek bazı öğeler için:

* Varsayılan abonelik kimliği erişimi vermek için ResourceGroup veya Subscriptionıd parametreleri sağlamak gerekmez.
* Yalnızca bir kaynak grubu için rol ataması kapsamını sınırlamak istediğinizde ResourceGroup parametresini belirtin.
* Bu örnekte, hizmet sorumlusu katkıda bulunan rolü ekleyin. Diğer roller için bkz: [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md).
* Komut dosyasını yeni hizmet için bir süre Azure Active Directory yaymak için asıl izin vermek 15 saniye için uyku moduna geçer. Kodunuzu yetecek kadar uzun süre beklemez belirten bir hata görürsünüz: "PrincipalNotFound: asıl {ID} dizininde yok."
* Daha fazla abonelik veya kaynak grupları için hizmet asıl erişimi vermeniz gerekiyorsa, çalıştırmak `New-AzureRMRoleAssignment` farklı kapsamlar yeniden cmdlet'iyle.

Varsa, **Windows 10 veya Windows Server 2016 Technical Preview gerekmez**, karşıdan yüklemek gereken [otomatik olarak imzalanan sertifika Oluşturucu](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center gelen. İçeriğini ayıklayın ve ihtiyacınız cmdlet'i içeri aktarın.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Komut dosyasında, sertifikayı oluşturmak için aşağıdaki iki satırı değiştirin.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Otomatik PowerShell komut dosyası aracılığıyla sertifikası sağlayın
Bir hizmet sorumlusu oturum olduğunda, Kiracı kimliği dizininin AD uygulamanız için sağlamanız gerekir. Bir kiracı, Azure Active Directory örneğidir. Yalnızca bir aboneliğiniz varsa, kullanabilirsiniz:

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
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

Doğrudan komut dosyanıza katıştırmak için uygulama kimliği ve Kiracı kimliği harfe duyarlı değildir. Kiracı Kimliği almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Uygulama Kimliğini almak gereken durumlarda kullanın:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Sertifika yetkilisinden sertifika ile hizmet sorumlusu oluşturma
Hizmet sorumlusu oluşturmak için bir sertifika yetkilisi tarafından verilen bir sertifika kullanmak için aşağıdaki komutu kullanın:

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

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId
 
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

Komut dosyası hakkında dikkat edilecek bazı öğeler için:

* Erişim aboneliği kapsamlıdır.
* Bu örnekte, hizmet sorumlusu katkıda bulunan rolü ekleyin. Diğer roller için bkz: [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md).
* Komut dosyasını yeni hizmet için bir süre Azure Active Directory yaymak için asıl izin vermek 15 saniye için uyku moduna geçer. Kodunuzu yetecek kadar uzun süre beklemez belirten bir hata görürsünüz: "PrincipalNotFound: asıl {ID} dizininde yok."
* Daha fazla abonelik veya kaynak grupları için hizmet asıl erişimi vermeniz gerekiyorsa, çalıştırmak `New-AzureRMRoleAssignment` farklı kapsamlar yeniden cmdlet'iyle.

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
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

Doğrudan komut dosyanıza katıştırmak için uygulama kimliği ve Kiracı kimliği harfe duyarlı değildir. Kiracı Kimliği almak gereken durumlarda kullanın:

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

Bir parola eklemek için kullanın:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

Bir sertifika değer eklemek için bu konudaki gösterildiği gibi otomatik olarak imzalanan bir sertifika oluşturun. Ardından, kullanın:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a>Oturum açma basitleştirmek için erişim belirteci Kaydet
Oturum açmak gereken her zaman hizmet asıl kimlik bilgilerini sağlayan önlemek için erişim belirteci kaydedebilirsiniz.

Bir sonraki oturumda geçerli erişim belirtecini kullanmak için profil kaydedin.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Profil açın ve içeriğini inceleyin. Bir erişim belirteci içerdiğine dikkat edin. El ile yeniden oturum açmayı yerine, profili yükleyin.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> Belirtecin geçerli olduğu sürece kaydedilmiş bir profil kullanarak yalnızca çalıştığı şekilde erişim belirtecinin süresi.
>  

Alternatif olarak, oturum açmak için PowerShell REST işlemlerini çağırabilirsiniz. Kimlik doğrulaması yanıtından, diğer işlemleri ile kullanmak için erişim belirtecini alabilirsiniz. REST işlemlerini çağırarak erişim belirtecini alma bir örnek için bkz: [bir erişim belirteci oluşturma](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Hata ayıklama

Bir hizmet sorumlusu oluşturma sırasında şu hatalarla karşılaşabilirsiniz:

* **"Authentication_Unauthorized"** veya **"abonelik bağlamda bulunamadı."** Hesabınızı olmadığı zaman-bu hatayı görmek [gerekli izinleri](#required-permissions) uygulama kaydetmek için Azure Active Directory üzerinde. Yalnızca yönetici kullanıcıların Azure Active Directory'de uygulamaları kaydedebilirsiniz ve hesabınızın bir yönetici değil, bu hata genellikle, bakın Ya da bir yönetici rolü atayın veya kullanıcıların uygulamaları kaydetmek yöneticinize başvurun.

* Hesabınızı **"kapsamı '/ subscriptions / {GUID}' üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi yok."**  -Hesabınız için bir kimlik rol atamak için yeterli izinlere sahip olmadığında bu hataya bakın. Kullanıcı erişimi Yöneticisi rolüne eklemek için abonelik yöneticinize başvurun.

## <a name="sample-applications"></a>Örnek uygulamalar
Farklı platformlarda üzerinden uygulama olarak oturum açma hakkında daha fazla bilgi için bkz:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Sonraki adımlar
* Kaynakları yönetmek için Azure'da bir uygulamayı tümleştirme ayrıntılı adımlar için bkz: [Geliştirici Kılavuzu'na yetkilendirme Azure Kaynak Yöneticisi API'si ile](resource-manager-api-authentication.md).
* Uygulamalar ve hizmet asıl adı daha ayrıntılı bir açıklaması için bkz: [uygulama ve hizmet sorumlusu nesneleri](../active-directory/active-directory-application-objects.md). 
* Azure Active Directory kimlik doğrulaması hakkında daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](../active-directory/active-directory-authentication-scenarios.md).
* Verilen veya kullanıcılar için reddedilen kullanılabilir eylemler listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../active-directory/role-based-access-control-resource-provider-operations.md).

