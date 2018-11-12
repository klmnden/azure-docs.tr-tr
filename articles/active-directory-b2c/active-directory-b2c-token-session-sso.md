---
title: Belirteç, oturum ve Azure Active Directory B2C, çoklu oturum açma yapılandırması | Microsoft Docs
description: Belirteç, oturum ve Azure Active Directory B2C, çoklu oturum açma yapılandırması.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0058ce8316fa8202cf53eaa1048a44b77efdecb5
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012454"
---
# <a name="token-session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Belirteç, oturum ve Azure Active Directory B2C, çoklu oturum açma yapılandırması

Bu özellik, hassas bir denetim üzerinde sağlar bir [ilkeye temel](active-directory-b2c-reference-policies.md), biri:

- Azure Active Directory (Azure AD) B2C tarafından yayılan güvenlik belirteçlerinin ömrü.
- Azure AD B2C tarafından yönetilen web uygulaması oturumları ömrü.
- Azure AD B2C tarafından yayılan güvenlik belirteçlerini önemli Taleplerde biçimleri.
- Çoklu oturum açma (SSO) davranışı birden fazla uygulama ve ilkeleri, Azure AD B2C kiracınızdaki.

Yerleşik ilkeler için bu özelliği Azure AD B2C dizininizde aşağıdaki gibi kullanabilirsiniz:

1. Tıklayın **oturum açma veya kaydolma ilkeleri'ni**. * Not: Bu özellik herhangi bir ilke türü üzerinde değil yalnızca üzerinde kullanabileceğiniz ** kaydolma veya oturum açma ilkeleri ***.
2. Tıklayarak bir ilkeyi açın. Örneğin, tıklayarak **b2c_1_siupın**.
3. Tıklayın **Düzenle** menüsünün üstünde.
4. Tıklayın **belirteç, oturum ve çoklu oturum açma yapılandırması**.
5. İstediğiniz değişiklikleri yapın. Sonraki bölümlerde kullanılabilir özellikler hakkında bilgi edinin.
6. **Tamam** düğmesine tıklayın.
7. Tıklayın **Kaydet** menüsünün üstünde.

## <a name="token-lifetimes-configuration"></a>Belirteç ömrünü yapılandırma

Azure AD B2C'yi destekleyen [OAuth 2.0 Yetkilendirme Protokolü](active-directory-b2c-reference-protocols.md) korunan kaynaklara güvenli erişimi etkinleştirmek için. Bu destek uygulamak için Azure AD B2C'yi çeşitli yayan [güvenlik belirteçleri](active-directory-b2c-reference-tokens.md). 

Aşağıdaki özellikler, Azure AD B2C tarafından yayılan güvenlik belirteçlerinin ömrü yönetmek için kullanılır:

- **Erişim ve kimlik belirteci ömrü (dakika)** - korunan bir kaynağa erişmek için kullanılan OAuth 2.0 taşıyıcı belirtecinin ömrü.
    - Varsayılan = 60 dakika.
    - (Sınırlar dahil) en az 5 dakika.
    - (Sınırlar dahil) en fazla 1440 dakika =.
- **Yenileme belirteci ömrü (gün)** - önüne bir yenileme belirteci yeni erişim veya kimlik belirteci almak için kullanılabilir en uzun süre (ve uygulamanızı verilen, isteğe bağlı olarak, yeni bir yenileme belirteci, `offline_access` kapsam).
    - Varsayılan = 14 gün.
    - (Sınırlar dahil) en az 1 gün =.
    - En fazla (sınırlar dahil) = 90 gün.
- **Yenileme belirteci kayan pencere ömrü (gün)** - kullanıcının kimliğinin yeniden kimlik doğrulaması, bağımsız olarak geçerlilik süresi en son yenileme belirtecinin uygulama tarafından bu zaman süre geçtikten sonra. Anahtar ayarlanırsa yalnızca sağlanabilir **sınırlanmış**. Büyük veya buna eşit olması gereken **yenileme belirteci ömrü (gün)** değeri. Anahtar ayarlanırsa **Unbounded**, belirli bir değerin sağlayamaz.
    - Varsayılan = 90 gün.
    - (Sınırlar dahil) en az 1 gün =.
    - (Sınırlar dahil) en fazla 365 günlük =.

Aşağıdaki kullanım örnekleri, bu özellikleri kullanarak etkinleştirilir:

- Kullanıcı uygulamayı sürekli olarak etkin olduğu sürece, bir mobil uygulamaya oturum açık kalsın açmasına izin verin. Ayarlayabileceğiniz **yenileme belirteci kayan pencere ömrü (gün)** için **Unbounded** , oturum açma ilkesi.
- Uygun erişim belirteç ömrünü ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerinizi karşılayın.

Bu ayarlar, parola sıfırlama ilkeleri için kullanılabilir değildir. 

## <a name="token-compatibility-settings"></a>Belirteç uyumluluk ayarları

Aşağıdaki özellikler, müşterilerin gerektiğinde iyileştirilmiş izin ver:

