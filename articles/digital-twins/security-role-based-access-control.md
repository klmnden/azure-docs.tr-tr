---
title: Azure dijital İkizlerini rol tabanlı erişim denetimini anlama | Microsoft Docs
description: Rol tabanlı erişim denetimi ile dijital İkizlerini kimlik doğrulaması hakkında bilgi edinin.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: lyrana
ms.openlocfilehash: bfc73a71a0ccda5c135e6a740d6f63bd37522a9b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787624"
---
# <a name="role-based-access-control-in-azure-digital-twins"></a>Azure dijital İkizlerini rol tabanlı erişim denetimi

Azure dijital İkizlerini, belirli veri kaynakları ve uzamsal grafınızı eylemleri için tam erişim denetimi sağlar. Bunu ayrıntılı rolü ve rol tabanlı erişim denetimi (RBAC) olarak adlandırılan izin Yönetimi yapar. RBAC oluşur _rolleri_ ve _rol atamaları_. Rol izinlerinin düzeyini belirleyin. Rol atamaları bir rol, bir kullanıcı veya cihaz ile ilişkilendirin.

RBAC kullanarak, için izin verilebilir:

- Bir kullanıcı.
- Bir cihaz.
- Bir hizmet sorumlusu.
- Bir kullanıcı tanımlı işlev.
- Bir etki alanına ait tüm kullanıcılar.
- Bir kiracı.

Erişim düzeyini de ince ayar olabilir.

Uzamsal grafiğin devralınan izinleri RBAC benzersizdir.

## <a name="what-can-i-do-with-rbac"></a>RBAC ile ne yapabilirim?

Bir geliştirici için RBAC kullanabilirsiniz:

- Bir kullanıcının tüm bir yapı için veya yalnızca belirli bir oda veya kat için cihazları yönetme olanağı vermesi.
- Bir yönetici genel erişim tüm uzamsal grafik düğümleri bir grafın tamamında veya yalnızca bir bölümünü graf verin.
- Erişim anahtarlarını dışında grafik bir destek uzmanı okuma erişimi verin.
- Her bir üyenin tüm graf nesneleri bir etki alanı okuma erişimi verin.

## <a name="rbac-best-practices"></a>RBAC en iyi uygulamalar

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roller

### <a name="role-definitions"></a>Rol tanımları

Rol tanımının, izinleri ve rol oluşturan diğer özniteliklerini oluşan bir koleksiyondur. Rol tanımının dahil izin verilen işlemler listeler *Oluştur*, *okuma*, *güncelleştirme*, ve *Sil* herhangi ile nesnesinin Rol gerçekleştirebilir. Nesne türleri izinleri uygulamak belirtir.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

>[!NOTE]
> Önceki rolleri için tam tanımları almak için sistem/roller API sorgulayın.
> Okuyarak daha fazla bilgi edinin [oluşturma ve rol atamalarını yönetme](./security-create-manage-role-assignments.md#all).

### <a name="object-identifier-types"></a>Nesne tanımlayıcısı türleri

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

>[!TIP]
> Hizmet sorumlunuzu okuyarak izinleri öğrenin [oluşturma ve rol atamalarını yönetme](./security-create-manage-role-assignments.md#grant).

Aşağıdaki başvuru belgeleri makaleler açıklar:

- Nasıl yapılır [sorgu veya bir kullanıcı nesnesi kimliği](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0).
- Nasıl yapılır [nesne Kimliğini almak için bir hizmet sorumlusu](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal).
- Nasıl yapılır [nesne Kimliğini almak için Azure AD kiracısı](../active-directory/develop/quickstart-create-new-tenant.md).

## <a name="role-assignments"></a>Rol atamaları

Azure dijital İkizlerini rol atama gibi bir kullanıcı ya da Azure AD kiracısı, bir nesne bir rolü ve bir alan ile ilişkilendirir. Bu alana ait tüm nesneleri için izinleri verilir. Tüm uzamsal grafiğin altındaki alanı içerir.

Örneğin, bir kullanıcı rolüne sahip bir rol ataması verilen `DeviceInstaller` uzamsal grafiğin kök düğüm için temsil eden bir yapı. Kullanıcı daha sonra okuyabilir ve cihazlar için düğüm ve diğer tüm alt alanları binada güncelleştirin.

Alıcıya izin vermek için bir rol ataması oluşturun. İzinleri iptal etmek için rol atamasını kaldırın.

>[!IMPORTANT]
> Rol atamaları hakkında daha fazla bilgi edinmek [oluşturma ve rol atamalarını yönetme](./security-create-manage-role-assignments.md).

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla hakkında oluşturup Azure dijital İkizlerini rol atamalarını yönetmek öğrenmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).
