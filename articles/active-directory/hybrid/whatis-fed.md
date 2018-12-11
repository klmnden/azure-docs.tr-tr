---
title: Azure AD ile Federasyon nedir? | Microsoft Docs
description: Azure AD ile Federasyon açıklar.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 0556a01490786ec4101176bc0437661723ce9b4f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109513"
---
# <a name="what-is-federation-with-azure-ad"></a>Azure AD ile Federasyon nedir?

Federasyon güveni olan etki alanları koleksiyonudur. Güven düzeyi farklılık gösterebilir, ancak genellikle kimlik doğrulaması içerir ve hemen her zaman yetkilendirme içerir. Tipik bir federasyon güveni için bir kaynak kümesi için paylaşılan erişim kuruluşların sayısı içerebilir.

Şirket içi ortamınızı Azure AD ile federasyona eklemek ve bu Federasyon kimlik doğrulaması ve yetkilendirme için kullanın.  Bu oturum açma yöntemi, tüm kullanıcı kimlik doğrulamasını şirket içi gerçekleştirilmesini sağlar.  Bu yöntem, daha ayrıntılı düzeyde erişim denetimi uygulamak yöneticilerin sağlar. AD FS ve PingFederate ile Federasyon kullanılabilir.

![Federasyon kimliği](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Active Directory Federation Services (AD FS) ile federasyon seçeneğini kullanmaya karar verirseniz AD FS altyapınızın kullanım dışı kalması durumunda yedek çözüm olarak kullanmak üzere parola karması eşitleme seçeneğini de ayarlayabilirsiniz.


## <a name="next-steps"></a>Sonraki Adımlar

- [Hibrit kimlik nedir?](whatis-phs.md)
- [Azure AD Connect ve Connect Health nedir?](whatis-azure-ad-connect.md)
- [Parola Karması eşitleme nedir?](whatis-phs.md)
- [Federasyon nedir?](whatis-fed.md)
- [Üzerinde çoklu oturum nedir?](how-to-connect-sso.md)
- [Federasyon nasıl çalışır?](how-to-connect-fed-whatis.md)
- [PingFederate ile Federasyon](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)