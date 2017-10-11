---
title: "Azure AD v2.0 uç noktasına değişiklikleri | Microsoft Docs"
description: "Uygulama modeli v2.0 Genel Önizleme protokolleri yapılan değişikliklerin bir açıklaması."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae73833a68db14804dc40eaf07ff7d3effaa9052
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a>Önemli güncelleştirmeleri v2.0 kimlik doğrulama protokolleri
Uyarı geliştiriciler! Sonraki iki hafta boyunca size birkaç güncelleştirmeleri bizim Önizleme dönemi boyunca yazılmış uygulamalar için önemli değişiklikler olduğu anlamına gelebilir bizim v2.0 kimlik doğrulama protokolleri için yapacak.  

## <a name="who-does-this-affect"></a>Kimin bu etkiliyor mu?
V2.0 kullanmak için yazılmış olan tüm uygulamaların kimlik doğrulama uç noktası Yakınsanan,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

V2.0 uç noktası hakkında daha fazla bilgi bulunabilir [burada](active-directory-appmodel-v2-overview.md).

V2.0 uç noktası doğrudan v2.0 protokol kodlayarak kullanarak bir uygulama oluşturduysanız bizim Openıd Connect veya OAuth web middlewares birini kullanarak veya kimlik doğrulaması gerçekleştirmek için 3 diğer taraf kitaplıklar kullanılarak, projelerinizi test ve değişiklikler yapmak için hazırlıklı olmalıdır Gerekirse.

## <a name="who-doesnt-this-affect"></a>Kimin bu etkilemez?
Üretim Azure AD kimlik doğrulama uç noktası karşı yazılan tüm uygulamalar

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Bu protokol taş ayarlanır ve herhangi bir değişiklik yaşayan değil.

Ayrıca, uygulamanızı **yalnızca** kimlik doğrulaması gerçekleştirmek için bizim ADAL kitaplığını kullanır değişikliği gerekmez.  ADAL değişiklikleri uygulamanızdan tam korumalı.  

## <a name="what-are-the-changes"></a>Değişiklikleri nelerdir?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>JWT üstbilgileri x5t değeri kaldırma
V2.0 uç belirteç hakkında ilgili meta veriler üstbilgi parametreleri bölümüyle içerir JWT belirteçleri yaygın, kullanır.  Bizim geçerli Jwt'ler birini üstbilgisi kod çözme, aşağıdakine benzer bulur:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Burada "x5t" ve "anlaşmalı" özellikleri Openıd Connect meta veri uç noktasından alınan olarak belirtecin imzayı doğrulamak için kullanılacak ortak anahtarı tanımlayın.

Burada yapıyoruz "x5t" özelliği kaldırmak için farklıdır.  Belirteçleri doğrulamak için mekanizmalarının aynısını kullanmaya devam edebilir, ancak doğru ortak anahtar Openıd Connect Protokolü belirtilen olarak almak için yalnızca "anlaşmalı" özelliğini yararlanmalıdır. 

> [!IMPORTANT]
> **İşinizi: uygulamanızı x5t değeri varlığını bağlı değildir emin olun.**
> 
> 

