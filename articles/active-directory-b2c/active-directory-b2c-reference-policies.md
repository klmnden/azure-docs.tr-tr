---
title: Yerleşik ilkeleri Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'in Genişletilebilir ilke çerçevesi ve çeşitli ilke türleri oluşturma konusunda konu.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4a0dfcbba1867d4792a0e4a383ee097df0ea410b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52835820"
---
# <a name="azure-active-directory-b2c-user-flows"></a>Azure Active Directory B2C: Kullanıcı akışları


Azure Active Directory (Azure AD) B2C'in Genişletilebilir ilke çerçevesi hizmet çekirdek gücünü ' dir. İlkeleri tam olarak açıklayan tüketici kimlik deneyimleri gibi kaydolma, oturum açma ve profil düzenleme. Yardımcı olması için en yaygın kimlik görevleri ayarlayın, adlı önceden tanımlanmış, yapılandırılabilir ilkeleri Azure AD B2C portal içerir **kullanıcı akışları**. Örneğin, kayıt kullanıcı akışı, aşağıdaki ayarları yapılandırarak davranışları denetlemenize olanak tanır:

* Tüketiciler, uygulama için kaydolmak için kullanabileceğiniz hesap türleri (Facebook gibi sosyal medya hesapları) veya e-posta adresleri gibi yerel hesaplar
* Kayıt sırasında tüketiciden toplanacak öznitelikleri (örneğin, ad, posta kodu ve ayakkabı)
* Azure çok faktörlü kimlik doğrulaması
* Tüm kayıt sayfaları Görünüm ve yapısını
* Bir belirtecinde talep olarak bildirimleri) bilgileri (uygulamanın ne zaman tamamlanmadan Çalıştır kullanıcı akışı alır

Kiracınızda farklı türlerde birden çok kullanıcı akışları oluşturabilir ve bunları uygulamalarınızda gerektiğinde kullanın. Kullanıcı akışları uygulamalar arasında yeniden kullanılabilir. Bu esnekliğin geliştiriciler tanımlayabilir ve tüketici kimlik deneyimi değişikliğiyle veya hiç değişiklik kendi kodunda değişiklik sağlar.

Kullanıcı akışları basit Geliştirici arabirimi aracılığıyla kullanılabilir. Uygulamanızı (istekte bir kullanıcı akışı parametre geçirerek), standart HTTP kimlik doğrulaması isteği'ni kullanarak bir kullanıcı akışı tetikler ve yanıt olarak özelleştirilmiş bir belirteç alır. Örneğin, kayıt kullanıcı akışı çağırma istekleri ve bir kullanıcı oturum açma akışını çağıran istekleri arasındaki tek fark, "p" sorgu dizesi parametresi kullanılan userjourney adı şöyledir:

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow

```

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow

```

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>Kaydolma veya oturum açma kullanıcı akışı oluştur

Bu kullanıcı akışını tek bir yapılandırma ile hem de tüketici kaydolma ve oturum açma deneyimlerini işler. Tüketiciler, bağlama bağlı olarak doğru yolunu (kaydolma veya oturum açma) gerektiriyordu. Ayrıca, başarılı kaydolma veya oturum açma sırasında uygulamanın alacağı belirteçlerin içeriğini açıklar.  Bir kod örneği için **kaydolma veya oturum açma** kullanıcı akışı [buradan kullanılabilir](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Üzerinden bu kullanıcı akışını kullanmanız önerilir bir **kaydolma** kullanıcı akışı veya **oturum** kullanıcı akışı.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-user-flow"></a>Kaydolma kullanıcı akışı oluştur

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-user-flow"></a>Oturum açma kullanıcı akışı oluştur

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-user-flow"></a>Kullanıcı akışı düzenleme profili oluşturma

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama kullanıcı akışı oluştur

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-user-flows"></a>Önizleme kullanıcı akışları

Yeni özellikler yayınlayabilir gibi bunlardan bazıları mevcut ilkeleri veya kullanıcı akışları mevcut olmayabilir.  Bu kullanıcı akışları büyüyecek girdikten sonra eski sürümleri aynı türde en son değiştirin planlıyoruz  Mevcut ilkeleri veya kullanıcı akışları değişmez ve bu yeni özelliklerden yararlanmak için yeni kullanıcı akışları oluşturma gerekecektir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-link-a-sign-up-or-sign-in-user-flow-with-a-password-reset-user-flow"></a>Parola sıfırlama kullanıcı akışı ile kaydolma veya oturum açma kullanıcı akışı nasıl bağlantı kurarım?
Oluştururken bir **kaydolma veya oturum açma** kullanıcı Akış (yerel hesaplar ile), gördüğünüz bir **parolanızı mı unuttunuz?** bağlantı deneyimi'nın ilk sayfasında. Bu bağlantıya tıkladığınızda otomatik olarak tetikleyici parola kullanıcı akışını sıfırlamak değil. 

Bunun yerine, hata kodu **`AADB2C90118`** uygulamanıza döndürülür. Bu hata kodu belirli bir parola sıfırlama kullanıcı akışı çağırarak işlemek uygulamanız gerekir. Daha fazla bilgi için bir [kullanıcı akışları bağlama yaklaşımı gösteren örnek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-user-flow-or-a-sign-up-user-flow-and-a-sign-in-user-flow"></a>Kaydolma veya oturum açma kullanıcı akışı veya kaydolma kullanıcı akışı ve kullanıcı oturum açma akışı kullanmalıyım?
Kullanmanızı öneririz bir **kaydolma veya oturum açma** kullanıcı akışı üzerinden bir **kaydolma** kullanıcı akışı ve **oturum** kullanıcı akışı.  

**Kaydolma veya oturum açma** kullanıcı akışı bulunanlardan daha fazla özelliğe sahip **oturum** kullanıcı akışı. Ayrıca, sayfa UI özelleştirmesi kullanmanıza olanak sağlar ve yerelleştirme için daha iyi destek sağlıyor. 

**Oturum** kullanıcı akışı önerilir kullanıcı akışlarınızı yerelleştirmeniz gerekmiyorsa, yalnızca markalar için küçük özelleştirme becerileri ve parola sıfırlama yerleşik.

## <a name="next-steps"></a>Sonraki adımlar
* [Belirteç, oturum ve çoklu oturum açma yapılandırması](active-directory-b2c-token-session-sso.md)
* [Tüketicinin kaydolma sırasında e-posta doğrulamayı devre dışı bırakma](active-directory-b2c-reference-disable-ev.md)

