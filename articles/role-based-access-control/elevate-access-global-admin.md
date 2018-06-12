---
title: Azure Active Directory'de genel bir yöneticinin erişimini yükseltme | Microsoft Docs
description: Azure Active Directory'de Azure portalı veya REST API kullanarak genel bir yöneticinin erişimini yükseltme açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e1e46d5fb786b09a4c006b61f52b3ac99aafd555
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266519"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Azure Active Directory'de genel bir yöneticinin erişimini yükseltme

Kullanıyorsanız bir [genel yönetici](../active-directory/active-directory-assign-admin-roles-azure-portal.md#global-administrator) Azure Active Directory (Azure AD), aşağıdakileri yapmak istediğinizde zamanlar olabilir:

- Kullanıcı erişim kesildiğinde bir Azure aboneliği erişimi yeniden kazanmak
- Başka bir kullanıcı veya kendiniz bir Azure aboneliğine erişim
- Bir kuruluştaki tüm Azure abonelikleri bakın
- Tüm Azure abonelikleri erişmek için bir Otomasyon uygulaması (örneğin, bir faturalama veya Denetim uygulama) izin ver

Varsayılan olarak, Azure AD yönetici rolleri ve Azure rol tabanlı erişim (RBAC) rollerini değil aralık Azure AD ve Azure denetler. Ancak, Azure AD'de bir genel Yöneticiyseniz, Azure abonelikleri ve Yönetim grupları yönetmek için erişim yükseltebilirsiniz. Erişiminizi yükseltmesine zaman verilecek [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) belirli bir kiracı için tüm abonelikleri rolü (RBAC rolü). Kullanıcı erişimi yöneticisi rolü, diğer kullanıcıların kök kapsamda Azure kaynaklara erişim izni sağlar (`/`).

Bu yükseltme geçicidir ve yalnızca gerekli olduğunda bitti olmalıdır.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="elevate-access-for-a-global-administrator-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir genel yönetici erişimini yükseltme

1. Oturum [Azure portal](https://portal.azure.com) veya [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com).

1. Gezinti listesinde tıklayın **Azure Active Directory** ve ardından **özellikleri**.

   ![Azure AD özellikleri - ekran görüntüsü](./media/elevate-access-global-admin/aad-properties.png)

1. Altında **genel yönetici Azure abonelikleri ve Yönetim gruplarını yönetebilir**, anahtar kümesine **Evet**.

   ![Genel yönetici Azure abonelikleri ve Yönetim grupları - ekran görüntüsü yönetebilir](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Anahtar ayarlandığında **Evet**, genel Yönetici hesabınızla (şu anda oturum açmış kullanıcı) Azure RBAC kök kapsamda kullanıcı erişimi Yöneticisi rolüne eklenir (`/`), size erişim Görünüm ve rapor üzerinde hangi verir Azure AD Kiracı ile ilişkilendirilen tüm Azure abonelikleri.

   Anahtar ayarlandığında **Hayır**, genel Yönetici hesabınızla (şu anda oturum açmış kullanıcı) Azure RBAC kullanıcı erişimi Yöneticisi rolünde kaldırılır. Azure AD Kiracı ile ilişkilendirilen tüm Azure abonelikleri göremez ve görüntülemek ve yalnızca erişim için verilmiş Azure Aboneliklerini yönetmek.

1. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

   Bu ayar, genel bir özellik değildir ve yalnızca şu anda oturum açmış kullanıcı için geçerlidir.

1. Yükseltilmiş erişim sağlamak için gereken görevleri gerçekleştirin. İşiniz bittiğinde, anahtar kümesi geri **Hayır**.

## <a name="list-role-assignment-at-the-root-scope--using-powershell"></a>Liste kapsamdaki rol atamasını PowerShell kullanarak kök (/)

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

## <a name="remove-a-role-assignment-at-the-root-scope--using-powershell"></a>Bir rol ataması PowerShell kullanarak kök kapsamda (/) Kaldır

Kök kapsamda bir kullanıcı için kullanıcı erişimi yöneticisi rol atamasını kaldırmak için (`/`), kullanın [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) komutu.

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="elevate-access-for-a-global-administrator-using-the-rest-api"></a>REST API kullanarak için genel yönetici erişimini yükseltme

REST API kullanarak için genel yönetici erişimini yükseltme için aşağıdaki temel adımları kullanın.

1. REST kullanarak, çağrı `elevateAccess`, verdiği, kök kapsamda kullanıcı erişimi yöneticisi rolü (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Oluşturma bir [rol ataması](/rest/api/authorization/roleassignments) herhangi kapsamındaki herhangi bir rol atamak için. Aşağıdaki örnek, kök kapsamda {Roledefinitionıd} rol atama için özellikleri gösterir (`/`):

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

1. Kullanıcı erişimi Yöneticisi sırasında rol atamalarını kök kapsamda da kaldırabilirsiniz (`/`).

1. Gerektiğinde kadar kullanıcı erişimi yöneticisi ayrıcalıkları kaldırın.

## <a name="list-role-assignments-at-the-root-scope--using-the-rest-api"></a>REST API kullanarak kök kapsamda (/) rol atamalarını listesi

Tüm kök kapsamda bir kullanıcı için rol atamalarını listelemek (`/`).

- Çağrı [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) burada `{objectIdOfUser}` rol atamalarını almak istediğiniz kullanıcı, nesne kimliği.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

## <a name="remove-elevated-access-using-the-rest-api"></a>REST API kullanarak yükseltilmiş erişimi Kaldır

Çağırdığınızda `elevateAccess`, kendiniz için bir rol ataması oluşturun, böylece bu ayrıcalıkları iptal etmek için atama kaldırmanız gerekir.

1. Çağrı [GET roleDefinitions](/rest/api/authorization/roledefinitions/get) burada `roleName` kullanıcı erişimi yöneticisi rolü adı Kimliğini belirlemek için kullanıcı erişimi Yöneticisi eşittir.

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

    Kimliğinden Kaydet `name` parametresi, bu durumda `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. Ayrıca, Kiracı kapsamda Kiracı yöneticisi rol ataması listesi gerekir. İçin Kiracı kapsamındaki tüm atamalarını listelemek `principalId` yükseltme erişim çağrı yapan bir kiracı yönetici. Bu Kiracı için objectID tüm atamalarını listeler.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Tüm atamaları Kiracı kapsam düzeyinde yeni sonra sonuçlara filtre uygulamak için önceki sorgu çok fazla atamaları, sorgu da döndürürse, bir kiracı Yöneticisi birçok atamaları olmamalıdır: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Önceki çağrılar rol atamalarını listesini döndürür. Rol ataması kapsam olduğu Bul `"/"` ve `roleDefinitionId` 1. adımda bulduğunuz rol adı kimliği ile biter ve `principalId` Kiracı yönetici objectID eşleşir. 
    
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

    3. Son olarak, tarafından eklenen atamasını kaldırmak için rol ataması kimliği kullanmak `elevateAccess`:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Rol tabanlı erişim denetimini REST ile](role-assignments-rest.md)
- [Erişim atamalarını yönetme](role-assignments-users.md)