### <a name="removing-profileinfo"></a>Profile_info kaldırma
Daha önce v2.0 uç noktası bir base64 ile kodlanmış JSON nesnesi adı verilen belirteç yanıtları döndürme edilmiş `profile_info`.  Bir erişim belirteci v2.0 uç noktasından bir istek göndererek isterken:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Yanıt aşağıdaki JSON nesnesi gibi görünür:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info` Uygulamaya - kendi görünen ad, ad, Soyadı, e-posta adresi, tanımlayıcı ve benzeri oturum açan kullanıcı bilgilerini bulunan değer.  Öncelikle, `profile_info` belirteç önbelleğe alma işlemi için kullanılan ve nedeniyle görüntüler.

Biz şimdi kaldırma `profile_info` değer – ancak endişelenmeyin, biz yine bu bilgiler biraz farklı bir yerde geliştiricilere sağlanmaktadır.  Yerine `profile_info`, v2.0 uç şimdi döndürülecek bir `id_token` her belirteci yanıt:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Kod çözme ve profile_info alınan aynı bilgileri almak için id_token ayrıştırılamadı.  İd_token bir JSON Web Token (JWT), Openıd Connect tarafından belirtilen içeriğiyle ' dir.  Bunu yapmak için kod kadar çok benzer olmalıdır – id_token Orta segment (gövde) ayıklamak yeterlidir ve base64 JSON nesnesi içinde erişmek için kod çözme.

Sonraki iki hafta içinde kullanıcı bilgilerini herhangi birinden almak için uygulamanızı kod `id_token` veya `profile_info`; hangisi mevcuttur.  Değişiklik yapıldığında bu şekilde, uygulamanızı sorunsuz geçiş işleyebilir `profile_info` için `id_token` kesinti olmadan.

> [!IMPORTANT]
> **İşinizi: uygulamanızı varlığını bağlı değildir emin olun `profile_info` değeri.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>İd_token_expires_in kaldırma
Benzer şekilde `profile_info`, biz de kaldırma `id_token_expires_in` yanıtlardan parametresi.  Daha önce v2.0 uç noktası için bir değer döndürmesi `id_token_expires_in` her id_token yanıtta, örneğin bir authorize yanıt yanı sıra:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Ya da belirteç yanıtı:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in` Değeri id_token kalması için geçerli saniye sayısını belirtmek.  Şimdi, biz kaldırıyorsunuz `id_token_expires_in` tamamen değeri.  Bunun yerine, Openıd Connect standart kullanabilir `nbf` ve `exp` bir id_token geçerliliğini incelemek talep.  Bkz: [v2.0 belirteç başvurusu](active-directory-v2-tokens.md) bu talepler hakkında daha fazla bilgi için.

> [!IMPORTANT]
> **İşinizi: uygulamanızı varlığını bağlı değildir emin olun `id_token_expires_in` değeri.**
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a>Kapsam tarafından döndürülen talep değiştirme openıd =
Bu değişiklik en önemli – aslında, v2.0 uç noktası kullanan neredeyse her uygulama etkiler.  Birçok uygulama, v2.0 uç noktası için kullanılacak istekleri göndermek `openid` gibi kapsam:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Bugün, ne zaman kullanıcıya izin verir `openid` kapsam, uygulamanızın elde edilen id_token çok sayıda kullanıcı hakkındaki bilgileri alır.  Bu talep adlarıyla, tercih edilen kullanıcı adı, e-posta adresi, nesne kimliği ve daha fazla bilgi içerebilir.

Bu güncelleştirme, biz bilgileri değiştiriyorsunuz, `openid` kapsam için daha iyi comform Openıd Connect belirtimiyle, uygulama erişimini ortaya koymaktadır.  `openid` Kapsam yalnızca kullanıcı oturum sağlamak ve kullanıcı için uygulamaya özgü tanımlayıcıyı alma `sub` id_token talep.  Yalnızca bir id_token Taleplerde `openid` verilen kapsam herhangi bir kişisel bilgi yoktur olacaktır.  Örnek id_token talep şunlardır:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Uygulamanızda kullanıcıyla ilgili kişisel bilgileri (PII) elde etmek istiyorsanız, uygulamanızı kullanıcıdan ek izinler istemeniz gerekir.  Openıd Connect spec – iki yeni kapsamlar için destek sunuyoruz `email` ve `profile` , bunu yapmak izin kapsamları –.

* `email` Kapsamı çok basit – kullanıcının birincil e-posta adresi uygulama erişiminizi sağlayan `email` id_token talep.  Unutmayın `email` talep her zaman olmayacak id_tokens içinde mevcut – yalnızca kullanıcının profilinde varsa dahil edilir.
* `profile` Kapsam ortaya koymaktadır tüm diğer ilgili temel bilgileri kendi adı, tercih edilen kullanıcı adı, bir kullanıcının, uygulama erişimini nesne kimliği ve benzeri.

