---
title: REST API - Azure kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi | Microsoft Docs
description: Liste öğrenin kullanıcıları, grupları ve uygulamaları, Azure kaynaklarını ve REST API için rol tabanlı erişim denetimi (RBAC) kullanarak atamalarını reddet.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ba1c60d45fb53be158d9e302748366ddf417f23e
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805486"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>REST API kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi

Şu anda atamaları Reddet **salt okunur** ve yalnızca Microsoft tarafından ayarlanabilir. Kendi reddetme atamalarınızı oluşturamıyor olsanız bile, reddetme atamalarını listeleyebilirsiniz çünkü bunlar sizin geçerli izinlerinizi etkileyebilir. Bu makalede, listelemek için atamaları RBAC ve REST API'sini kullanarak nasıl Reddet açıklanır.

## <a name="list-a-single-deny-assignment"></a>İzin verilmeyenler listesi tek bir atama

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI içinde değiştirin *{kapsamı}* Reddet atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{Reddet-atama-kimliği}* almak istediğiniz Reddet atama tanımlayıcısına sahip.

## <a name="list-multiple-deny-assignments"></a>Birden çok listesinde atamaları Reddet

1. Aşağıdaki isteklerini biriyle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    İsteğe bağlı parametrelerle:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. URI içinde değiştirin *{kapsamı}* Reddet atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{filter}* reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulu.

    | Filtre | Açıklama |
    | --- | --- |
    | (filtre yok) | Tüm atamaları en üstünde ve aşağıda belirtilen kapsam reddet. |
    | `$filter=atScope()` | Atamalar ve üzeri yalnızca belirtilen kapsam için izin verilmeyenler listesi. Reddetme atamaları subscopes olarak içermez. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Belirtilen ada sahip atamaları izin verilmeyenler listesi. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Atamalarını (/) kök kapsamda izin verilmeyenler listesi

1. Açıklandığı gibi erişimini yükseltme [için Azure Active Directory'de genel yönetici erişimini yükseltme](elevate-access-global-admin.md).

1. Aşağıdaki isteği kullanın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Değiştirin *{filter}* reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulu. Bir filtre gereklidir.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=atScope()` | Atamalar yalnızca kök kapsamı için izin verilmeyenler listesi. Reddetme atamaları subscopes olarak içermez. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Belirtilen ada sahip atamaları izin verilmeyenler listesi. |

1. Yükseltilmiş erişim kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Anlamak Azure kaynakları için atamaları Reddet](deny-assignments.md)
- [Azure Active Directory'de Genel Yönetici erişimini yükseltme](elevate-access-global-admin.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
