---
title: Azure dijital İkizlerini rol tabanlı erişim denetimini anlama | Microsoft Docs
description: Rol tabanlı erişim denetimi ile dijital İkizlerini kimlik doğrulaması hakkında bilgi edinin.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: b9ccdb9030a24520be8f24f757c279241f3a07e1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014797"
---
# <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

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

* Bir kullanıcının tüm bir yapı için veya yalnızca belirli bir oda veya kat için cihazları yönetme olanağı vermesi.
* Bir yönetici genel erişim tüm uzamsal grafik düğümleri bir grafın tamamında veya yalnızca bir bölümünü graf verin.
* Erişim anahtarlarını dışında grafik bir destek uzmanı okuma erişimi verin.
* Her bir üyenin tüm graf nesneleri bir etki alanı okuma erişimi verin.

## <a name="rbac-best-practices"></a>RBAC en iyi uygulamalar

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roller

### <a name="role-definitions"></a>Rol tanımları

Rol tanımı izinleri koleksiyonudur ve bazen bir rol olarak da adlandırılır. Rol tanımı listeleri oluşturma dahil izin verilen işlemleri, okuma, güncelleştirme ve silme. Ayrıca, bu izinler hangi nesne türleri için geçerli belirtir.

Azure dijital İkizlerini aşağıdaki rollerin kullanılabilir:

* **Alan yönetici**: oluşturma, okuma, güncelleştirme ve silme izni belirtilen alan ve altındaki tüm düğümleri. Genel izni.
* **Kullanıcı Yöneticisi**: oluşturma, okuma, güncelleştirme ve silme izni kullanıcılar ve kullanıcı ile ilgili nesneler için. Alanları için okuma izni.
* **Cihaz Yöneticisi**: oluşturma, okuma, güncelleştirme ve silme izni cihazları ve cihaz ile ilgili nesneler için. Alanları için okuma izni.
* **Anahtar Yöneticisi**: oluşturma, okuma, güncelleştirme ve silme izni için erişim tuşları. Alanları için okuma izni.
* **Belirteç yönetici**: erişim anahtarları için okuma ve güncelleştirme izni. Alanları için okuma izni.
* **Kullanıcı**: boşluk, algılayıcılar ve kullanıcıların izinlerini okuma, bunlara karşılık gelen içeren ilgili nesneler.
* **Destek Uzmanı**: erişim anahtarlarını dışında her şeyi için okuma izni.
* **Cihaz yükleyici**: karşılık gelen içeren okuma ve güncelleştirme izniniz cihazlar ve algılayıcılar için ilgili nesneleri. Alanları için okuma izni.
* **Ağ geçidi cihazı**: oluşturma izni'algılayıcıları için. Cihazlardan ve sensörlerden izinlerini okuma, bunlara karşılık gelen içeren ilgili nesneler.

>[!NOTE]
> Önceki rolleri için tam tanımları almak için sistem/roller API sorgulayın.

### <a name="object-types"></a>Nesne türleri

`ObjectIdType` Rol verdiği kimlik türüne başvuruyor. Gelen apart `DeviceId` ve `UserDefinedFunctionId` türleri türlerine, bir Azure Active Directory (Azure AD) nesnenin bir özelliğine karşılık gelir:
  
* `UserId` Türü, bir kullanıcıya rol atar.
* `DeviceId` Türü, bir cihaza bir rolü atar.
* `DomainName` Türü, bir etki alanı adı için bir rolü atar. Her kullanıcı belirtilen etki alanı adına karşılık gelen rolü erişim haklarına sahiptir.
* `TenantId` Türü bir kiracıya bir rolü atar. Belirtilen ait her kullanıcının Azure AD Kiracı kimliği, karşılık gelen rolü erişim haklarına sahip.
* `ServicePrincipalId` Türü atar bir rol için bir hizmet sorumlusu nesne kimliği
* `UserDefinedFunctionId` Türü kullanıcı tanımlı bir işlev için (UDF) bir rolü atar.

> [!div class="nextstepaction"]
> [Sorgu veya bir kullanıcı nesnesi kimliği](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Nesne Kimliğini almak için bir hizmet sorumlusu](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Nesne Kimliğini almak için Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Rol atamaları

Alıcıya izin vermek için bir rol ataması oluşturun. İzinleri iptal etmek için rol atamasını kaldırın. Azure dijital İkizlerini rol atama gibi bir kullanıcı ya da Azure AD kiracısı, bir nesne bir rolü ve bir alan ile ilişkilendirir. Bu alana ait tüm nesneleri için izinleri verilir. Tüm uzamsal grafiğin altındaki alanı içerir.

Örneğin, bir kullanıcı rolüne sahip bir rol ataması verilen `DeviceInstaller` uzamsal grafiğin kök düğüm için temsil eden bir yapı. Kullanıcı daha sonra okuyabilir ve cihazlar için düğüm ve diğer tüm alt alanları binada güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini güvenliği hakkında bilgi edinmek için [oluşturma ve rol atamalarını yönetmek](./security-create-manage-role-assignments.md).
