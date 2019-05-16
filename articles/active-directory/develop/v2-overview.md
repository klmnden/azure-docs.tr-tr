---
title: Microsoft kimlik Platformu (v2.0) genel bakış - Azure
description: Microsoft kimlik Platformu (v2.0) uç noktası ve platform hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd3a1525d7b1f9c0a0b6516014ea83fb8abd0376
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544821"
---
# <a name="microsoft-identity-platform-v20-overview"></a>Microsoft kimlik Platformu (v2.0) genel bakış

Microsoft kimlik platformu, Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Bu, geliştiricilerin Microsoft Graph'i ya da geliştiriciler için tasarlanmış API'leri gibi Microsoft APIs çağırmak için belirteçleri almak ve tüm Microsoft kimliklerini oturum uygulamaları oluşturmalarına olanak sağlar. Microsoft kimlik platformu oluşur:

- **OAuth 2.0 ve Openıd Connect uyumlu standart kimlik doğrulama hizmeti** geliştiricilerin herhangi bir Microsoft kimliği kimlik doğrulaması sağlayan dahil olmak üzere:
  - İş veya Okul hesapları (Azure AD sağlanan)
  - Kişisel Microsoft hesapları (örneğin, Skype, Xbox ve Outlook.com gibi)
  - Sosyal veya yerel hesap (aracılığıyla Azure AD B2C'de)
- **Açık kaynak kitaplıkları**: Microsoft kimlik doğrulama kitaplığı (MSAL) ve diğer standartlarıyla uyumlu kitaplıklar için destek
- **Yönetim Portalı'nı uygulama**: Tüm diğer Azure yönetim özelliklerinizi yanı sıra Azure portalında oluşturulan bir kayıt ve yapılandırma deneyimi.
- **Uygulama yapılandırması API ve PowerShell**: DevOps görevleri otomatik hale getirmek için uygulamalarınızı (Microsoft Graph ve Azure Active Directory Graph 1.6) REST API ve PowerShell, program yapılandırması sağlar.
- **Geliştirici içeriği**: kavramsal ve başvuru belgeleri, hızlı başlangıç örnekleri, kod örnekleri, öğreticileri ve nasıl yapılır kılavuzlarından.

Geliştiriciler için Microsoft kimlik platformu parolasız kimlik doğrulaması, kimlik doğrulamayı yükseltme ve koşullu erişim gibi kimlik ve güvenlik alanında yenilikler sorunsuz tümleştirmenin sunar.  Bu işlevselliğin kendiniz uygulamanız gerekmez: Microsoft kimlik platformu ile yerel olarak tümleşik uygulamalar gibi yeniliklerden yararlanın.

Microsoft kimlik platformu ile bir kez kod yazın ve herhangi bir kullanıcı ulaşın. Bir uygulama bir kez oluşturun ve sahip birçok platformlar arası çalışması ya da kaynak uygulaması (API) yanı sıra bir istemci çalışan bir uygulama oluşturabilirsiniz.

## <a name="getting-started"></a>Başlarken

Kimlikle çalışmanın zor olması şart değil. Uygun bir senaryo seçin — bir hızlı başlangıç ve dakikalar içinde çalıştırmaya başlamanızı sağlayacak bir genel bakış sayfasında her senaryo yolu vardır:

- [Tek sayfa uygulaması derleme](scenario-spa-overview.md)
- [Kullanıcılar oturum açtığında bir web uygulaması derleme](scenario-web-app-sign-user-overview.md)
- [Web API'leri çağıran bir web uygulaması derleme](scenario-web-app-call-api-overview.md)
- [Korumalı bir web API'si oluşturma](scenario-protected-web-api-overview.md)
- [Web API'leri çağıran bir web API'si oluşturma](scenario-web-api-call-api-overview.md)
- [Bir masaüstü uygulaması oluşturma](scenario-desktop-overview.md)
- [Bir arka plan programı uygulaması derleme](scenario-daemon-overview.md)
- [Bir mobil uygulama oluşturun](scenario-mobile-overview.md)

Genel kimlik doğrulaması uygulama senaryolarını aşağıdaki tabloda özetlenmiştir: Microsoft kimlik platformu uygulamanızla tümleştirmek, bir referans olarak kullanın.

[![Uygulama senaryolarında Microsoft kimlik platformu](./media/v2-overview/application-scenarios-identity-platform.png)](./media/v2-overview/application-scenarios-identity-platform.svg#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Temel kimlik doğrulaması kavramları hakkında daha fazla bilgi edinmek istiyorsanız, şu konularla başlattığınız öneririz:

- [Kimlik doğrulaması temel bilgileri](authentication-scenarios.md)
- [Uygulama ve hizmet sorumluları](app-objects-and-service-principals.md)
- [İzleyiciler](v2-supported-account-types.md)
- [İzinler ve onay](v2-permissions-and-consent.md)
- [Kimlik belirteçlerini](id-tokens.md) ve [erişim belirteçleri](access-tokens.md)

Çağıran bir veri açısından zengin uygulama derleme [Microsoft Graph](https://docs.microsoft.com/graph/overview).

Uygulamanıza başlatmaya hazır olduğunuzda bir **üretim ortamına**, bu en iyi uygulamaları gözden geçirin:

- [Günlüğe kaydetmeyi etkinleştirme](msal-logging.md) uygulamanızdaki.
- Uygulamanızda telemetriyi etkinleştirin.
- Etkinleştirme [proxy'leri ve HTTP istemcilerini özelleştirme](msal-net-provide-httpclient.md).
- Tümleştirmenizi izleyerek test [Microsoft kimlik platformu tümleştirme denetim](identity-platform-integration-checklist.md).

## <a name="learn-more"></a>Daha fazla bilgi edinin

Sosyal ve yerel kimliklerini imzalar müşteri Internet'e yönelik bir uygulama oluşturmak Planlama olsaydı göz atın [Azure AD B2C genel bakış](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-add-identity-providers).