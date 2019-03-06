---
title: Tarayıcı olmayan cihazlardaki kullanıcılar oturum açma için Azure AD v2.0 kullanın | Microsoft Docs
description: Cihaz kodu verme kullanan katıştırılmış ve tarayıcı olmayan kimlik doğrulaması akışlar oluşturun.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 780eec4d-7ee1-48b7-b29f-cd0b8cb41ed3
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1f34796a98acb3ed4241edff57e506990479d1a
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443276"
---
# <a name="azure-active-directory-v20-and-the-oauth-20-device-code-flow"></a>Azure Active Directory v2.0 ve OAuth 2.0 cihaz kod akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Azure AD destekler [cihaz kodu verme](https://tools.ietf.org/html/draft-ietf-oauth-device-flow-12), giriş kısıtlı cihazları akıllı TV gibi IOT cihaz veya yazıcı oturum açmalarını sağlar.  Bu akış etkinleştirmek için cihaz tarayıcısında oturum açmak için başka bir cihazda kullanıcı bir Web sayfasını ziyaret edin sahiptir.  Kullanıcı oturum açtıktan sonra cihazın erişim belirteci alma ve gerektiğinde yenileme belirteçlerini kullanabilirsiniz.  

> [!Important] 
> Şu anda v2.0 uç noktası yalnızca Azure AD kiracıları, ancak değil kişisel hesaplar için cihaz akışını destekler.  Başka bir deyişle, bir kiracı olarak ayarlanmış bir uç nokta veya kuruluşlar uç nokta kullanmanız gerekir.  
>
> Bir Azure AD kiracısına davet kişisel hesaplar, cihaz akış grant, ancak yalnızca Kiracı bağlamında kullanmanız mümkün olacaktır.

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>

## <a name="protocol-diagram"></a>Protokol diyagramı

Cihazın tamamını kod akışı, sonraki diyagrama benzer. Bu makaledeki adımların her biri açıklanmaktadır.

![Cihaz kod akışı](media/v2-oauth2-device-flow/v2-oauth-device-flow.png)

## <a name="device-authorization-request"></a>Cihaz yetkilendirme isteği

İstemci kimlik doğrulama sunucusu için kimlik doğrulaması'nı başlatmak için kullanılan bir cihaz ve kullanıcı kodu ile ilk işaretlemeniz gerekir.  İstemci bu istekten toplar `/devicecode` uç noktası. Bu istekte istemcinin kullanıcıdan almak için gerekli olan izinleri de içermelidir.  Bu istek gönderilir andan itibaren kullanıcının oturum açmak için yalnızca 15 dakika sahip (normal değeri `expires_in`), bu nedenle yalnızca kullanıcının oturum açmak hazır olduklarını istendiğinde bu isteği yapmak.

```
// Line breaks are for legibility only.

POST https://login.microsoftonline.com/{tenant}/devicecode
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
scope=user.read%20openid%20profile

```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| kiracı |Gerekli |İzni istemek için istediğiniz dizinin Kiracı. Bu GUID veya kolay adı biçiminde olabilir.  |
| client_id |Gerekli |Uygulama Kimliği [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) uygulamanıza atanan. |
| scope | Önerilen | Boşlukla ayrılmış bir listesini [kapsamları](v2-permissions-and-consent.md) onay kullanıcıya istiyor.  |

### <a name="device-authorization-response"></a>Cihaz kimlik doğrulama yanıtı

Başarılı yanıt, oturum açmak izin vermek için gerekli bilgileri içeren bir JSON nesnesi olur.  

