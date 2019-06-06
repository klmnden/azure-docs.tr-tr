---
title: Azure Active Directory B2C'de kullanılabilir uygulama türleri | Microsoft Docs
description: Azure Active Directory B2C'de kullanabileceğiniz uygulama türleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e4a9931fa6e2edf8989036eb8dbfbef3638cbbe0
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507518"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>Active Directory B2C'de kullanılabilir uygulama türleri

Azure Active Directory (Azure AD) B2C, çeşitli modern uygulama mimarilerinin için kimlik doğrulamasını destekler. Bunların tümü [OAuth 2.0](active-directory-b2c-reference-protocols.md) veya [OpenID Connect](active-directory-b2c-reference-protocols.md) endüstri standardı protokollerine dayalıdır. Bu belge, oluşturabileceğiniz uygulama türleri açıklayan, dilden veya platformdan bağımsız tercih ettiğiniz. Ayrıca, uygulama oluşturmaya başlamadan önce üst düzey senaryoları anlamanıza yardımcı olur.

Azure AD B2C'yi kullanan her uygulamanın kayıtlı olması gerekir, [Azure AD B2C kiracısı](active-directory-b2c-get-started.md) kullanarak [Azure portalında](https://portal.azure.com/). Uygulama kayıt işlemi toplar ve değerleri gibi atar:

* Bir **uygulama kimliği** , uygulamanızın benzersiz olarak tanımlar.
* A **yanıt URL'si** yanıtları uygulamanıza geri yönlendirmek için kullanılabilir.

Azure AD B2C'ye gönderilen her isteği belirtir bir **kullanıcı akışı**, Azure AD davranışını denetleyen bir ilke olduğu. Bu uç noktaları, yüksek oranda ölçeklenebilir kullanıcı deneyimi kümesi oluşturmak için de kullanabilirsiniz. Kullanıcı akışları genel ilkeleri de dahil olmak üzere, ayarlama kaydolma, oturum açma ve profil düzenleme ilkeleri yardımcı olacak bir kümesini sunuyoruz. Ancak, kendi özel ilkeleri de oluşturabilirsiniz. İlkeler hakkında bilginiz yoksa devam etmeden önce Azure AD B2C [genişletilebilir ilke çerçevesini](active-directory-b2c-reference-policies.md) okumanız gerekir.

Her uygulamanın etkileşimini benzer bir üst düzey deseni izler:

1. Uygulamayı yürütmek için kullanıcıyı v2.0 uç noktasına yönlendirir bir [ilke](active-directory-b2c-reference-policies.md).
2. Kullanıcı, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama v2.0 uç noktasından bir güvenlik belirteci alır.
4. Uygulama, korunan bilgilere veya korunan bir kaynağa erişmek için güvenlik belirtecini kullanır.
5. Kaynak sunucu, erişim izni verilebileceğini doğrulamak için güvenlik belirtecini doğrular.
6. Uygulama, güvenlik belirtecini düzenli aralıklarla yeniler.

Bu adımlar biraz oluşturmakta uygulama türüne göre değişebilir.

## <a name="web-applications"></a>Web uygulamaları

Bir sunucuda barındırılan ve tarayıcı aracılığıyla erişilen (.NET, PHP, Java, Ruby, Python ve Node.js dahil) web uygulamaları için Azure AD B2C'yi destekleyen [Openıd Connect](active-directory-b2c-reference-protocols.md) tüm kullanıcı deneyimleri. Openıd Connect'e ilişkin Azure AD B2C uygulamasında, web uygulamanızı Azure AD'ye kimlik doğrulaması istekler göndererek kullanıcı deneyimlerini başlatır. İstek sonucu `id_token` şeklindedir. Bu güvenlik belirteci, kullanıcının kimliğini temsil eder. Ayrıca kullanıcı hakkındaki bilgileri talep biçiminde sağlar:

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

Bir uygulamada kullanılabilir talepler ve belirteç türleri hakkında daha fazla bilgi edinin [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md).

Bir web uygulaması, her yürütmesinden bir [ilke](active-directory-b2c-reference-policies.md) bu üst düzey adımları uygular:

