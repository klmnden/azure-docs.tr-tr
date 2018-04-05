---
title: Kimlik doğrulama ve yetkilendirme Azure App Service'te | Microsoft Docs
description: Kavramsal başvurusu ve kimlik doğrulamasına genel bakış / yetkilendirme özelliğini Azure uygulama hizmeti
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: 0563f071083080de7f99b3a77c680f791d80bcdb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde kimlik doğrulaması ve yetkilendirme

Azure uygulama hizmeti yerleşik kimlik doğrulama ve yetkilendirme destekler, böylece kullanıcılar oturum ve en az yazma veya hiçbir web uygulaması, API ve mobil arka uç kodu tarafından erişim verileri sağlar ve ayrıca [Azure işlevleri](../azure-functions/functions-overview.md). Bu makale, App Service'nın kimlik doğrulama ve yetkilendirme, uygulamanız için basitleştirmek nasıl yardımcı olduğunu açıklar. 

Güvenli kimlik doğrulaması ve yetkilendirme gerektirir derin anlayış Federasyonu dahil olmak üzere güvenlik, şifreleme, [JSON web belirteçleri (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) yönetimi, [verme türlerini](https://oauth.net/2/grant-types/)ve benzeri. Uygulama hizmeti bu yardımcı programları sunar, böylece iş değerini müşterinize sağlayarak daha fazla zaman ve enerji harcayabilir.

> [!NOTE]
> Uygulama hizmeti kimlik doğrulama ve yetkilendirme için kullanmak zorunda değilsiniz. Birçok web çerçeveleri güvenlik özellikleri ile paketlenmiştir ve isterseniz kullanın. Uygulama hizmeti sağlayan daha fazla esneklik gerekiyorsa, ayrıca kendi yardımcı programları yazabilirsiniz.  
>

Yerel mobil uygulamalar için özel bilgi için bkz: [kullanıcı kimlik doğrulaması ve yetkilendirme Azure uygulama hizmetiyle mobil uygulamalar için](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama ve yetkilendirme modülü uygulama kodunuz aynı sanal çalıştırır. Etkinleştirildiğinde, her gelen HTTP istek üzerinden uygulama kodu tarafından işlenen önce geçirir.

![](media/app-service-authentication-overview/architecture.png)

Bu modül, uygulamanız için birkaç şey gerçekleştirir:

- Belirtilen sağlayıcı ile kullanıcıların kimliğini doğrular
- Doğrular, depolar ve belirteçleri yeniler
- Kimliği doğrulanan oturum yönetir
- Kimlik bilgileri istek üstbilgileri içine yerleştirir

Modül uygulama kodunuzdan ayrı olarak çalışır ve uygulama ayarları kullanılarak yapılandırılır. Hiçbir SDK'ları, belirli diller veya değişiklikleri uygulama kodunuz için gereklidir. 

### <a name="user-claims"></a>Kullanıcı talepleri

Tüm dil altyapılarından için uygulama hizmeti kullanıcının talepleri kodunuza istek üstbilgileri ekleyerek kullanılabilmesini sağlar. ASP.NET 4.6 uygulamalar için uygulama hizmeti doldurur [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) standart .NET kod deseni takip edebilirsiniz şekilde kimliği doğrulanmış kullanıcının talepleri ile de dahil olmak üzere `[Authorize]` özniteliği. Benzer şekilde, PHP uygulamaları için uygulama hizmeti doldurur `_SERVER['REMOTE_USER']` değişkeni.

İçin [Azure işlevleri](../azure-functions/functions-overview.md), `ClaimsPrincipal.Current` .NET kodu için hydrated değil ancak kullanıcı taleplerini istek üst bilgilerinde yine de bulabilirsiniz.

Daha fazla bilgi için bkz: [erişim kullanıcı taleplerini](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Belirteç Deposu

App Service web apps, API veya yerel mobil uygulamalar, kullanıcılarla ilişkilendirilen belirteçleri deposudur yerleşik bir belirteç deposu sağlar. Herhangi bir sağlayıcıyı ile kimlik doğrulaması etkinleştirdiğinizde, bu belirteç deposu uygulamanız için hemen kullanılabilir. Uygulama kodunuz gibi veri bu sağlayıcılardan kullanıcı adınıza erişim gerekiyorsa: 

- Kimliği doğrulanmış kullanıcının Facebook gönderi
- Azure Active Directory grafik API'sini veya hatta Microsoft Graph kullanıcının kurumsal verileri okuma

Kimliği doğrulanan oturum için kimliği belirteçleri, erişim belirteçleri ve yenileme belirteçleri önbelleğe ve yalnızca ilişkili kullanıcı tarafından erişilebilir.  

Tipik olarak toplamak, depolamak ve bu belirteçler, uygulamanızda yenilemek için kod yazmanız gerekir. Belirteç Deposu ile yeni [belirteçlerini almasını](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) gerektiğinde bunları ve [bunları yenilemek için uygulama hizmeti söyleyin](app-service-authentication-how-to.md#refresh-access-tokens) zaman olduklarında geçersiz. 

Uygulamanızı belirteçleri çalışmak gerekmiyorsa, belirteç deposu devre dışı bırakabilirsiniz.

### <a name="logging-and-tracing"></a>Günlüğe kaydetme ve izleme

Varsa, [Uygulama günlüğünü etkinleştirin](web-sites-enable-diagnostic-log.md), kimlik doğrulama ve yetkilendirme izlemeleri doğrudan günlük dosyalarınızı görürsünüz. Beklemediğiniz bir kimlik doğrulama hatası görürseniz, var olan uygulama günlüklerine bakarak tüm ayrıntıları kolayca bulabilirsiniz. Etkinleştirirseniz, [başarısız istek izleme](web-sites-enable-diagnostic-log.md), kimlik doğrulaması tam olarak hangi rolü görebilirsiniz ve yetkilendirme modülü, başarısız bir istek oynanan. Adlı bir modül başvurular izleme günlükleri arayın `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Kimlik sağlayıcıları

Uygulama hizmeti kullanan [federe kimlik](https://en.wikipedia.org/wiki/Federated_identity), hangi üçüncü taraf kimlik sağlayıcısı'kullanıcı kimlikleri ve sizin için kimlik doğrulaması akışı yönetir. Beş kimlik sağlayıcıları varsayılan olarak kullanılabilir: 

| Sağlayıcı | Oturum açma uç noktası |
| - | - |
| [Azure Active Directory](../active-directory/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft Hesabı](../active-directory/develop/active-directory-appmodel-v2-overview.md) | `/.auth/login/microsoft` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/docs/basics/authentication) | `/.auth/login/twitter` |

Kimlik doğrulama ve yetkilendirme bu sağlayıcılardan biri ile etkinleştirdiğinizde, kullanıcı kimlik doğrulaması ve kimlik doğrulama belirteçleri sağlayıcısından doğrulanması için oturum açma uç noktasında kullanılabilir. Bu oturum açma seçenekleri herhangi bir sayı ile kullanıcılarınız kolaylıkla sağlayabilir. Başka bir kimlik sağlayıcısı de tümleştirebilir veya [kendi özel kimlik çözümü][custom-auth].

## <a name="authentication-flow"></a>Kimlik doğrulama akışı

Kimlik doğrulaması akışı tüm sağlayıcılar için aynıdır, ancak oturum sağlayıcının SDK oturum istediğiniz bağlı olarak farklılık gösterir:

- Sağlayıcı SDK olmadan: federe oturum açma App Service'e uygulama atar. Bu genellikle sağlayıcının oturum açma sayfasına kullanıcıya sunabilir tarayıcı uygulamalarıyla durumdur. Ayrıca adlı için sunucu kodu oturum açma işlemini yönetir _sunucu yönlendirilmiş akış_ veya _server akış_. Bu durumda, web uygulamaları için geçerlidir. SDK'yı bir web görünümü kullanıcılar App Service kimlik bilgilerinizle oturum açtığından Mobile Apps istemci SDK'sını kullanarak kullanıcıların oturum yerel uygulamalar için de geçerlidir. 
- SDK sağlayıcısıyla: uygulama el ile oturum açtığında ve doğrulama için uygulama hizmeti için kimlik doğrulama belirteci gönderir. Bu genellikle sağlayıcının oturum açma sayfası kullanıcıya sunmak olamaz tarayıcı olmayan uygulamaları ile bir durumdur. Ayrıca adlı şekilde uygulama kodu oturum açma işlemini yönetir _istemci yönlendirilmiş akış_ veya _istemci akışı_. Bu durumda REST API'leri için geçerlidir [Azure işlevleri](../azure-functions/functions-overview.md)ve JavaScript tarayıcı istemcilerinin, yanı sıra oturum açma işleminde daha fazla esneklik gereken web uygulamaları. Sağlayıcının SDK'sını kullanarak kullanıcıların oturum yerel mobil uygulamalar için de geçerlidir.

> [!NOTE]
> App Service'te bir güvenilen tarayıcı uygulamasından aramaları başka bir REST API App Service'te veya [Azure işlevleri](../azure-functions/functions-overview.md) sunucu yönlendirilmiş akış kullanılarak doğrulanabilir. Daha fazla bilgi için bkz: [Azure uygulama hizmeti ile kullanıcıların kimliklerini]().
>

Aşağıdaki tabloda, kimlik doğrulaması akışı adımları gösterir.

| Adım | SDK sağlayıcı | SDK sağlayıcı ile |
| - | - | - |
| 1. Kullanıcı olarak oturum açın | İstemciye yönlendirir `/.auth/login/<provider>`. | İstemci kodu doğrudan sağlayıcının SDK ile kullanıcı oturum açtığında ve kimlik doğrulama belirtecini alır. Bilgi için sağlayıcının belgelerine bakın. |
| 2. Kimlik doğrulaması sonrası | Sağlayıcı istemciye yönlendirir `/.auth/login/<provider>/callback`. | İstemci kodu yazılarını belirteci için sağlayıcısından `/.auth/login/<provider>` doğrulama için. |
| 3. Kimliği doğrulanmış oturumu | Uygulama hizmeti kimliği doğrulanmış tanımlama bilgisi yanıta ekler. | Uygulama hizmeti için istemci kodu kendi kimlik doğrulama belirtecini döndürür. |
| 4. Kimliği doğrulanmış içerik sunmak | İstemci kimlik doğrulama tanımlama bilgisi (tarayıcı tarafından otomatik olarak ele) sonraki istekleri içerir. | İstemci kodu gösterir kimlik doğrulama simgesi `X-ZUMO-AUTH` üstbilgi (Mobile Apps istemci SDK'ları tarafından otomatik olarak işlenir). |

İstemci tarayıcıları için uygulama hizmeti otomatik olarak tüm kimliği doğrulanmamış kullanıcıları yönlendirebilirsiniz `/.auth/login/<provider>`. Kullanıcılar bir veya daha fazla ile sunabilir `/.auth/login/<provider>` sağlayıcılarına tercih kullanarak uygulamanızı oturum açmak için bağlantılar.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Yetkilendirme davranışı

İçinde [Azure portal](https://portal.azure.com), uygulama hizmeti yetkilendirme davranışı bir sayısını yapılandırabilirsiniz.

![](media/app-service-authentication-overview/authorization-flow.png)

Aşağıdaki başlıkları seçenekleri açıklanmaktadır.

### <a name="allow-all-requests-default"></a>Tüm istekleri (varsayılan)

Kimlik doğrulama ve yetkilendirme yönetilmiyor uygulama (Kapalı) hizmeti tarafından. 

Kimlik doğrulama ve yetkilendirme gerekmiyorsa veya kendi kimlik doğrulama ve yetkilendirme kodu yazmak istiyorsanız bu seçeneği belirleyin.

### <a name="allow-only-authenticated-requests"></a>Yalnızca kimliği doğrulanmış istekler izin ver

Seçenek **ile oturum \<sağlayıcısı >**. Uygulama hizmeti tüm anonim isteklerine yönlendiren `/.auth/login/<provider>` seçtiğiniz sağlayıcısı. Anonim İstek yerel mobil uygulamadan geliyorsa, döndürülen yanıt olan bir `HTTP 401 Unauthorized`.

Bu seçenek ile tüm kimlik doğrulama kodu, uygulamanızda yazma gerekmez. Role özgü yetkilendirme gibi daha hassas yetkilendirme kullanıcının talepleri inceleyerek işlenebilir (bkz [erişim kullanıcı taleplerini](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Tüm isteklere izin vermek, ancak kimliği doğrulanmış istekler doğrula

Seçenek **Anonime istekleri**. Bu seçenek, kimlik doğrulama ve yetkilendirme App Service'te açar ancak yetkilendirme kararları, uygulama kodunuzun erteler. Kimliği doğrulanmış istekler için App Service kimlik doğrulama bilgilerini HTTP üst bilgilerinde boyunca da geçirir. 

Bu seçenek anonim istekler işleme daha fazla esneklik sağlar. Örneğin, sağlar [çoklu oturum açma seçenekleri sunmak](app-service-authentication-how-to.md#configure-multiple-sign-in-options) kullanıcılarınıza. Bununla birlikte, kod yazmak zorunda. 

## <a name="more-resources"></a>Diğer kaynaklar

[Kimlik doğrulama ve yetkilendirme App Service'te özelleştirme](app-service-authentication-how-to.md)

Sağlayıcıya özgü nasıl yapılır kılavuzları:

* [Azure Active Directory oturum açma kullanmak için uygulamanızı yapılandırma][AAD]
* [Facebook oturum açma kullanmak için uygulamanızı yapılandırma][Facebook]
* [Google oturum açma kullanmak için uygulamanızı yapılandırma][Google]
* [Microsoft Account oturum açma kullanmak için uygulamanızı yapılandırma][MSA]
* [Twitter oturum açma kullanmak için uygulamanızı yapılandırma][Twitter]
* [Nasıl yapılır: uygulamanız için özel kimlik doğrulamasını kullan][custom-auth]

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
