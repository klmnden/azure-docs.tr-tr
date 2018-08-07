---
title: Azure AD Android ile çalışmaya başlama | Microsoft Docs
description: Azure AD oturum açma ve Azure AD aramaları için ile tümleşen bir Android uygulaması oluşturmak nasıl OAuth2.0 kullanarak API'leri korumalı.
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
ms.openlocfilehash: cdd0d9ccff608d5882480d1394e2188579cefe75
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576809"
---
# <a name="azure-ad-android-getting-started"></a>Azure AD Android kullanmaya başlama
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Bir Android uygulaması geliştiriyorsanız, Microsoft, Azure Active Directory (Azure AD) kullanıcılarının oturumunu açmak sade ve basit kılar. Azure AD, uygulamanızın Microsoft Graph veya kendi korumalı web API'si aracılığıyla kullanıcı verilerine erişim sağlar. 

Azure AD Authentication Library (ADAL) Android kitaplığı uygulamanızı kullanmaya başlamak olanağı sağlar. [Microsoft Azure bulut](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) destekleyerek [ Microsoft Azure Active Directory hesaplarını](https://azure.microsoft.com/services/active-directory/) sektör kullanarak standart OAuth2 ve Openıd Connect. Bu örnek, uygulamanızın dahil olmak üzere deneyimi tüm normal yaşam döngüleri gösterir:

* Microsoft Graph için bir belirteç Al
* Bir belirteci yenileyin
* Microsoft Graph'i çağırmaya
* Kullanıcının oturumunu kapatmaz

Başlamak için kullanıcı oluşturma ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](quickstart-create-new-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Senaryo: kullanıcıların oturumu ve Microsoft Graph'i çağırmaya

![Topoloji](./media/quickstart-v1-android/active-directory-android-topology.png)

Bu uygulama, tüm Azure AD hesapları için kullanılabilir. Bu, hem tek hem de çok kuruluş senaryoları (adımlarda açıklanmıştır) destekler. Bir geliştirici Kurumsal kullanıcılarla bağlantı kurun ve bunların Azure + O365 erişmek için uygulamaları nasıl oluşturabileceğinizi gösterir Microsoft Graph aracılığıyla veri. Kimlik doğrulama akışı sırasında son kullanıcılar oturum açmak için gereklidir ve uygulamanın ve bazı durumlarda izinler için izin bir yönetici uygulamaya onayı gerektirebilir. Bu örnekteki mantıksal çoğunu nasıl bir son kullanıcı kimlik doğrulama ve oluşturma için temel bir çağırmak için Microsoft Graph gösterir.

## <a name="example-code"></a>Örnek kod

Tam örnek kodu bulabilirsiniz [github'da](https://github.com/Azure-Samples/active-directory-android). 

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

## <a name="steps-to-run"></a>Çalıştırmak için adımlar

### <a name="register-and-configure-your-app"></a>Kaydetme ve uygulamanızı yapılandırma 
Yerel istemci uygulaması kullanarak Microsoft ile kayıtlı olması gerekir [Azure portalında](https://portal.azure.com). 

1. Uygulama kaydı alma
    - [Azure portalına](https://aad.portal.azure.com) gidin. 
    - Seçin ***Azure Active Directory*** > ***uygulama kayıtları***. 

2. Uygulama oluşturma
    - **Yeni uygulama kaydı**’nı seçin. 
    - Bir uygulama adı girin **adı** alan. 
    - İçinde **uygulama türü** seçin **yerel**. 
    - İçinde **yeniden yönlendirme URI'si**, girin `http://localhost`. 

3. Microsoft Graph'ı yapılandırma
    - Seçin **Ayarları > gerekli izinler**.
    - Seçin **Ekle**içine **bir API seçin** seçin ***Microsoft Graph***. 
    - İzin Seç **oturum açın ve kullanıcı profilini okuma**, ardından isabet **seçin** kaydetmek için. 
        - Bu izne eşlendiği `User.Read` kapsam. 
    - İsteğe bağlı: İç **gerekli izinler > Windows Azure Active Directory**, seçili izinleri kaldırmak **oturum açın ve kullanıcı profilini okuma**. Bu izni iki kez listeleniyor kullanıcı onay sayfası uğraşmasına gerek kalmaz. 

4. Tebrikler! Uygulamanız başarıyla yapılandırıldı. Sonraki bölümde, şunları yapmanız gerekir:
    - `Application ID`
    - `Redirect URI`

### <a name="get-the-sample-code"></a>Örnek kodunu alma

1. Kodu kopyalayın.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Örnek Android Studio'da açın.
    - Seçin **var olan bir Android Studio projesini Aç**.

### <a name="configure-your-code"></a>Kodunuzu yapılandırın

Bu kod örneğinde için tüm yapılandırma bulabilirsiniz ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** dosya. 

1. Sabit değiştirin `CLIENT_ID` ile `ApplicationID`.
2. Sabit değiştirin `REDIRECT URI` ile `Redirect URI` daha önce yapılandırılmış (`http://localhost`). 

### <a name="run-the-sample"></a>Örneği çalıştırma

1. Seçin **Yapı > temiz proje**. 
2. Seçin **çalıştırın > Uygulama Çalıştırma**. 
3. Uygulamayı oluşturun ve bazı temel UX'i Göster Tıkladığınızda `Call Graph API` düğmesi, bu oturum açma istemi ve sessiz bir şekilde yeni bir belirteç ile Microsoft Graph API çağrısı. 

## <a name="important-info"></a>Önemli bilgiler

1. Kullanıma alma [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) kitaplığı mekanizması ve yeni senaryolar ve Özellikler'i yapılandırma konusunda daha fazla bilgi için. 
2. Yerel senaryolarda uygulama katıştırılmış bir Webview kullanır ve uygulama bırakmamasını. `Redirect URI` Rastgele olabilir. 
3. Sorunları bulmak veya istekleri mı? Sorun oluştur veya sonrası Stackoverflow etiketiyle `azure-active-directory`. 

### <a name="cross-app-sso"></a>Uygulamalar arası SSO'nun
Bilgi [ADAL kullanarak uygulamalar arası SSO'yu android'de nasıl](howto-v1-enable-sso-on-android.md). 

### <a name="auth-telemetry"></a>Kimlik doğrulama telemetri
ADAL kitaplığı, uygulama geliştiricilerin uygulamalarını nasıl davranmakta anlamak ve daha iyi deneyimleri oluşturmanıza yardımcı olmak için kimlik doğrulama telemetri kullanıma sunar. Bu oturum açma başarı, etkin kullanıcılar ve diğer birçok ilgi çekici bilgiler yakalamanıza olanak sağlar. Auth telemetri kullanarak uygulama geliştiricileri, toplama ve olayları depolamak için bir telemetri hizmeti oluşturmak gerekli.

Kimlik doğrulama telemetrisi hakkında daha fazla bilgi edinmek için kullanıma alma [Android ADAL kimlik doğrulaması telemetri](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
