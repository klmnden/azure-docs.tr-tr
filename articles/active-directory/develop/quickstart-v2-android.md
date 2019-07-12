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
ms.date: 04/26/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 547eafac8cc1acf2b60416f93804e819a1c549b0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67702754"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir Android uygulamasından Microsoft Graph API çağırma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıç, bir Android uygulaması ile kişisel, iş ve okul hesaplarının oturumunu açmayı, erişim belirteci almayı ve Microsoft Graph API’sini çağırmayı gösteren bir kod örneği içerir.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Önkoşullar**
> * Android Studio 
> * Android 16 + gereklidir 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1\. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek için
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2\. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://aka.ms/MobileAppReg) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AndroidQuickstart`.
>      - Bu sayfada diğer yapılandırmalar atlayabilirsiniz. 
>      - İsabet `Register` düğmesi.
> 1. Yeni uygulama tıklayın > Git `Authentication`  >  `Add Platform`  >  `Android`.    
>      - Android studio projenizden paket adı girin. 
>      - İmza karma oluşturun. Yönergeler için portalına bakın.
> 1. Seçin `Configure` kaydedip ***MSAL yapılandırma*** JSON için daha sonra. 

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>1\. adım: Uygulamanızı yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yeniden yönlendirme URI'si ile kimlik doğrulama Aracısı uyumlu eklemeniz gerekir. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-android/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış

#### <a name="step-2-download-the-project"></a>2\. adım: Projenizi indirin

* [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3\. adım: Projenizi yapılandırın

> [!div renderon="docs"]
> Yukarıdaki seçeneği 1'i seçtiyseniz, şu adımları atlayabilirsiniz. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Projeyi ayıklayın ve Android Studio’da açın.
> 1. İçinde **uygulama** > **src** > **ana** > **res**  >   **Ham**açın **auth_config.json**.
> 1. Düzen **auth_config.json** Azure portalından JSON ile değiştirin. Bunun yerine el ile değişiklik yapmak istiyorsanız:
>    ```javascript
>    {
>       "client_id" : "Enter_the_Application_Id_Here",
>       "authorization_user_agent" : "DEFAULT",
>       "redirect_uri" : "Enter_the_Redirect_Uri_Here",
>       "authorities" : [
>          {
>             "type": "AAD",
>             "audience": {
>                "type": "Enter_the_Audience_Info_Here",
>                "tenant_id": "Enter_the_Tenant_Info_Here"
>             }
>          }
>       ]
>    }
> 1. Inside **app** > **manifests**, open  **AndroidManifest.xml**.
> 1. Paste the following activity to the **manifest\application** node: 
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> > 1. Uygulamayı çalıştırın! 

> [!div renderon="docs"]
> 1. Projeyi ayıklayın ve Android Studio’da açın.
> 1. İçinde **uygulama** > **res** > **ham**açın **auth_config.json**.
> 1. Düzen **auth_config.json** Azure portalından JSON ile değiştirin. Bunun yerine el ile bu değişiklik yapmak istiyorsanız:
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
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Decoded_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Değiştirin `Enter_the_Package_Name` ve `Enter_the_Signature_Hash` Azure portalında kaydettiğiniz değerlerle. 
> 1. Uygulamayı çalıştırın! 

## <a name="more-information"></a>Daha Fazla Bilgi

Bu hızlı başlangıç hakkında daha fazla bilgi için aşağıdaki bölümleri okuyun.

### <a name="getting-msal"></a>MSAL alma

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) kullanıcılarının oturumunu ve Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteci istemek için kullanılan bir kitaplık sunulmaktadır. Gradle 3.0 + aşağıdakileri ekleyerek yüklemek için kullanabileceğiniz **Gradle betiklerini** > **build.gradle (modül: uygulama)** altında **bağımlılıkları**:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.3.+'
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
> |`R.raw.auth_config` | Bu dosya, uygulama/istemci kimliği, oturum açma hedef kitle, yeniden yönlendirme URI'sini ve çeşitli diğer özelleştirme seçeneklerinin dahil olmak üzere, uygulama yapılandırmalarını içerir. |

### <a name="requesting-tokens"></a>Belirteç isteme

MSAL’in belirteç almak için kullanılan iki yöntemi vardır: `acquireToken` ve `acquireTokenSilentAsync`

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Etkileşimli bir belirteci alma

Bazı durumlarda kullanıcıların Microsoft kimlik platformu ile etkileşime geçmesini gerektirir. Bu durumlarda, son kullanıcı hesabını seçin, kimlik bilgilerini girin veya uygulamanızı istedi izni onayı için gerekebilir. Örneğin, 

* Kullanıcılar uygulamada ilk kez oturum açtığında
* Bir kullanıcının parolasını sıfırlar, bunlar kimlik bilgilerini girmeniz gerekir 
* İzni iptal edilirse 
* Uygulamanızı açıkça onayı gerektirip gerektirmediğini. 
* Ne zaman, uygulama bir kaynağa erişim ilk kez istiyor
* Mfa'yı veya diğer koşullu erişim ilkelerini gerekli olduğunda

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Konumlar:||
> |---------|---------|
> | `SCOPES` | İstenen kapsamları barındırır (Microsoft Graph için `{ "user.read" }` veya Web API’leri için `{ "<Application ID URL>/scope" }` (başka bir deyişle `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Kimlik doğrulamasından sonra denetim uygulamaya geri verildiğinde yürütülen geri arama |

#### <a name="acquiretokensilent-getting-a-user-token-silently"></a>acquireTokenSilent: Kullanıcı belirtecini sessizce alma

Uygulamaları bir belirteç istediklerinde her zaman oturum açmak kullanıcıları gerekmez. Kullanıcı zaten açtıysa, bu yöntem sessizce belirteçler istemek uygulamalar sağlar.

```java
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (!accounts.isEmpty()) {
                sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
            } else {
                /* No accounts */
            }
        }
    });
```

> |Konumlar:||
> |---------|---------|
> | `SCOPES` | İstenen kapsamları barındırır (Microsoft Graph için `{ "user.read" }` veya Web API’leri için `{ "<Application ID URL>/scope" }` (başka bir deyişle `api://<Application ID>/access_as_user`) |
> | `getAccounts(...)` | Belirteçleri için'ı sessizce almaya çalıştığınız hesabın içerir |
> | `getAuthSilentCallback()` | Kimlik doğrulamasından sonra denetim uygulamaya geri verildiğinde yürütülen geri arama |

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
