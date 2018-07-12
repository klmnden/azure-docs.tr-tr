---
title: Yerleşik ilkeleri Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'in Genişletilebilir ilke çerçevesi ve çeşitli ilke türleri oluşturma konusunda konu.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5c89f39b2f94309ea3d99230f5265d834c7093d9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38477458"
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Yerleşik ilkeler


Azure Active Directory (Azure AD) B2C'in Genişletilebilir ilke çerçevesi hizmet çekirdek gücünü ' dir. İlkeleri tam olarak açıklayan tüketici kimlik deneyimleri gibi kaydolma, oturum açma ve profil düzenleme. Örneğin bir kaydolma ilkesi aşağıdaki ayarları yapılandırarak davranışları denetlemenizi sağlar:

* Tüketiciler, uygulama için kaydolmak için kullanabileceğiniz hesap türleri (Facebook gibi sosyal medya hesapları) veya e-posta adresleri gibi yerel hesaplar
* Kayıt sırasında tüketiciden toplanacak öznitelikleri (örneğin, ad, posta kodu ve ayakkabı)
* Azure çok faktörlü kimlik doğrulaması
* Tüm kayıt sayfaları Görünüm ve yapısını
* (Bu talep bir belirteç olarak bildirimleri) bilgi uygulama bittiğinde çalıştırma İlkesi zaman alır

Kiracınızda farklı türlerde birden çok ilke oluşturup bunları gerektiği şekilde uygulamalarınızda kullanabilirsiniz. İlkeler, uygulamalar arasında yeniden kullanılabilir. Bu esnekliğin geliştiriciler tanımlayabilir ve tüketici kimlik deneyimi değişikliğiyle veya hiç değişiklik kendi kodunda değişiklik sağlar.

İlkeleri bir basit Geliştirici arabirimi aracılığıyla kullanılabilir. Uygulamanız bir ilke (bir ilke parametresi istekte geçirerek), standart HTTP kimlik doğrulaması isteği'ni kullanarak tetikler ve yanıt olarak özelleştirilmiş bir belirteç alır. Örneğin, bir kaydolma İlkesi çağırma istekleri ve oturum açma ilke çağırmak istekleri arasındaki tek fark "p" sorgu dizesi parametresi kullanılan ilke adı şöyledir:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

## <a name="create-a-sign-up-or-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi oluşturma

Bu ilke, tek bir yapılandırma ile hem de tüketici kaydolma ve oturum açma deneyimlerini işler. Tüketiciler, bağlama bağlı olarak doğru yolunu (kaydolma veya oturum açma) gerektiriyordu. Ayrıca, başarılı kaydolma veya oturum açma sırasında uygulamanın alacağı belirteçlerin içeriğini açıklar.  Bir kod örneği için **kaydolma veya oturum açma** ilke [buradan kullanılabilir](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Üzerinden bu ilkeyi kullanmak önerilir bir **kaydolma** İlkesi veya **oturum** ilkesi.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Kaydolma ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Oturum açma ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Profil düzenleme ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Parola sıfırlama ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-policies"></a>Önizleme ilkeleri

Yeni özellikler yayınlayabilir gibi bunlardan bazıları mevcut ilkeleri kullanılabilir olmayabilir.  Bu ilkeler büyüyecek girdikten sonra eski sürümleri aynı türde en son değiştirin planlıyoruz  Mevcut ilkelerinizi değişmez ve bu yeni özelliklerden yararlanmak için yeni ilkeler oluşturacağınız gerekecektir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Bir kaydolma veya oturum açma ilkesi bir parola sıfırlama İlkesi ile nasıl bağlantı kurarım?
Oluştururken bir **kaydolma veya oturum açma** ilkesiyle (yerel hesaplar için), gördüğünüz bir **parolanızı mı unuttunuz?** bağlantı deneyimi'nın ilk sayfasında. Bu bağlantıya tıkladığınızda otomatik olarak tetikleyici bir parola sıfırlama İlkesi yoktur. 

Bunun yerine, hata kodu **`AADB2C90118`** uygulamanıza döndürülür. Bu hata kodu belirli bir parola sıfırlama İlkesi çağırarak işlemek uygulamanız gerekir. Daha fazla bilgi için bir [ilkeleri bağlama yaklaşımı gösteren örnek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Kaydolma veya oturum açma ilkesi veya bir kaydolma İlkesi ve bir oturum açma ilkesi kullanmalıyım?
Kullanmanızı öneririz bir **kaydolma veya oturum açma** ilkesine göre bir **kaydolma** İlkesi ve bir **oturum** ilkesi.  

**Kaydolma veya oturum açma** ilkeye sahip daha fazla özellik **oturum** ilkesi. Ayrıca, sayfa UI özelleştirmesi kullanmanıza olanak sağlar ve yerelleştirme için daha iyi destek sağlıyor. 

**Oturum** ilkelerinizi yerelleştirmeniz gerekmiyorsa, yalnızca markalar için küçük özelleştirme becerileri ve parola ilkesi önerilir yerleşik sıfırlama.

## <a name="next-steps"></a>Sonraki adımlar
* [Belirteç, oturum ve çoklu oturum açma yapılandırması](active-directory-b2c-token-session-sso.md)
* [Tüketicinin kaydolma sırasında e-posta doğrulamayı devre dışı bırakma](active-directory-b2c-reference-disable-ev.md)

