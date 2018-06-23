---
title: RBAC ve REST API - Azure kullanarak erişimini yönetme | Microsoft Docs
description: Kullanıcılar, gruplar ve uygulamalar, rol tabanlı erişim denetimi (RBAC) ve REST API kullanarak erişimi yönetmek üzere öğrenin. Bu erişim listesinde, erişim ve erişimi kaldırmak nasıl içerir.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: cfcb87fdff8105b25d4f7e63b775aaf9243d2a90
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317016"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>RBAC ve REST API kullanarak erişimini yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) Azure kaynaklarına erişimi yönetme yoludur. Bu makalede, kullanıcılar, gruplar ve RBAC ve REST API kullanarak uygulamalar için erişim yönetme açıklanmaktadır.

## <a name="list-access"></a>Liste erişim

RBAC, liste erişim için rol atamalarını listeleyin. Rol atamaları listesinde, aşağıdakilerden birini kullanmak için [rol atamalarını - liste](/rest/api/authorization/roleassignments/list) REST API'leri. Sonuçlarınızı daraltmak için bir kapsam ve isteğe bağlı bir Filtresi belirtin. API'yi çağırmak için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/read` belirtilen kapsamda işlemi. Birkaç [yerleşik roller](built-in-roles.md) bu işlem için erişim verilir.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde Değiştir *{kapsamı}* rol atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{filtre}* rol ataması listesini filtrelemek için uygulamak istediğiniz koşulu.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=atScope()` | Rol atamaları subscopes olarak dahil değil yalnızca belirtilen kapsam için rol atamalarını listeleyin. |
    | `$filter=principalId%20eq%20'{objectId}'` | Belirtilen kullanıcı, Grup veya hizmet sorumlusu için rol atamalarını listeleyin. |
    | `$filter=assignedTo('{objectId}')` | Olanları gruplarından devralınan dahil olmak üzere belirli bir kullanıcı için rol atamalarını listeleyin. |

## <a name="grant-access"></a>Erişim verme

RBAC, erişim vermek için bir rol ataması oluşturun. Bir rol ataması oluşturmak üzere kullanmanız [rol atamalarını - oluşturmak](/rest/api/authorization/roleassignments/create) REST API ve güvenlik sorumlusu, rol tanımı ve kapsam belirtin. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/write` işlemi. Yerleşik roller, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim verilir.

1. Kullanım [rol tanımları - liste](/rest/api/authorization/roledefinitions/list) REST API veya bkz [yerleşik roller](built-in-roles.md) atamak istediğiniz rolü tanımı tanımlayıcısı alınamıyor.

1. Rol ataması tanımlayıcısı için kullanılan benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcı biçime sahiptir: `00000000-0000-0000-0000-000000000000`

1. Aşağıdaki isteği ve gövde başlatın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. URI içinde Değiştir *{kapsamı}* rol atamasının kapsamı olan.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{roleAssignmentName}* , rol atamasının GUID tanımlayıcısı.

1. İstek gövdesinde Değiştir *{Subscriptionıd}* , abonelik tanımlayıcısı.

1. Değiştir *{Roledefinitionıd}* , rol tanımı tanımlayıcısı.

1. Değiştir *{Principalıd}* kullanıcı, Grup veya rolü atanmış hizmet asıl nesne tanımlayıcısı.

## <a name="remove-access"></a>Erişimi kaldır

RBAC, erişimi kaldırmak için bir rol ataması kaldırın. Bir rol atamasını kaldırmak için kullanın [rol atamalarını - Sil](/rest/api/authorization/roleassignments/delete) REST API. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/delete` işlemi. Yerleşik roller, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim verilir.

1. Rol ataması tanımlayıcı (GUID) alın. Bu tanımlayıcı, rol ataması ilk oluşturduğunuzda veya rol atamalarını listeleyerek elde edebilirsiniz döndürülür.

1. Aşağıdaki isteği başlatın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. URI içinde Değiştir *{kapsamı}* rol atamasını kaldırmak için kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{roleAssignmentName}* , rol atamasının GUID tanımlayıcısı.

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
- [REST API kullanarak özel roller oluşturma](custom-roles-rest.md)