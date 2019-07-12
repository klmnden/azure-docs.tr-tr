---
title: Azure Deniz için önerilen güvenlik uygulamaları
description: Azure'ı kullanarak temsilci kaynak yönetimi, güvenlik göz önünde bulundurun ve erişim denetimi önemlidir.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 843b965e6ea74a7c11dc11459ff5d30ddbe5c987
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809872"
---
# <a name="recommended-security-practices"></a>Önerilen güvenlik uygulamaları

Azure'ı kullanarak temsilci kaynak yönetimi, güvenlik göz önünde bulundurun ve erişim denetimi önemlidir. Kiracınızın güvenliğini sağlamak için adımlar atmak güncelleştirdiğinden kiracınızdaki kullanıcılar müşteri aboneliği ve kaynak gruplarını, doğrudan erişim gerekir. Yalnızca etkin bir şekilde müşterilerinizin kaynaklarını yönetmek için gerekli olan erişime izin ver emin olmak istersiniz. Bu konuda, bunu yapmanıza yardımcı olacak öneriler sağlanır.

## <a name="require-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması gerektir

[Azure multi-Factor Authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (iki aşamalı doğrulama olarak da bilinir), saldırganlar birden çok kimlik doğrulama adımı gerektirerek bir hesaba erişim elde etmesini önlemeye yardımcı olur. Müşteri kaynaklara erişimi olacak kullanıcılar da dahil olmak üzere hizmet sağlayıcısı, kiracınızdaki tüm kullanıcıları için çok faktörlü kimlik doğrulaması istemeniz gerekir.

Azure multi-Factor Authentication, kiracıda uygulamak için müşterileriniz sorun öneririz.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>En düşük öncelik ilkesini kullanarak gruplar için izinleri atayın

Yönetimini kolaylaştırmak için Azure AD kullanıcı gruplarını, müşterilerinizin kaynaklarını yönetmek için gereken her rol için kullanmanızı öneririz. Bu, doğrudan o kullanıcıya izin atamak yerine Grup gerektiği gibi bireysel kullanıcı eklemek veya kaldırmak sağlar.

İzni yapınızı oluştururken, böylece yalnızca kullanıcıların yanlışlıkla hata olasılığını azaltmaya yardımcı olur, işi tamamlamak için gerekli izinlere sahip en az ayrıcalık ilkesini uygulayın emin olun.

Örneğin, şunun gibi bir yapı kullanmak isteyebilirsiniz:

|Grup adı  |Type  |Principalıd  |Rol tanımı  |Rol tanımı kimliği  |
|---------|---------|---------|---------|---------|
|Mimarları     |Kullanıcı grubu         |\<Principalıd\>         |Katılımcı         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Değerlendirme     |Kullanıcı grubu         |\<Principalıd\>         |Okuyucu         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|VM uzmanlarıyla     |Kullanıcı grubu         |\<Principalıd\>         |VM katkıda bulunanı         |9980e02c-c2be-4d73-94e8-173b1dc7cf3c  |
|Otomasyon     |Hizmet asıl adı (SPN)         |\<Principalıd\>         |Katılımcı         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Bu grupları oluşturduktan sonra gerektiğinde kullanıcılara atayabilirsiniz. Yalnızca gerçekten erişimi gereken kullanıcılar ekleyin. Grup üyeliği düzenli olarak gözden geçirin ve artık uygun veya eklemek gerekli olan tüm kullanıcıları kaldırma emin olun.

Göz önünde bulundurun kullanırken, [ortak yönetilen hizmet teklifi aracılığıyla müşteri](../how-to/publish-managed-services-offers.md), tüm Grup (veya kullanıcı veya hizmet sorumlusu) dahil planı satın her müşteri için aynı izinlere sahip. Her müşteri ile çalışan için farklı gruplara atamak için Azure Resource Manager şablonlarını kullanarak tek tek her müşteri veya müşteri için özel olan ayrı, özel bir plan yayımlamak gerekir. Örneğin, çok olan bir genel planda yayımlayabilirsiniz sınırlı erişim, ardından kaynaklarını gerektiği gibi ek erişim verme özelleştirilmiş bir Azure kaynak şablonu ile ek erişim müşteriye doğrudan ekleme çalışmak.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure çok faktörlü kimlik doğrulaması dağıtmak](../../active-directory/authentication/howto-mfa-getstarted.md).
- Hakkında bilgi edinin [kiracılar arası yönetim deneyimleri](cross-tenant-management-experience.md).
