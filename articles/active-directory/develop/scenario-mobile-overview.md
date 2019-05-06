---
title: Web API'leri - çağıran mobil uygulamasının genel bakış | Microsoft kimlik platformu
description: Bir mobil uygulama oluşturmayı öğrenin çağrıları veritabanını web API'leri (Genel)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb0acd1534bab11eb57a7aa0e695f192b5999ed2
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076503"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Senaryo: Mobil uygulama web API'leri çağrıları

Web API'leri çağıran bir mobil uygulama oluşturmak için gereken her şeyi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

İlk mobil uygulamanızı oluşturun ve hızlı başlangıcı deneyin!

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir belirteç almak ve bir Android uygulamasından Microsoft Graph API çağırma](./quickstart-v2-android.md)
>
> [Hızlı Başlangıç: Bir belirteç almak ve bir iOS uygulamasından Microsoft Graph API çağırma](./quickstart-v2-ios.md)
>
> [Hızlı Başlangıç: Bir belirteç almak ve bir Xamarin iOS & Android uygulaması Microsoft Graph API çağırma](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Genel Bakış

Kişiselleştirilmiş ve sorunsuz son kullanıcı deneyimi, bir mobil uygulama oluştururken gereklidir.  Microsoft kimlik platformu, iOS ve Android kullanıcıları için tam olarak bunu yapmak mobil uygulama geliştiricileri sağlar. Uygulamanızı Azure AD'de, kişisel Microsoft hesabı oturum açabilirsiniz ve Azure AD B2C kullanıcıları ve onların adına bir web API'sini çağırmak için belirteçlerini almak. Bu akışları uygulamak için Microsoft kimlik doğrulama kitaplığı (endüstri standardı uygulayan MSAL) kullanacağız [OAuth2.0 yetkilendirme kod akışı](v2-oauth2-auth-code-flow.md).

![Daemon uygulamaları](./media/scenarios/mobile-app.svg)

Mobil uygulama dikkate alınacak noktalar:

- ***Kullanıcı deneyimi anahtarıdır***: Kullanıcıların oturum açmak için soran önce uygulamanızı değerini görmek ve yalnızca gerekli izinlere ilişkin istek.
- ***Tüm kullanıcı yapılandırmalarını destekler***: Birçok mobil iş kullanıcıları, koşullu erişim ve cihaz uyumluluk ilkeleri kapsamında değildir. Bu anahtar senaryolarını desteklemek üzere emin olun.
- ***Çoklu oturum açma (SSO) uygulayan***: MSAL ve Microsoft kimlik platformu çoklu oturum açmayı etkinleştirme cihazın tarayıcı veya Microsoft Authenticator (ve android'de Intune Şirket portalı) aracılığıyla basit hale getirir.

## <a name="specifics"></a>Özellikleri

Microsoft kimlik platformu üzerinde bir mobil uygulama oluştururken, uçtan uca deneyim bazı önemli noktalar vardır:

- Platforma bağlı olarak, herhangi bir etkileşim oturum açarken ilk oturum açma mümkün olmayabilir. iOS, örneğin, Microsoft Authenticator'ı (ve android'de Intune Şirket portalı) üzerinden SSO ilk zaman alırken kullanıcı etkileşimi göstermek için uygulamalar gerektirir.
- İOS ve Android üzerinde MSAL kullanıcılarının oturumunu açmak (Bu, uygulamanızın üzerinde görünebilir) bir dış tarayıcı kullanabilir. Uygulama içi görünümlerinden kullanacak şekilde özelleştirilebilir.
- Hiçbir zaman bir mobil uygulama gizli anahtarı kullanmak, tüm kullanıcılar için erişilebilir olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-mobile-app-registration.md)
