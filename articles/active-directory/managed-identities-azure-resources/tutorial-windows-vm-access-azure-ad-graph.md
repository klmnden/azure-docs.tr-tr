---
title: Azure AD Graph API hizmetine erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma
description: Windows VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure AD Graph API hizmetine erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60938f26c27b9f94046b1be8e3d0cb6b247017c9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307803"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Öğretici: Azure AD Graph API hizmetine erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

Bu öğretici, sistem tarafından atanan yönetilen bir kimlik bir Windows sanal makine (VM) için kendi grup üyeliklerini almak için Azure AD Graph API'sine erişmek için nasıl kullanılacağını gösterir. Azure kaynaklarına yönelik yönetilen kimlikler Azure tarafından otomatik olarak yönetilir ve kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır.  Bu öğreticide, VM kimliğinizin Azure AD gruplarındaki üyeliğini sorgulayacaksınız. Örnek verecek olursak, grup bilgileri genellikle yetkilendirme kararlarında kullanılır. Arka planda, sanal makinenizin yönetilen kimliği Azure AD’de bir **Hizmet Sorumlusu** ile temsil edilir. Grup sorgusu yapmadan önce, VM'nin kimliğini temsil eden hizmet sorumlusunu Azure AD'deki bir gruba ekleyin. Azure PowerShell, Azure AD PowerShell veya Azure CLI kullanarak bunu yapabilirsiniz.

> [!div class="checklist"]
> * Azure AD'ye Bağlanma
> * VM kimliğinizi Azure AD'de bir gruba ekleme 
> * VM kimliğine Azure AD Graph erişimi verme 
> * VM kimliğini kullanarak erişim belirteci alma ve Azure AD Graph çağrısı yapmak için kullanma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- Bir VM kimliğine Azure AD Graph erişimi vermek için hesabınıza Azure AD’de **Global Admin** rolünün atanmış olması gerekir.
- Son yükleme [Azure AD PowerShell](/powershell/azure/active-directory/install-adv2) henüz yapmadıysanız. 

## <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma

VM’yi bir gruba atamak ve VM’ye grup üyeliklerini alma izni vermek için Azure AD’ye bağlanmanız gerekir. Birden fazla Kiracı varsa doğrudan veya Tenantıd parametresi ile Connect-AzureAD cmdlet'ini kullanabilirsiniz.

```powershell
Connect-AzureAD
```
OR
```powershell
Connect-AzureAD -TenantId "Object Id of the tenant"
```

## <a name="add-your-vm-identity-to-a-group-in-azure-ad"></a>VM kimliğinizi Azure AD'de bir gruba ekleme

Windows VM’de sistem tarafından atanmış yönetilen kimliği etkinleştirdiğinizde, Azure AD’de bir hizmet sorumlusu oluşturulmuştur.  Şimdi VM’yi bir gruba eklemeniz gerekir.  Aşağıdaki örnek, Azure AD'de yeni bir grup oluşturur ve sanal makinenizin hizmet sorumlusunu bu gruba ekler:

```powershell
New-AzureADGroup -DisplayName "myGroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$AzureADGroup = Get-AzureADGroup -Filter "displayName eq 'myGroup'"
$ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
Add-AzureADGroupMember -ObjectId $AzureADGroup.ObjectID -RefObjectId $ManagedIdentitiesServicePrincipal.ObjectId
```
## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Sanal makinenize Azure AD Graph API erişimi verme

Azure kaynakları için yönetilen kimlikler kullanıldığında kodunuz Azure AD kimlik doğrulamasını destekleyen kaynaklarda kimlik doğrulaması yapmak için erişim belirteçleri alabilir. Microsoft Azure AD Graph API, Azure AD kimlik doğrulamasını destekler. Bu adımda, VM kimliğinizin hizmet sorumlusuna grup üyeliklerini sorgulayabilmesi için Azure AD Graph erişimi vereceksiniz. Hizmet sorumlularına Microsoft veya Azure AD Graph erişimi, **Uygulama İzinleri** aracılığıyla verilir. Vermeniz gereken uygulama izninin türü, MS veya Azure AD Graph’ta erişmek istediğiniz varlığa bağlıdır.

Bu öğreticide VM kimliğinize ```Directory.Read.All``` uygulama iznini kullanarak grup üyeliğini sorgulama olanağı vereceksiniz. Bu izni vermek için Azure AD'de Genel Yönetici rolü atanmış bir kullanıcı hesabı gerekir. Normalde Azure portalda uygulamanızın kaydını ziyaret edip izni oraya ekleyerek bir uygulama izni verebilirsiniz. Ancak, Azure kaynakları için yönetilen kimlikler uygulama nesnelerini Azure AD'ye kaydetmez, yalnızca hizmet sorumlularını kaydeder. Uygulama iznini kaydetmek için Azure AD PowerShell komut satırı aracını kullanırsınız. 

