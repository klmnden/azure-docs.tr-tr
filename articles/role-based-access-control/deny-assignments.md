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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: fa1a979c01999bd79c45d24e4c7771edaf346dd8
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632424"
---
# <a name="understand-deny-assignments"></a>Reddetme atamalarını anlama

Bir rol ataması için benzer bir *atamasını Reddet* kümesini ekler, bir kullanıcı, Grup veya hizmet sorumlusu erişimi engelleme amacıyla belirli bir kapsamda eylemleri reddet. Bir rol ataması bunları erişim verse bile belirli eylemleri gerçekleştirmeyi atamaları blok kullanıcıların erişimini engelleyin. Artık azure'da sağlayıcıları dahil, bazı kaynak atamaları reddet. Şu anda, reddetme atamaları **salt okunurdu** ve yalnızca Azure tarafından ayarlanabilir.

Bazı yönlerden, atamalar rol atamaları farklı reddet. Atamalar sorumluları hariç tutmak ve alt kapsamlar için devralma önlemek reddet. Atamalar da uygulamak için reddetme [Klasik Abonelik Yöneticisi](rbac-and-directory-admin-roles.md) atamaları.

Bu makalede nasıl atamaları tanımlanmış reddet.

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
> | `Principals[i].Id` | Evet | String[] | Azure AD sorumlusu nesnesi Reddet atama uygulandığı kimlikleri (kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) dizisi. Boş bir GUID kümesi `00000000-0000-0000-0000-000000000000` tüm ilkeleri temsil etmek için. |
> | `Principals[i].Type` | Hayır | String[] | İlkeleri [i] .id tarafından temsil edilen nesne türleri dizisi. Kümesine `SystemDefined` tüm ilkeleri temsil etmek için. |
> | `ExcludePrincipals[i].Id` | Hayır | String[] | Azure AD sorumlusu nesnesi (kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) Reddet atama değil uygulama kimlikleri dizisi. |
> | `ExcludePrincipals[i].Type` | Hayır | String[] | [İ] ExcludePrincipals .id tarafından temsil edilen nesne türleri dizisi. |
> | `IsSystemProtected` | Hayır | Boole | Bu atama Azure tarafından oluşturulmuş ve düzenlenemez veya silinmiş Reddet belirtir. Şu anda korumalı sistemi atamalardır tümünü reddet. |

## <a name="system-defined-principal"></a>Sistem tarafından tanımlanan asıl

İçin destek Reddet atamaları **tanımlı asıl** tanıtılmıştır. Bu asıl tüm kullanıcıları, grupları, hizmet sorumluları ve yönetilen bir Azure AD dizini kimliklerini temsil eder. Sorumlu Kimliği bir GUID sıfır ise `00000000-0000-0000-0000-000000000000` ve asıl tür `SystemDefined`, sorumlu, tüm ilkeleri temsil eder. `SystemDefined` birleştirilebilir `ExcludePrincipals` bazı kullanıcılar hariç tüm ilkeleri reddetmek için. `SystemDefined` şu kısıtlamalara tabidir:

- Yalnızca kullanılan `Principals` ve kullanılamaz `ExcludePrincipals`.
- `Principals[i].Type` ayarlanmalıdır `SystemDefined`.

## <a name="next-steps"></a>Sonraki adımlar

* [RBAC ve REST API kullanarak atamaları izin verilmeyenler listesi](deny-assignments-rest.md)
* [Rol tanımları anlama](role-definitions.md)
