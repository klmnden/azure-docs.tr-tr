---
title: Azure AD B2C | Microsoft Belgeleri
description: "Azure Active Directory B2C’de oluşturabileceğiniz uygulama türleri."
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: bb9d4abe-0db7-4bd9-b0c4-2f43b2c9cf33
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: 4b13c040a15bef2f04d2cd2126e2270d061898bd
ms.openlocfilehash: 7d582960e615962a3952dd2f58c74ed91e5c450d


---
# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Uygulama türleri
Azure Active Directory (Azure AD) B2C, çeşitli modern uygulama mimarilerine yönelik kimlik doğrulamasını destekler. Bunların tümü [OAuth 2.0](active-directory-b2c-reference-protocols.md) veya [OpenID Connect](active-directory-b2c-reference-protocols.md) endüstri standardı protokollerine dayalıdır. Bu belgede kısaca, tercih ettiğiniz dilden veya platformdan bağımsız olarak oluşturabileceğiniz uygulama türleri açıklanmaktadır. [Uygulama oluşturmaya başlamadan](active-directory-b2c-overview.md#get-started) önce üst düzey senaryoları anlamanıza da yardımcı olur.

## <a name="the-basics"></a>Temel bilgiler
Azure AD B2C'yi kullanan her uygulamanın [Azure Portal](https://portal.azure.com/) aracılığıyla [B2C dizininizde](active-directory-b2c-get-started.md) kayıtlı olması gerekir. Uygulama kayıt işlemi birkaç değer toplayarak uygulamanıza atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama Kimliği**.
* Yanıtları uygulamanıza geri yönlendirmek için kullanılabilen **Yeniden Yönlendirme URI'si**.
* Tüm diğer senaryoya özel değerler. Daha ayrıntılı bilgi için [uygulama kaydetmeyi](active-directory-b2c-app-registration.md) öğrenin.

Uygulama kaydedildikten sonra Azure AD v2.0 uç noktasına istek göndererek Azure AD ile iletişim kurar:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Azure AD B2C'ye gönderilen her istek bir **ilke** belirtir. Bir ilke Azure AD davranışını denetler. Bu uç noktaları, yüksek oranda ölçeklenebilir kullanıcı deneyimi kümesi oluşturmak için de kullanabilirsiniz. Ortak ilkelere kaydolma, oturum açma ve profil düzenleme ilkeleri dahildir. İlkeler hakkında bilginiz yoksa devam etmeden önce Azure AD B2C [genişletilebilir ilke çerçevesini](active-directory-b2c-reference-policies.md) okumanız gerekir.

Her uygulamanın v2.0 uç noktası ile etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, [ilke](active-directory-b2c-reference-policies.md) yürütmek için kullanıcıyı v2.0 uç noktasına yönlendirir.
2. Kullanıcı, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama, v2.0 uç noktasından bir tür güvenlik belirteci alır.
4. Uygulama, korunan bilgilere veya korunan bir kaynağa erişmek için güvenlik belirtecini kullanır.
5. Kaynak sunucu, erişim izni verilebileceğini doğrulamak için güvenlik belirtecini doğrular.
6. Uygulama, güvenlik belirtecini düzenli aralıklarla yeniler.

<!-- TODO: Need a page for libraries to link to -->
Bu adımlar, oluşturduğunuz uygulama türüne göre biraz değişebilir. Açık kaynak kitaplıkları sizin için ayrıntıları ele alabilir.

## <a name="web-apps"></a>Web uygulamaları
Azure AD B2C, bir sunucuda barındırılan ve tarayıcı aracılığıyla erişilen web uygulamalarında (.NET, PHP, Java, Ruby, Python ve Node.js dahil) tüm kullanıcı deneyimleri için [OpenID Connect](active-directory-b2c-reference-protocols.md)'i destekler. Buna oturum açma, kaydolma ve profil yönetimi dahildir. OpenID Connect'e ilişkin Azure AD B2C uygulamasında web uygulamanız, Azure AD'ye kimlik doğrulaması istekleri sağlayarak bu kullanıcı deneyimlerini başlatır. İstek sonucu `id_token` şeklindedir. Bu güvenlik belirteci, kullanıcının kimliğini temsil eder. Ayrıca kullanıcı hakkındaki bilgileri talep biçiminde sağlar:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

[B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md) bir uygulama için kullanılabilir talep ve belirteç türleri hakkında bilgi edinin.

Bir web uygulamasında, her bir [ilke](active-directory-b2c-reference-policies.md) yürütme işlemi şu üst düzey adımları uygular:

![Web Uygulaması Kulvarları Görüntüsü](./media/active-directory-b2c-apps/webapp.png)

Azure AD'den alınan bir ortak imzalama anahtarı kullanarak `id_token` doğrulaması yapma, kullanıcının kimliğini doğrulamak için yeterlidir. Bu ayrıca sonraki sayfa isteklerinde kullanıcı kimliğini belirlemek için kullanılabilecek oturum tanımlama bilgisi belirler.

Bu senaryoyu çalışırken görmek üzere, [Başlarken bölümümüzde](active-directory-b2c-overview.md#get-started) bulunan web uygulaması oturum açma kodu örneklerinden birini deneyin.

Bir web sunucusu uygulamasının, oturum açmayı kolaylaştırmanın yanı sıra bir arka uç web hizmetine erişmesi de gerekebilir. Bu durumda web uygulaması biraz farklı bir [OpenID Connect akışı](active-directory-b2c-reference-oidc.md) gerçekleştirebilir, ayrıca yetkilendirme kodları ve yenileme belirteçleri kullanarak belirteçler alabilir. Bu senaryo, aşağıdaki [Web API'leri bölümünde](#web-apis) belirtilmiştir.

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Web API'leri
Uygulamanızın RESTful web API'si gibi web hizmetlerini güvence altına almak için Azure AD B2C'yi kullanabilirsiniz. Web API’leri belirteçleri kullanarak gelen HTTP isteklerinin kimliğini doğrulama yoluyla verilerinin güvenliğini sağlamak üzere OAuth 2.0 kullanabilir. Web API'si çağıranı, HTTP isteğinin yetkilendirme üst bilgisine bir belirteç ekler:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API'si daha sonra, API çağıranının kimliğini doğrulamak ve belirteçte kodlanmış taleplerden çağıran hakkında bilgi ayıklamak için belirteci kullanabilir. [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md) bir uygulama için kullanılabilir talepler ve belirteç türleri hakkında bilgi edinin.

> [!NOTE]
> Azure AD B2C şu anda yalnızca kendi iyi bilinen istemcileri aracılığıyla erişilen web API'lerini desteklemektedir. Örneğin, tam uygulamanız bir iOS uygulaması, Android uygulaması ve arka uç web API'si içerebilir. Bu mimari tam olarak desteklenir. Başka bir iOS uygulaması gibi bir iş ortağı istemcisine aynı web API'sine erişme izni vermek şu anda desteklenmemektedir. Tam uygulamanızın tüm bileşenleri tek bir uygulama kimliği paylaşmalıdır.
>
>

Web API'si; web uygulamaları, masaüstü ve mobil uygulamalar, tek sayfa uygulamaları, sunucu tarafı daemon'ları ve diğer web API'leri dahil olmak üzere birçok istemci türünden belirteç alabilir. Web API'si çağıran bir web uygulamasına yönelik tam akış örneği:

![Web Uygulaması Web API'si Kulvarları Görüntüsü](./media/active-directory-b2c-apps/webapi.png)

Yetkilendirme kodları, yenileme belirteçleri ve belirteç alma adımları hakkında daha fazla bilgi edinmek için [OAuth 2.0 protokolünü](active-directory-b2c-reference-oauth-code.md) okuyun.

Azure AD B2C'yi kullanarak bir web API'sini nasıl güvence altına alacağınızı öğrenmek için [Başlarken bölümümüzdeki](active-directory-b2c-overview.md#get-started) web API'si eğitimlerine bakın.

## <a name="mobile-and-native-apps"></a>Mobil ve yerel uygulamalar
Mobil ve masaüstü uygulamaları gibi cihazlara yüklenen uygulamaların çoğunlukla kullanıcılar adına arka uç hizmetlerine veya web API'lerine erişmesi gerekir. Azure AD B2C'yi ve [OAuth 2.0 yetkilendirme kodu akışını](active-directory-b2c-reference-oauth-code.md) kullanarak yerel uygulamalarınıza özelleştirilmiş kimlik yönetimi deneyimleri ekleyebilir ve arka uç hizmetlerini güvenle çağırabilirsiniz.  

Bu akışta uygulama, [ilkeleri](active-directory-b2c-reference-policies.md) yürütür ve kullanıcı ilkeyi tamamladıktan sonra Azure AD'den bir `authorization_code` alır. `authorization_code`, uygulamanın o anda oturum açmış olan kullanıcı adına arka uç hizmetlerini çağırma iznini temsil eder. Uygulama daha sonra `id_token` ve `refresh_token` için arka planda `authorization_code` öğesini değiştirebilir.  Uygulama, HTTP isteklerinde arka uç web API'si için kimlik doğrulaması yapma amacıyla `id_token` öğesini kullanabilir. Ayrıca eskisinin süresi dolduğunda yeni bir `id_token` almak için `refresh_token` öğesini kullanabilir.

> [!NOTE]
> Azure AD B2C şu anda yalnızca bir uygulamanın kendi arka uç web hizmetine erişmek için kullanılan kimlik belirteçlerini desteklemektedir. Örneğin, tam uygulamanız bir iOS uygulaması, Android uygulaması ve arka uç web API'si içerebilir. Bu mimari tam olarak desteklenir. iOS uygulamanızın, OAuth 2.0 erişim belirteçleri kullanarak bir iş ortağı web API'sine erişmesine izin verme şu anda desteklenmemektedir. Tam uygulamanızın tüm bileşenleri tek bir uygulama kimliği paylaşmalıdır.
>
>

![Yerel Uygulama Kulvarları Görüntüsü](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Geçerli sınırlamalar
Azure AD B2C şu anda aşağıdaki uygulama türlerini desteklememektedir, ancak desteklemesi planlanmaktadır. Azure AD B2C ile ilişkili ek sınırlamalar ve kısıtlamalar, [Sınırlamalar ve kısıtlamalar](active-directory-b2c-limitations.md) başlığı altında açıklanmaktadır.

### <a name="daemonsserver-side-apps"></a>Daemon'lar/sunucu tarafı uygulamalar
Uzun süre çalışan işlemler içeren veya kullanıcı olmadan çalışan uygulamalar da web API'leri gibi güvenli kaynaklara erişme yöntemi gerektirir. Bu uygulamalar, uygulamanın kimliğini (bir kullanıcının devredilmiş kimliği yerine) ve OAuth 2.0 istemci kimlik bilgileri akışını kullanarak kimlik doğrulaması yapabilir ve belirteç alabilir.

Bu akış şu anda Azure AD B2C tarafından desteklenmemektedir. Bu uygulamalar ancak etkileşimli kullanıcı akışı gerçekleştikten sonra belirteç alabilir.

### <a name="web-api-chains-on-behalf-of-flow"></a>Web API'si zincirleri (temsili akış)
Çoğu mimari başka bir aşağı akış web API'si çağırmayı gerektiren bir web API'si içerir; her iki API de Azure AD B2C tarafından güvence altına alınır. Bu senaryo, Web API'si arka ucuna sahip yerel istemcilerde yaygındır. Bu, ardından Azure AD Grafik API'si gibi bir Microsoft çevrimiçi hizmetini çağırır.

Bu zincirli web API'si senaryosu, temsili akış olarak da bilinen OAuth 2.0 JWT taşıyıcı kimlik bilgisi yetkisi kullanılarak desteklenebilir.  Ancak temsili akış şu anda Azure AD B2C’de uygulanmamıştır.



<!--HONumber=Dec16_HO4-->


