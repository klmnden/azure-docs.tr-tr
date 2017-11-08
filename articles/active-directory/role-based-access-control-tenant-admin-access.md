---
title: "Kiracı yönetici yükseltmesine erişim - Azure AD | Microsoft Docs"
description: "Bu konu için rol tabanlı erişim denetimi (RBAC) rollerdeki yerleşik açıklar."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2017
ms.author: andredm
ms.openlocfilehash: cb6e5a398a1d7e20efbcc4a8900f9e8dea43ad2c
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Rol tabanlı erişim denetimi ile bir kiracı Yöneticisi olarak erişimini yükseltme

Rol tabanlı erişim denetimi normalden daha yüksek izinleri verebilirsiniz geçici yükseltmeleri erişim elde Kiracı Yöneticiler yardımcı olur. Bir kiracı Yöneticisi kendisini gerektiğinde kullanıcı erişimi Yöneticisi rolüne yükseltebilirsiniz. Bu rol Kiracı kendisini ya da diğer rolleri "/" kapsamındaki vermek için yönetici izinleri verir.

Bu özellik, bir kuruluşta mevcut tüm abonelikleri görmek Kiracı yönetici izin verdiği için önemlidir. Faturalama ve tüm abonelikleri erişmek ve faturalama veya varlık yönetimi için kuruluş durumunu doğru bir görünümünü sağlamak için denetleme gibi automation uygulamalar için de sağlar.  

## <a name="use-elevateaccess-for-tenant-access-with-azure-ad-admin-center"></a>Azure AD Yönetim Merkezi ile Kiracı erişim elevateAccess kullanın

1. Git [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) ve oturum, kimlik bilgileri.

2. Seçin **özellikleri** Azure AD'den menü sol.

3. İçinde **özellikleri** dikey penceresinde Bul **genel yönetici, Azure abonelikleri yönetebilir**, seçin **Evet**, ardından **kaydetmek**.
    > [!IMPORTANT] 
    > Seçtiğinizde **Evet**, atar **kullanıcı erişimi Yöneticisi** rol kökünde "/" (kök kapsamı) ile şu anda oturum Portalı'na kullanıcı için. **Bu, diğer tüm Azure abonelikleri görmek kullanıcının sağlar.**
    
    > [!NOTE] 
    > Seçtiğinizde **Hayır**, kaldırır **kullanıcı erişimi Yöneticisi** rol kökünde "/" (kök kapsamı) ile şu anda oturum Portalı'na kullanıcı için.

> [!TIP] 
> İzlenim bu Azure Active Directory için genel bir özellik olan, ancak şu anda oturum açmış kullanıcı için kullanıcı başına temelinde işlevleri olur. Azure Active Directory'de genel yönetici haklarına sahip olduğunuzda, Azure Active Directory Yönetim Merkezi açmış kullanıcı elevateAccess özelliğini çağırabilirsiniz.

![Azure AD Yönetim Merkezi - özellikleri - Globaladmin Azure aboneliği - ekran görüntüsü yönetebilirsiniz](./media/role-based-access-control-tenant-admin-access/aad-azure-portal-global-admin-can-manage-azure-subscriptions.png)

## <a name="use-elevateaccess-to-give-tenant-access-with-the-rest-api"></a>REST API ile Kiracı erişim vermek için elevateAccess kullanın

Temel işlem aşağıdaki adımlarla çalışır:

1. REST kullanarak, çağrı *elevateAccess*, hangi kullanıcı erişimi yöneticisi rolü verir "/" kapsam.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Oluşturma bir [rol ataması](/rest/api/authorization/roleassignments) herhangi kapsamındaki herhangi bir rol atamak için. Aşağıdaki örnek, okuyucu rolüne atamak için özellikleri gösterir "/" Kapsam:

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. Kullanıcı erişim yönetimi sırasında de rol atamaları olarak sil "/" kapsam.

4. Gerektiğinde kadar kullanıcı erişimi yönetici ayrıcalıkları iptal edin.


## <a name="how-to-undo-the-elevateaccess-action-with-the-rest-api"></a>REST API elevateAccess eylemiyle geri alma

Çağırdığınızda *elevateAccess* atama silmeniz gerekiyorsa bu ayrıcalıkları iptal etmek için bir rol ataması kendiniz için oluşturun.

1.  GET rol tanımları çağırma olduğu roleName GUID kullanıcı erişimi yöneticisi rolünün adını belirlemek için kullanıcı erişimi Yöneticisi =.
    1.  Alma *https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$ Filtre roleName + eq + =' Kullanıcı + Erişim + Yöneticisi*

        ```
        {"value":[{"properties":{
        "roleName":"User Access Administrator",
        "type":"BuiltInRole",
        "description":"Lets you manage user access to Azure resources.",
        "assignableScopes":["/"],
        "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
        "createdOn":"0001-01-01T08:00:00.0000000Z",
        "updatedOn":"2016-05-31T23:14:04.6964687Z",
        "createdBy":null,
        "updatedBy":null},
        "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
        "type":"Microsoft.Authorization/roleDefinitions",
        "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
        "nextLink":null}
        ```

        GUID değerinden Kaydet *adı* parametresi, bu durumda **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Kiracı yönetici Kiracı kapsamda rol ataması listesi gerekir. Çağrı yükseltme erişim yapan TenantAdmin Principalıd için Kiracı kapsamındaki tüm atamaları listeler. Bu Kiracı için objectID tüm atamalarını listeler. 
    1. Alma *https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$ Filtre = Principalıd + eq + '{objectID}'*
    
        >[!NOTE] 
        >Tüm atamaları yalnızca Kiracı kapsam düzeyinde sonra sonuçlara filtre uygulamak için yukarıdaki sorgu çok fazla atamaları, sorgu da döndürürse, birçok atamaları, Kiracı yönetici sahip olmamalıdır: GET *https://management.azure.com/providers/ Microsoft.Authorization/roleAssignments? api-version = 2015-07-01 & $filter=atScope()*
        
    2. Yukarıdaki çağrıları rol atamalarını listesini döndürür. Rol ataması kapsam olduğu Bul "/" 1. adımda bulduğunuz GUID'i rol adı Roledefinitionıd biter ve Principalıd eşleşen objectID Kiracı yönetici Rol ataması şöyle görünür:

        ```
        {"value":[{"properties":{
        "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
        "principalId":"{objectID}",
        "scope":"/",
        "createdOn":"2016-08-17T19:21:16.3422480Z",
        "updatedOn":"2016-08-17T19:21:16.3422480Z",
        "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
        "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
        "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
        "type":"Microsoft.Authorization/roleAssignments",
        "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
        "nextLink":null}
        ```
        
        Yeniden adresinden GUID Kaydet *adı* parametresi, bu durumda **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

    3. Son olarak, vurgulanan kullanmak **RoleAssignment kimliği** yükseltmesine erişim tarafından eklenen atamasını silmek için:

        Https://Management.Azure.com /providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01 Sil

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [rol tabanlı erişim denetimini REST ile yönetme](role-based-access-control-manage-access-rest.md)

- [Erişim atamalarını yönetmeyi](role-based-access-control-manage-assignments.md) Azure portalında
