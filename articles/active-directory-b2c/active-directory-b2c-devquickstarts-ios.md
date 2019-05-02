---
title: Bir iOS uygulaması Azure Active Directory B2C AppAuth kullanarak | Microsoft Docs
description: Bu makalede, kullanıcı kimliklerini yönetmek ve kullanıcıların kimliğini doğrulamak için Azure Active Directory B2C ile AppAuth kullanan bir iOS uygulamasının nasıl oluşturulacağını gösterir.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: dc429861d97715505ed48e06d216bd2c8292addf
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703094"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Bir iOS uygulaması kullanarak oturum açın

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Açık standart protokolü kullanılarak, hizmetlerimizle tümleştirmek için bir kitaplık seçerken daha fazla Geliştirici seçenekleri sunar. Bu izlenecek yolda ve diğerleri gibi geliştiricilerin Microsoft Identity platformuna bağlanmak uygulamalar yazmaya yardımcı olmak için sağladık. Uygulayan çoğu kitaplık [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) Microsoft Identity platformuna bağlanmak olanağına sahip olursunuz.

> [!WARNING]
> Microsoft, üçüncü taraf kitaplıklar için düzeltmeler ve bu kitaplık bir gözden geçirme yapmış değil sağlamaz. Bu örnek, Azure AD B2C ile temel senaryolarda uyumluluk için test edilmiştir AppAuth adlı bir üçüncü taraf kitaplığı kullanıyor. Sorunları ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilebilir. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) bakın.
>
>

OAuth2 veya Openıd Connect kullanmaya yeni başladıysanız Bu örnek yapılandırmanın büyük kadar sizin için anlamlı olmayabilir. [burada belgelediğimiz protokolün genel bakışına](active-directory-b2c-reference-protocols.md) kısaca bakmanız önerilir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma
Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Bir dizin, tüm kullanıcılarınız, uygulamalar, gruplar ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](tutorial-create-tenant.md).

## <a name="create-an-application"></a>Uygulama oluşturma
Ardından B2C dizininizde uygulama oluşturmanız gerekir. Uygulama kaydı, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bir mobil uygulama oluşturmak için takip [bu yönergeleri](active-directory-b2c-app-registration.md). Şunları yaptığınızdan emin olun:

* Dahil bir **yerel istemci** uygulama.
* Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Daha sonra bu GUID gerekir.
* Ayarlanmış bir **yeniden yönlendirme URI'si** (örneğin, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect) özel bir düzen ile. Daha sonra bu URI gerekir.

## <a name="create-your-user-flows"></a>Kullanıcı akışlarınızı oluşturun
Azure AD B2C'de, her kullanıcı deneyimi tarafından tanımlanan bir [kullanıcı akışı](active-directory-b2c-reference-policies.md). Bu uygulama bir kimlik deneyimi içerir: birleşik bir oturum açma ve kaydolma. İlkeyi oluştururken şunları yaptığınızdan emin olun:

* Altında **kaydolma özniteliklerini**, öznitelik seçin **görünen ad**.  Diğer öznitelikler de seçebilirsiniz.
* Altında **uygulama taleplerini**, talepleri seçmek **görünen ad** ve **kullanıcının nesne kimliği**. Diğer talepleri de seçebilirsiniz.
* Kopyalama **adı** oluşturduktan sonra her kullanıcı akış. Kullanıcı akışı adınızı ön ekine sahip `b2c_1_` kullanıcı akışı kaydettiğinizde.  Userjourney adı daha sonra gerekir.

Kullanıcı akış oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin
AppAuth kullanan Azure AD B2C ile çalışma örnek sağladık [github'da](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Kodu indirin ve çalıştırın. Azure AD B2C kiracınızı kullanmak için yönergeleri izleyin. [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Bu örnek tarafından Benioku yönergeleri takip ederek oluşturulduğu [iOS AppAuth proje Github'da](https://github.com/openid/AppAuth-iOS). Örnek ve kitaplığı nasıl çalıştığı hakkında daha fazla bilgi için github'da AppAuth Benioku başvuru.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Azure AD B2C ile AppAuth kullanmak için uygulamanızı değiştirme

> [!NOTE]
> AppAuth destekleyen iOS 7 ve üzeri.  Ancak, sosyal oturumların Google'da desteklenmesi için SFSafariViewController gerekli iOS 9 veya üstü gerektirir.
>

### <a name="configuration"></a>Yapılandırma

Yetkilendirme uç noktası ve belirteç uç noktası URI belirterek Azure AD B2C ile iletişim yapılandırabilirsiniz.  Bu bir URI'leri oluşturmak için aşağıdaki bilgiler gereklidir:
* Kiracı kimliği (örneğin, contoso.onmicrosoft.com)
* Userjourney adı (örneğin, B2C\_1\_SignUpIn)

URI Kiracının değiştirerek oluşturulabilir ve belirteç uç noktasına\_kimliği ve ilke\_aşağıdaki URL adı:

```objc
static NSString *const tokenEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

URI Kiracının değiştirerek oluşturulabilir yetkilendirme uç noktası\_kimliği ve ilke\_aşağıdaki URL adı:

```objc
static NSString *const authorizationEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

AuthorizationServiceConfiguration nesneyi oluşturmak için aşağıdaki kodu çalıştırın:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Yetkilendiriliyor

Yapılandırma veya bir yetkilendirme hizmet yapılandırması alma sonra bir yetkilendirme isteği oluşturulabilir. İsteği oluşturmak için aşağıdaki bilgiler gereklidir:  
* İstemci kimliği (örneğin, 00000000-0000-0000-0000-000000000000)
* Özel bir düzen ile (örneğin, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect) yeniden yönlendirme URI'si

Olduğunda, her iki öğe kaydedilmiş olması [uygulamanızı kaydetme](#create-an-application).

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

Uygulamanızın yeniden yönlendirme URI'sine özel şema ile işlemesini ayarlamak için uygulamanızın Info.plist dosyasındaki 'URL şemalarını' listesini güncelleştirmeniz gerekiyor:
* Info.plist açın.
* 'Paket işletim sistemi türü kodu' gibi bir satır üzerine gelin ve tıklayın \+ simgesi.
* Yeni satır 'URL türleri' olarak yeniden adlandırın.
* 'URL türleri' solundaki oka tıklayın ağaç açın.
* Solundaki oka tıklayın ' 0 ağaç açmak için ' öğesi.
* 0 öğesine 'URL şemalarını' altında ilk öğeyi yeniden adlandırın.
* Ağaç açmak için 'URL şemalarını' solundaki oka tıklayın.
* 'Value' sütununda solundaki boş bir alan yoktur 'Öğesi 0' 'URL şemalarını altında'.  Değeri, uygulamanızın benzersiz düzenine ayarlayın.  Değer redirectURL OIDAuthorizationRequest nesnesi oluşturulurken kullanılan şema eşleşmelidir.  Bu örnekte 'com.onmicrosoft.fabrikamb2c.exampleapp' şeması kullanılır.

Başvurmak [AppAuth Kılavuzu](https://openid.github.io/AppAuth-iOS/) nasıl işlemi tamamlayın. İle çalışan bir uygulamayı hızlıca başlamak ihtiyacınız varsa, kullanıma [örnek](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Bağlantısındaki [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) kendi Azure AD B2C yapılandırmasını girmek için.

Her zaman geri bildirim ve öneriler için açık duyuyoruz! Bu makalede herhangi bir güçlük sahip veya bu içeriğin geliştirilmesi için öneri, sayfanın sonundaki geri BİLDİRİMİNİZE değer veriyoruz. Özellik istekleri için ekleyebilmesi [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
