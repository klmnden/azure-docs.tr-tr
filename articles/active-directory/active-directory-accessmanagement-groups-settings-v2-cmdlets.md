---
title: Azure Active Directory'deki grupları yönetmek için PowerShell örnekleri | Microsoft Docs
description: Bu sayfa, Azure Active Directory'de gruplarınızı yönetmenize yardımcı olmak için PowerShell örnekleri sağlar.
keywords: Azure AD, Azure Active Directory PowerShell grupları, Grup Yönetimi
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 12/06/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: 312efd6233546ae32e498907e04fbf8aea73f7b7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Grup Yönetimi için Azure Active Directory sürüm 2 cmdlet'leri
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Bu makale, Azure Active Directory'de (Azure AD), grupları yönetmek için PowerShell kullanma örnekleri içerir.  Bu ayrıca, Azure AD PowerShell modülü ile ayarlanmasını anlatır. İlk olarak, şunları yapmalısınız [Azure AD PowerShell modülünü indirin](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>Azure AD PowerShell modülünü yükleyin
Azure AD PowerShell modülünü yüklemek için aşağıdaki komutları kullanın:

    PS C:\Windows\system32> install-module azuread

Modül yüklendiğini doğrulamak için aşağıdaki komutu kullanın:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

Şimdi modüldeki cmdlet'ler kullanmaya başlayabilirsiniz. Azure AD modüldeki cmdlet'ler tam bir açıklaması için lütfen için çevrimiçi başvuru belgelerine bakın [Azure Active Directory PowerShell sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connect-to-the-directory"></a>Dizinine bağlanma
Azure AD PowerShell cmdlet'lerini kullanarak gruplarını yönetme başlamadan önce yönetmek istediğiniz dizine PowerShell oturumunuzun bağlanmanız gerekir. Aşağıdaki komutu kullanın:

    PS C:\Windows\system32> Connect-AzureAD

Cmdlet, dizininize erişim için kullanmak istediğiniz kimlik bilgilerini ister. Bu örnekte, kullanıyoruz karen@drumkit.onmicrosoft.com tanıtım dizine erişmek için. Cmdlet oturum dizininize başarıyla bağlandığını göstermek için bir onay döndürür:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Şimdi dizininizde grupları yönetmek için Azuread'i cmdlet'leri kullanmaya başlayabilirsiniz.

## <a name="retrieve-groups"></a>Grupları alma
Var olan grupları dizininizden almak için Get-AzureADGroups cmdlet'ini kullanın. 

Dizindeki tüm grupları almak için parametresiz cmdlet'i kullanın:

    PS C:\Windows\system32> get-azureadgroup

Cmdlet bağlı dizindeki tüm grupları döndürür.

Belirli bir grubu grubun objectID belirtmek almak için - objectID parametresini kullanabilirsiniz:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Cmdlet, objectID girdiğiniz parametresinin değeri ile eşleşen Grup şimdi döndürür:

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

Belirli bir grup kullanmak için arama yapabilirsiniz filtre parametresi. Bu parametre bir ODATA filtre yan tümcesi alır ve aşağıdaki örnekteki gibi filtreyle eşleşen tüm grupları döndürür:

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
> Azure AD PowerShell cmdlet'leri OData sorgu standart uygulayın. Daha fazla bilgi için bkz: **$filter** içinde [OData uç noktasını kullanarak OData sorgu seçeneklerinin](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="create-groups"></a>Grupları oluşturma
Dizininizde yeni bir grup oluşturmak için New-AzureADGroup cmdlet'ini kullanın. Bu cmdlet "Pazarlama" adlı yeni bir güvenlik grubu oluşturur:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="update-groups"></a>Güncelleştirme grupları
Varolan bir grubu güncelleştirmek için Set-AzureADGroup cmdlet'ini kullanın. Bu örnekte, biz "Intune yöneticileri" grubu DisplayName özelliğini değiştirirsiniz İlk olarak, biz Get-AzureADGroup cmdlet'ini kullanarak Grup bulma ve DisplayName özniteliği kullanarak Filtrele:

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

Ardından, biz yeni değer "Intune cihaz yöneticileri" Description özelliği değiştirirsiniz:

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Şimdi biz grubu yeniden bulamazsanız Description özelliği, yeni değer yansıtacak şekilde güncelleştirilir bakın:

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
Dizininizden grubunu silmek için Remove-AzureADGroup cmdlet'ini aşağıdaki şekilde kullanın:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="manage-group-membership"></a>Grup üyeliğini yönet 
### <a name="add-members"></a>Üye ekle
Bir gruba yeni üye eklemek için Add-AzureADGroupMember cmdlet'ini kullanın. Bu komut, önceki örnekte kullandık Intune Administrators grubuna üye ekler:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

-ObjectID parametre biz üye eklemek istediğiniz grubu objectID ve - RefObjectId biz gruba üye olarak eklemek istediğiniz kullanıcının objectID.

### <a name="get-members"></a>Üyeleri Al
Varolan bir grubu üyeleri almak için bu örnekte olduğu gibi Get-AzureADGroupMember cmdlet'ini kullanın:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

### <a name="remove-members"></a>Üyeleri kaldır
Gruba daha önce eklediğimiz üye kaldırmak için aşağıda gösterildiği gibi Remove-AzureADGroupMember cmdlet'i kullanın:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

### <a name="verify-members"></a>Üyeleri doğrulayın
Bir kullanıcı grup üyeliklerini doğrulamak için Select AzureADGroupIdsUserIsMemberOf cmdlet'ini kullanın. Bu cmdlet, parametre olarak grup üyeliklerini Denetlenecek olan kullanıcının objectID ve grup üyeliklerini denetlemek üzere listesini alır. "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" türünde bir karmaşık değişken formunda, böylece biz öncelikle bir değişken bu türüyle oluşturmanız gerekir sağlanan gruplarının listesini olması gerekir:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Ardından, bu karmaşık değişkeninin "grup kimlikleri" özniteliğinde denetlemek grup kimlikleri için değerler sağlar:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Şimdi, biz objectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea karşı $g gruplara sahip bir kullanıcı grup üyeliklerini denetlemek istiyorsanız, biz kullanmanız gerekir:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Döndürülen değer, bu kullanıcının üye olduğu grupları listesidir. Select AzureADGroupIdsContactIsMemberOf, AzureADGroupIdsGroupIsMemberOf seçin ya da seçin AzureADGroupIdsServicePrincipalIsMemberOf kullanarak grupları, belirli bir listesi için kişiler, gruplar ya da hizmet asıl adı üyeliği denetlemek için bu yöntem aynı zamanda uygulanabilir

## <a name="disable-group-creation-by-your-users"></a>Grup oluşturma, kullanıcı tarafından devre dışı bırak
Güvenlik grupları oluşturma, yönetici olmayan kullanıcıların engelleyebilirsiniz. Microsoft Online Dizin Hizmetleri (MSODS) içinde varsayılan davranışı, yönetici olmayan kullanıcıların gruplar oluşturmasına, Self Servis Grup Yönetimi (SSGM) da etkin olup olmadığını izin vermektir. SSGM ayarı yalnızca My uygulamaları erişim panelinde davranışını denetler. 

Yönetici olmayan kullanıcılar için Grup oluşturma devre dışı bırakmak için:

1. Yönetici olmayan kullanıcıların gruplar oluşturmasına izin verileceğini doğrulayın:
   
  ````
  PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
  ````
  
2. Döndürürse `UsersPermissionToCreateGroupsEnabled : True`, ardından yönetici olmayan kullanıcıların gruplar oluşturabilirsiniz. Bu özellik devre dışı bırakmak için:
  
  ```` 
  Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
  ````
  
## <a name="manage-owners-of-groups"></a>Sahiplerini gruplarının Yönet
Sahipleri bir gruba eklemek için Add-AzureADGroupOwner cmdlet'i kullanın:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

-ObjectID parametre biz sahibi eklemek istediğiniz grubu objectID ve - RefObjectId biz grubun sahibi olarak eklemek istediğiniz kullanıcının objectID.

Bir grubun sahiplerini almak için Get-AzureADGroupOwner cmdlet'i kullanın:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Cmdlet'i belirtilen grubun sahiplerini listesini döndürür:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Bir sahip bir gruptan kaldırmak istiyorsanız, Remove-AzureADGroupOwner cmdlet'i kullanın:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Ayrılmış diğer adlar 
Bir grup, uç noktaları mailNickname veya e-posta adresi grubunun bir parçası olarak kullanılacak diğer belirtmek son kullanıcı izin belirli oluşturulduğunda. Aşağıdaki üst düzey ayrıcalıklı e-posta diğer adlar gruplarıyla yalnızca Azure AD genel yönetici tarafından oluşturulabilir. 
  
* Uygunsuz kullanım 
* yönetici 
* Yönetici 
* hostmaster 
* majordomo 
* Yöneticisi 
* kök 
* Güvenli 
* güvenlik 
* SSL-Yönetim 
* Web Yöneticisi 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla Azure Active Directory PowerShell belgeleri bulabilirsiniz [Azure Active Directory cmdlet'leri](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
