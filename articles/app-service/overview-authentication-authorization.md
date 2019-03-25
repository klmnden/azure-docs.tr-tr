---
title: Kimlik doğrulama ve yetkilendirme - Azure App Service | Microsoft Docs
description: Kavramsal başvurusu ve genel kimlik doğrulama / yetkilendirme özelliğini Azure App Service için
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/24/2018
ms.author: mahender,cephalin
ms.custom: seodec18
ms.openlocfilehash: d914e3ad3043b2671e154d1616c6800f34415c11
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402754"
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde kimlik doğrulaması ve yetkilendirme

> [!NOTE]
> Şu anda AAD V2 (MSAL dahil) desteklenmez Azure App Services ve Azure işlevleri için. Lütfen geri güncelleştirmeleri denetleyin.
>

Azure App Service, yerleşik kimlik doğrulama ve yetkilendirme destekler, böylece kullanıcılarının oturumunu ve en az yazma ya da web uygulaması, RESTful API ve mobil arka uç, kod erişim verileri sağlar ve ayrıca [Azure işlevleri](../azure-functions/functions-overview.md). Bu makalede, App Service'nın kimlik doğrulama ve uygulamanız için yetkilendirme basitleştirmek nasıl yardımcı olduğunu açıklar. 

Güvenli kimlik doğrulaması ve yetkilendirme gerektirir derin bir anlayış, Federasyon dahil olmak üzere güvenlik, şifreleme, [JSON web belirteçleri (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) Yönetimi [verme türlerini](https://oauth.net/2/grant-types/)ve benzeri. App Service bu yardımcı programlar sağladığından müşterinize iş değeri sağlama hakkında daha fazla zaman ve enerji harcar.

> [!NOTE]
> App Service kimlik doğrulama ve yetkilendirme için kullanmak zorunda değilsiniz. Birçok web çerçeveleri ile güvenlik özellikleri paketlenir ve isterseniz kullanabilirsiniz. App Service sağladığından daha fazla esneklik gerekiyorsa kendi yardımcı programları da yazabilirsiniz.  
>

Yerel mobil uygulamalar için özel bilgiler için bkz: [kullanıcı kimlik doğrulaması ve Azure App Service ile mobil uygulamalar için yetkilendirme](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama ve yetkilendirme modülü uygulama kodunuz aynı sanal çalıştırır. Etkinleştirildiğinde, gelen her HTTP isteği aracılığıyla uygulama kodunuz tarafından işlenen önce geçirir.

![](media/app-service-authentication-overview/architecture.png)

Bu modül, uygulamanız için birkaç şey işler:

- Belirtilen sağlayıcı ile kullanıcıların kimliğini doğrular
- Doğrular, depolar ve belirteçleri yeniler
- Kimliği doğrulanan oturum yönetir
- İstek üst bilgisini kimlik ekler

Modül, uygulama kodunuz içinden ayrı olarak çalışır ve uygulama ayarlarını kullanarak yapılandırılır. Hiçbir SDK'ları, belirli diller veya uygulama kodunuzda değişiklikler gereklidir. 

### <a name="user-claims"></a>Kullanıcı talepleri

Tüm dil çerçeveleri için App Service kullanıcı talepleri istek üst bilgileri düzeyine ekleyerek kodunuzu kullanılmasını sağlar. ASP.NET 4.6 apps için App Service doldurur [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) kimliği doğrulanmış kullanıcının talepleri ile standart .NET kod desenini izleyebilmeniz dahil olmak üzere `[Authorize]` özniteliği. Benzer şekilde, App Service için PHP uygulamaları, doldurur `_SERVER['REMOTE_USER']` değişkeni.

İçin [Azure işlevleri](../azure-functions/functions-overview.md), `ClaimsPrincipal.Current` .NET kodu için aktarıldıktan değildir ancak yine de kullanıcı taleplerini istek üst bilgilerinde bulabilirsiniz.

Daha fazla bilgi için [erişim kullanıcı taleplerini](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Belirteç Deposu

App Service web uygulamaları, API'leri, yerel mobil uygulamalar veya kullanıcılarla ilişkili belirteçleri deposudur yerleşik bir belirteç deposu sağlar. Herhangi bir sağlayıcı ile kimlik doğrulaması etkinleştirdiğinizde, bu belirteç deposu uygulamanıza hemen kullanılabilir. Uygulama kodunuzun verileri kullanıcının adına bu sağlayıcılardan gibi erişmesi gerekiyorsa: 

- Kimliği doğrulanmış kullanıcının Facebook zaman tünelinde Yayınla
- Azure Active Directory Graph API'sini ya da Microsoft Graph bile kullanıcının şirket verilerini okuyun

Genellikle toplamak, depolamak ve bu belirteçler, uygulamanızda yenileme için kod yazmanız gerekir. Belirteç Deposu ile yeni [belirteçlerini almak](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) gerektiğinde bunları ve [yenilemek için App Service söyleyin](app-service-authentication-how-to.md#refresh-identity-provider-tokens) zaman haline gelmeden geçersiz. 

Kimliği doğrulanan oturum için kimliği belirteçleri, erişim belirteci ve yenileme belirteçleri önbelleğe ve bunlar yalnızca ilişkili kullanıcı tarafından erişilebilir.  

Uygulamanızda belirteçlerle çalışma gerekmiyorsa, belirteç deposu devre dışı bırakabilirsiniz.

### <a name="logging-and-tracing"></a>Günlüğe kaydetme ve izleme

Varsa, [Uygulama günlüğünü etkinleştirin](troubleshoot-diagnostic-logs.md), kimlik doğrulama ve yetkilendirme izlemeleri doğrudan günlük dosyalarınızı görürsünüz. Beklemediğiniz bir kimlik doğrulama hatası görürseniz, mevcut uygulama günlüklerinizi bakarak tüm ayrıntılarını kolayca bulabilirsiniz. Etkinleştirirseniz [başarısız istek izlemeyi](troubleshoot-diagnostic-logs.md)gördüğünüz tam olarak hangi rolü kimlik doğrulaması ve yetkilendirme modülü, başarısız bir istek içinde yürütülen. Başvurular adlı bir modül için izleme günlüklerine bakın `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Kimlik sağlayıcıları

App Service kullanan [Federasyon kimliği](https://en.wikipedia.org/wiki/Federated_identity), hangi üçüncü taraf kimlik sağlayıcısı kullanıcı kimliklerini ve kimlik doğrulama akışı, yönetir. Beş adet kimlik sağlayıcısı, varsayılan olarak kullanılabilir: 

| Sağlayıcı | Oturum açma uç noktası |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft Hesabı](../active-directory/develop/v2-overview.md) | `/.auth/login/microsoftaccount` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |

Kimlik doğrulaması ve yetkilendirme ile bu sağlayıcılardan birini etkinleştirdiğinizde, oturum açma uç noktası sağlayıcıdan kimlik doğrulama belirteçleri doğrulama ve kullanıcı kimlik doğrulaması için kullanılabilir. Bu oturum açma seçenekleri herhangi bir sayıda ile kullanıcılarınıza kolayca sağlayabilir. Başka bir kimlik sağlayıcısı tümleştirebilirler veya [kendi özel kimlik çözümünüz][custom-auth].

## <a name="authentication-flow"></a>Kimlik doğrulama akışı

Kimlik doğrulama akışı, tüm sağlayıcılar için aynıdır, ancak oturum sağlayıcısının SDK imzalamak istediğinize bağlı olarak farklılık gösterir:

- SDK sağlayıcısı: Federasyon oturum açma için App Service uygulama atar. Sağlayıcının oturum açma sayfasının kullanıcıya sunabilir tarayıcı uygulamaları, durum genellikle budur. Ayrıca çağrıldığı için sunucu kodunu oturum açma işlemi yönetir _sunucu yönlendirilmiş akış_ veya _sunucu akışı_. Bu durumda, tarayıcı uygulamaları için geçerlidir. SDK'sı App Service kimlik doğrulaması kullanıcıların oturum açmak için bir web görünümü açar çünkü Mobile Apps istemci SDK'sı kullanarak kullanıcıların oturum yerel uygulamalar için de geçerlidir. 
- SDK sağlayıcısı ile: Uygulama, kullanıcıların sağlayıcısına el ile imzalar ve ardından App Service doğrulaması için kimlik doğrulama belirteci gönderir. Sağlayıcının oturum açma sayfasının, kullanıcıya sunmak olamaz, tarayıcı olmayan uygulamaları ile durum genellikle budur. Ayrıca çağrıldığı için uygulama kodu oturum açma işlemini yönetir _istemci yönlendirilmiş akış_ veya _istemci akışı_. Bu durumda REST API'leri için geçerli [Azure işlevleri](../azure-functions/functions-overview.md)ve JavaScript tarayıcı istemcilerinin, hem de tarayıcı uygulamaları, oturum açma işleminde daha fazla esnekliğe ihtiyacınız. Sağlayıcının SDK'sını kullanarak kullanıcıların oturum yerel mobil uygulamalar için de geçerlidir.

> [!NOTE]
> App Service güvenilen tarayıcı uygulamasında çağrılarından App Service'te başka bir REST API çağrıları veya [Azure işlevleri](../azure-functions/functions-overview.md) sunucu yönlendirilmiş akışını kullanarak kimlik doğrulaması yapılabilir. Daha fazla bilgi için [özelleştirme kimlik doğrulaması ve yetkilendirme App Service'te](app-service-authentication-how-to.md).
>

Aşağıdaki tabloda kimlik doğrulama akışının adımları gösterilmektedir.

| Adım | SDK sağlayıcısı | SDK sağlayıcısı ile |
| - | - | - |
| 1. Kullanıcı olarak oturum açın | İstemciye yönlendirir `/.auth/login/<provider>`. | İstemci kodu doğrudan sağlayıcısının SDK ile kullanıcı oturum açtığında ve bir kimlik doğrulama belirteci alır. Bilgi için sağlayıcının belgelerine bakın. |
| 2. Kimlik doğrulaması sonrası | Sağlayıcı istemciye yönlendirir `/.auth/login/<provider>/callback`. | İstemci kodu [sağlayıcısından belirteç gönderir](app-service-authentication-how-to.md#validate-tokens-from-providers) için `/.auth/login/<provider>` doğrulama için. |
| 3. Kimliği doğrulanmış oturumu | App Service, kimliği doğrulanmış bir tanımlama bilgisi yanıta ekler. | App Service, istemci kodu için kendi kimlik doğrulama belirteci döndürür. |
| 4. Kimliği doğrulanmış içerik sunma | İstemci kimlik doğrulama tanımlama bilgisi (tarayıcı tarafından otomatik olarak işlenir) sonraki istekleri içerir. | İstemci kodu sunan kimlik doğrulaması belirtecinde `X-ZUMO-AUTH` üst bilgisi (Mobile Apps istemci SDK'ları tarafından otomatik olarak işlenir). |

İstemci tarayıcıları için App Service otomatik olarak tüm kimliği doğrulanmamış kullanıcılar için yönlendirebilir `/.auth/login/<provider>`. Kullanıcıların bir veya daha fazla sunabilir `/.auth/login/<provider>` seçtiğiniz sağlayıcının kullanarak uygulamanıza oturum açmak için bağlantılar.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Yetkilendirme davranışı

İçinde [Azure portalında](https://portal.azure.com), App Service yetkilendirme davranışları bir dizi yapılandırabilirsiniz.

![](media/app-service-authentication-overview/authorization-flow.png)

Aşağıdaki başlıklar seçenekleri açıklanmaktadır.

### <a name="allow-all-requests-default"></a>(Varsayılan) tüm isteklere izin ver

Kimlik doğrulama ve yetkilendirme (Kapalı) App Service tarafından yönetilmez. 

Kimlik doğrulama ve yetkilendirme ihtiyacınız yoksa veya kendi kimlik doğrulama ve yetkilendirme kodu yazmak istiyorsanız bu seçeneği belirleyin.

### <a name="allow-only-authenticated-requests"></a>Yalnızca kimliği doğrulanmış isteklere izin ver

Seçenek **oturum açmada \<sağlayıcısı >**. App Service, tüm anonim isteklere yönlendiren `/.auth/login/<provider>` seçtiğiniz sağlayıcısı. Anonim İstek yerel mobil uygulamadan geliyorsa, döndürülen yanıt bir `HTTP 401 Unauthorized`.

Bu seçenek belirtilmişse, uygulamanızda kimlik doğrulaması kod yazmaya gerek yoktur. Role özgü yetkilendirme gibi hassas yetkilendirme, kullanıcı talepleri inceleyerek işlenebilir (bkz [erişim kullanıcı taleplerini](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Tüm isteklere izin ver, ancak kimliği doğrulanmış istekler doğrula

Seçenek **Anonime istekleri**. Bu seçenek, kimlik doğrulama ve yetkilendirme App Service'te açar, ancak uygulama kodunuzda yetkilendirme kararlarını erteleyen. Kimliği doğrulanmış istekler için App Service kimlik doğrulama bilgilerini HTTP üstbilgileri boyunca ayrıca geçirir. 

Bu seçenek, anonim isteklerin işlenmesinden daha fazla esneklik sağlar. Örneğin, sağlar [birden çok oturum açma sağlayıcısı sunmak](app-service-authentication-how-to.md#use-multiple-sign-in-providers) kullanıcılarınıza. Ancak, kod yazmanız gerekir. 

## <a name="more-resources"></a>Diğer kaynaklar

[Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca (Windows) Azure App Service'te yetkilendirme](app-service-web-tutorial-auth-aad.md)  
[Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca Azure App Service'te Linux için yetkilendirme](containers/tutorial-auth-aad.md)  
[Kimlik doğrulama ve yetkilendirme App Service'te özelleştirme](app-service-authentication-how-to.md)

Sağlayıcıya özgü nasıl yapılır kılavuzları:

* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma][AAD]
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma][Facebook]
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma][Google]
* [Microsoft Account login kullanmak için uygulamanızı yapılandırma][MSA]
* [Uygulamanızı twitter oturum açma bilgilerini kullanacak şekilde yapılandırma][Twitter]
* [Nasıl yapılır: Uygulamanız için özel kimlik doğrulaması kullan][custom-auth]

[AAD]: configure-authentication-provider-aad.md
[Facebook]: configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: configure-authentication-provider-microsoft.md
[Twitter]: configure-authentication-provider-twitter.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
