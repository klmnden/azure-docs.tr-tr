---
title: En iyi güvenlik uygulamaları, Azure dijital İkizlerini anlama | Microsoft Docs
description: Azure dijital İkizlerini en iyi güvenlik uygulamaları
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: adgera
ms.openlocfilehash: 28eb8b5dc0f75b5e031070803d35c8a1ceb1f000
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364355"
---
# <a name="security-best-practices"></a>En iyi güvenlik uygulamaları

Azure dijital İkizlerini güvenlik belirli kaynaklara ve işlemlere, IOT Graph'te tam erişim sağlar. Bunu rol tabanlı erişim denetimi adlı ayrıntılı rol ve izin yönetiminden yapar.

Azure dijital İkizlerini, Azure Active Directory de dahil olmak üzere Azure IOT üzerinde mevcut olan diğer güvenlik özellikleri de yararlanır. Bu nedenle, Azure dijital İkizlerini uygulamanızı birçok aynı kullanarak yapılandırılmasını [Azure IOT güvenlik uygulamaları](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) şu anda önerilir.

Bu makalede izlemek için anahtar en iyi yöntemler özetlenmiştir.

> [!IMPORTANT]
> IOT alanınızın düzeyde güvenlik sağlamak için (cihaz satıcılarınıza dahil) ek güvenlik kaynakları gözden geçirin.

## <a name="iot-security-best-practices"></a>IOT güvenlik en iyi uygulamalar

Güvenle IOT cihazlarınızı güvenli hale getirmek için bazı önemli yöntemler şunlardır:

> [!div class="checklist"]
> * IOT alanınıza artıklığının şekilde bağlı her cihazı güvenli hale getirin.
> * Her cihaz, sensör ve IOT alanınız içinde kişi rolünü sınırlayın. İhlal edilmesi durumunda, etkiyi en aza indirilir.
> * Aygıt IP adresi filtresi olası kullanın.
> * Performansı artırmak için g/ç ve cihaz bant genişliğini sınırlayın. Hız sınırlama, hizmet reddi saldırılarını önleyerek güvenliğini artırabilirsiniz.

Güvenle IOT alanına güvenli hale getirmek için bazı önemli yöntemler şunlardır:

> [!div class="checklist"]
> * Kaydedilmiş, depolanmış veya kalıcı verileri şifreleyin.
> * Parola veya anahtarlarını düzenli aralıklarla değiştirildi veya yenilenmesi gerekir.
> * Dikkatli bir şekilde role göre erişimi ve izinlerini kısıtlamak (rol tabanlı erişim denetimi en iyi uygulamalar aşağıda bakın).
> * Güçlü şifreleme kullanın. İki öğeli kimlik doğrulama ile güvenli protokolleri, uzun bir parola gerektirme anlamına gelir.

IOT kaynakları aykırı değerleri, tehditleri veya normal işlem aralığı dışında kalan kaynak parametreleri izlemek üzere izleme, Azure analiz yönetilir.

> [!NOTE]
> Olay hakkında daha fazla bilgi işlem ve izleme, bakın makalemizi [olay > yönlendirme](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Azure Active Directory en iyi uygulamaları

Azure dijital İkizlerini, kullanıcıların kimliklerini doğrulamak ve uygulamaları korumak için Azure Active Directory kullanır. Azure Active Directory kimlik doğrulaması için OAuth 2.0 veya Openıd Connect gibi endüstri standardı protokoller temel alınarak bunlar tüm modern mimarileri çeşitli destekler. IOT alanınız için Azure Active Directory güvenliğini sağlamak için birkaç önemli yöntemler şunlardır:

> [!div class="checklist"]
> * Azure Active Directory Uygulama gizli dizileri ve anahtarları güvenli bir konuma gibi Store [Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Güvenilen bir tarafından yayınlanan bir sertifika kullanmak [sertifika yetkililerini](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) kimliğini doğrulamak için uygulama gizli anahtarlarının yerine.
> * OAuth 2.0 belirteç için erişim kapsamını sınırlandırın.
> * Bir belirtecin geçerli olduğu süreyi doğrulayın ve bir belirteç olup geçerli kalır.
> * Uygun uzunlukta belirteçleri için geçerli olan süreyi ayarlayın.
> * Süresi dolan yenileyin.

## <a name="role-based-access-control-best-practices"></a>Rol tabanlı erişim denetimi en iyi uygulamalar

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT en iyi uygulamalar hakkında daha fazla bilgi edinmek için [IOT güvenlik en iyi](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Rol tabanlı erişim denetimi hakkında bilgi edinmek için [rol tabanlı erişim denetimi](./security-role-based-access-control.md).

Kimlik doğrulaması için okuma [API'leri ile kimlik doğrulaması](./security-authenticating-apis.md).
