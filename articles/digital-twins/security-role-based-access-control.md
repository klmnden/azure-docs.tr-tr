---
title: Understanding Azure dijital İkizlerini rol tabanlı erişim denetimi | Microsoft Docs
description: Rol tabanlı erişim denetimi ile dijital İkizlerini kimlik doğrulaması hakkında bilgi edinin.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: 7a6d8565a0f85b4cb81d9f5f23b04fe6b2edc53e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324327"
---
# <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Azure dijital İkizlerini, belirli veri kaynakları ve uzamsal grafınızı eylemleri için tam erişim denetimi sağlar. Bunu ayrıntılı rol ve adlı izin Yönetimi yapar _rol tabanlı erişim denetimi_. Rol tabanlı erişim denetimi oluşur _rolleri_, ya da izin düzeyini ve _rol atamaları_, veya bir rolü bir kullanıcı veya cihaz ilişkisini.

Rol tabanlı erişim denetimi kullanarak, izni bir kullanıcı, bir cihaz, bir hizmet sorumlusu, kullanıcı tanımlı bir işlev için bir etki alanı veya bir kiracıya ait tüm kullanıcıların verilebilir. Buna ek olarak, erişim düzeyini de ince ayar olabilir.

Rol tabanlı erişim denetimi uzamsal graf devralınan izinleri açısından benzersizdir.

## <a name="what-can-i-do-with-role-based-access-control"></a>Rol tabanlı erişim denetimi ile ne yapabilirim?

Bir geliştirici, rol tabanlı erişim denetimi için kullanabilirsiniz:

* Bir kullanıcının tüm bir yapı için veya yalnızca belirli bir oda veya kat için cihazları yönetme olanağı vermesi.
* Bir yönetici genel erişim tüm uzamsal grafik düğümleri bir grafın tamamında veya yalnızca bir bölümünü graf verin.
* Erişim anahtarlarını dışında grafik bir destek uzmanı okuma erişimi verin.
* Her bir üyenin tüm graf nesneleri bir etki alanı okuma erişimi verin.

## <a name="role-based-access-control-best-practices"></a>Rol tabanlı erişim denetimi en iyi uygulamalar

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roller

### <a name="role-definitions"></a>Rol tanımları

A **rol tanımı** izinleri koleksiyonudur ve bazen adlı bir **rol**. Rol tanımı dahil olmak üzere izin verilen işlemler listeler *oluşturma*, *okuma*, *güncelleştirme*, ve *Sil*. Ayrıca, bu izinler hangi nesne türleri için geçerli belirtir.

Azure dijital İkizlerini aşağıdaki rollerin kullanılabilir:

* **Alan yönetici**: oluşturma, okuma, güncelleştirme ve silme izinleri belirtilen alan ve altındaki tüm düğümleri. Genel izni.
* **Kullanıcı Yöneticisi**: oluşturma, okuma, güncelleştirme ve silme izinleri kullanıcılar ve kullanıcı ile ilgili nesneler için. Alanları için okuma izni.
* **Cihaz Yöneticisi**: oluşturma, okuma, güncelleştirme ve silme izinleri cihazları ve cihaz ile ilgili nesneler için. Alanları için okuma izni.
* **Anahtar Yöneticisi**: oluşturma, okuma, güncelleştirme ve silme izinleri için erişim tuşları. Alanları için okuma izni.
* **Belirteç yönetici**: erişim anahtarları için okuma ve güncelleştirme izni. Alanları için okuma izni.
* **Kullanıcı**: boşluk, algılayıcılar ve kullanıcıların izinlerini okuma, bunlara karşılık gelen dahil olmak üzere ilgili nesneler.
* **Destek Uzmanı**: erişim anahtarlarını dışında her şeyi için okuma izni.
* **Cihaz yükleyici**: cihazlar ve algılayıcılar, bunlara karşılık gelen dahil olmak üzere için okuma ve güncelleştirme izin ile ilgili nesneleri. Alanları için okuma izni.
* **Ağ geçidi cihazı**: oluşturma izni'algılayıcıları için. Cihazlardan ve sensörlerden izinlerini okuma, bunlara karşılık gelen dahil olmak üzere ilgili nesneler.

>[!NOTE]
> *Sistem/roller API sorgulayarak yukarıdaki tam tanımlarında alınabilir.*

### <a name="object-types"></a>Nesne türleri

`ObjectIdType` Bir rolü verilen kimlik türüne başvuruyor. Gelen apart `DeviceId` ve `UserDefinedFunctionId` türleri türlerine, bir Azure Active Directory (Azure AD) nesnenin bir özelliğine karşılık gelir:
  
* `UserId` Türü, bir kullanıcıya rol atar.
* `DeviceId` Türü, bir cihaza bir rolü atar.
* `DomainName` Türü, bir etki alanı adı için bir rolü atar. Her kullanıcı belirtilen etki alanı adına sahip, karşılık gelen rolü erişim hakkına sahip olursunuz.
* `TenantId` Türü bir kiracıya bir rolü atar. Belirtilen ait her bir kullanıcı Azure AD Kiracı kimliği, karşılık gelen rolü erişim haklarına sahip.
* `ServicePrincipalId` Türü atar bir rol için bir hizmet sorumlusu nesne kimliği
* `UserDefinedFunctionId` Türü, bir kullanıcı tanımlı işlev (UDF) için bir rolü atar.

> [!div class="nextstepaction"]
> [Sorgu veya bir kullanıcı nesnesi kimliği](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Nesne Kimliğini almak için bir hizmet sorumlusu](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Nesne Kimliğini almak için Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Rol atamaları

İzinler alıcıya bir rol ataması oluşturma tarafından verilen ve rol atamasını kaldırma tarafından iptal edildi. Bir Azure dijital İkizlerini rol ataması, bir nesne (kullanıcı, Azure AD kiracısı, vb.), rol ve boşluk ilişkilendirir. İzinleri, ardından tüm uzamsal grafiğin altındaki dahil olmak üzere, bu alanına ait tüm nesnelere verilir.

Örneğin, bir kullanıcı rolüne sahip bir rol ataması verilen `DeviceInstaller` uzamsal grafiğin kök düğüm için temsil eden bir yapı. Kullanıcı, ardından okuma ve yalnızca bu düğüm, ancak diğer tüm alt alanları binada cihazları güncelleştirmek mümkün değil.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).
