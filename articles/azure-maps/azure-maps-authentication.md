---
title: Azure Haritalar ile kimlik doğrulaması | Microsoft Docs
description: Azure haritalar Hizmetleri kullanarak kimlik doğrulaması.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b97c303433eb8fadcda51257d37447f052ce4a3b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119282"
---
# <a name="authentication-with-azure-maps"></a>Azure Haritalar ile kimlik doğrulaması

Azure haritalar isteklerinin kimliğini doğrulamak için iki şekilde destekler. Azure haritalar için gönderilen her isteği yetkilendirmek için anahtar veya Azure Active Directory (Azure AD) teklif farklı yöntemleri paylaşılan. Bu makalenin amacı, kimlik doğrulaması Uygulama Kılavuzu yardımcı olmak için her iki kimlik doğrulama yöntemleri açıklayan sağlamaktır.

## <a name="shared-key-authentication"></a>Paylaşılan Anahtar Kimlik Doğrulaması

Azure haritalar için her isteği ile Azure haritalar oluşturulan hesap anahtarlarını geçirme paylaşılan anahtar kimlik doğrulaması kullanır.  Azure haritalar hesabınız oluşturulduğunda iki anahtar oluşturulur.  Azure haritalar hizmetleri her isteğin URL'sine bir parametre olarak eklenecek abonelik anahtarı gerektirir.

> [!Tip]
> Anahtarlarınızı düzenli olarak yeniden oluşturmanızı öneririz. Bir anahtar yeniden oluştururken diğeri ile bağlantı koruyabilir, böylece iki anahtar sağlanır. Anahtarlarınızı yeniden oluşturduğunuzda Yeni anahtarları kullanmak için bu hesaba herhangi bir uygulamayı güncelleştirmeniz gerekir.

Anahtarlarınızı görüntülemek için bkz: [kimlik doğrulama ayrıntıları](https://aka.ms/amauthdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) ile kimlik doğrulaması

Azure haritalar şimdi tekliflerini [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) isteklerinin Azure haritalar Hizmetleri kimlik doğrulaması için tümleştirme.  Azure AD sağlar tanımlamak tabanlı kimlik doğrulaması dahil olmak üzere [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) Azure haritalar kaynaklarına kullanıcı veya uygulama düzeyi erişimi vermek için. Bu makalenin amacı, Azure haritalar'ı Azure AD tümleştirme bileşenleri ve kavramları anlamanıza yardımcı olmaktır.

## <a name="authentication-with-oauth-access-tokens"></a>OAuth erişim belirteçleri ile kimlik doğrulaması

Azure haritalar'ı kabul **OAuth 2.0** erişim belirteçlerini Azure haritalar hesabı içeren Azure aboneliği ile ilişkili Azure AD kiracıları için.  Azure haritalar için belirteçleri kabul eder:

* Azure AD kullanıcıları 
* Kullanıcıların yönetici temsilcisi izinlerini kullanarak üçüncü taraf uygulamalar
* Azure kaynakları için yönetilen kimlikler

Azure haritalar'ı oluşturan bir `unique identifier (client ID)` her bir Azure haritalar hesabı.  İstemci kimliği ek parametreler ile birleştirildiğinde, aşağıdaki değeri belirterek Azure AD'den belirteçleri istenebilir:

```
https://login.microsoftonline.com
```
Azure haritalar için belirteçler istemek ve Azure AD'yi yapılandırma hakkında daha fazla bilgi için bkz. [nasıl yönetme kimlik doğrulaması](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Azure AD'den isteyen belirteçleri hakkında genel bilgi için bkz: [Azure Active Directory kimlik doğrulaması temellerini](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="requesting-azure-map-resources-with-oauth-tokens"></a>OAuth belirteçlerini kaynaklarla Azure harita isteme

Bir belirteç, Azure AD'den alınan sonra aşağıdaki iki gerekli istek üst bilgileri kümesi ile Azure haritalar için bir istek gönderilebilir:

| İstek üstbilgisi    |    Değer    |
|:------------------|:------------|
| x-ms-istemci kimliği    | 30d7cc….9f55|
| Yetkilendirme     | Taşıyıcı eyJ0e HNIVN |

> [!Note]
> `x-ms-client-id` Azure haritalar kimlik doğrulaması sayfasında görüntülenen GUID Azure haritalar hesabı bağlı

OAuth belirteci kullanarak Azure haritalar rota isteği örnek aşağıda verilmiştir:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

İstemci Kimliğiniz görüntülemek için bkz: [kimlik doğrulama ayrıntıları](https://aka.ms/amauthdetails).

## <a name="control-access-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile erişimi denetleme

Temel bir özelliği, Azure AD RBAC aracılığıyla güvenli kaynaklara erişim denetliyor. Azure AD kiracınızda Azure haritalar'ı Azure AD uygulamasını kaydetme ve Azure haritalar hesabınız oluşturulduktan sonra artık bir kullanıcı, uygulama veya Azure harita hesabı portal sayfasının içinden Azure kaynağı için RBAC yapılandırmak kullanabilirsiniz. 

Azure haritalar şu anda kişiye ait okuma erişimi destekleyen Azure AD kullanıcıları, uygulamaları veya Azure kaynakları için yönetilen kimlikleri aracılığıyla Azure Hizmetleri.

![Kavram](./media/azure-maps-authentication/concept.png)

RBAC ayarlarınızı görüntülemek için bkz: [nasıl için RBAC Azure haritalar için yapılandırma](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Azure kaynakları ve Azure haritalar için yönetilen kimlik

[Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) Azure hizmetleri sağlama (Azure App service, Azure işlevleri, sanal makineler, vb.) ile Azure haritalar hizmetlerine erişim için yetkili bir otomatik olarak yönetilen bir kimlik.  

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanın Azure AD ile kimlik doğrulama hakkında daha fazla bilgi edinmek ve Azure haritalar [nasıl yönetme kimlik doğrulaması](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Azure haritalar, harita denetimi ve Azure AD kimlik doğrulama hakkında daha fazla bilgi için bkz. [Azure AD ve Azure haritalar harita denetimi](https://aka.ms/amaadmc).