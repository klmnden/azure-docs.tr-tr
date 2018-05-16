---
title: Azure AD Android Başlarken | Microsoft Docs
description: Oturum açma ve Azure AD aramalar için Azure AD ile tümleşen bir Android uygulamasının nasıl oluşturulacağını OAuth2.0 kullanarak API'leri korumalı.
services: active-directory
documentationcenter: android
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 20618fff8d253bfab195ce2847a8848a28960ae4
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="azure-ad-android-getting-started"></a>Azure AD Android Başlarken
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Android uygulama geliştiriyorsanız, Microsoft, basit ve Azure Active Directory (Azure AD) kullanıcılar oturum kolay hale getirir. Azure AD uygulamanızı Microsoft Graph veya kendi korumalı web API'si aracılığıyla kullanıcı verilerine erişim sağlar. 

Azure AD Authentication Library (ADAL) Android kitaplığı uygulamanızı kullanmaya başlamak için sağlar [Microsoft Azure bulut](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) destekleyen tarafından [ Microsoft Azure Active Directory hesaplarını](https://azure.microsoft.com/services/active-directory/) endüstri kullanarak standart OAuth2 ve Openıd Connect. Bu örnek dahil olmak üzere, uygulamanızın yaşamazsınız normal ömürleri gösterir:

* Microsoft Graph için belirteç alın
* Bir belirteç Yenile
* Microsoft Graph çağırın
* Out kullanıcı oturum açabilir

Başlamak için kullanıcılar oluşturduğunuz ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Senaryo: kullanıcılar oturum ve Microsoft Graph çağırın

![Topoloji](./media/active-directory-android-topology.png)

Bu uygulama tüm Azure AD hesapları için kullanılabilir. Hem tek hem de birden çok kuruluş senaryoları (adımda açıklanan) destekler. Bir geliştirici Kurumsal kullanıcılarla bağlanmak ve bunların Azure + O365 erişmek için uygulamaları nasıl oluşturabilirsiniz gösteren Microsoft Graph üzerinden veri. Kimlik doğrulama akışı sırasında son kullanıcılar oturum açmak için gereken ve uygulamanın ve bazı durumlarda izinler onay uygulamaya onayı için bir yönetici gerektirebilir. Bu örnek mantık çoğunluğu nasıl auth bir son kullanıcı ve marka temel bir çağrı Microsoft Graph gösterir.

## <a name="example-code"></a>Örnek kod

Tam örnek kod bulabilirsiniz [github'da](https://github.com/Azure-Samples/active-directory-android). 

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this, 
        AUTHORITY, 
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(), 
    RESOURCE_ID, 
    CLIENT_ID, 
    REDIRECT_URI,  
    PromptBehavior.Auto, 
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## <a name="steps-to-run"></a>Çalıştırmak için adımları

### <a name="register-and-configure-your-app"></a>Kaydolun ve uygulamanızı yapılandırma 
Bir yerel istemci uygulaması Microsoft kullanılarak ile kayıtlı olması gerekir [Azure portal](https://portal.azure.com). 

1. Uygulama kayıt alınıyor
    - [Azure portalına](https://aad.portal.azure.com) gidin. 
    - Seçin ***Azure Active Directory*** > ***uygulama kayıtlar***. 

2. Uygulama oluşturma
    - **Yeni uygulama kaydı**’nı seçin. 
    - Bir uygulama adı girin **adı** alan. 
    - İçinde **uygulama türü** seçin **yerel**. 
    - İçinde **yeniden yönlendirme URI'si**, girin `http://localhost`. 

3. Microsoft Graph yapılandırın
    - Seçin **ayarlar > gerekli izinleri**.
    - Seçin **Ekle**içine **bir API seçin** seçin ***Microsoft Graph***. 
    - İzni seçin **oturum açın ve kullanıcı profilini okuma**, ardından isabet **seçin** kaydetmek için. 
        - Bu izni eşlendiği `User.Read` kapsam. 
    - İsteğe bağlı: İç **gerekli izinleri > Windows Azure Active Directory**, seçili iznini kaldırın **oturum açın ve kullanıcı profilini okuma**. Bu izni iki kez listeleme kullanıcı onay sayfasına önler. 

4. Tebrikler! Uygulamanızı başarıyla yapılandırıldı. Sonraki bölümde ihtiyacınız vardır:
    - `Application ID`
    - `Redirect URI`

### <a name="get-the-sample-code"></a>Örnek kodunu alma

1. Kod kopyası.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Örnek Android Studio'da açın.
    - Seçin **varolan Android Studio projesini açın**.

### <a name="configure-your-code"></a>Kodunuzu yapılandırın

Bu kod örneği için tüm yapılandırmayı bulabilirsiniz ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** dosya. 

1. Sabit Değiştir `CLIENT_ID` ile `ApplicationID`.
2. Sabit Değiştir `REDIRECT URI` ile `Redirect URI` daha önce yapılandırılmış (`http://localhost`). 

### <a name="run-the-sample"></a>Örneği çalıştırma

1. Seçin **Yapı > temiz proje**. 
2. Seçin **Çalıştır > çalıştırma uygulama**. 
3. Uygulama oluşturmak ve bazı temel UX Göster Tıkladığınızda `Call Graph API` düğmesi, onu sor oturum açmak için ve sessiz bir şekilde yeni belirteci ile Microsoft Graph API çağrısı. 

## <a name="important-info"></a>Önemli bilgiler

1. Checkout [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) kitaplığı mekanizması ve yeni senaryolar ve özellikler nasıl yapılandırılacağı hakkında daha fazla bilgi için. 
2. Yerel senaryolarda uygulama katıştırılmış bir Web görünümü kullanır ve uygulama bırakmamasını. `Redirect URI` Rasgele olabilir. 
3. Sorunları bulmak veya istekleri? Bir sorun oluşturabilir veya sonrası Stackoverflow üzerinde etiketiyle `azure-active-directory`. 

### <a name="cross-app-sso"></a>Uygulamalar arası SSO'nun
Bilgi [ADAL kullanarak uygulamalar arası SSO'nun android'de etkinleştirmek nasıl](active-directory-sso-android.md). 

### <a name="auth-telemetry"></a>Kimlik doğrulama telemetri
ADAL kitaplığını uygulamalarını nasıl davranmakta olduğunu anlamak ve daha iyi deneyimler yapı uygulama geliştiriciler yardımcı olmak için telemetriyi auth kullanıma sunar. Bu oturum açma başarılı, etkin kullanıcılar ve diğer birçok ilginç Öngörüler yakalamanıza olanak sağlar. Kimlik doğrulama telemetri kullanarak toplama ve olayları depolamak için bir telemetri hizmeti oluşturmak uygulama geliştiriciler gerektirir.

Kimlik doğrulama telemetri hakkında daha fazla bilgi edinmek için checkout [ADAL Android auth telemetri](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
