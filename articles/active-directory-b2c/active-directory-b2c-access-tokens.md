---
title: Azure Active Directory B2C erişim belirteçlerinde isteme | Microsoft Docs
description: Bu makalede erişim belirteci alma ve bir istemci uygulamanın kurulumunu yapmayı gösterir.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/09/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f3db56c7ce61960fca0e5347b2385bcc65a88354
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845155"
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Erişim belirteçleri isteniyor

Bir erişim belirteci (olarak gösterilen **erişim\_belirteci** yanıtlarındaki Azure AD B2C'den) tarafından korunan bir istemci, kaynaklara erişmek için kullanabileceğiniz güvenlik belirteci biçimidir bir [yetkilendirme sunucusu](active-directory-b2c-reference-protocols.md), web API'si gibi. Erişim belirteçleri olarak temsil edilir [Jwt'ler](active-directory-b2c-reference-tokens.md) ve hedeflenen kaynak sunucu ve sunucu için verilen izinleri hakkındaki bilgileri içerir. Kaynak sunucuda çağırırken erişim belirteci HTTP isteğinde bulunması gerekir.

Bu makalede, bir istemci uygulamasını yapılandırmak ve almak için web API'si anlatılmaktadır bir **erişim\_belirteci**.

> [!NOTE]
> **Web API'si zincirleri (On-Behalf-Of) Azure AD B2C tarafından desteklenmiyor.**
>
> Çoğu mimari başka bir aşağı akış web API'si, hem Azure AD B2C ile güvenliği sağlanan çağırmak için gereken API web içerir. Bu senaryoda, sırasıyla bir Azure AD Graph API gibi Microsoft çevrimiçi hizmeti çağıran bir web API arka ucu sahip yerel istemcilerde yaygındır.
>
> Bu Zincirli web API'si senaryosu, aksi takdirde On-Behalf-Of akışı bilinen OAuth 2.0 JWT taşıyıcı kimlik bilgileri verme kullanılarak desteklenebilir. Ancak, On-Behalf-Of akışı şu anda Azure AD B2C'de uygulanmamıştır.

## <a name="register-a-web-api-and-publish-permissions"></a>Web API'si kaydetme ve izinleri yayımlama

Bir erişim belirteci isteğinde bulunmadan önce ilk web API'si kaydetme ve verilebilecek izinleri (kapsamları) istemci uygulaması yayımlama gerekir.

### <a name="register-a-web-api"></a>Web API’si kaydetme

1. Azure portalında Azure AD B2C özellikleri menüsünde tıklatın **uygulamaları**.
2. Tıklayın **+ Ekle** menüsünün üstünde.
3. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso API" girebilirsiniz.
4. **Web uygulamasını / web API’sini dahil et** anahtarını **Evet**’e getirin.
5. Rastgele bir değer girin **yanıt URL'leri**. Örneğin, `https://localhost:44316/` girin. API belirteci doğrudan Azure AD B2C'den almalıdır değil beri değer önemli değildir.
6. Bir **Uygulama Kimliği URI'si** girin. Bu değer, web API’niz için kullanılan tanımlayıcıdır. Örneğin, 'Not' kutuya girin. **Uygulama kimliği URI'si** ardından olacaktır `https://{tenantName}.onmicrosoft.com/notes`.
7. Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.
8. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.

### <a name="publishing-permissions"></a>Yayımlama izinleri

Kapsamlar, izinler için benzer bir API uygulamanızı çağırırken gereklidir. Bazı kapsamların "Okuma" veya "yazma" verilebilir. Web veya yerel uygulamanızı "API'den okumak için" istediğinizi varsayalım. Uygulamanız Azure AD B2C'yi arayın ve "Okuma" kapsamı için erişim sağlayan bir erişim belirteci isteği. Bu tür bir erişim belirteci yaymak Azure AD B2C için uygulamayı "belirli API'SİNDEN okuma" izni gerekir. Bunu yapmak için API'NİZİN ilk "Okuma" kapsam yayımlamak gerekir.

1. Azure AD B2C içinde **uygulamaları** menüsünde, open web API'si uygulaması ("Contoso API").
2. **Yayımlanan kapsamlar**’a tıklayın. Burada diğer uygulamalara verilebilecek izinleri (kapsamları) tanımlarsınız.
3. Ekleme **kapsam değerleri** gerektiğinde (örneğin, "Okuma"). Varsayılan olarak, "user_impersonation" kapsamı tanımlanır. İsterseniz, bunu göz ardı edebilirsiniz. Kapsam içinde bir açıklama girin **kapsam adı** sütun.
4. **Kaydet**’e tıklayın.

> [!IMPORTANT]
> **Kapsam adı** açıklamasıdır **kapsam değeri**. Kapsam kullanırken kullandığınızdan emin olun **kapsam değeri**.

## <a name="grant-a-native-or-web-app-permissions-to-a-web-api"></a>Bir Web API'sine uygulama izinleri web veya yerel verin

API kapsamları yayımlamak için yapılandırıldıktan sonra istemci uygulamasının Azure portalından bu kapsamları verilmesi gerekir.

1. Gidin **uygulamaları** Azure AD B2C özellikleri menüsünde menüsü.
2. Bir istemci uygulama kaydetme ([web uygulaması](active-directory-b2c-app-registration.md) veya [yerel istemci](active-directory-b2c-app-registration.md)) zaten yoksa. Başlangıç noktası olarak bu kılavuzu takip ediyorsanız, bir istemci uygulaması kaydetmeniz gerekir.
3. Tıklayarak **API erişimi**.
4. **Ekle**'ye tıklayın.
5. Web API'nizi ve vermek istediğiniz kapsamlarınızı (izinler) seçin.
6. **Tamam** düğmesine tıklayın.

> [!NOTE]
> Azure AD B2C, istemci uygulama kullanıcıları için kendi onay istemez. Bunun yerine, tüm onay, yukarıda açıklanan uygulamalar arasında yapılandırılmış izinlere dayalı olarak bir yönetici tarafından sağlanır. Bir uygulama için bir iznini iptal edilirse, bu izni almak daha önce mümkün tüm kullanıcılar artık bunu mümkün olacaktır.

## <a name="requesting-a-token"></a>Bir belirteç isteme

İstemci uygulama bir erişim belirteci isteğinde bulunurken istenen izinler belirtilmesi gerekiyor **kapsam** isteğinin parametresi. Örneğin, belirtmek için **kapsam değeri** "Okuma" olan API için **uygulama kimliği URI'si** , `https://contoso.onmicrosoft.com/notes`, kapsamı olacaktır `https://contoso.onmicrosoft.com/notes/read`. Bir yetkilendirme kodu isteği örneği aşağıdadır `/authorize` uç noktası.

> [!NOTE]
> Şu anda, erişim belirteçleri ile birlikte özel etki alanları desteklenmez. İstek URL'sinde tenantName.onmicrosoft.com etki alanınızı kullanmanız gerekir.

```
https://<tenantName>.b2clogin.com/tfp/<tenantName>.onmicrosoft.com/<yourPolicyId>/oauth2/v2.0/authorize?client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

Aynı istekte birden fazla izin almak için birden çok girişi tek ekleyebilirsiniz **kapsam** boşluklarla ayırarak parametresi. Örneğin:

Kodu çözülen URL'si:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

URL olarak kodlanmış:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

İstemci uygulamanız için verilen değerinden bir kaynak için daha fazla kapsamlarınızı (izinler) isteyebilir. Bu durum söz konusu olduğunda, en az bir izin verilirse, çağrı başarılı olur. Ortaya çıkan **erişim\_belirteci** başarıyla verilmiş olan izinleriyle doldurulmuş kendi "scp" talep sahip olur.

> [!NOTE] 
> İzinler isteyen iki farklı web kaynaklarda aynı istekte desteklemiyoruz. Bu tür bir istek başarısız olur.

### <a name="special-cases"></a>Özel durumlar

Openıd Connect standart birkaç özel "scope" değeri belirtir. Aşağıdaki özel kapsamları "kullanıcının profilini erişim" izni temsil eder:

* **openıd**: Bu istekleri bir kimlik belirteci
* **Çevrimdışı\_erişim**: Bu bir yenileme belirteci isteklerini (kullanarak [kimlik doğrulama kodu akışları](active-directory-b2c-reference-oauth-code.md)).

Varsa `response_type` parametresinde bir `/authorize` istek içerir `token`, `scope` parametresi en az bir kaynak kapsam içermelidir (dışında `openid` ve `offline_access`), verilecek. Aksi takdirde, `/authorize` isteği ile hata sonlandırılacak.

## <a name="the-returned-token"></a>Döndürülen belirteç

İçinde başarıyla minted **erişim\_belirteci** (veya `/authorize` veya `/token` uç nokta), aşağıdaki talep mevcut olacaktır:

| Ad | İste | Açıklama |
| --- | --- | --- |
|Hedef kitle |`aud` |**Uygulama kimliği** tek kaynağının erişim belirteci verir. |
|Kapsam |`scp` |Kaynak için verilen izinler. Birden çok verilen izinler, boşlukla ayrılmış. |
|Yetkili taraf |`azp` |**Uygulama kimliği** istemci uygulamasının isteği başlatıldı. |

API'nizi aldığında **erişim\_belirteci**, aşağıdakileri yapmalıdır [belirteci doğrulamak](active-directory-b2c-reference-tokens.md) belirteç gerçek ve doğru talep olduğundan kanıtlamak için.

Her zaman geri bildirim ve öneriler için açık duyuyoruz! Bu konu ile ilgili zorluklarla varsa etiketini kullanarak Stack Overflow sitesinde sonrası ['azure-ad-b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Özellik istekleri için ekleyebilmesi [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Sonraki adımlar

* Bir web API'sini kullanarak yapı [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Bir web API'sini kullanarak yapı [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
