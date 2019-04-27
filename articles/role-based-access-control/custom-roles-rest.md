---
title: REST API - Azure kullanarak Azure kaynakları için özel roller oluşturma | Microsoft Docs
description: REST API kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel roller oluşturma konusunda bilgi edinin. Bu liste, oluşturma, güncelleştirme ve özel roller silme nasıl içerir.
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
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4024f6fdb40c752ef61f348d15f681e81d81c08c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60596687"
---
# <a name="create-custom-roles-for-azure-resources-using-the-rest-api"></a>REST API kullanarak Azure kaynakları için özel roller oluşturma

Varsa [Azure kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, REST API kullanarak özel roller oluşturmak ve yönetmek açıklar.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Bir dizindeki tüm özel roller listelemek için kullanın [rol tanımları: liste](/rest/api/authorization/roledefinitions/list) REST API.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Değiştirin *{filter}* rolü türüne sahip.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | CustomRole türüne göre filtrele |

## <a name="list-custom-roles-at-a-scope"></a>Bir kapsamda listesi özel roller

Bir kapsamda özel roller listelemek için kullanın [rol tanımları: liste](/rest/api/authorization/roledefinitions/list) REST API.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde değiştirin *{kapsamı}* rolleri listesinde istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{filter}* rolü türüne sahip.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | CustomRole türüne göre filtrele |

## <a name="list-a-custom-role-definition-by-name"></a>Adıyla bir özel rol tanımını listeleme

Görünen adıyla bir özel rol hakkında bilgi almak için kullanın [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) REST API.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde değiştirin *{kapsamı}* rolleri listesinde istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{filter}* rolü için görünen adına sahip.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Rolün tam görünen adı, URL kodlanmış formu kullanın. Örneğin, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

## <a name="list-a-custom-role-definition-by-id"></a>Kimliğe göre bir özel rol tanımını listeleme

Benzersiz kimliğine göre özel bir rol hakkında bilgi almak için kullanın [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) REST API.

1. Kullanım [rol tanımları: liste](/rest/api/authorization/roledefinitions/list) rol için GUID tanımlayıcısını almak için REST API.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde değiştirin *{kapsamı}* rolleri listesinde istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{Roledefinitionıd}* , rol tanımı GUID tanımlayıcısı.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için kullanın [rol tanımları: oluşturma veya güncelleştirme](/rest/api/authorization/roledefinitions/createorupdate) REST API. Bu API'yi çağırmak için sahip rolü atanmış bir kullanıcı hesabıyla oturum açmış `Microsoft.Authorization/roleDefinitions/write` tüm izin `assignableScopes`. Yerleşik rollerin, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu izni buna dahil.

1. Listesini gözden geçirin [kaynak sağlayıcısı işlemleri](resource-provider-operations.md) özel rolünüz için izinleri oluşturmak kullanılabilir.

1. Özel rol tanımlayıcısı için kullanılacak bir benzersiz tanımlayıcısını oluşturmak için bir GUID aracını kullanın. Tanımlayıcı şu biçimdedir: `00000000-0000-0000-0000-000000000000`

1. Aşağıdaki isteği ve gövde ile başlayın:

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

1. URI içinde değiştirin *{kapsamı}* ilk `assignableScopes` özel rolü.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{Roledefinitionıd}* ile özel rolü GUID tanımlayıcısı.

1. İstek gövdesi içinde içinde `assignableScopes` özelliğini değiştirin *{Roledefinitionıd}* , GUID tanımlayıcısı.

1. Değiştirin *{Subscriptionıd}* , abonelik tanımlayıcısı ile.

1. İçinde `actions` özelliği gerçekleştirilecek rolü sağlar işlemleri ekleyin.

1. İçinde `notActions` özelliği, hariç tutulan işlemler Ekle izin verilen gelen `actions`.

1. İçinde `roleName` ve `description` özellikleri, bir rol benzersiz ad ve açıklama belirtin. Özellikleri hakkında daha fazla bilgi için bkz. [özel roller](custom-roles.md).

    İstek gövdesi bir örneği aşağıda gösterilmiştir:

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
              "Microsoft.ResourceHealth/availabilityStatuses/read",
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

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel bir rol güncelleştirmek için [rol tanımları: oluştur veya Güncelleştir](/rest/api/authorization/roledefinitions/createorupdate) REST API. Bu API'yi çağırmak için sahip rolü atanmış bir kullanıcı hesabıyla oturum açmış `Microsoft.Authorization/roleDefinitions/write` tüm izin `assignableScopes`. Yerleşik rollerin, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu izni buna dahil.

1. Kullanım [rol tanımları: liste](/rest/api/authorization/roledefinitions/list) veya [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) özel rol hakkında bilgi almak için REST API. Daha fazla bilgi için bkz. önceki [listesinde özel roller](#list-custom-roles) bölümü.

1. Aşağıdaki isteği başlatın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde değiştirin *{kapsamı}* ilk `assignableScopes` özel rolü.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{Roledefinitionıd}* ile özel rolü GUID tanımlayıcısı.

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

1. İstek gövdesi, özel bir rol için yapmak istediğiniz değişiklikleri ile güncelleştirin.

    Aşağıdaki örnek bir istek gövdesi olarak eklenen yeni bir tanılama ayarları eylemiyle gösterir:

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
              "Microsoft.ResourceHealth/availabilityStatuses/read",
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

## <a name="delete-a-custom-role"></a>Özel rolü silme

Bir özel rolü silmek için kullanın [rol tanımları: silme](/rest/api/authorization/roledefinitions/delete) REST API. Bu API'yi çağırmak için sahip rolü atanmış bir kullanıcı hesabıyla oturum açmış `Microsoft.Authorization/roleDefinitions/delete` tüm izin `assignableScopes`. Yerleşik rollerin, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu izni buna dahil.

1. Kullanım [rol tanımları: liste](/rest/api/authorization/roledefinitions/list) veya [rol tanımlarını - Al](/rest/api/authorization/roledefinitions/get) özel rolü GUID tanımlayıcısını almak için REST API. Daha fazla bilgi için bkz. önceki [listesinde özel roller](#list-custom-roles) bölümü.

1. Aşağıdaki isteği başlatın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI içinde değiştirin *{kapsamı}* özel rolü silmek istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{Roledefinitionıd}* ile özel rolü GUID tanımlayıcısı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için özel roller](custom-roles.md)
- [RBAC ve REST API kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)