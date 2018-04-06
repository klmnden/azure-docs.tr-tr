---
title: 'Azure Active Directory B2C: Yerleşik ilkeleri | Microsoft Docs'
description: Bir konu Azure Active Directory B2C Genişletilebilir ilke çerçevesini ve çeşitli ilke türleri oluşturma
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/26/2017
ms.author: davidmu
ms.openlocfilehash: c733f919189dadcf1181ddbe2a1057b2bcf66fc4
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Yerleşik ilkeleri


Azure Active Directory (Azure AD) B2C Genişletilebilir ilke hizmet çekirdek gücünü çerçevedir. İlkeleri tam olarak açıklayan tüketici kimlik deneyimi gibi kaydolma, oturum açma ve profil düzenleme. Örneği için bir kayıt ilkesi, aşağıdaki ayarları yapılandırarak davranışları denetlemenize olanak sağlar:

* Tüketiciler uygulama için kaydolmak için kullanabileceğiniz hesap türleri (facebook sosyal hesapları) ya da e-posta adresleri gibi yerel hesaplar
* Kayıt sırasında tüketiciden toplanacak öznitelikleri (örneğin, ad, posta kodu ve ayakkabı boyut)
* Azure çok faktörlü kimlik doğrulaması
* Tüm kayıt sayfaları Görünüm ve yapısını
* Bir belirteç talep olarak bildirimleri) bilgileri (uygulama sonlandığında Çalıştırma İlkesi zaman alır

Kiracınızda farklı türlerde birden çok ilke oluşturup, bunları gerektiği gibi uygulamalarınızda kullanabilirsiniz. İlkeler, uygulamalar arasında yeniden kullanılabilir. Bu esneklik, geliştiricilerin tanımlama ve tüketici kimlik deneyimi ile en az veya kendi kodunda değişiklik değiştirme sağlar.

İlkeleri basit Geliştirici arabirimi aracılığıyla kullanılabilir. Uygulamanız (bir ilke parametre istekte geçirme) standart bir HTTP kimlik doğrulaması isteği kullanarak bir ilke tetikler ve yanıt olarak özelleştirilmiş bir belirteç alır. Örneğin, bir kayıt ilkesi çağırma istekleri ve oturum açma ilke çağırmak istekleri arasındaki tek fark "p" sorgu dizesi parametresi kullanılan ilke adı şudur:

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

Bu ilke, her iki tüketici kaydolma ve oturum açma deneyimlerini tek bir yapılandırmasına sahip işler. Tüketiciler, bağlam bağlı olarak doğru yolu (kaydolma veya oturum açma) aşağı gerektiriyordu. Ayrıca, uygulama başarılı oturum ups veya oturum açma işlemleri almaz belirteçleri içeriğini açıklar.  Kaydolma veya oturum açma ilkesi için bir kod örneğidir [kullanılabilir burada](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Kayıt İlkesi ve oturum açma ilkesi bu ilkeyi kullanmak önerilir.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Kayıt ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Bir oturum açma ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Profil düzenleme ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Parola sıfırlama ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Bir kayıt veya oturum açma ilkesi bir parola sıfırlama İlkesi ile nasıl bağlanır?
Bir oturum açma veya kaydolma ilkesiyle (yerel hesaplar için) oluşturduğunuzda, gördüğünüz bir **unuttunuz parola?** bağlantı deneyimi ilk sayfasında. Bu bağlantıyı tıklatarak otomatik olarak tetikleyici bir parola sıfırlama İlkesi değil. 

Bunun yerine, hata kodu **`AADB2C90118`** uygulamanıza döndürülür. Bu hata kodu belirli parolası sıfırlama ilkesini çağırarak işlemek uygulamanız gerekir. Daha fazla bilgi için bir [ilkeleri bağlama yaklaşımı gösteren örnek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Bir kayıt veya oturum açma ilkesi veya bir kayıt ilkesi ve bir oturum açma ilkesi kullanmalıyım?
Bir kayıt ilkesi ve bir oturum açma ilkesi bir kayıt veya oturum açma ilkesi kullanmanızı öneririz.  

Kaydolma veya oturum açma ilkesi oturum açma ilkesinden daha fazla özellik içerir. Ayrıca, sayfa UI Özelleştirme kullanmanıza olanak tanır ve yerelleştirme için daha iyi destek sahiptir. 

Oturum açma ilkesi ilkelerinizi yerelleştirme gerekmiyorsa, yalnızca markalama için ikincil özelleştirme özellikleri gerekir ve parola istediğiniz önerilir yerleşik sıfırlama.

## <a name="next-steps"></a>Sonraki adımlar
* [Belirteç, oturum ve tek oturum açma yapılandırması](active-directory-b2c-token-session-sso.md)
* [E-posta doğrulama tüketici kaydolma sırasında devre dışı bırak](active-directory-b2c-reference-disable-ev.md)