Azure AD Graph:
- Hizmet sorumlusu uygulama kimliği (uygulama izni verilirken kullanılır): 00000002-0000-0000-c000-000000000000
- Kaynak Kimliği (Azure kaynakları için yönetilen kimliklerden erişim belirteci istenirken kullanılır): https://graph.windows.net
- İzin kapsam başvurusu: [Azure AD Graph izinleri başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak uygulama izinleri verme

Bu seçeneği kullanmak için Azure AD PowerShell gerekir. Makinenizde yüklü değilse, devam etmeden önce [en son sürümü indirin](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

1. Bir PowerShell penceresi açın ve Azure AD'ye bağlanın:

   ```powershell
   Connect-AzureAD
   ```
   Belirli bir Azure Active Directory'ye bağlanın _Tenantıd_ parametresini aşağıdaki gibi:

   ```powershell
   Connect-AzureAD -TenantId "Object Id of the tenant"
   ```

   
2. Sanal makinenizin kimliğini temsil eden hizmet sorumlusuna ``Directory.Read.All`` uygulama iznini atamak için aşağıdaki PowerShell komutlarını çalıştırın.

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   New-AzureAdServiceAppRoleAssignment -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId -PrincipalId $ManagedIdentitiesServicePrincipal.ObjectId -ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole.Id
   ``` 

   Son komutun çıkışı, atamanın kimliğini döndürecek şekilde buna benzemelidir:
        
   `ObjectId`:`gzR5KyLAiUOTiqFhNeWZWBtK7ZKqNJxAiWYXYVHlgMs`

   `ResourceDisplayName`:`Windows Azure Active Directory`

   `PrincipalDisplayName`:`myVM` 

   `New-AzureAdServiceAppRoleAssignment` çağrısı `bad request, one or more properties are invalid` hatasıyla başarısız oluyorsa, uygulama izni zaten VM kimliğinin hizmet sorumlusuna atanmış olabilir. Aşağıdaki PowerShell komutlarını kullanarak, uygulama izninin zaten sanal makinenizin kimliği ile Azure AD Graph arasında olup olmadığını denetleyebilirsiniz:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   ```

   Aşağıdaki PowerShell komutlarını kullanarak Azure AD Graph’e verilen tüm uygulama izinlerini listeleyebilirsiniz:

   ```powershell
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId
   ``` 

   Aşağıdaki PowerShell komutlarını kullanarak Azure AD Graph için sanal makinenizin kimliğine verilen tüm uygulama izinlerini kaldırabilirsiniz:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}   
   $ServiceAppRoleAssignment = Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   Remove-AzureADServiceAppRoleAssignment -AppRoleAssignmentId $ServiceAppRoleAssignment.ObjectId -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId
   ```
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>VM kimliğini kullanarak erişim belirteci alma ve Azure AD Graph çağrısı yapma 

Azure AD Graph kimlik doğrulamasında VM sistem tarafından atanan yönetilen kimliğini kullanmak için VM’den istek göndermelisiniz.

1. Portalda, **Sanal Makineler**'e ve Windows VM’nize gidin, **Genel Bakış** dikey penceresinde **Bağlan**'a tıklayın.  
2. Windows VM'sini oluştururken kullandığınız **Kullanıcı adı** ve **Parola**’nızı girin.
3. Artık sanal makineyle Uzak Masaüstü Bağlantısı'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.  
4. PowerShell’in Invoke-WebRequest komutunu kullanarak, Azure kaynakları uç noktasına yönelik yerel yönetilen kimliklere Azure AD Graph için erişim belirteci alma isteğinde bulunun.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -Method GET -Headers @{Metadata="true"}
   ```

   Yanıtı JSON nesnesinden PowerShell nesnesine dönüştürün.

   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Yanıttan erişim belirtecini ayıklayın.

   ```powershell
   $AccessToken = $content.access_token
   ```

5. Sanal makine kimliğinizin hizmet sorumlusuna ait Nesne Kimliğini (bu değeri önceki adımlarda bildirilen değişkenden alabilirsiniz: ``$ManagedIdentitiesServicePrincipal.ObjectId``) kullanarak, grup üyeliklerini almak üzere Azure AD Graph API’yi sorgulayabilirsiniz. Değiştirin `<OBJECT ID>` önceki adımdan gelen nesne Kimliğine sahip ve <`ACCESS-TOKEN>` daha önce alınan erişim belirteci ile:

   ```powershell
   Invoke-WebRequest 'https://graph.windows.net/<Tenant ID>/servicePrincipals/<VM Object ID>/getMemberGroups?api-version=1.6' -Method POST -Body '{"securityEnabledOnly":"false"}' -Headers @{Authorization="Bearer $AccessToken"} -ContentType "application/json"
   ```
   
   Yanıt, daha önceki adımlarda sanal makinenizin hizmet sorumlusunu eklediğiniz grubun `Object ID` değerini içerir.

   Yanıt:

   ```powershell   
   Content : {"odata.metadata":"https://graph.windows.net/<Tenant ID>/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure AD Graph'a erişmek için Windows VM sistem tarafından atanan yönetilen kimlik kullanmayı öğrendiniz.  Azure AD Graph hakkında daha fazla bilgi için bkz.:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
