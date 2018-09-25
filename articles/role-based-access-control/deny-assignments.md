---
title: Anlamak Azure RBAC atamaları Reddet | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) atamalarını Reddet azure'daki kaynakları öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 8ef3a2ec44c5eff80d3a50a6c56805667e164ba8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980177"
---
# <a name="understand-deny-assignments"></a>Anlamak atamaları Reddet

Bir rol ataması için benzer bir *atamasını Reddet* bağlamalar kümesi, bir kullanıcı, Grup veya hizmet sorumlusu erişimi engelleme amacıyla belirli bir kapsamda eylemleri reddet. Reddet atama, ayrıca sorumluları hariç ve rol atamalarını farklı olduğu için alt kapsamlar, devralma engelleyebilirsiniz. Şu anda atamaları Reddet **salt okunur** ve yalnızca Azure tarafından ayarlanabilir. Bu makalede nasıl atamaları tanımlanmış reddet.

## <a name="deny-assignment-properties"></a>Atama özelliklerini Reddet

 Bir reddetme ataması aşağıdaki özelliklere sahiptir:

> [!div class="mx-tableFixed"]
> | Özellik | Gerekli | Tür | Açıklama |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Evet | Dize | Reddetme atamasının görünen adı. Adları belirli bir kapsam için benzersiz olmalıdır. |
> | `Description` | Hayır | Dize | Reddetme atama açıklaması. |
> | `Permissions.Actions` | En az bir eylem veya bir DataActions | String[] | Yönetim işlemleri için reddetme atama erişimi engeller belirten bir dize dizisi. |
> | `Permissions.NotActions` | Hayır | String[] | Reddetme atamadan dışlamak için yönetim işlemleri belirten bir dize dizisi. |
> | `Permissions.DataActions` | En az bir eylem veya bir DataActions | String[] | İçin erişimi reddet atama engeller veri işlemleri belirten bir dize dizisi. |
> | `Permissions.NotDataActions` | Hayır | String[] | Reddetme atamadan dışlamak için veri işlemleri belirten bir dize dizisi. |
> | `Scope` | Hayır | Dize | Reddetme atama uygulandığı kapsam belirten bir dize. |
> | `DoNotApplyToChildScopes` | Hayır | Boole | Reddetme atama alt kapsamlar için geçerli olup olmadığını belirtir. Varsayılan değer false'tur. |
> | `Principals[i].Id` | Evet | String[] | Azure AD sorumlusu nesnesi Reddet atama uygulandığı kimlikleri (kullanıcı, Grup veya hizmet sorumlusu) dizisi. Boş bir GUID kümesi `00000000-0000-0000-0000-000000000000` herkesin temsil etmek için. |
> | `Principals[i].Type` | Hayır | String[] | İlkeleri [i] .id tarafından temsil edilen nesne türleri dizisi. Kümesine `Everyone` herkesin temsil etmek için. |
> | `ExcludePrincipals[i].Id` | Hayır | String[] | Azure AD sorumlusu nesnesi (kullanıcı, Grup veya hizmet sorumlusu) Reddet atama değil uygulama kimlikleri dizisi. |
> | `ExcludePrincipals[i].Type` | Hayır | String[] | [İ] ExcludePrincipals .id tarafından temsil edilen nesne türleri dizisi. |
> | `IsSystemProtected` | Hayır | Boole | Bu atama Azure tarafından oluşturulmuş ve düzenlenemez veya silinmiş Reddet belirtir. Şu anda korumalı sistemi atamalardır tümünü reddet. |

## <a name="everyone-principal"></a>Asıl herkes

İçin destek Reddet atamaları, herkesin asıl tanıtılmıştır. Herkes asıl tüm kullanıcıları, grupları ve Azure AD Directory Hizmet sorumluları temsil eder. Sorumlu Kimliği bir GUID sıfır ise `00000000-0000-0000-0000-000000000000` ve asıl tür `Everyone`, sorumlu, herkesin temsil eder. Herkes sorumlusu ile birleştirilebilir `ExcludePrincipals` bazı kullanıcılar dışında herkesin reddetmek için. Herkes asıl aşağıdaki sınırlamalara sahiptir:

- Yalnızca kullanılan `Principals` ve kullanılamaz `ExcludePrincipals`.
- `Principals[i].Type` ayarlanmalıdır `Everyone`.

## <a name="next-steps"></a>Sonraki adımlar

* [RBAC ve REST API kullanarak atamaları izin verilmeyenler listesi](deny-assignments-rest.md)
* [Rol tanımları anlama](role-definitions.md)
