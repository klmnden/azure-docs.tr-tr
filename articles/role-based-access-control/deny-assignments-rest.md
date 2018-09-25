---
title: RBAC ve REST API - Azure kullanarak atamaları izin verilmeyenler listesi | Microsoft Docs
description: Liste öğrenin kullanıcıları, grupları ve uygulamaları, rol tabanlı erişim denetimi (RBAC) ve REST API kullanarak atamalarını reddet.
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
ms.openlocfilehash: 44c1d3b18bb9bdc63247379fe3f277cb6542f2da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975596"
---
# <a name="list-deny-assignments-using-rbac-and-the-rest-api"></a>RBAC ve REST API kullanarak atamaları izin verilmeyenler listesi

Şu anda atamaları Reddet **salt okunur** ve yalnızca Azure tarafından ayarlanabilir. Kendi oluşturulamıyor olsa bile atamaları, reddetme, listeleyebilirsiniz etkili izinlerinizi olumsuz etkilenebilir, çünkü atamaları reddet. Bu makalede, listelemek için atamaları RBAC ve REST API'sini kullanarak nasıl Reddet açıklanır.

## <a name="list-a-single-deny-assignment"></a>İzin verilmeyenler listesi tek bir atama

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI içinde değiştirin *{kapsamı}* Reddet atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Tür |
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

    | Kapsam | Tür |
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

- [Anlamak atamaları Reddet](deny-assignments.md)
- [İçin Azure Active Directory'de genel yönetici erişimini yükseltme](elevate-access-global-admin.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
