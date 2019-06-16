---
title: Arka plan programı uygulama çağıran web API'leri (genel bakış) - Microsoft kimlik platformu
description: Web API'leri çağıran bir arka plan programı uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 578b7cdb38b7df3fab5885d773354a36f76a4cfb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075888"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Senaryo: Daemon uygulamasının web API'leri çağrıları

Tüm yapmanız gereken web API'lerini çağıran bir arka plan programı uygulaması oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Genel Bakış

Uygulamanız bir web API'si kendi adına (olmayan bir kullanıcı adına) çağırmak için bir belirteç elde edebilirsiniz. Bu senaryo, arka plan programı uygulamaları için yararlıdır. Standart OAuth 2.0 kullanan [istemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) verin.

![Daemon uygulamaları](./media/scenario-daemon-app/daemon-app.svg)

Arka plan programı uygulamaları için kullanım örnekleri bazı örnekleri aşağıda verilmiştir:

- Sağlama için kullanılan veya kullanıcıları yönetmek ya da toplu işlemler bir dizinde web uygulamaları
- Toplu işleri ya da arka planda çalışan bir işletim sistemi hizmetidir gerçekleştirin (Windows üzerinde windows Hizmetleri) ya da Linux Daemon'ları işlemleri gibi Masaüstü uygulamaları
- Web dizinler, özel olmayan kullanıcıları yönetmek için gereken API'leri

Burada olmayan uygulamalar kullanım istemci kimlik bilgileri başka bir yaygın örneği yoktur: bile, kullanıcıların adına hareket zaman, bir web API'sini veya kaynak kimliklerini ile teknik nedenlerle erişmek ihtiyaç duydukları. Gizli anahtar kasası veya Azure SQL veritabanı erişimi bir önbellek için buna bir örnektir.

Kendi kimlikleri için bir belirteç almak uygulamalar:

- Gizli istemci uygulamalardır. Bir kullanıcı bağımsız olarak bunlar kaynaklara koşuluyla, kimliğini kanıtlamak bu uygulamaları gerekir. Bunlar, ayrıca Azure Active Directory (Azure AD) Kiracı yöneticileri tarafından onaylanması için ihtiyaç duydukları yerine hassas uygulamalar hedeflenmiştir.
- Bir gizli dizi (uygulama parola veya sertifika) Azure AD'ye kaydettiniz. Bu gizli dizinin bir belirteç almak üzere Azure ad araması sırasında aktarılan.

## <a name="specifics"></a>Özellikleri

> [!IMPORTANT]
>
> - Kullanıcı etkileşimi bir arka plan programı uygulamayla mümkün değildir. Arka plan programı uygulama kendi kimlik gerektirir. Bu tür bir uygulama bir erişim belirteci kullanarak uygulama kimliğini ve bunun uygulama kimliği, kimlik bilgisi (parola veya sertifika) ve uygulama sunma ister Azure AD'ye kimliği URI'si. Başarılı kimlik doğrulamasından sonra arka plan programı daha sonra web API'sini çağırmak için kullanılır (ve gerekirse yenilenir) Microsoft kimlik platformu uç noktasından bir erişim belirteci (ve bir yenileme belirteci) alır.
> - Kullanıcı etkileşimi mümkün olmadığı için artımlı onay mümkün olmayacaktır. Gerekli API izinleri uygulama kaydı sırasında yapılandırılmış olması gerekmez ve uygulama kodunda yalnızca statik olarak tanımlı izinler ister. Bu, aynı zamanda arka plan programı uygulamaları artımlı onay desteklemeyeceği anlamına gelir.

Geliştiriciler için bu senaryo için uçtan uca deneyim aşağıdaki durumlara sahiptir:

- Arka plan programı uygulamaları, yalnızca Azure AD kiracılar çalışabilir. Bu, kişisel Microsoft hesapları yönetmek için çalışır bir arka plan programı uygulamasının anlamsız olmasını. Siz bir satır iş kolu (LOB) uygulama geliştiricisi gibiyseniz, kiracınızda arka plan programı uygulamanızı oluşturmayı öğreneceksiniz. Bir ISV iseniz, çok kiracılı arka plan programı uygulaması oluşturmak isteyebilirsiniz. Her bir kiracı Yöneticisi tarafından onaylı gerekir
- Sırasında [uygulama kaydı](./scenario-daemon-app-registration.md), **yanıt URI'si** gerek yoktur. Uygulama izinleri isteme ve bu uygulama izinleri kullanmak için yönetici onayı vermek ihtiyacınız ve gizli dizileri veya sertifikaları, Azure AD ile paylaşmak ihtiyacınız.
- [Uygulama yapılandırması](./scenario-daemon-app-configuration.md) uygulama kaydı sırasında Azure AD ile paylaşılan olarak istemci kimlik bilgileri sağlaması gerekir.
- [Kapsam](scenario-daemon-acquire-token.md#scopes-to-request) akışların ihtiyaçlarını statik kapsamı için istemci kimlik bilgileri ile bir belirteç almak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arka plan programı app - uygulama kaydı](./scenario-daemon-app-registration.md)
