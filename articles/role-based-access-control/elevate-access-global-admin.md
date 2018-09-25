---
title: İçin Azure Active Directory'de genel yönetici erişimini yükseltme | Microsoft Docs
description: İçin Azure portalı veya REST API'sini kullanarak Azure Active Directory'de genel yönetici erişimini yükseltme işlemini açıklamaktadır.
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
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: fb0fb4e0f23413cb56b1bb5ec419c44dfc52e7b6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996851"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>İçin Azure Active Directory'de genel yönetici erişimini yükseltme

Eğer bir [genel yönetici](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) Azure Active Directory'de (Azure AD), aşağıdakileri yapmak istediğinizde zamanlar olabilir:

- Kullanıcı erişimini kaybettiğinde, bir Azure aboneliğine erişim kazanabilmesi
- Başka bir kullanıcı veya kendiniz bir Azure aboneliğine erişim
- Bir kuruluştaki tüm Azure abonelikleri bakın
- Tüm Azure abonelikleri erişmek için bir Otomasyon uygulaması (örneğin, bir faturalama veya Denetim uygulama) izin ver

Varsayılan olarak, Azure AD yönetici rollerini ve Azure rol tabanlı erişim (RBAC) rolleri değil aralık Azure AD ve Azure denetler. Ancak, Azure AD'de bir genel Yöneticiyseniz, Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişiminizi yükseltebilirsiniz. Erişiminizi yükseltebilir, verilecek [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) belirli bir kiracının tüm Aboneliklerde rol (RBAC rolü). Diğer kullanıcılara kök kapsamda Azure kaynaklarına erişim verme kullanıcı erişimi yöneticisi rolü sağlar (`/`).

Bu yükseltme geçici ve yalnızca gerektiğinde bitti olmalıdır.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="azure-portal"></a>Azure portal

İçin genel Azure portalını kullanarak yönetici erişimini yükseltme için bu adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

1. Gezinti listesinde **Azure Active Directory** ve ardından **özellikleri**.

   ![Azure AD özellikleri - ekran görüntüsü](./media/elevate-access-global-admin/aad-properties.png)

1. Altında **genel yönetici Azure aboneliklerini ve Yönetim gruplarını yönetebilir**, anahtar kümesine **Evet**.

   ![Genel yönetici Azure aboneliklerini ve Yönetim gruplarını - ekran görüntüsü yönetebilir.](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Anahtar ayarlandığında **Evet**, Azure RBAC kök kapsamda kullanıcı erişimi yöneticisi rolü genel yönetici hesabınızın (şu anda oturum açmış kullanıcı olarak) eklenir (`/`), size erişim görünümünü ve raporunu için hangi verir Tüm Azure abonelikleri Azure AD kiracınız ile ilişkili.

   Anahtar ayarlandığında **Hayır**, Azure RBAC kullanıcı erişimi yöneticisi rolü genel yönetici hesabınızın (şu anda oturum açmış kullanıcı olarak) kaldırılır. Azure AD kiracısı ile ilişkili tüm Azure abonelikleri göremez ve görüntüleyebilir ve yalnızca erişim için verilmiş Azure aboneliklerini yönetme.

1. Tıklayın **Kaydet** ayarlarınızı kaydetmek için.

   Bu ayar, genel bir özellik değildir ve yalnızca şu anda oturum açmış kullanıcı için geçerlidir.

1. Yükseltilmiş erişim sağlamak için gereken görevleri gerçekleştirin. İşiniz bittiğinde, anahtar kümesi geri **Hayır**.

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-role-assignment-at-the-root-scope-"></a>Liste Rol Ataması (/) kök kapsamda

Kök kapsamda bir kullanıcı için kullanıcı erişimi yöneticisi rol ataması listelemek için (`/`), kullanın [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) komutu.

```azurepowershell
Get-AzureRmRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
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
```

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>(/) Kök kapsamda bir rol atamasını Kaldır

Kök kapsamda bir kullanıcı için bir kullanıcı erişimi yöneticisi rolü atamasını kaldırmak için (`/`), kullanın [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) komutu.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
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

2. Ayrıca, Kiracı kapsamında Kiracı yöneticiye ait rol ataması listesi gerekir. İçin Kiracı kapsamında tüm atamalarını listeleme `principalId` yükseltme erişim çağrısı yapan bir kiracı yönetici. Bu Kiracı için objectID tüm atamaları listeler.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Tüm atamaları Kiracı kapsam düzeyinde yeni ve ardından sonuçları filtrelemek için önceki sorgu çok fazla atamaları da sorgulayabilirsiniz döndürürse, bir kiracı Yöneticisi birden fazla atama sahip olmamalıdır: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Önceki çağrılar rol atamaları listesini döndürür. Rol ataması kapsam olduğu Bul `"/"` ve `roleDefinitionId` 1. adımda bulduğunuz rol adı kimliği ile sona erer ve `principalId` Kiracı yöneticisinin objectID eşleşir. 
    
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

    3. Son olarak, tarafından eklenen atamasını kaldırmak için rol atama kimliği kullanın `elevateAccess`:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [REST ile rol tabanlı erişim denetimi](role-assignments-rest.md)
- [Privileged Identity Management ile Azure kaynaklarına erişimi yönetme](pim-azure-resource.md)
- [Koşullu erişim ile Azure yönetim erişimi yönetme](conditional-access-azure-management.md)
