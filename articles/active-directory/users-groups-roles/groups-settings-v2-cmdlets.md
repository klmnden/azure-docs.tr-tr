---
title: Grupları - Azure Active Directory yönetmek için PowerShell örnekleri | Microsoft Docs
description: Bu sayfa, Azure Active Directory'de grupları yönetmenize yardımcı olmak için PowerShell örnekleri sağlar.
keywords: Azure AD, Azure Active Directory PowerShell, grupları, Grup Yönetimi
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb48d37e1cf552f9ad375906d8cd05301ac2dd0c
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407856"
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Grup Yönetimi için Azure Active Directory sürüm 2 cmdlet'leri

> [!div class="op_single_selector"]
> * [Azure portal](../fundamentals/active-directory-groups-create-azure-portal.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
> * [PowerShell](groups-settings-v2-cmdlets.md)
>
>

Bu makale, Azure Active Directory (Azure AD) grupları yönetmek için PowerShell kullanma örnekleri içerir.  Bu ayrıca, Azure AD PowerShell modülü ile ayarlamak anlatır. İlk olarak, şunları yapmalısınız [Azure AD PowerShell modülünü indirme](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modülünü yükleme
Azure AD PowerShell modülünü yüklemek için aşağıdaki komutları kullanın:

    PS C:\Windows\system32> install-module azuread
    PS C:\Windows\system32> import-module azuread

Modül kullanıma hazır olduğunu doğrulamak için aşağıdaki komutu kullanın:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Artık modülünde cmdlet'leri kullanmaya başlayabilirsiniz. Azure AD modülündeki cmdlet'ler tam bir açıklaması için lütfen çevrimiçi başvuru belgelerine bakın [Azure Active Directory PowerShell sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connect-to-the-directory"></a>Dizinine bağlanma
Azure AD PowerShell cmdlet'lerini kullanarak grupları yönetme başlamadan önce PowerShell oturumunuzda, yönetmek istediğiniz dizine bağlanmanız gerekir. Aşağıdaki komutu kullanın:

    PS C:\Windows\system32> Connect-AzureAD

Cmdlet, dizine erişmek için kullanmak istediğiniz kimlik bilgilerini ister. Bu örnekte, kullanıyoruz karen@drumkit.onmicrosoft.com tanıtım dizinine erişilemedi. Cmdlet, oturum dizininize başarıyla bağlandığını göstermek için bir onay döndürür:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Şimdi AzureAD cmdlet dizininizdeki grupları yönetmek için kullanmaya başlayabilirsiniz.

## <a name="retrieve-groups"></a>Grupları Al
Var olan grupları dizininizden almak için Get-AzureADGroups cmdlet'ini kullanın. 

Dizin içindeki tüm gruplar almak için parametresiz bir cmdlet'i kullanın:

    PS C:\Windows\system32> get-azureadgroup

Cmdlet tüm grupları bağlı dizinde döndürür.

-Nesne kimliği parametresi, belirli bir grubu grubun objectID belirttiğiniz almak için kullanabilirsiniz:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Cmdlet artık, objectID girdiğiniz parametresinin değerini eşleşen Grup döndürür:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Belirli bir grup kullanmak için arama yapabilirsiniz filtre parametresi. Bu parametre, bir ODATA filtre yan tümcesi alır ve aşağıdaki örnekte olduğu gibi filtreyle eşleşen tüm grupları döndürür:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> Azure AD PowerShell cmdlet'leri, OData sorgu standart uygulayın. Daha fazla bilgi için **$filter** içinde [OData uç noktasını kullanarak OData sorgu seçeneklerinde](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="create-groups"></a>Grup oluşturma
Dizininizde yeni grup oluşturmak için New-AzureADGroup cmdlet'ini kullanın. Bu cmdlet, "Pazarlama" adlı yeni bir güvenlik grubu oluşturur:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="update-groups"></a>Güncelleştirme grupları
Varolan bir grubu güncelleştirmek için Set-AzureADGroup cmdlet'ini kullanın. Bu örnekte, sizi DisplayName özelliği "Intune yöneticileri" grubu değiştirmiyorsanız İlk olarak biz Get-AzureADGroup cmdlet'ini kullanarak Grup bulma ve DisplayName özniteliğini kullanarak Filtrele:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Ardından, biz Description özelliğinin yeni değeri "Intune cihaz yöneticileri" değiştirmiyorsanız:

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Grubu yeniden buluyoruz ise Description özelliğinin yeni değeri yansıtmak üzere güncelleştirilir görmek şimdi:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="delete-groups"></a>Grupları sil
Dizininizden grupları silmek için Remove-AzureADGroup cmdlet şu şekilde kullanın:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="manage-group-membership"></a>Grup üyeliğini yönet 
### <a name="add-members"></a>Üye ekle
Bir gruba yeni üye eklemek için Add-AzureADGroupMember cmdlet'ini kullanın. Bu komut, önceki örnekte kullandık Intune Administrators grubuna üye ekler:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

-Nesne kimliği parametresi objectID grubunun bir üyesi eklemek istediğimiz, ve - RefObjectId gruba üye olarak eklemek istediğiniz kullanıcının objectID.

### <a name="get-members"></a>Üyelerini alma
Bir grubun mevcut üyeleri almak için bu örnekte olduğu gibi Get-AzureADGroupMember cmdlet'i kullanın:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

### <a name="remove-members"></a>Üyeleri kaldır
Gruba daha önce eklediğimiz üye kaldırmak için Remove-AzureADGroupMember cmdlet'ini aşağıda gösterildiği gibi kullanın:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

### <a name="verify-members"></a>Üyeleri doğrulayın
Bir kullanıcının grup üyeliklerini doğrulamak için seçim AzureADGroupIdsUserIsMemberOf cmdlet'ini kullanın. Bu cmdlet, parametre olarak kullanıcının grup üyeliklerini denetle istediğiniz objectID ve gruplarının üyeliklerini denetlemek istediğiniz bir listesini alır. Karmaşık tür bir değişken, "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" biçiminde, böylece biz öncelikle bir değişken türle oluşturmanız gerekir sağlanan grupları listesinde olmalıdır:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Ardından, bu karmaşık değişkeninin "Grup" kimlikleri özniteliğinde denetlemek grup kimlikleri için değerler sunuyoruz:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Şimdi biz objectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea $g gruplarında karşı sahip bir kullanıcı grup üyeliklerini denetlemek istiyorsanız, biz kullanmanız gerekir:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Döndürülen değer, bu kullanıcının üyesi olduğu grupların listesidir. Select-AzureADGroupIdsContactIsMemberOf, Select AzureADGroupIdsGroupIsMemberOf kullanarak grupları, belirli bir listesi için kişilere, gruplara veya hizmet sorumlularına üyelik denetlemek için bu yöntemi de uygulayabilirsiniz veya AzureADGroupIdsServicePrincipalIsMemberOf seçin

## <a name="disable-group-creation-by-your-users"></a>Grup oluşturma, kullanıcı tarafından devre dışı bırak
Yönetici olmayan kullanıcılar güvenlik grupları oluşturmasını engelleyebilirsiniz. Microsoft çevrimiçi Dizin Hizmetleri (MSODS) olarak varsayılan davranışı, Self Servis Grup Yönetimi (SSGM) de etkin olup olmadığını grupları oluşturmak, yönetici olmayan kullanıcılar izin vermektir. SSGM ayarı yalnızca uygulamalarım erişim panelinde davranışını denetler. 

Yönetici olmayan kullanıcılar için Grup oluşturma devre dışı bırakmak için:

1. Yönetici olmayan kullanıcılar grupları oluşturmak için izin verildiğini doğrulayın:
   
   ```
   PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
   ```
  
2. Döndürürse `UsersPermissionToCreateGroupsEnabled : True`, sonra da yönetici olmayan kullanıcılar, gruplar oluşturabilirsiniz. Bu özellik devre dışı bırakmak için:
  
   ``` 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```
  
## <a name="manage-owners-of-groups"></a>Grup sahiplerini yönetme
Sahip bir gruba eklemek için Add-AzureADGroupOwner cmdlet'i kullanın:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

-ObjectID parametresi grubun sahibi eklemek istediğimiz objectID, ve - RefObjectId objectID kullanıcı veya hizmet sorumlusu grubun bir sahibi eklenecek istiyoruz.

Bir grubun sahipleri almak için Get-AzureADGroupOwner cmdlet'i kullanın:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Cmdlet'i, belirtilen grubun sahipleri (kullanıcılar ve hizmet sorumluları) listesini döndürür:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Sahibi gruptan kaldırmak istiyorsanız, Remove-AzureADGroupOwner cmdlet'i kullanın:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Ayrılmış diğer adları 
Bir grup, uç noktaları mailNickname veya e-posta adresi grubunun bir parçası olarak kullanılacak diğer ad belirtin son kullanıcının izin belirli oluşturulduğunda. Grupları aşağıdaki üst düzeyde ayrıcalıklı bir e-posta diğer adlar ile yalnızca Azure AD genel yönetici tarafından oluşturulabilir. 
  
* Uygunsuz kullanımı 
* Yönetici 
* yönetici 
* hostmaster 
* majordomo 
* Yöneticisi 
* kök 
* güvenliğini sağlama 
* güvenlik 
* SSL admin 
* Web Yöneticisi 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla Azure Active Directory PowerShell belgeleri bulabilirsiniz [Azure Active Directory cmdlet'leri](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
