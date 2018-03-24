---
title: 'Azure Active Directory B2C: bir Android uygulamasını kullanarak bir belirteç alınırken | Microsoft Docs'
description: Bu makale AppAuth Azure Active Directory B2C ile kullanıcı kimliklerini yönetmek ve kullanıcıların kimliğini doğrulamak için kullandığı bir Android uygulamasının nasıl oluşturulacağını gösterir.
services: active-directory-b2c
documentationcenter: android
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.topic: article
ms.date: 03/06/2017
ms.author: davidmu
ms.openlocfilehash: 6c4c9359571882fbbea4e7701305e30e0f49f460
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Bir Android uygulamasını kullanarak oturum açın

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Bunun yapılması geliştiricilerin hizmetlerimizle tümleştirmek istediği herhangi bir kitaplıktan yararlanmasını sağlar. Geliştiricilerin platformumuzu diğer kitaplıklarla birlikte kullanmasına yardımcı olmak için 3 taraf kitaplıkların Microsoft identity platformuna bağlanmak için yapılandırma göstermek için bunun gibi birkaç izlenecek yazdıktan. [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) uygulayan çoğu kitaplık Microsoft Identity platformuna bağlanabilecektir.

> [!WARNING]
> Microsoft, 3 düzeltmelerin kitaplıkları taraf ve bu kitaplıkları gözden yapılmadı sağlamaz. Bu örnek, Azure AD B2C ile temel senaryolarda uyumluluk için test edilmiştir AppAuth adlı 3 bir taraf kitaplık kullanıyor. Kitaplığın açık kaynaklı proje yönlendirilmiş sorunları ve özellik istekleri. Lütfen bakın [bu makalede](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) daha fazla bilgi için.  
>
>

OAuth2 veya OpenID Connect kullanmaya yeni başladıysanız bu örnek yapılandırmanın büyük bölümü sizin için çok anlamlı olmayabilir. [burada belgelediğimiz protokolün genel bakışına](active-directory-b2c-reference-protocols.md) kısaca bakmanız önerilir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## <a name="create-an-application"></a>Uygulama oluşturma

Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Mobil bir uygulama oluşturmak için izlemeniz [bu yönergeleri](active-directory-b2c-app-registration.md). Şunları yaptığınızdan emin olun:

* Dahil bir **Native Client** uygulamadaki.
* Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Bu daha sonra ihtiyacınız olacak.
* Bir yerel istemcisi ayarlama **yeniden yönlendirme URI'si** (örneğin com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Buna da daha sonra ihtiyacınız olacak.

## <a name="create-your-policies"></a>İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama bir kimlik deneyimi içerir: bir birleşik oturum açma ve kaydolma. Bölümünde açıklandığı gibi bu ilkeyi oluşturmak gereken [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). İlkeyi oluştururken şunları yaptığınızdan emin olun:

* Seçin **görünen adı** ilkenizde kaydolma özniteliği olarak.
* Her ilkede uygulamanın talep ettiği **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır.  Bu ilke adına daha sonra ihtiyacınız olacaktır.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

İlkelerinizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin

Azure AD B2C ile AppAuth kullanan bir çalışma örneği sağladık [github'da](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Kodu indirin ve çalıştırın. Hızlı bir şekilde kendi Azure AD B2C Yapılandırması'ndaki yönergeleri izleyerek kullanılarak kendi uygulamanızı kullanmaya [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

Örnek tarafından sağlanan örnek bir değişikliktir [AppAuth](https://openid.github.io/AppAuth-Android/). Lütfen AppAuth ve özellikleri hakkında daha fazla bilgi edinmek için kendi sayfasını ziyaret edin.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Azure AD B2C ile AppAuth kullanmak için uygulamanızı değiştirme

> [!NOTE]
> AppAuth destekleyen Android API 16 (Jellybean) ve üstü. API 23 kullanmanızı öneririz ve üstü.
>

### <a name="configuration"></a>Yapılandırma

Azure AD B2C ile iletişim URI bulma belirterek veya yetkilendirme uç noktası ve belirteç uç noktası URI belirterek yapılandırabilirsiniz. Her iki durumda da, aşağıdaki bilgiler gerekir:

* Kiracı kimliği (ör. contoso.onmicrosoft.com)
* İlke adı (örneğin B2C\_1\_SignUpIn)

Yetkilendirme ve belirteç uç noktası URI otomatik olarak bulmayı seçerseniz, bulma URI bilgi fetch gerekecektir. Kiracı değiştirerek URI oluşturulabilir bulma\_kimliği ve ilke\_aşağıdaki URL adı:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Ardından, yetkilendirme ve belirteç uç noktası URI edinmeli ve aşağıdaki çalıştırarak AuthorizationServiceConfiguration nesneyi oluşturun:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

Yetkilendirme ve belirteç uç noktası URI elde etmek için bulma kullanmak yerine, ayrıca bunları açıkça Kiracı değiştirerek belirtebilirsiniz\_kimliği ve ilke\_adı URL'nin aşağıdaki:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

AuthorizationServiceConfiguration nesnesi oluşturmak için aşağıdaki kodu çalıştırın:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Yetkilendiriliyor

Yapılandırma veya bir yetkilendirme hizmet yapılandırmasını alma sonra bir yetkilendirme isteği oluşturulabilir. İsteği oluşturmak için aşağıdaki bilgiler gerekir:

* İstemci kimliği (örneğin 00000000-0000-0000-0000-000000000000)
* Özel bir şema ile (örneğin com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect) yeniden yönlendirme URI'si

Olursa, her iki öğe kaydedildi [uygulamanızı kaydetme](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Lütfen [AppAuth Kılavuzu](https://openid.github.io/AppAuth-Android/) işleminin geri kalanında tamamlamak hakkında. Hızlı bir şekilde bir çalışma uygulaması ile çalışmaya başlamak ihtiyacınız varsa kullanıma [bizim örnek](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Adımları [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) kendi Azure AD B2C yapılandırma girmek için.

Biz her zaman görüş ve öneriler için açık! Bu konu ile bir güçlükle sahip veya bu içeriğin geliştirilmesi için öneriler varsa, sayfanın sonundaki görüşlerinize değer veriyoruz. Özellik istekleri için bunları Ekle [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

