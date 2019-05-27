---
title: Tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimini yükseltme | Microsoft Docs
description: İçin tüm abonelikleri ve Azure portalı veya REST API'sini kullanarak Azure Active Directory'de yönetim grupları yönetmek genel yönetici erişimini yükseltme işlemini açıklamaktadır.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ede7037aabc85739ee47636f1390c15e0b0d1639
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66158390"
---
# <a name="elevate-access-to-manage-all-azure-subscriptions-and-management-groups"></a>Tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimini yükseltme

Azure Active Directory'de (Azure AD) bir genel yönetici olarak, tüm abonelikler ve yönetim gruplarına erişim dizininizde olmayabilir. Bu makalede, tüm abonelikler ve yönetim grubuna erişiminizi yükseltebilir yolları açıklanır.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="why-would-you-need-to-elevate-your-access"></a>Neden, erişimini yükseltme gerekiyor mu?

Bir genel Yöneticiyseniz, aşağıdakileri yapmak istediğinizde zamanlar olabilir:

- Bir kullanıcı erişimini kaybettiğinde Azure abonelik veya yönetim grubuna erişim kazanabilmesi
- Başka bir kullanıcıya veya kendiniz bir Azure abonelik veya yönetim grubuna erişim
- Tüm Azure abonelikleri veya yönetim gruplarıyla bir kuruluşta bakın
- Tüm Azure abonelikleri veya yönetim gruplarıyla erişmek için bir Otomasyon uygulaması (örneğin, bir faturalama veya Denetim uygulama) izin ver

## <a name="how-does-elevate-access-work"></a>Nasıl erişim iş yükseltmesine?

Azure AD ve Azure kaynakları güvenli bağımsız olarak birbirinden. Diğer bir deyişle, Azure AD rol atamaları Azure kaynaklarına erişimi veremez ve Azure rol atamaları Azure AD'ye erişim izni yok. Ancak, eğer bir [genel yönetici](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) Azure AD'de, kendiniz erişim tüm Azure aboneliklerini ve Yönetim gruplarını dizininizde atayabilirsiniz. Sanal makinelerin veya depolama hesapları gibi Azure abonelik kaynaklarına erişiminiz yoksa ve bu kaynakları erişmek için genel yönetici ayrıcalığı kullanmak istiyorsanız bu özelliği kullanın.

Erişiminizi yükseltebilir, atanacak [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) kök kapsamda Azure rolünde (`/`). Bu, tüm kaynakları görüntülemek ve herhangi bir aboneliğe erişim veya dizin yönetim grubuna atamak sağlar. PowerShell kullanarak kullanıcı erişimi yöneticisi rol atamaları kaldırılabilir.

Kök kapsamda yapmanız gereken değişiklikleri yaptıktan sonra bu yükseltilmiş erişim kaldırmanız gerekir.

![Erişimini yükseltme](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Azure portal

İçin genel Azure portalını kullanarak yönetici erişimini yükseltme için bu adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) genel Yöneticisi olarak.

1. Gezinti listesinde **Azure Active Directory** ve ardından **özellikleri**.

   ![Azure AD özellikleri - ekran görüntüsü](./media/elevate-access-global-admin/aad-properties.png)

