---
title: Azure Active Directory B2C'ndaki bir iOS uygulamasında AppAuth kullanarak | Microsoft Docs
description: Bu makalede kullanıcı kimliklerini yönetmek ve kullanıcıların kimliğini doğrulamak için Azure Active Directory B2C ile AppAuth kullanan bir iOS uygulamasının nasıl oluşturulacağını gösterir.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1da0187fc4ffa139eb7b245dd20d708eb98e0cf5
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34710316"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Bir iOS uygulaması kullanarak oturum açın

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Açık bir standart protokol kullanarak daha fazla Geliştirici seçenekleri hizmetlerimizle tümleştirmek için bir kitaplık seçerken sunar. Bu kılavuzda ve diğerleri gibi Microsoft Identity platformuna bağlanmak uygulamaları yazma ile geliştiricilere yardımcı olmak için sağladık. Uygulayan çoğu kitaplık [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) Microsoft Identity platformuna bağlanamıyorsunuz.

> [!WARNING]
> Microsoft, üçüncü taraf kitaplıklar için düzeltmeler ve bu kitaplıkları gözden yapılmadı sağlamaz. Bu örnek temel senaryolarda Azure AD B2C ile uyumluluk için test edilmiştir AppAuth adlı bir üçüncü taraf kitaplık kullanıyor. Kitaplığın açık kaynaklı proje yönlendirilmiş sorunları ve özellik istekleri. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) bakın.
>
>

OAuth2 veya Openıd Connect kullanmaya yeni başladıysanız Bu örnek yapılandırmanın çoğunu kadar size anlamı olmayabilir. [burada belgelediğimiz protokolün genel bakışına](active-directory-b2c-reference-protocols.md) kısaca bakmanız önerilir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma
Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Bir dizin, tüm kullanıcıların, uygulamaları, grupları ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## <a name="create-an-application"></a>Uygulama oluşturma
Ardından B2C dizininizde uygulama oluşturmanız gerekir. Uygulama kaydı, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Mobil bir uygulama oluşturmak için izlemeniz [bu yönergeleri](active-directory-b2c-app-registration.md). Şunları yaptığınızdan emin olun:

* Dahil bir **Native client** uygulamadaki.
* Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Bu GUID daha sonra gerekir.
* Ayarlanmış bir **yeniden yönlendirme URI'si** özel şema (örneğin, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect) sahip. Bu URI daha sonra gerekir.

## <a name="create-your-policies"></a>İlkelerinizi oluşturma
Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama bir kimlik deneyimi içerir: bir birleşik oturum açma ve kaydolma. Bu ilkeyi açıklandığı gibi oluşturmak [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). İlkeyi oluştururken şunları yaptığınızdan emin olun:

* Altında **kaydolma özniteliklerini**, özniteliği seçin **görünen adı**.  Diğer öznitelikler de seçebilirsiniz.
* Altında **uygulama talepleri**, talepleri seçmek **görünen adı** ve **kullanıcının nesne kimliği**. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her ilkenin **Adını** kaydedin. İlke adı önekine sahip `b2c_1_` ilkeyi kaydettiğinizde.  İlke adı daha sonra gerekir.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

İlkelerinizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin
Azure AD B2C ile AppAuth kullanan bir çalışma örneği sağladık [github'da](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Kodu indirin ve çalıştırın. Kendi Azure AD B2C kiracısı kullanmak için ' ndaki yönergeleri izleyin [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Bu örnek tarafından Benioku yönergeleri izleyerek oluşturulduğu [iOS AppAuth proje Github'da](https://github.com/openid/AppAuth-iOS). Örnek ve kitaplık nasıl çalıştığı hakkında daha fazla ayrıntı için github'da AppAuth Benioku başvuru.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Azure AD B2C ile AppAuth kullanmak için uygulamanızı değiştirme

> [!NOTE]
> AppAuth destekleyen iOS 7 ve üstü.  Ancak, sosyal oturum açmalar üzerinde Google desteklemek için SFSafariViewController gereklidir iOS 9 veya üstü gerektirir.
>

### <a name="configuration"></a>Yapılandırma

Yetkilendirme uç noktası ve belirteç uç noktası URI belirterek Azure AD B2C ile iletişim yapılandırabilirsiniz.  Bu URI oluşturmak için aşağıdaki bilgiler gereklidir:
* Kiracı kimliği (örneğin, contoso.onmicrosoft.com)
* İlke adı (örneğin, B2C\_1\_SignUpIn)

Belirteç uç noktası URI Kiracı değiştirerek oluşturulabilir\_kimliği ve ilke\_aşağıdaki URL adı:

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

Kiracı değiştirerek URI oluşturulabilir yetkilendirme uç noktası\_kimliği ve ilke\_aşağıdaki URL adı:

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

AuthorizationServiceConfiguration nesnesi oluşturmak için aşağıdaki kodu çalıştırın:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Yetkilendiriliyor

Yapılandırma veya bir yetkilendirme hizmet yapılandırmasını alma sonra bir yetkilendirme isteği oluşturulabilir. İsteği oluşturmak için aşağıdaki bilgiler gereklidir:  
* İstemci kimliği (örneğin, 00000000-0000-0000-0000-000000000000)
* Özel bir şema ile (örneğin, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect) yeniden yönlendirme URI'si

Olursa, her iki öğe kaydedildi [uygulamanızı kaydetme](#create-an-application).

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

Uygulamanızın yeniden yönlendirme URI'si özel şema ile ayarlamak için Info.pList 'URL şemalarını' listesi güncelleştirmeniz gerekir:
* Info.plist açın.
* ' I tıklatın ve 'Paket işletim sistemi türü kodu' gibi bir satır üzerine gelerek \+ simgesi.
* Yeni satır 'URL türleri' olarak yeniden adlandırın.
* 'URL türleri' solundaki oka tıklayın ağaç açın.
* Solundaki oka tıklayın ' 0 ağaç açmak için ' öğesi.
* 'URL şemalarını' 0 öğesine altındaki ilk öğeyi yeniden adlandırın.
* Ağaç açmak için 'URL şemalarını' solundaki oka tıklayın.
* 'Value' sütununda solundaki boş alan yok 'Öğesi 0' 'Altında URL şemalarını'.  Değeri, uygulamanızın benzersiz düzenine ayarlayın.  Değer Redirecturl'yi OIDAuthorizationRequest nesnesi oluşturulurken kullanılan şema eşleşmelidir.  Örnekte 'com.onmicrosoft.fabrikamb2c.exampleapp' düzeni kullanılır.

Başvurmak [AppAuth Kılavuzu](https://openid.github.io/AppAuth-iOS/) işleminin geri kalanında tamamlamak hakkında. Hızlı bir şekilde bir çalışma uygulaması ile çalışmaya başlamak ihtiyacınız varsa kullanıma [örnek](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Adımları [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) kendi Azure AD B2C yapılandırma girmek için.

Biz her zaman görüş ve öneriler için açık! Bu makale ile bir güçlükle sahip veya bu içeriğin geliştirilmesi için öneriler varsa, sayfanın sonundaki görüşlerinize değer veriyoruz. Özellik istekleri için bunları Ekle [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