1. Kullanıcı, web uygulamasına göz atılmaktadır.
2. Web uygulamasını yürütmek için ilkeyi gösteren kullanıcıyı Azure AD B2C'ye yönlendirir.
3. Kullanıcı İlkesi tamamlar.
4. Azure AD B2C'in döndürdüğü bir `id_token` tarayıcıya.
5. `id_token` Yeniden yönlendirme URI'si gönderilir.
6. `id_token` Doğrulanır ve oturum tanımlama bilgisinin ayarlanır.
7. Güvenli bir sayfa, kullanıcıya döndürülür.

Azure AD'den alınan bir ortak imzalama anahtarı kullanarak `id_token` doğrulaması yapma, kullanıcının kimliğini doğrulamak için yeterlidir. Bu işlem ayrıca sonraki sayfa isteklerinde kullanıcı tanımlamak için kullanılan bir oturum tanımlama bilgisini ayarlar.

Bu senaryoyu çalışırken görmek için web uygulaması oturum açma kodu örneklerinden birini deneyin bizim [Başlarken bölümümüzdeki](active-directory-b2c-overview.md).

Basit oturum açma kolaylaştırmanın yanı sıra bir web sunucusu uygulaması, bir arka uç web hizmetine erişmek de gerekebilir. Bu durumda, web uygulaması biraz farklı gerçekleştirebilirsiniz [Openıd Connect akışı](active-directory-b2c-reference-oidc.md) yetkilendirme kodları kullanarak belirteçlerini almak ve yenileme belirteçleri. Bu senaryo, aşağıdaki [Web API'leri bölümünde](#web-apis) belirtilmiştir.

## <a name="web-apis"></a>Web API'leri

Uygulamanızın RESTful web API'si gibi web hizmetlerinin güvenliğini sağlamak için Azure AD B2C'yi kullanabilirsiniz. Web API’leri belirteçleri kullanarak gelen HTTP isteklerinin kimliğini doğrulama yoluyla verilerinin güvenliğini sağlamak üzere OAuth 2.0 kullanabilir. Web API'si çağıranı, HTTP isteğinin yetkilendirme üst bilgisine bir belirteç ekler:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API'si daha sonra, API çağıranının kimliğini doğrulamak ve belirteçte kodlanmış taleplerden çağıran hakkında bilgi ayıklamak için belirteci kullanabilir. [Azure AD B2C belirteç başvurusunda](active-directory-b2c-reference-tokens.md) bir uygulama için kullanılabilir talepler ve belirteç türleri hakkında bilgi edinin.

Web API'si, istemcilerin, web uygulamaları, masaüstü ve mobil uygulamalar, tek sayfalık uygulamalar, sunucu tarafı Daemon'ları ve diğer web API'leri dahil olmak üzere birçok türlerinden belirteçleri alabilir. Bir web API'si çağıran bir web uygulamasına yönelik tam akış örneği aşağıda verilmiştir:

1. Web uygulaması bir ilke yürütür ve kullanıcı deneyimi kullanıcının tamamlar.
2. Azure AD B2C'in döndürdüğü bir (Openıd Connect) `id_token` ve tarayıcıya bir yetkilendirme kodu.
3. Tarayıcı gönderileri `id_token` ve yeniden yönlendirme URI'sine yetkilendirme kodu.
4. Web sunucusu doğrulama `id_token` ve oturum tanımlama bilgisini ayarlar.
5. Web sunucusu için Azure AD B2C soran bir `access_token` yetkilendirme kodu, uygulama istemci Kimliğini ve istemci kimlik bilgileri sağlayarak.
6. `access_token` Ve `refresh_token` web sunucusuna döndürülür.
7. Web API'si ile adlandırılır `access_token` bir yetkilendirme üst bilgisinde.
8. Web API belirteci doğrular.
9. Güvenli veri web sunucusuna döndürülür.

Yetkilendirme kodları, yenileme belirteçleri ve belirteç alma adımları hakkında daha fazla bilgi edinmek için [OAuth 2.0 protokolünü](active-directory-b2c-reference-oauth-code.md) okuyun.

Azure AD B2C'yi kullanarak bir web API'sini nasıl güvence altına alacağınızı öğrenmek için [Başlarken bölümümüzdeki](active-directory-b2c-overview.md) web API'si eğitimlerine bakın.

## <a name="mobile-and-native-applications"></a>Mobil ve yerel uygulamalar

Mobil ve Masaüstü uygulamaları gibi cihazlara yüklenen uygulamaların çoğunlukla arka uç hizmetlerine veya web API'lerine kullanıcılar adına erişim gerekir. Yerel uygulamalarınıza özelleştirilmiş kimlik yönetimi deneyimleri ekleyebilir ve güvenli bir şekilde Azure AD B2C'yi kullanarak arka uç hizmetlerini çağırma ve [OAuth 2.0 yetkilendirme kod akışı](active-directory-b2c-reference-oauth-code.md).  

Bu akışta uygulama yürütür [ilkeleri](active-directory-b2c-reference-policies.md) ve alan bir `authorization_code` kullanıcı ilkeyi tamamladıktan sonra Azure AD'den. `authorization_code` Uygulamanın şu anda oturum açan kullanıcı adına arka uç hizmetlerini çağırma izni temsil eder. Uygulama ardından exchange `authorization_code` arka planda bir `access_token` ve `refresh_token`.  Uygulamanın kullanabileceği `access_token` HTTP isteklerinde arka uç web API'si için kimlik doğrulaması. Ayrıca eskisinin süresi dolduğunda yeni bir `access_token` almak için `refresh_token` öğesini kullanabilir.

## <a name="current-limitations"></a>Geçerli sınırlamalar

### <a name="application-not-supported"></a>Uygulama desteklenmiyor 

#### <a name="daemonsserver-side-applications"></a>Daemon'lar/sunucu tarafı uygulamalar

Uzun süre çalışan işlemler içeren veya bir kullanıcı varlığı çalışan uygulamalar da web API'leri gibi güvenli kaynaklara erişmek için bir yol gerekir. Bu uygulamalar, kimlik doğrulaması ve kimlik bilgileri akışı uygulamanın kimliğini (bir kullanıcının Devredilmiş kimliği yerine) kullanarak ve OAuth 2.0 istemci kullanarak belirteçleri alın. Sunucudan sunucuya kimlik doğrulaması için şirket adına-akış ve şirket adına-akış kullanılmamalıdır istemci kimlik bilgileri akışı aynı değildir.

İstemci kimlik bilgileri akışı şu anda Azure AD B2C tarafından desteklenmiyor olsa da, Azure AD kullanarak istemci kimlik bilgileri akışı ayarlayabilirsiniz. Azure AD B2C kiracısı ile Azure AD kuruluş bazı işlevler paylaşır kiracılar.  İstemci kimlik bilgileri akışı, Azure AD B2C kiracısının Azure AD işlevi kullanılarak desteklenir. 

İstemci kimlik bilgileri akışı ayarlamak için bkz [Azure Active Directory v2.0 ve OAuth 2.0 istemci kimlik bilgileri akışı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Azure AD tarafından açıklandığı gibi kullanılabilir olacak şekilde biçimlendirilmiş bir belirteç kaydınızda başarılı bir kimlik doğrulaması sonuçlanır [Azure AD belirteç başvurusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

#### <a name="web-api-chains-on-behalf-of-flow"></a>Web API'si zincirleri (temsili akış)

Çoğu mimari başka bir aşağı akış web API'si çağırmayı gerektiren bir web API'si içerir; her iki API de Azure AD B2C tarafından güvence altına alınır. Bu senaryo, bir Web API'si arka ucuna sahip yerel istemcilerde yaygındır ve bir Azure AD Graph API gibi Microsoft çevrimiçi hizmeti çağırır.

Bu zincirli web API'si senaryosu, temsili akış olarak da bilinen OAuth 2.0 JWT taşıyıcı kimlik bilgisi yetkisi kullanılarak desteklenebilir.  Ancak temsili akış şu anda Azure AD B2C’de uygulanmamıştır.

### <a name="faulted-apps"></a>Hatalı uygulamalar

Azure AD B2C uygulamaları şu yollarla düzenlemeyin:

- Diğer uygulama yanıt portallarında gibi [uygulama kayıt portalı](https://apps.dev.microsoft.com/).
- Graph API'si veya PowerShell kullanarak.

Azure portalında dışında Azure AD B2C uygulamasını düzenlerseniz, hatalı bir uygulama haline gelir ve Azure AD B2C ile artık kullanılamaz. Uygulamayı silip yeniden oluşturun.

Uygulamayı silmek için Git [uygulama kayıt portalı](https://apps.dev.microsoft.com/) ve uygulamayı silin. Uygulamanın görünür olması için uygulamanın sahibi olmanız (ve yalnızca kiracının yöneticisi olmamanız) gerekir.

