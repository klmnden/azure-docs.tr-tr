---
title: Microsoft kimlik platformu Android hızlı başlangıç | Azure
description: Bilgi nasıl Android uygulamaları Microsoft kimlik platformu uç noktası tarafından erişim belirteçlerini gerektiren bir API çağrısı.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1f174229da565627c0e5791f53031b338880cb3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299038"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir Android uygulamasından Microsoft Graph API çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, bir Android uygulaması ile kişisel, iş ve okul hesaplarının oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * Android Studio 3+
> * Android 21 + gereklidir 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek için
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `Android-Quickstart`.
>      - İsabet `Register` düğmesi.
> 1. Git `Authentication`  >  `Redirect URIs`  >  `Suggested Redirect URIs for public clients`, yeniden yönlendirme URI'si biçiminin seçip **msal {AppID} :/ / auth**. Yaptığınız değişikliği kaydedin.


> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>1. Adım: Uygulamanızı yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için, **msal{AppId}://auth** gibi bir yanıt URL'si eklemelisiniz (burada {AppId}, uygulamanızın kimliğidir).
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-android/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-the-project"></a>2. Adım: Projenizi indirin

* [Android Studio Projesini İndirme](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3. Adım: Projenizi yapılandırın

> [!div renderon="docs"]
> Yukarıdaki seçeneği 1'i seçtiyseniz, şu adımları atlayabilirsiniz. Android Studio projesi açın ve uygulamayı çalıştırın. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Projeyi ayıklayın ve Android Studio’da açın.
> 1. İçinde **uygulama** > **res** > **ham**açın **auth_config.json**.
> 1. Düzen **auth_config.json** değiştirin `client_id` ve `tenant_id`:
>    ```javascript
>    "client_id" : "Enter_the_Application_Id_Here",
>    "type": "Enter_the_Audience_Info_Here",
>    "tenant_id" : "Enter_the_Tenant_Info_Here"
>    ```
> 1. İçinde **uygulama** > **bildirimlerini**açın **AndroidManifest.xml**.
> 1. Aşağıdaki etkinliği **manifest\application** düğümüne ekleyin. Microsoft bu kodu uygulamanıza geri çağırma sağlar:   
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msalEnter_The_Application_Id_Here"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```

> [!div renderon="docs"]
> 1. Projeyi ayıklayın ve Android Studio’da açın.
> 1. İçinde **uygulama** > **res** > **ham**açın **auth_config.json**.
> 1. Düzen **auth_config.json** değiştirin `client_id` ve `redirect_uri`:
>    ```javascript
>    "client_id" : "ENTER_YOUR_APPLICATION_ID",
>    "redirect_uri": "ENTER_YOUR_REDIRECT_URI", 
>     ```
> 1. İçinde **uygulama** > **bildirimlerini**açın **AndroidManifest.xml**.
> 1. Aşağıdaki etkinliği **manifest\application** düğümüne ekleyin. Bu kod parçacığı, kimlik doğrulaması tamamlandıktan sonra işletim sisteminin uygulamanızı sürdürmesini sağlamak için bir **BrowserTabActivity** kaydeder:
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msal<ENTER_YOUR_APPLICATION_ID>"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```
> 1.  `<ENTER_THE_APPLICATION_ID_HERE>` kısmını uygulamanız için olan *Uygulama Kimliği* ile değiştirin. *Uygulama Kimliği*’ni bulmanız gerekiyorsa *Genel Bakış* sayfasına gidin.

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi için aşağıdaki bölümleri okuyun.

### <a name="msal"></a>MSAL

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) kullanıcılarının oturumunu ve Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteci istemek için kullanılan bir kitaplık sunulmaktadır. Bunu Gradle kullanarak yüklemek için, **Bağımlılıklar**'ın altında **Gradle Betikleri** > **build.gradle (Module: app)** içine aşağıdaki ekleyebilirsiniz:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```java
import com.microsoft.identity.client.*;
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```java
    sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);
```

> |Konumlar: ||
> |---------|---------|
> |`R.raw.auth_config` | Bu dosya, uygulama/istemci kimliği, oturum, İzleyici ve çeşitli diğer özelleştirme seçeneklerinin dahil olmak üzere, uygulama yapılandırmalarını içerir. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL’in belirteç almak için kullanılan iki yöntemi vardır: `acquireToken` ve `acquireTokenSilentAsync`

#### <a name="getting-a-user-token-interactively"></a>Kullanıcı belirtecini etkileşimli olarak alma

Bazı durumlarda, kullanıcılar ya da kullanıcı kimlik bilgilerini doğrulamak için sistemi tarayıcı veya onay için hangi sonuçları bir bağlamda geçiş, Microsoft kimlik platformu uç ile etkileşim kurmak için zorlama gerektirir. Bazı örnekler:

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
* Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
* İki faktörlü kimlik doğrulama gerektiğinde

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Konumlar:||
> |---------|---------|
> | `SCOPES` | İstenen kapsamları barındırır (Microsoft Graph için `{ "user.read" }` veya Web API’leri için `{ "<Application ID URL>/scope" }` (başka bir deyişle `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Kimlik doğrulamasından sonra denetim uygulamaya geri verildiğinde yürütülen geri arama |

#### <a name="getting-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

Kullanıcının bir kaynağa her erişmesi gerektiğinde kimlik bilgilerini doğrulamak zorunda kalmasını istemezsiniz. Çoğu zaman belirteç alımları ve yenilemelerinin kullanıcı etkileşimi olmadan gerçekleşmesini istersiniz. İlk `acquireToken` yönteminden sonra korumalı kaynaklara erişecek belirteçleri almak için `AcquireTokenSilentAsync` yöntemini kullanabilirsiniz:

```java
List<IAccount> accounts = sampleApp.getAccounts();
if (sample.size() == 1) {
    sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
} else {
    // No or multiple accounts
}
```

> |Konumlar:||
> |---------|---------|
> | `SCOPES` | İstenen kapsamları barındırır (Microsoft Graph için `{ "user.read" }` veya Web API’leri için `{ "<Application ID URL>/scope" }` (başka bir deyişle `api://<Application ID>/access_as_user`) |
> | `accounts.get(0)` | Belirteçleri için'ı sessizce almaya çalıştığınız hesabın içerir |
> | `getAuthInteractiveCallback` | Kimlik doğrulamasından sonra denetim uygulamaya geri verildiğinde yürütülen geri arama |

## <a name="next-steps"></a>Sonraki adımlar

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

Uygulama oluşturma işlemiyle ve yeni özelliklerle ilgili eksiksiz adım adım yönergeleri almak için Android öğreticisini deneyin. Orada, bu hızlı başlangıcın tam açıklaması da yer alır.

> [!div class="nextstepaction"]
> [Çağrı Grafı API'si Android öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Android için MSAL kitaplığı wiki'si

Android için MSAL kitaplığı hakkındaki diğer yazıları okuyun:

> [!div class="nextstepaction"]
> [Android için MSAL kitaplığı wiki'si](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