| Parametre | Biçimlendir | Açıklama |
| ---              | --- | --- |
|`device_code`     |String| Yetkilendirme sunucusu ile istemci arasında oturum doğrulamak için kullanılan bir uzun dize.  Yetkilendirme sunucusundan erişim belirteci istemek için bu istemci tarafından kullanılır. |
|`user_code`       |String| İkincil bir cihazda oturum tanımlamak için kullanılan kullanıcıya gösterilen kısa bir dize.|
|`verification_uri`|URI| Kullanıcı Git ile URI `user_code` oturum açmak için. |
|`verification_uri_complete`|URI| Bir URI birleştirme `user_code` ve `verification_uri`metinsel olmayan iletilmesi kullanıcı (örneğin, Bluetooth aracılığıyla bir cihaz için veya bir QR kodu aracılığıyla) için kullanılır.  |
|`expires_in`      |int| Saniye önce `device_code` ve `user_code` süresi dolar. |
|`interval`        |int| İstemci yoklama istekleri arasında beklemesi gereken saniye sayısı. |
| `message`        |String| Kullanıcı için yönergeler insanlar tarafından okunabilen bir dize.  Bu dahil ederek yerelleştirilebilen bir **sorgu parametresi** istek formunun `?mkt=xx-XX`, uygun dil kültür kodu doldurma. |

## <a name="authenticating-the-user"></a>Kullanıcı kimlik doğrulaması

Alma sonra `user_code` ve `verification_uri`, istemci bu kullanıcı için bunları cep telefonu veya bilgisayar tarayıcı kullanarak oturum açması söyleyen görüntüler.  İstemci görüntülemek için bir QR kodu veya benzer bir mekanizma ayrıca kullanabilirsiniz `verfication_uri_complete`, girme adım alacağınız `user_code` kullanıcı.

Kullanıcı, kimlik doğrulaması sırasında `verification_uri`, istemci yoklama `/token` istenen belirteç kullanmaya yönelik uç nokta `device_code`.

``` 
POST https://login.microsoftonline.com/tenant/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type: urn:ietf:params:oauth:grant-type:device_code
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8
```

|Parametre | Gerekli | Açıklama|
| -------- | -------- | ---------- |
|`grant_type` | Gerekli| olmalıdır `urn:ietf:params:oauth:grant-type:device_code`|
|`client_id`  | Gerekli| Eşleşmelidir `client_id` ilk istekte kullanılan. |
|`device_code`| Gerekli| `device_code` Cihaz yetkilendirme isteğine döndürdü.  |

### <a name="expected-errors"></a>Beklenen hataları

Cihaz kod akışı, bir yoklama Protokolü olduğundan, istemci kullanıcı kimlik doğrulaması bitmeden önce hataları almayı gerekir.  

| Hata | Açıklama | İstemci eylemi |
|------ | ----------- | -------------|
| `authorization_pending` |  Kullanıcı kimlik doğrulaması henüz tamamlanmadı ancak Akış iptal etti değil. | İstekten sonra yineleyin en az `interval` saniye. |
| `authorization_declined`|  Son kullanıcı yetkilendirme isteği reddedildi.| Yoklama durdurun ve kimliği doğrulanmamış bir duruma geri döndürebilir.  |
| `bad_verification_code`|`device_code` Gönderilen `/token` uç nokta tanınmadı. | İstemci doğru gönderdiğini doğrulayın `device_code` istek. |
| `expired_token`|  En az `expires_in` saniye geçtikten ve kimlik doğrulama artık bu mümkün `device_code`. | Yoklama durdurun ve kimliği doğrulanmamış bir duruma geri döndürebilir. |


### <a name="successful-authentication-response"></a>Başarılı kimlik doğrulaması yanıtını

Başarılı bir token yanıt şöyle görünecektir:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parametre | Biçimlendir | Açıklama |
| --------- | ------ | ----------- |
|`token_type` | String| Her zaman "Bearer. |
|`scope` | Ayrılmış boşluk dizeleri | Bu, bir erişim belirteci döndürdüyse kapsamlar için erişim belirteci geçerliyse listeler. |
|`expires_in`| int | Dahil edilen erişim belirtecinin saniye için geçerlidir. |
|`access_token`| Donuk dize | Çıkarılan [kapsamları](v2-permissions-and-consent.md) , istendi.  |
|`id_token`   | JWT | Verilen if özgün `scope` parametresi dahil `openid` kapsam.  |
|`refresh_token` | Donuk dize | Verilen if özgün `scope` parametresi dahil `offline_access`.  |

Yenileme belirtecinin yeni erişim belirteçleri almak ve yenileme belirteçlerini kullanarak ayrıntılı aynı akışı kullanılabilir [OAuth kod flow belgeleri](v2-oauth2-auth-code-flow.md#refresh-the-access-token).  
