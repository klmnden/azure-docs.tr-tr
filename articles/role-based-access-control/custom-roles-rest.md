---
title: REST API - Azure kullanarak özel roller oluşturmanızı | Microsoft Docs
description: REST API kullanarak rol tabanlı erişim denetimi (RBAC) için özel roller oluşturmayı öğrenin. Bu liste, oluşturma, güncelleştirme ve özel rol silmek içerir.
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
ms.openlocfilehash: 8267846fed30baf2c37dcddd453ae9ead9341da9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320593"
---
# <a name="create-custom-roles-using-the-rest-api"></a>REST API kullanarak özel roller oluşturma

Varsa [yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel roller oluşturabilirsiniz. Bu makalede, REST API kullanarak özel rollerini oluşturmak ve yönetmek açıklar.

## <a name="list-roles"></a>Liste rolleri

Tüm rolleri listesi veya görünen adını kullanarak tek bir rol hakkında bilgi almak için kullanın [rol tanımları - liste](/rest/api/authorization/roledefinitions/list) REST API. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/read` kapsamındaki işlemi. Birkaç [yerleşik roller](built-in-roles.md) bu işlem için erişim verilir.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde Değiştir *{kapsamı}* rollerini listelemek istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{filtre}* rol listesini filtrelemek için uygulamak istediğiniz koşulu.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=atScopeAndBelow()` | Kendi alt kapsamlar ataması için belirtilen kapsamda ve diğer kullanılabilir rolleri listeler. |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Rolün tam görünen adı, URL kodlanmış form kullanın. Örneğin, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="get-information-about-a-role"></a>Bir rolü hakkında bilgi edinin

Rol tanımı tanımlayıcısını kullanarak bir rolü hakkında bilgi almak için [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) REST API. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/read` kapsamındaki işlemi. Birkaç [yerleşik roller](built-in-roles.md) bu işlem için erişim verilir.

Görünen adını kullanarak tek bir rol hakkında bilgi almak için önceki bakın [listesinde rolleri](custom-roles-rest.md#list-roles) bölümü.

1. Kullanım [rol tanımları - liste](/rest/api/authorization/roledefinitions/list) rol için GUID tanımlayıcısı almak için REST API. Yerleşik roller için tanımlayıcıdan de alabilirsiniz [yerleşik roller](built-in-roles.md).

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde Değiştir *{kapsamı}* rollerini listelemek istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{Roledefinitionıd}* , rol tanımı GUID tanımlayıcısı.

## <a name="create-a-custom-role"></a>Özel bir rol oluşturun

Özel bir rol oluşturmak üzere kullanmanız [rol tanımları - oluştur veya Güncelleştir](/rest/api/authorization/roledefinitions/createorupdate) REST API. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm işlemi `assignableScopes`. Yerleşik roller, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim verilir. 

1. Listesini gözden geçirin [kaynak sağlayıcısı işlemleri](resource-provider-operations.md) özel rolünüze izinlerini oluşturmak kullanılabilir.

1. Özel rol tanımlayıcısı için kullanılan benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcı biçime sahiptir: `00000000-0000-0000-0000-000000000000`

1. Aşağıdaki isteği ve gövde başlatın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. URI içinde Değiştir *{kapsamı}* ilk ile `assignableScopes` özel rolü.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{Roledefinitionıd}* , özel rolünün bir GUID tanımlayıcısı.

1. İstek gövdesi içinde içinde `assignableScopes` özelliğini değiştirin *{Roledefinitionıd}* , GUID tanımlayıcısı.

1. Değiştir *{Subscriptionıd}* , abonelik tanımlayıcısı.

1. İçinde `actions` özelliği gerçekleştirilecek rolü sağlar işlemleri ekleyin.

1. İçinde `notActions` özelliği, hariç tutulan işlemler eklemek izin verilen gelen `actions`.

1. İçinde `roleName` ve `description` özellikleri, benzersiz rol adı ve açıklama belirtin. Özellikleri hakkında daha fazla bilgi için bkz: [özel roller](custom-roles.md).

    Bir istek gövdesi örneği gösterilmektedir:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Özel bir rol güncelleştirme

Özel bir rol güncelleştirmek için [rol tanımları - oluşturma veya güncelleştirme](/rest/api/authorization/roledefinitions/createorupdate) REST API. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm işlemi `assignableScopes`. Yerleşik roller, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim verilir. 

1. Kullanım [rol tanımları - liste](/rest/api/authorization/roledefinitions/list) veya [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) özel rol hakkında bilgi almak için REST API. Daha fazla bilgi için önceki bkz [listesinde rolleri](custom-roles-rest.md#list-roles) bölümü.

1. Aşağıdaki isteği başlatın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde Değiştir *{kapsamı}* ilk ile `assignableScopes` özel rolü.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{Roledefinitionıd}* , özel rolünün bir GUID tanımlayıcısı.

1. Özel rol bilgilerini bağlı olarak, bir istek gövdesi aşağıdaki biçimde oluşturun:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. İstek gövdesi için özel rol yapmak istediğiniz değişiklikleri ile güncelleştirin.

    Aşağıdaki örnek bir istek gövdesi olarak eklenen yeni bir tanılama ayarlarını eylemiyle gösterir:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Insights/diagnosticSettings/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi

Bir özel rolü silmek için kullanın [rol tanımları - Sil](/rest/api/authorization/roledefinitions/delete) REST API. Bu API çağrısı için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/delete` tüm işlemi `assignableScopes`. Yerleşik roller, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim verilir. 

1. Kullanım [rol tanımları - liste](/rest/api/authorization/roledefinitions/list) veya [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) özel rol GUID tanıtıcısı almak için REST API. Daha fazla bilgi için önceki bkz [listesinde rolleri](custom-roles-rest.md#list-roles) bölümü.

1. Aşağıdaki isteği başlatın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde Değiştir *{kapsamı}* özel rolü silmek istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştir *{Roledefinitionıd}* , özel rolünün bir GUID tanımlayıcısı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel roller](custom-roles.md)
- [RBAC ve REST API kullanarak erişimini yönetme](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)