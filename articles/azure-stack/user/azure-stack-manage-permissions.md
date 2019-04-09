---
title: Azure stack'teki kullanıcı başına kaynaklarıyla ilgili izinleri yönetme | Microsoft Docs
description: Hizmet Yöneticisi veya Kiracı olarak RBAC izinlerinin yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 58c16b8a102ea27499fc464c209d4ca1c0d4db33
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59264718"
---
# <a name="manage-access-to-resources-with-azure-stack-role-based-access-control"></a>Azure Stack Role-Based erişim denetimi ile kaynaklara erişimi yönetme

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack, rol tabanlı erişim denetimi (RBAC), aynı destekler [erişim yönetimi için güvenlik modeli](https://docs.microsoft.com/azure/role-based-access-control/overview) , Microsoft Azure'ı kullanır. RBAC, kullanıcı, Grup veya uygulama erişimi için abonelik, kaynakları ve hizmetleri yönetmek için kullanabilirsiniz.

## <a name="basics-of-access-management"></a>Erişim yönetimi ile ilgili temel bilgiler

Rol tabanlı erişim denetimi, ortamınızın güvenliğini sağlamak için kullanabileceğiniz ayrıntılı erişim denetimi sağlar. Kullanıcıların ihtiyaç duydukları belirli bir kapsamda bir RBAC rolü atanarak tam izinleri verin. Rol atama kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir. Okuma [Azure portalında rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview) makalede erişim yönetimi hakkında daha ayrıntılı bilgi almak için.

### <a name="built-in-roles"></a>Yerleşik roller

Azure Stack, tüm kaynak türleri için uygulayabileceğiniz üç temel rolüne sahiptir:

* **Sahibi** kaynaklara erişim dahil her şeyi yönetebilir.
* **Katkıda bulunan** kaynaklara erişim dışında her şeyi yönetebilir.
* **Okuyucu** her şeyi görüntüleyebilir ancak değişiklik yapamazsınız.

### <a name="resource-hierarchy-and-inheritance"></a>Kaynak hiyerarşisi ve devralma

Azure Stack aşağıdaki kaynak hiyerarşi vardır:

* Her abonelik bir dizine ait.
* Her kaynak grubu, tek bir aboneliğe ait.
* Her kaynak bir kaynak grubuna aittir.

Bir üst kapsamda verdiğiniz erişim alt kapsamların devralınır. Örneğin:

* Atadığınız **okuyucu** abonelik kapsamında bir Azure AD grubu rol. Bu grubun üyeleri, her kaynak grubu ve kaynak abonelikte görüntüleyebilirsiniz.
* Atadığınız **katkıda bulunan** kaynak grubu kapsamındaki bir uygulama rolü. Uygulama, bu kaynak grubu, ancak diğer kaynak gruplar aboneliği içindeki tüm türlerin kaynakları yönetebilir.

### <a name="assigning-roles"></a>Rol atama

Bir kullanıcı için birden çok rol atayabilirsiniz ve her rol farklı bir kapsam ile ilişkili olabilir. Örneğin:

* Abonelik-1'den ' de TestUser-A okuyucu rolüne atayın.
* TestVM-1'de TestUser-A sahip rolü atayın.

Azure [rol atamaları](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) makale görüntüleme, atama ve rollerin silinmesine hakkında ayrıntılı bilgi sağlar.

## <a name="set-access-permissions-for-a-user"></a>Bir kullanıcı için erişim izinlerini ayarlayın

Aşağıdaki adımları izinlerin bir kullanıcı için nasıl yapılandırılacağını açıklar.

1. Yönetmek istediğiniz kaynağa sahip izinlerine sahip bir hesapla oturum açın.
2. Sol gezinti bölmesinden **Kaynak Grupları**'nı seçin.
3. İzinlerini ayarlamak istediğiniz kaynak grubu adını seçin.
4. Kaynak grubu Gezinti bölmesinde **erişim denetimi (IAM)**. **Rol atamaları** görünümü kaynak grubuna erişimi olan öğeleri listeler. Filtre uygulayabilir ve Grup sonuçları.
5. Üzerinde **erişim denetimi** menü çubuğundan **Ekle**.
6. Üzerinde **izinleri eklemek** bölmesi:

   * Atamak istediğiniz rolü seçin **rol** aşağı açılan listesi.
   * Atamak istediğiniz kaynağı seçin **erişim Ata** aşağı açılan listesi.
   * Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

7. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md)
