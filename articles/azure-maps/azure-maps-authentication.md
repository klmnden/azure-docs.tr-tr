---
title: Azure Haritalar ile kimlik doğrulaması | Microsoft Docs
description: Azure haritalar Hizmetleri kullanarak kimlik doğrulaması.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 22aba19e16e4349a5b495b307c9906f7ded5a636
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393671"
---
# <a name="authentication-with-azure-maps"></a>Azure Haritalar ile kimlik doğrulaması

Azure haritalar isteklerinde kimlik doğrulamak için iki yol destekler: Paylaşılan anahtar ile Azure Active Directory (Azure AD). Bu makalede, uygulamanızı Kılavuzu yardımcı olmak için bu kimlik doğrulama yöntemleri açıklanmaktadır.

## <a name="shared-key-authentication"></a>Paylaşılan anahtar kimlik doğrulaması

Paylaşılan anahtar kimlik doğrulaması, bir Azure haritalar hesabı Azure haritalar için her bir istekle tarafından oluşturulan anahtarları geçirir.  Azure haritalar hesabınız oluşturulduğunda iki anahtar oluşturulur. Azure haritalar hizmetleri her istek için abonelik anahtarını URL'sine bir parametre olarak eklenmesi gerekir.

> [!Tip]
> Anahtarlarınızı düzenli olarak yeniden oluşturmanızı öneririz. Yeniden oluştururken diğeri ile bir anahtar bağlantı koruyabilir, böylece iki anahtar ile sunulur. Anahtarlarınızı yeniden oluşturduğunuzda Yeni anahtarları kullanmak için hesabına erişen tüm uygulamaları güncelleştirmeniz gerekiyor.

Anahtarlarınızı görüntüleme hakkında daha fazla bilgi için bkz: [kimlik doğrulama ayrıntıları görüntüle](https://aka.ms/amauthdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) ile kimlik doğrulaması

Azure haritalar şimdi tekliflerini [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) isteklerinin Azure haritalar Hizmetleri kimlik doğrulaması için tümleştirme. Azure AD kimlik tabanlı kimlik doğrulaması sağlar dahil olmak üzere [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview)kullanıcı düzeyinde veya uygulama düzeyinde Azure haritalar kaynaklara erişim için. Aşağıdaki bölümlerde, Azure AD ile Azure haritalar tümleştirme bileşenlerinin ve kavramları anlamanıza yardımcı olabilir.

## <a name="authentication-with-oauth-access-tokens"></a>OAuth erişim belirteçleri ile kimlik doğrulaması

Azure haritalar'ı kabul **OAuth 2.0** erişim belirteçlerini Azure haritalar hesabı içeren bir Azure aboneliği ile ilişkili Azure AD kiracıları için. Azure haritalar için belirteçleri kabul eder:

* Azure AD kullanıcıları. 
* Kullanıcı tarafından temsilci izinleri kullanan iş ortağı uygulamalar.
* Azure kaynakları için yönetilen kimlikleri.

Azure haritalar'ı oluşturan bir *benzersiz tanımlayıcı (istemci kimliği)* her bir Azure haritalar hesabı. Bu istemci kimliği ile ek parametreler birleştirdiğinizde, aşağıdaki değeri belirterek Azure AD'den belirteçleri isteyebilirsiniz:

```
https://login.microsoftonline.com
```
Azure haritalar için belirteçler istemek ve Azure AD'yi yapılandırma hakkında daha fazla bilgi için bkz. [Yönet kimlik doğrulaması Azure haritalar](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Azure AD'den isteyen belirteçleri hakkında genel bilgi için bkz: [kimlik doğrulaması nedir?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>OAuth belirteçlerini kaynaklarla Azure eşlemesi iste

Azure AD'den bir belirteç alındığında, aşağıdaki iki gerekli istek üst bilgileri kümesi ile Azure haritalar için bir istek gönderilebilir:

| İstek üstbilgisi    |    Değer    |
|:------------------|:------------|
| x-ms-istemci kimliği    | 30d7cc….9f55|
| Yetkilendirme     | Taşıyıcı eyJ0e HNIVN |

> [!Note]
> `x-ms-client-id` Azure haritalar kimlik doğrulaması sayfasında görüntülenen Azure haritalar hesabı tabanlı GUID'dir.

Bir OAuth belirteci kullanan bir Azure haritalar rota isteğinin bir örneği aşağıda verilmiştir:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

İstemci Kimliğiniz görüntüleme hakkında daha fazla bilgi için bkz: [kimlik doğrulama ayrıntıları görüntüle](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>RBAC ile erişimi denetleme

RBAC kullanarak güvenlikli kaynaklara erişimi denetlemek, azure AD olanak tanır. Azure haritalar hesabınızı oluşturun ve Azure AD kiracınızda Azure haritalar'ı Azure AD uygulamanızı kaydetmeniz sonra bir kullanıcı, uygulama veya Azure haritalar hesabı portal sayfasında Azure kaynağı için RBAC ayarlayabilirsiniz.

Azure haritalar destekler okuma erişim denetimi için tek tek Azure AD kullanıcıları, uygulamaları ve Azure kaynakları için yönetilen kimlikleri aracılığıyla Azure Hizmetleri.

![Azure haritalar verileri Okuyucu (Önizleme)](./media/azure-maps-authentication/concept.png)

RBAC ayarlarınızı görüntüleme hakkında daha fazla bilgi için bkz: [Azure haritalar için RBAC yapılandırma](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Azure kaynaklarını ve Azure haritalar için yönetilen kimlik

[Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) Azure haritalar hizmetlerine erişim için yetkilendirilebilir otomatik olarak yönetilen bir kimlikle Azure Hizmetleri (Azure App Service, Azure işlevleri, Azure sanal makineler ve benzerleri) sağlayın.  

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanın Azure AD ile kimlik doğrulama hakkında daha fazla bilgi edinmek ve Azure haritalar [Azure haritalar Yönet kimlik doğrulaması](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Azure haritalar harita denetimi ve Azure AD kimlik doğrulama hakkında daha fazla bilgi için bkz. [Azure haritalar harita denetimini kullanma](https://aka.ms/amaadmc).