Bu sayede uygulamanız açığa en az bir şekilde kod – yalnızca uygulamanızı işini yapmak için gerektirdiği bilgiler kümesi için kullanıcıya sor.  Kümesini uygulamanız şu anda alıyor kullanıcı bilgileri alma devam etmek istiyorsanız, tüm üç kapsamlar yetkilendirme isteklerinizi içermelidir:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Uygulamanızı göndermeye başlamak `email` ve `profile` hemen kapsamlar ve v2.0 uç noktası bu iki kapsamları kabul ve gerektiği şekilde kullanıcıların izinleri isteyen başlamak.  Ancak, yorumu değişikliği `openid` kapsamı değil kazanacak için birkaç hafta.

> [!IMPORTANT]
> **İşinizi: eklemek `profile` ve `email` uygulamanızı kullanıcı hakkındaki bilgileri gerektiriyorsa kapsamları.**  ADAL bu izinlerin ikisini de isteklerinde varsayılan olarak dahil edeceğini unutmayın. 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a>Sondaki eğik çizgi veren kaldırılıyor.
Daha önce v2.0 uç noktasından belirteçleri görünen veren değeri form sürdü

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

GUID, belirtecin Azure AD kiracısı Tenantıd olduğu.  Bu değişikliklerle veren değeri olur

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

Her iki simge ve Openıd Connect bulma belge.

> [!IMPORTANT]
> **İşinizi: uygulamanızı veren doğrulama sırasında hem ile hem de eğik olmadan veren değeri kabul ettiğinden emin olun.**
> 
> 

## <a name="why-change"></a>Neden değişiyor?
Bu değişiklikler tanıtımı için birincil motivasyon Openıd Connect standart belirtimi ile uyumlu olacak.  Openıd Connect uyumlu olma yoluyla Microsoft Identity hizmetleriyle ve diğer endüstri kimlik Hizmetleri ile tümleştirme arasındaki farklar en aza indirmek umuyoruz.  Microsoft farklara uyum sağlamak için kitaplıkları değiştirmek zorunda kalmadan kendi sık kullanılan açık kaynak kimlik doğrulama kitaplıkları kullanabilirsiniz, geliştiricilerin istiyoruz.

## <a name="what-can-you-do"></a>Ne yapabilirsiniz?
Bugün itibariyle, yukarıda açıklanan tüm değişiklikleri yapma başlayabilirsiniz.  Hemen yapmanız gerekir:

1. **Bağımlılıkları kaldırmak `x5t` Üstbilgi parametresi.**
2. **Geçiş işleyebilmesini `profile_info` için `id_token` belirteci yanıtlarındaki.**
3. **Bağımlılıkları kaldırmak `id_token_expires_in` yanıt parametresi.**
4. **Ekleme `profile` ve `email` uygulamanızı temel kullanıcı bilgileri gerektiriyorsa, uygulamanızın kapsamları.**
5. **Hem ile hem de eğik olmadan belirteçleri veren değerleri kabul edin.**

Bizim [v2.0 protokolü belgeleri](active-directory-v2-protocols.md) zaten kodunuzu güncelleştirin yardımcı olacak başvuru olarak kullanabilir şekilde bu değişiklikleri yansıtacak şekilde güncelleştirildi.

Değişiklikleri kapsamını herhangi bir sorunuz varsa, lütfen Twitter'da Bize Ulaşın çekinmeyin @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Protokol değişiklikleri sıklığını?
Şu kimlik doğrulama protokolleri tüm daha fazla kesme değiştirir öngörüyor musunuz.  İstediğiniz zaman tekrar yakında güncelleştirme işlemini bu tür gitmek zorunda kalmazsınız böylece Biz bu değişiklikleri bir yayın alanına bilerek paketleme.  Elbette, avantajlarından yararlanmak yakınsanmış v2.0 kimlik doğrulama hizmeti özellikleri eklemek devam, ancak bu değişiklikleri ADDITIVE ve kod varolan sonu olması gerekir.

Son olarak, Önizleme dönemi boyunca şeyler çalışırken için teşekkür ederiz söylemek isteriz.  Öngörüler ve bizim erken Benimseyenler deneyimlerini bugüne kadarki çok atanmış olması ve sizin görüşlerini paylaşın devam edeceğiz umuyoruz.

Mutluluk kodlama!

Microsoft Identity bölme

