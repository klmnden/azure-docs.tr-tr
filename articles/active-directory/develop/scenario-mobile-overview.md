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
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f007ad1d5bf99136328ec5706f7ccbb5f6593c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111223"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Senaryo: Mobil uygulama web API'leri çağrıları

Web API'leri çağıran bir mobil uygulama oluşturmak için bilmeniz gereken her şeyi öğrenin.

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

Kişiselleştirilmiş ve sorunsuz bir kullanıcı deneyimi, mobil uygulamalar için gereklidir.  Microsoft kimlik platformu, iOS ve Android kullanıcıları için bu deneyimi oluşturmak üzere mobil geliştiricilerin sağlar. Uygulamanızı Azure Active Directory (Azure AD) kullanıcılarını, kişisel Microsoft hesabı kullanıcılarını ve Azure AD B2C kullanıcıları oturum ve onların adına bir web API'sini çağırmak için belirteçlerini almak. Bu akışları uygulamak için endüstri standardı uygulayan Microsoft kimlik doğrulama Kitaplığı'ne (MSAL) kullanacağız [OAuth2.0 yetkilendirme kod akışı](v2-oauth2-auth-code-flow.md).

![Daemon uygulamaları](./media/scenarios/mobile-app.svg)

Mobil uygulamalar için dikkate alınacak noktalar:

- **Kullanıcı deneyimi anahtarıdır**: Kullanıcıların oturum açmak için soran önce uygulamanızı değerini görmek ve yalnızca gerekli izinlere ilişkin istek.
- **Tüm kullanıcı yapılandırmalarını destekler**: Birçok mobil iş kullanıcıları, koşullu erişim ve cihaz uyumluluk ilkeleri kapsamında değildir. Bu anahtar senaryolarını desteklemek üzere emin olun.
- **Çoklu oturum açma (SSO) uygulayan**: MSAL ve Microsoft kimlik platformu çoklu oturum açmayı etkinleştirme cihazın tarayıcı veya Microsoft Authenticator (ve android'de Intune Şirket portalı) aracılığıyla basit hale getirir.

## <a name="specifics"></a>Özellikleri

Microsoft kimlik platformu üzerinde mobil bir uygulama oluşturduğunuzda bu noktalar göz önünde bulundurun:

- Platforma bağlı olarak, kullanıcı etkileşimi kullanıcılar oturum ilk kez gerekli olabilir. Örneğin, iOS uygulamaları SSO Microsoft Authenticator'ı (ve android'de Intune Şirket portalı) üzerinden ilk kez kullanırken kullanıcı etkileşimi göstermek üzere gerektirir.
- İOS ve Android üzerinde MSAL kullanıcılarının oturumunu açmak (Bu, uygulamanızın üzerinde görünebilir) bir dış tarayıcı kullanabilir. Uygulama içi görünümlerinden kullanmayı yapılandırmasını özelleştirebilirsiniz.
- Hiçbir zaman bir mobil uygulama gizli anahtarı kullanın. Tüm kullanıcılar için erişilebilir olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-mobile-app-registration.md)
