---
title: Anlamak atamaları Azure kaynakları için reddetme | Microsoft Docs
description: İlgili Azure kaynakları için rol tabanlı erişim denetimi (RBAC) atamalarını Reddet öğrenin.
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
ms.openlocfilehash: 53716fa343df25026dcc668ed8483673d934d1ad
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56339133"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Anlamak Azure kaynakları için atamaları Reddet

Bir rol ataması için benzer bir *atamasını Reddet* kümesini ekler, bir kullanıcı, Grup veya hizmet sorumlusu erişimi engelleme amacıyla belirli bir kapsamda eylemleri reddet. Bir rol ataması bunları erişim verse bile belirli bir Azure kaynak Eylemler gerçekleştirmeyi atamaları blok kullanıcıların erişimini engelleyin. Artık azure'da sağlayıcıları dahil, bazı kaynak atamaları reddet. Şu anda atamaları Reddet **salt okunur** ve yalnızca Microsoft tarafından ayarlanabilir.

Bazı yönlerden, atamalar rol atamaları farklı reddet. Atamalar sorumluları hariç tutmak ve alt kapsamlar için devralma önlemek reddet. Atamalar da uygulamak için reddetme [Klasik Abonelik Yöneticisi](rbac-and-directory-admin-roles.md) atamaları.

Bu makalede nasıl atamaları tanımlanmış reddet.

## <a name="deny-assignment-properties"></a>Atama özelliklerini Reddet

 Bir reddetme ataması aşağıdaki özelliklere sahiptir:

> [!div class="mx-tableFixed"]
> | Özellik | Gerekli | Tür | Açıklama |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Evet | String | Reddetme atamasının görünen adı. Adları belirli bir kapsam için benzersiz olmalıdır. |
> | `Description` | Hayır | String | Reddetme atama açıklaması. |
> | `Permissions.Actions` | En az bir eylem veya bir DataActions | String[] | Yönetim işlemleri için reddetme atama erişimi engeller belirten bir dize dizisi. |
> | `Permissions.NotActions` | Hayır | String[] | Reddetme atamadan dışlamak için yönetim işlemleri belirten bir dize dizisi. |
> | `Permissions.DataActions` | En az bir eylem veya bir DataActions | String[] | İçin erişimi reddet atama engeller veri işlemleri belirten bir dize dizisi. |
> | `Permissions.NotDataActions` | Hayır | String[] | Reddetme atamadan dışlamak için veri işlemleri belirten bir dize dizisi. |
> | `Scope` | Hayır | String | Reddetme atama uygulandığı kapsam belirten bir dize. |
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

* [REST API kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi](deny-assignments-rest.md)
* [Azure kaynakları için rol tanımları anlama](role-definitions.md)