- **Verici (iss) talebi** -bu özellik, belirteci veren Azure AD B2C kiracısı tanımlar.
    - `https://<domain>/{B2C tenant GUID}/v2.0/` -Varsayılan değer budur.
    - `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` -Bu değer, B2C kiracısının hem belirteç istekte kullanılan ilkeyi kimliklerini içerir. Uygulama veya kitaplık ile uyumlu olması için Azure AD B2C gerekip gerekmediğini [Openıd Connect bulma 1.0 belirtimi](http://openid.net/specs/openid-connect-discovery-1_0.html), bu değeri kullanın.
- **Konu (sub) talebi** -belirteç için bilgilerini onayladığı varlık bu özelliği tanımlar.
    - **ObjectID** -bu özellik varsayılan değerdir. Dizine kullanıcının nesne kimliği doldurur `sub` belirtecinde talep.
    - **Desteklenmeyen** - bu özellik, yalnızca geriye dönük uyumluluk için sağlanır ve için geçiş öneririz **objectID** yapabilecekleriniz hemen sonra.
- **İlke Kimliğini temsil eden talep** -bu özellik, belirteç istekte kullanılan ilke kimliği doldurulur talep türü tanımlar.
    - **tfp** -bu özellik varsayılan değerdir.
    - **ACR** -bu özellik, yalnızca geriye dönük uyumluluk için sağlanır.

## <a name="session-behavior"></a>Oturum davranışı

Azure AD B2C'yi destekleyen [Openıd Connect kimlik doğrulama protokolü](active-directory-b2c-reference-oidc.md) güvenli oturum açma web uygulamalarını etkinleştirmek için. Web uygulaması oturumları yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

- **Web uygulaması oturumunun ömrü (dakika)** - başarılı kimlik doğrulamadan sonra kullanıcının tarayıcısında depolanan Azure AD B2C'in oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - (Sınırlar dahil) en az 15 dakika =.
    - (Sınırlar dahil) en fazla 1440 dakika =.
- **Web uygulaması oturumu zaman aşımı** - bu anahtarı ayarlanırsa **mutlak**, kullanıcı tarafından belirtilen zaman aralığı yeniden sonra kimlik doğrulaması için zorlanır **Web uygulaması oturumunun ömrü (dakika)** geçen. Bu anahtar ayarlanırsa **çalışırken** (varsayılan ayar), kullanıcı web uygulamanızı sürekli olarak etkin olduğu sürece, kullanıcının oturum açmış durumda kalır.

Aşağıdaki kullanım örnekleri, bu özellikleri kullanarak etkinleştirilir:

- Uygun web uygulaması oturumu ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerini karşılamak yaşam süresi yok.
- Kimlik doğrulamasından sonra ayarlanmış bir süre içinde web uygulamanızı yüksek güvenlikli bir parçası olan bir kullanıcı etkileşimi zorlar. 

Bu ayarlar, parola sıfırlama ilkeleri için kullanılabilir değildir.

## <a name="single-sign-on-sso-configuration"></a>Çoklu oturum açma (SSO) yapılandırması

B2C kiracınızda birden çok uygulama ve ilkeleri varsa, Kullanıcı etkileşimlerine arasında yönetebileceğiniz kullanarak **çoklu oturum açma yapılandırması** özelliği. Özelliği aşağıdaki ayarlardan birini ayarlayabilirsiniz:

- **Kiracı** -Bu ayar varsayılan ayardır. Bu ayarı kullanarak B2C kiracınıza aynı kullanıcı oturumuna paylaşmak için birden çok uygulama ve ilkeleri sağlar. Bir uygulamaya bir kullanıcı oturum açtıktan sonra Örneğin, kullanıcı aynı zamanda sorunsuz bir şekilde başka bir Contoso eriştiği üzerine ilaç, içine oturum açabilirsiniz.
- **Uygulama** -diğer uygulamalar bağımsız bir uygulama için yalnızca bir kullanıcı oturumu korumak bu ayarı sağlar. Örneğin, Contoso ilaç için (aynı kimlik bilgileri ile), oturum açmak için kullanıcının kullanıcı zaten Contoso alışveriş imzalansa bile isterseniz, başka bir uygulama aynı B2C Kiracı. 
- **İlke** -Bu ayar, bir kullanıcı oturumu şemayı kullanan uygulamaların bağımsız bir ilke için özel olarak korumak sağlar. Kullanıcı zaten açık ve çok faktörlü kimlik doğrulaması (MFA) adım tamamlandı, ilkeye bağlı oturumu süresi dolmadığı sürece Örneğin, kullanıcı erişim daha yüksek güvenlik için birden çok uygulama bölümlerini verilebilir.
- **Devre dışı bırakılmış** - bu orces tüm kullanıcı gezintisinde her yürütme İlkesi'nin üzerinde çalıştırmak için kullanıcı ayarı. Örneğin, böylece uygulamanızda (paylaşılan bir masaüstü senaryo) oturum açmak birden fazla kullanıcı, tek bir kullanıcı çalışırken bile tüm süre boyunca oturum açmış durumda kalır.

Bu ayarlar, parola sıfırlama ilkeleri için kullanılabilir değildir. 