1. Altında **Azure kaynakları için Access management**, getirin **Evet**.

   ![Azure kaynakları - ekran görüntüsü için erişim yönetimi](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   İki durumlu düğme ayarlandığında **Evet**, Azure RBAC (/) kök kapsamda kullanıcı erişimi yöneticisi rolü atanır. Bu, tüm Azure abonelikleri ve bu Azure AD dizini ile ilişkili yönetim gruplarını rol atama izni verir. Bu geçiş, yalnızca Azure AD'de genel Yönetici rolüne atanan kullanıcılar için kullanılabilir.

   İki durumlu düğme ayarlandığında **Hayır**, kullanıcı hesabınızın Azure RBAC kullanıcı erişimi yöneticisi rolü kaldırılır. Artık tüm Azure abonelikleri ve bu Azure AD dizini ile ilişkili olan Yönetim grupları roller atayabilirsiniz. Görüntüleyebilir ve yalnızca Azure aboneliklerini ve yönetim gruplarına erişim için verilmiş yönetin.

1. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

   Bu ayar, genel bir özellik değildir ve yalnızca şu anda oturum açmış kullanıcı için geçerlidir. Genel yönetici rolünün tüm üyelerinin erişimini yükseltme yapamazsınız.

1. Oturumu kapatın ve erişiminizi yenilemek için yeniden oturum açın.

    Artık, dizininizdeki tüm abonelikler ve yönetim grubu erişimi olmalıdır. Kök kapsamda kullanıcı erişimi yöneticisi rolü atanmış olduğunu fark edeceksiniz.

   ![Abonelik rol atamaları kök kapsam - ekran görüntüsü](./media/elevate-access-global-admin/iam-root.png)

1. Yükseltilmiş erişim yapmanız gereken değişiklikleri yapın.

    Rol atama hakkında daha fazla bilgi için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](role-assignments-portal.md). Azure AD Privileged Identity Management (PIM) kullanıyorsanız bkz [PIM'de yönetmek için keşfetmek Azure kaynaklarını](../active-directory/privileged-identity-management/pim-resource-roles-discover-resources.md) veya [PIM atama bir Azure kaynak rolleri](../active-directory/privileged-identity-management/pim-resource-roles-assign-roles.md).

1. İşiniz bittiğinde ayarlamak **Azure kaynakları için Access management** geri geçiş **Hayır**. Bu kullanıcı başına ayarı olduğundan, aynı kullanıcı olarak erişimini yükseltme için kullanılan oturum açmanız gerekir.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

### <a name="list-role-assignment-at-the-root-scope-"></a>Liste Rol Ataması (/) kök kapsamda

Kök kapsamda bir kullanıcı için kullanıcı erişimi yöneticisi rol ataması listelemek için (`/`), kullanın [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) komutu.

```azurepowershell
Get-AzRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
CanDelegate        : False
```

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>(/) Kök kapsamda bir rol atamasını Kaldır

Kök kapsamda bir kullanıcı için bir kullanıcı erişimi yöneticisi rolü atamasını kaldırmak için (`/`), şu adımları izleyin.

1. Yükseltilmiş erişimi kaldırmak bir kullanıcı olarak oturum açın. Bu erişim veya başka bir genel yönetici kök kapsamda yükseltilmiş erişim ile yükseltmek için kullanılan aynı kullanıcı olabilir.


1. Kullanım [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) kullanıcı erişimi yöneticisi rol atamasını kaldırmak için komutu.

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName <username@example.com> `
      -RoleDefinitionName "User Access Administrator" -Scope "/"
    ```

## <a name="rest-api"></a>REST API

### <a name="elevate-access-for-a-global-administrator"></a>İçin genel yönetici erişimini yükseltme

Aşağıdaki temel adımları için genel bir REST API kullanarak yönetici erişimini yükseltme için kullanın.

1. REST kullanarak, çağrı `elevateAccess`, veren, kök kapsamda kullanıcı erişimi yöneticisi rolü (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Oluşturma bir [rol ataması](/rest/api/authorization/roleassignments) herhangi bir kapsamda herhangi bir rol atamak için. Aşağıdaki örnek, kök kapsamda {Roledefinitionıd} rol ataması için özellikleri gösterir. (`/`):

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Kullanıcı erişimi Yöneticisi çalışırken kök kapsamda rol atamaları da kaldırabilirsiniz (`/`).

1. Yeniden ihtiyaç duyulan kadar kullanıcı erişimi yöneticisi ayrıcalıkları kaldırın.

### <a name="list-role-assignments-at-the-root-scope-"></a>Rol atamalarını listelemek kök kapsamda (/)

Tüm kök kapsamda bir kullanıcı için rol atamalarını listeleyebilir (`/`).

- Çağrı [GET rol](/rest/api/authorization/roleassignments/listforscope) burada `{objectIdOfUser}` rol atamaları almak istediğiniz kullanıcının nesne kimliği.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-the-root-scope-"></a>Atamalarını (/) kök kapsamda izin verilmeyenler listesi

Tüm kök kapsamda bir kullanıcı için reddetme atamalarını listeleyebilir (`/`).

- GET denyAssignments çağrı burada `{objectIdOfUser}` ayarlanmış Reddet atamaları almak istediğiniz kullanıcının nesne kimliği.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi Kaldır

Çağırdığınızda `elevateAccess`, kendiniz için bir rol ataması oluşturun, böylece bu ayrıcalıkları iptal etme, atama kaldırmanız gerekir.

1. Çağrı [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) burada `roleName` kullanıcı erişimi yöneticisi rolü adı kimliği belirlemek için kullanıcı erişimi Yöneticisi eşittir.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
        "roleName": "User Access Administrator",
        "type": "BuiltInRole",
        "description": "Lets you manage user access to Azure resources.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Authorization/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedOn": "2016-05-31T23:14:04.6964687Z",
        "createdBy": null,
        "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Kimlik, Kaydet `name` parametresi bu durumda `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. Rol ataması için dizin Yöneticisi directory kapsamda listelemek gerekir. İçin dizin kapsamındaki tüm atamalarını listeleme `principalId` yükseltme erişim çağrısı yapan bir dizin yönetici. Bu objectID dizinde bulunan tüm atamaları listeler.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Dizin Yöneticisi tüm atamaları directory kapsam düzeyinde yeni ve ardından sonuçları filtrelemek için önceki sorgu çok fazla atamaları da sorgulayabilirsiniz döndürürse, birden fazla atama sahip olmamalıdır: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
   1. Önceki çağrılar rol atamaları listesini döndürür. Rol ataması kapsam olduğu Bul `"/"` ve `roleDefinitionId` 1. adımda bulduğunuz rol adı kimliği ile sona erer ve `principalId` dizin Yöneticisi objectID eşleşir. 
    
      Örnek rol ataması:

       ```json
       {
         "value": [
           {
             "properties": {
               "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
               "principalId": "{objectID}",
               "scope": "/",
               "createdOn": "2016-08-17T19:21:16.3422480Z",
               "updatedOn": "2016-08-17T19:21:16.3422480Z",
               "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
               "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
             },
             "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
             "type": "Microsoft.Authorization/roleAssignments",
             "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
           }
         ],
         "nextLink": null
       }
       ```
        
      Yeniden Kimliğinden Kaydet `name` parametresi, bu durumda e7dd75bc-06f6-4e71-9014-ee96a929d099.

   1. Son olarak, tarafından eklenen atamasını kaldırmak için rol atama kimliği kullanın `elevateAccess`:

      ```http
      DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
      ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki farklı rolleri anlama](rbac-and-directory-admin-roles.md)
- [RBAC ve REST API kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-rest.md)
