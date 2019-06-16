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
ms.date: 06/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 432703b5acb4cd56dac9b25edf99165ca26b0aa0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67118268"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Anlamak Azure kaynakları için atamaları Reddet

Bir rol ataması için benzer bir *atamasını Reddet* kümesini ekler, bir kullanıcı, Grup veya hizmet sorumlusu erişimi engelleme amacıyla belirli bir kapsamda eylemleri reddet. Bir rol ataması bunları erişim verse bile belirli bir Azure kaynak Eylemler gerçekleştirmeyi atamaları blok kullanıcıların erişimini engelleyin.

Bu makalede nasıl atamaları tanımlanmış reddet.

## <a name="how-deny-assignments-are-created"></a>Atamalar oluşturulan nasıl Reddet

Atamalar oluşturulur ve kaynakları korumak için Azure tarafından yönetilen reddet. Örneğin, Azure şemaları ve Azure yönetilen uygulamalar kullanımını engellemek sistem yönetilen kaynakları korumak için atamaları. Daha fazla bilgi için [yeni kaynaklar ile Azure Blueprint kaynak kilitleri korumak](../governance/blueprints/tutorials/protect-new-resources.md).

## <a name="compare-role-assignments-and-deny-assignments"></a>Rol atamaları karşılaştırın ve atamaları Reddet

Atamalar reddet, ancak bazı farklılıkları da vardır atamaları izleme benzer bir desen reddet.

| Özellik | Rol ataması | Atamasını Reddet |
| --- | --- | --- |
| Erişim verme | :heavy_check_mark: |  |
| Erişimi engelleme |  | :heavy_check_mark: |
| Doğrudan oluşturulabilir | :heavy_check_mark: |  |
| Bir kapsamda geçerlidir | :heavy_check_mark: | :heavy_check_mark: |
| İlkeleri Dışla |  | :heavy_check_mark: |
| Alt kapsamlar için Devralmayı Engelle |  | :heavy_check_mark: |
| Geçerli [Klasik Abonelik Yöneticisi](rbac-and-directory-admin-roles.md) atamaları |  | :heavy_check_mark: |

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
> | `DoNotApplyToChildScopes` | Hayır | Boolean | Reddetme atama alt kapsamlar için geçerli olup olmadığını belirtir. Varsayılan değer false'tur. |
> | `Principals[i].Id` | Evet | String[] | Azure AD sorumlusu nesnesi Reddet atama uygulandığı kimlikleri (kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) dizisi. Boş bir GUID kümesi `00000000-0000-0000-0000-000000000000` tüm ilkeleri temsil etmek için. |
> | `Principals[i].Type` | Hayır | String[] | İlkeleri [i] .id tarafından temsil edilen nesne türleri dizisi. Kümesine `SystemDefined` tüm ilkeleri temsil etmek için. |
> | `ExcludePrincipals[i].Id` | Hayır | String[] | Azure AD sorumlusu nesnesi (kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimlik) Reddet atama değil uygulama kimlikleri dizisi. |
> | `ExcludePrincipals[i].Type` | Hayır | String[] | [İ] ExcludePrincipals .id tarafından temsil edilen nesne türleri dizisi. |
> | `IsSystemProtected` | Hayır | Boolean | Bu atama Azure tarafından oluşturulmuş ve düzenlenemez veya silinmiş Reddet belirtir. Şu anda korumalı sistemi atamalardır tümünü reddet. |

## <a name="the-all-principals-principal"></a>Tüm ilkeleri sorumlusu

İçin destek Reddet atamaları adlı bir sistem tarafından tanımlanan sorumlusu *tüm ilkeleri* tanıtılmıştır. Bu asıl tüm kullanıcıları, grupları, hizmet sorumluları ve yönetilen bir Azure AD dizini kimliklerini temsil eder. Sorumlu Kimliği bir GUID sıfır ise `00000000-0000-0000-0000-000000000000` ve asıl tür `SystemDefined`, sorumlu, tüm ilkeleri temsil eder. Azure PowerShell çıktısında tüm ilkeleri aşağıdaki gibi görünür:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

Tüm ilkeleri birleştirilebilir `ExcludePrincipals` bazı kullanıcılar hariç tüm ilkeleri reddetmek için. Tüm ilkeleri, aşağıdaki kısıtlamalara sahiptir:

- Yalnızca kullanılan `Principals` ve kullanılamaz `ExcludePrincipals`.
- `Principals[i].Type` ayarlanmalıdır `SystemDefined`.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak Azure kaynakları için atamaları izin verilmeyenler listesi](deny-assignments-portal.md)
* [Azure kaynakları için rol tanımları anlama](role-definitions.md)
