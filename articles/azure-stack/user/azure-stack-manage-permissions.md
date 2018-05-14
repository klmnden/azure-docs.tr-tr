---
title: Azure yığınında kullanıcı başına kaynaklarına izinleri yönetme | Microsoft Docs
description: Hizmet Yöneticisi veya Kiracı RBAC izinleri yönetmeyi öğrenin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 4f9354426ba584b26213f8a104c14122a831a453
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="manage-access-to-resources-with-azure-stack-role-based-access-control"></a>Azure Stack Role-Based erişim denetimi ile kaynaklara erişimi yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığını destekler rol tabanlı erişim denetimi (RBAC), aynı [erişim yönetimi için güvenlik modeli](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) , Microsoft Azure kullanır. Kullanıcı, Grup veya uygulama erişimi abonelikler, kaynakları ve hizmetleri yönetmek için RBAC kullanabilirsiniz.

## <a name="basics-of-access-management"></a>Erişim Yönetimi temelleri

Rol tabanlı erişim denetimi ortamınızın güvenliğini sağlamak için kullanabileceğiniz ayrıntılı erişim denetimi sağlar. Kullanıcılara ihtiyaç duydukları belirli bir kapsamda RBAC rolü atanarak tam izinleri verin. Rol atama kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir. Okuma [Azure portalında rol tabanlı erişim denetimi](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview) erişim yönetimi hakkında daha ayrıntılı bilgi almak için makale.

### <a name="built-in-roles"></a>Yerleşik roller

Azure yığını, tüm kaynak türleri için uygulayabileceğiniz üç temel rol vardır:

* **Sahibi** kaynaklarına erişim de dahil olmak üzere her şeyi yönetebilir.
* **Katkıda bulunan** kaynaklara erişim dışında her şeyi yönetebilir.
* **Okuyucu** her şeyi görüntüleyebilir ancak değişiklik yapamazsınız.

### <a name="resource-hierarchy-and-inheritance"></a>Kaynak hiyerarşisi ve devralma

Azure yığın aşağıdaki kaynak hiyerarşi vardır:

* Her abonelik için bir dizin aittir.
* Her kaynak grubu bir aboneliğe ait.
* Her kaynak bir kaynak grubuna ait.

Bir üst kapsamda vermek erişim alt kapsamların devralınır. Örneğin:

* Abonelik kapsamında bir Azure AD grubundaki okuyucu rolüne atayın. Bu grubun üyeleri, her kaynak grubu ve kaynak abonelikte görüntüleyebilirsiniz.
* Kaynak grubu kapsamındaki bir uygulama katılımcı rolü atar. Uygulama bu kaynak grubu, ancak diğer kaynak gruplarının değil Abonelikteki tüm türlerinin kaynakları yönetebilir.

### <a name="assigning-roles"></a>rol atama

Bir kullanıcıya birden çok rol atayabilirsiniz ve her rol farklı bir kapsam ile ilişkili olabilir. Örneğin:

* Abonelik-1'den TestUser-A okuyucu rolüne atayın.
* TestVM-1'den TestUser-A sahip rolü atayın.

Azure [rol atamalarını](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal) makale görüntüleme, atama ve rollerini silme hakkında ayrıntılı bilgi sağlar.

### <a name="resource-hierarchy-and-inheritance"></a>Kaynak hiyerarşisi ve devralma

Azure yığın aşağıdaki kaynak hiyerarşi vardır:

* Her abonelik için bir dizin aittir.
* Her kaynak grubu bir aboneliğe ait.
* Her kaynak bir kaynak grubuna ait.

Bir üst kapsamda vermek erişim alt kapsamların devralınır. Örneğin:

* Atadığınız **okuyucu** abonelik kapsamında bir Azure AD grubuna rol. Bu grubun üyeleri, her kaynak grubu ve kaynak abonelikte görüntüleyebilirsiniz.
* Atadığınız **katkıda bulunan** kaynak grup kapsamındaki bir uygulama rolü. Uygulama bu kaynak grubu, ancak diğer kaynak gruplarının değil Abonelikteki tüm türlerinin kaynakları yönetebilir.

### <a name="assigning-roles"></a>rol atama

Bir kullanıcıya birden çok rol atayabilirsiniz ve her rol farklı bir kapsam ile ilişkili olabilir. Örneğin:

* Abonelik-1'den TestUser-A okuyucu rolüne atayın.
* TestVM-1'den TestUser-A sahip rolü atayın.

Azure [rol atamalarını](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal) makale görüntüleme, atama ve rollerini silme hakkında ayrıntılı bilgi sağlar.

## <a name="set-access-permissions-for-a-user"></a>Bir kullanıcı için erişim izinlerini ayarlayın

Aşağıdaki adımlar, bir kullanıcı için izinlerin nasıl yapılandırılacağını açıklar.

1. Yönetmek istediğiniz kaynak sahibi izinlerine sahip bir hesapla oturum açın.
2. Sol gezinti bölmesinde seçin **kaynak grupları**.
3. İçin izinleri ayarlamak istediğiniz kaynak grubunun adını seçin.
4. Kaynak grubu Gezinti bölmesinde seçin **erişim denetimi (IAM)**. **Erişim denetimi** görünüm kaynak grubu erişimi öğeleri listeler. Bu sonuçları filtrelemek ve izinleri eklemek veya kaldırmak için bir menü çubuğu kullanın.
5. Üzerinde **erişim denetimi** menü çubuğunda, seçin **+ Ekle**.
6. Üzerinde **izinleri eklemek**:

   * Atamak istediğiniz rolü seçin **rol** aşağı açılan liste.
   * Atamak istediğiniz kaynağı seçin **atamak için erişim** aşağı açılan liste.
   * Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

7. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md)