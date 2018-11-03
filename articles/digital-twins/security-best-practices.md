---
title: En iyi güvenlik uygulamaları, Azure dijital İkizlerini anlama | Microsoft Docs
description: Azure dijital İkizlerini en iyi güvenlik uygulamaları
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: 6ca01523744dbce15f8fdb3bbe2d5a9b44510f3f
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959486"
---
# <a name="security-best-practices"></a>En iyi güvenlik uygulamaları

Azure dijital İkizlerini güvenlik belirli kaynaklara ve işlemlere, IOT Graph'te tam erişim sağlar. Bunu ayrıntılı rolü ve rol tabanlı erişim denetimi olarak adlandırılan izin Yönetimi yapar.

Azure dijital İkizlerini, Azure Active Directory (Azure AD) dahil Azure IOT üzerinde mevcut olan diğer güvenlik özellikleri de kullanır. Bu nedenle, Azure dijital İkizlerini uygulamanızı birçok aynı kullanarak yapılandırılmasını [Azure IOT güvenlik uygulamaları](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) şu anda önerilir.

Bu makalede izlemek için anahtar en iyi yöntemler özetlenmiştir.

> [!IMPORTANT]
> IOT alanınızın düzeyde güvenlik sağlamak için ek güvenlik kaynakları gözden geçirin. Cihaz satıcılarınıza eklediğinizden emin olun.

## <a name="iot-security-best-practices"></a>IoT güvenliği için en iyi uygulamalar

Güvenle IOT cihazlarınızı güvenli hale getirmek için bazı önemli yöntemler şunlardır:

> [!div class="checklist"]
> * IOT alanınıza artıklığının şekilde bağlı her cihazı güvenli hale getirin.
> * Her cihaz, sensör ve IOT alanınız içinde kişi rolünü sınırlayın. İhlal edilmesi durumunda etkisini en aza indirilir.
> * Cihaz IP olası kullanımını göz önünde bulundurun filtreleme, adres ve bağlantı noktası kısıtlama.
> * Performansı artırmak için g/ç ve cihaz bant genişliğini sınırlayın. Hız sınırlama, hizmet reddi saldırılarını önleyerek güvenliğini artırabilirsiniz.
> * Cihaz üretici yazılımını güncel tutun.

Güvenle IOT alanına güvenli hale getirmek için bazı önemli yöntemler şunlardır:

> [!div class="checklist"]
> * Kaydedilmiş, depolanmış veya kalıcı verileri şifreleyin.
> * Parola veya anahtarlarını düzenli aralıklarla değiştirildi veya yenilenmesi gerekir.
> * Role göre dikkatli bir şekilde erişim ve izinleri kısıtlayın. "Rol tabanlı erişim denetimi en iyi yöntemler." bölümüne bakın
> * Güçlü şifreleme kullanın. Uzun parola gerektirir ve güvenli protokolleri ve iki öğeli kimlik doğrulaması kullanın.

Aykırı değerleri, tehditleri veya normal işlem aralığı dışında kalan kaynak parametreleri izlemek üzere IOT kaynakları izleyin. İzleme yönetmek için Azure Analytics'i kullanma.

> [!NOTE]
> Olay işleme ve izleme hakkında daha fazla bilgi için bkz. [rota olayları ve iletileri Azure dijital çiftleri ile](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Azure Active Directory en iyi uygulamaları

Azure dijital İkizlerini, kullanıcıların kimliklerini doğrulamak ve uygulamaları korumak için Azure AD kullanır. Azure AD, çeşitli modern mimarileri için kimlik doğrulamasını destekler. Tüm OAuth 2.0 veya Openıd Connect gibi endüstri standardı protokoller temel. Azure AD için IOT alanınızı güvenli hale getirmek için birkaç önemli yöntemler şunlardır:

> [!div class="checklist"]
> * Azure AD uygulama gizli dizileri ve anahtarları güvenli bir konumda gibi Store [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/).
> * Güvenilen bir tarafından yayınlanan bir sertifika kullanmak [sertifika yetkilisi](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) kimliğini doğrulamak için uygulama gizli anahtarlarının yerine.
> * OAuth 2.0 belirteç için erişim kapsamını sınırlandırın.
> * Bir belirtecin geçerli olduğu süreyi doğrulayın ve bir belirteç olup geçerli kalır.
> * Uygun uzunlukta belirteçleri için geçerli olan süreyi ayarlayın.
> * Süresi dolan yenileyin.

## <a name="role-based-access-control-best-practices"></a>Rol tabanlı erişim denetimi en iyi uygulamalar

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT en iyi uygulamalar hakkında daha fazla bilgi edinmek için [IOT güvenlik en iyi](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Rol tabanlı erişim denetimi hakkında bilgi edinmek için [rol tabanlı erişim denetimi](./security-role-based-access-control.md).

Kimlik doğrulaması hakkında bilgi edinmek için [API'leri ile kimlik doğrulama](./security-authenticating-apis.md).
