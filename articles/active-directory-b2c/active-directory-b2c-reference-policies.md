---
title: Azure Active Directory B2C kullanıcı akışlarında | Microsoft Docs
description: Azure Active Directory B2C çeşitli kullanıcı akışları oluşturma ve Genişletilebilir ilke çerçevesi hakkında daha fazla bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 67db13c8a36977f2614ba7b0e263919bd0405bc7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316961"
---
# <a name="user-flows-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de kullanıcı akışları

Azure Active Directory (Azure AD) B2C'in Genişletilebilir ilke çerçevesi hizmet çekirdek gücünü ' dir. İlkeleri tam olarak açıklayan kimlik deneyimleri gibi kaydolma, oturum açma ve profil düzenleme. Yardımcı olması için en yaygın kimlik görevleri ayarlayın, adlı önceden tanımlanmış, yapılandırılabilir ilkeleri Azure AD B2C portal içerir **kullanıcı akışları**. 

## <a name="what-are-user-flows"></a>Kullanıcı akışları nelerdir?

Kullanıcı akışı, aşağıdaki ayarları yapılandırarak uygulamalarınızın davranışları denetlemenize olanak sağlar:

- Hesap türleri için oturum açma, sosyal medya hesaplarını gibi kullanılan ister bir Facebook veya yerel hesaplar
- Ad, posta kodu gibi tüketici toplanmasına ve boyutu ayakkabı öznitelikleri
- Azure Multi-Factor Authentication
- Kullanıcı arabirimini özelleştirme
- Bir belirtecinde talep olarak uygulamanın aldığı bilgileri 

Kiracınızda farklı türdeki çok sayıda kullanıcı akışları oluşturabilir ve bunları uygulamalarınızda gerektiğinde kullanın. Kullanıcı akışları uygulamalar arasında yeniden kullanılabilir. Bu esnekliğin tanımlama ve kimlik deneyimi değişikliğiyle veya hiç değişiklik kodunuzda değişiklik sağlar. Uygulamanız, bir kullanıcı akışı parametre içeren bir standart HTTP kimlik doğrulama isteği'ni kullanarak bir kullanıcı akışı tetikler. Özelleştirilmiş [belirteci](active-directory-b2c-reference-tokens.md) bir yanıt aldı. 

Aşağıdaki örnekler, kullanılacak kullanıcı akışını belirtir "p" sorgu dizesi parametresi gösterir:

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

## <a name="user-flow-versions"></a>Kullanıcı akışı sürümleri

Yeni Azure portalında [kullanıcı akışları sürümleri](user-flow-versions.md) her zaman eklenir. Azure AD B2C ile çalışmaya başlama, kullanıcı akışları kullanmak için önerilen test. Yeni bir kullanıcı Akış oluşturduğunuzda, seçtiğiniz gelen gereksinim duyduğunuz kullanıcı akışı **önerilen** sekmesi.

Aşağıdaki kullanıcı akışları şu anda önerilir:

- **Kaydolma ve oturum açma** -hem de tek bir yapılandırma ile kaydolma ve oturum açma deneyimlerini işler. Kullanıcılar, bağlama bağlı olarak doğru yolunu gerektiriyordu. Üzerinden bu kullanıcı akışını kullanmanız önerilir bir **kaydolma** kullanıcı akışı veya **oturum** kullanıcı akışı.
- **Profil düzenleme** -kullanıcıların kendi profil bilgilerini düzenlemesine olanak sağlar.
- **Parola sıfırlama** -olup olmadığını ve kullanıcıların kendi parolalarını sıfırlayabilir nasıl yapılandırmanızı sağlar.

## <a name="linking-user-flows"></a>Bağlama kullanıcı akışları

A **kaydolma veya oturum açma** yerel hesapların olan kullanıcı akışı içeren bir **parolanızı mı unuttunuz?** bağlantı deneyimi'nın ilk sayfasında. Bu bağlantıya tıkladığınızda otomatik olarak tetikleyici parola kullanıcı akışını sıfırlamak değil. 

Bunun yerine, hata kodu `AADB2C90118` uygulamanıza döndürülür. Bu hata kodu parolasını sıfırlar bir kullanıcıya akış çalıştırarak işlemek uygulamanız gerekir. Bir örnek görmek için göz atın. bir [basit ASP.NET örnek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI) kullanıcı akışlarını bağlamayı gösterir.

## <a name="email-address-storage"></a>E-posta adresi depolama

Bir e-posta adresi, bir kullanıcı akışı bir parçası olarak gerekli olabilir. Sosyal kimlik sağlayıcısı ile kullanıcı kimliğini doğrular, e-posta adresi depolanan **otherMails** özelliği. Ardından bir yerel hesap kullanıcı adı temel alıyorsa, e-posta adresi bir güçlü kimlik doğrulama ayrıntısı özelliğinde depolanır. Yerel bir hesap, bir e-posta adresine göre sonra e-posta adresi depolanan **signInNames** özelliği.
 
E-posta adresi, tüm durumlarda doğrulanacak garanti yoktur. Kiracı Yöneticisi, yerel hesaplar için temel ilkeleri e-posta doğrulamayı devre dışı bırakabilirsiniz. E-posta adresi doğrulama etkin olsa bile adresleri doğrulanabilir değil, bir sosyal kimlik sağlayıcısından gelen ve bunlar değiştirilmemiştir.
 
Yalnızca **otherMails** ve **signInNames** özellikleri, Active Directory Graph API'si sunulur. E-posta adresi güçlü kimlik doğrulama ayrıntısı özelliğinde kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen kullanıcı akışları oluşturmak için yönergeleri izleyin. [Öğreticisi: Kullanıcı akışı Oluştur](tutorial-create-user-flows.md).


