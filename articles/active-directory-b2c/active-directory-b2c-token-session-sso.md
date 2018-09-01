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
ms.openlocfilehash: 061e2257200b6d660a421a86c540f43597112c5e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337894"
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Belirteç, oturum ve çoklu oturum açma yapılandırması

Bu özellik, hassas bir denetim üzerinde sağlar bir [ilkeye temel](active-directory-b2c-reference-policies.md), biri:

1. Azure Active Directory (Azure AD) B2C tarafından yayılan güvenlik belirteçlerinin ömrü.
2. Azure AD B2C tarafından yönetilen web uygulaması oturumları ömrü.
3. Azure AD B2C tarafından yayılan güvenlik belirteçlerini önemli Taleplerde biçimleri.
4. Çoklu oturum açma (SSO) davranışı birden fazla uygulama ve ilkeleri B2C kiracınızda.

Yerleşik ilkeler için bu özelliği Azure AD B2C dizininizde aşağıdaki gibi kullanabilirsiniz:

1. Bu adımları [B2C özellikleri menüsüne gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. Tıklayın **oturum açma veya kaydolma ilkeleri'ni**. * Not: Bu özellik herhangi bir ilke türü üzerinde değil yalnızca üzerinde kullanabileceğiniz ** kaydolma veya oturum açma ilkeleri ***.
3. Tıklayarak bir ilkeyi açın. Örneğin, tıklayarak **b2c_1_siupın**.
4. Tıklayın **Düzenle** menüsünün üstünde.
5. Tıklayın **belirteç, oturum ve çoklu oturum açma yapılandırması**.
6. İstediğiniz değişiklikleri yapın. Sonraki bölümlerde kullanılabilir özellikler hakkında bilgi edinin.
7. **Tamam** düğmesine tıklayın.
8. Tıklayın **Kaydet** menüsünün üstünde.

## <a name="token-lifetimes-configuration"></a>Belirteç ömrünü yapılandırma

Azure AD B2C'yi destekleyen [OAuth 2.0 Yetkilendirme Protokolü](active-directory-b2c-reference-protocols.md) korunan kaynaklara güvenli erişimi etkinleştirmek için. Bu destek uygulamak için Azure AD B2C'yi çeşitli yayan [güvenlik belirteçleri](active-directory-b2c-reference-tokens.md). Azure AD B2C tarafından yayılan güvenlik belirteçlerinin ömrü yönetmek için kullanabileceğiniz özellikleri şunlardır:

* **Erişim ve kimlik belirteci ömrü (dakika)**: OAuth 2.0 taşıyıcı belirtecinin ömrü korunan bir kaynağa erişmek için kullanılır.
  * Varsayılan = 60 dakika.
  * (Sınırlar dahil) en az 5 dakika.
  * (Sınırlar dahil) en fazla 1440 dakika =.
* **Yenileme belirteci ömrü (gün)**: önce bir yenileme belirteci kullanılabilen yeni erişim veya kimlik belirteci almak için en uzun süre (ve uygulamanızı verilen, isteğe bağlı olarak, yeni bir yenileme belirteci, `offline_access` kapsam).
  * Varsayılan = 14 gün.
  * (Sınırlar dahil) en az 1 gün =.
  * En fazla (sınırlar dahil) = 90 gün.
* **Yenileme belirteci kayan pencere ömrü (gün)**: kullanıcı belirtilen sürenin geçmesinden sonra zorunlu yeniden kimlik doğrulaması, bağımsız olarak en son geçerlilik süresi uygulama tarafından alınan belirteci yenileyin. Anahtar ayarlanırsa yalnızca sağlanabilir **sınırlanmış**. Büyük veya buna eşit olması gereken **yenileme belirteci ömrü (gün)** değeri. Anahtar ayarlanırsa **Unbounded**, belirli bir değerin sağlayamaz.
  * Varsayılan = 90 gün.
  * (Sınırlar dahil) en az 1 gün =.
  * (Sınırlar dahil) en fazla 365 günlük =.

Bu özellikleri kullanarak etkinleştirebilirsiniz kullanım örnekleri birkaç şunlardır:

* İzinli uygulamayı sürekli olarak etkin olduğu sürece, bir mobil uygulamaya kalmak bir kullanıcı izin verin. Bunu ayarlayarak yapabilirsiniz **yenileme belirteci kayan pencere ömrü (gün)** geçin **Unbounded** , oturum açma ilkesi.
* Uygun erişim belirteç ömrünü ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerinizi karşılayın.

    > [!NOTE]
    > Bu ayarlar, parola sıfırlama ilkeleri için kullanılabilir değildir.
    > 
    > 

## <a name="token-compatibility-settings"></a>Belirteç uyumluluk ayarları

Azure AD B2C tarafından yayılan güvenlik belirteçleri önemli talepleri için biçimlendirme değişiklikler yaptık. Bu bizim standart protokol desteğini geliştirmek için yapıldı ve üçüncü taraf kimlik kitaplıkları ile daha iyi birlikte çalışabilirlik. Bununla birlikte, mevcut uygulamaları bozmayı önlemek için müşterilerin gerektiğinde katılımı izin vermek için aşağıdaki özellikleri oluşturduk:

* **Verici (iss) talebi**: Bu belirteci veren Azure AD B2C kiracısı tanımlar.
  * `https://<domain>/{B2C tenant GUID}/v2.0/`: Bu varsayılan değerdir.
  * `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Bu değer, B2C kiracısının hem belirteç istekte kullanılan ilkeyi kimlikleri içerir. Uygulama veya kitaplık ile uyumlu olması için Azure AD B2C gerekip gerekmediğini [Openıd Connect bulma 1.0 belirtimi](http://openid.net/specs/openid-connect-discovery-1_0.html), bu değeri kullanın.
* **Konu (sub) talebi**: Bu belirteci onaylar bilgileri başka bir deyişle, kullanıcı varlığı tanımlar.
  * **ObjectID**: Bu varsayılan değerdir. Dizine kullanıcının nesne kimliği doldurur `sub` belirtecinde talep.
  * **Desteklenmeyen**: Bu, yalnızca geriye dönük uyumluluk için sağlanır ve için geçiş öneririz **objectID** yapabilecekleriniz hemen sonra.
* **İlke Kimliğini temsil eden talep**: Bu belirteci istekte kullanılan ilke kimliği doldurulmuş talep türü tanımlar.
  * **tfp**: Bu varsayılan değerdir.
  * **ACR**: Bu, yalnızca geriye dönük uyumluluk için sağlanır ve için geçiş öneririz `tfp` yapabilecekleriniz hemen sonra.

## <a name="session-behavior"></a>Oturum davranışı

Azure AD B2C'yi destekleyen [Openıd Connect kimlik doğrulama protokolü](active-directory-b2c-reference-oidc.md) güvenli oturum açma web uygulamalarını etkinleştirmek için. Web uygulaması oturumları yönetmek için kullanabileceğiniz özellikleri şunlardır:

* **Web uygulaması oturumunun ömrü (dakika)**: başarılı kimlik doğrulamadan sonra kullanıcının tarayıcısında depolanan Azure AD B2C'in oturum tanımlama bilgisinin ömrü.
  * Varsayılan = 1440 dakika.
  * (Sınırlar dahil) en az 15 dakika =.
  * (Sınırlar dahil) en fazla 1440 dakika =.
* **Web uygulaması oturumu zaman aşımı**: Bu anahtar ayarlanırsa **mutlak**, kullanıcı tarafından belirtilen bir süre sonra yeniden kimlik doğrulaması zorlanır **Web uygulaması oturumunun ömrü (dakika)** geçen. Bu anahtar ayarlanırsa **çalışırken** (varsayılan ayar), kullanıcı web uygulamanızı sürekli olarak etkin olduğu sürece, kullanıcının oturum açmış durumda kalır.

Bu özellikleri kullanarak etkinleştirebilirsiniz kullanım örnekleri birkaç şunlardır:

* Uygun web uygulaması oturumu ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerini karşılamak yaşam süresi yok.
* Yeniden kimlik doğrulamasından sonra ayarlanmış bir süre içinde web uygulamanızı yüksek güvenlikli bir parçası olan bir kullanıcı etkileşimi zorlar. 

    > [!NOTE]
    > Bu ayarlar, parola sıfırlama ilkeleri için kullanılabilir değildir.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Çoklu oturum açma (SSO) yapılandırması
B2C kiracınızda birden çok uygulama ve ilkeleri varsa, Kullanıcı etkileşimlerine arasında yönetebileceğiniz kullanarak **çoklu oturum açma yapılandırması** özelliği. Özelliği aşağıdaki ayarlardan birini ayarlayabilirsiniz:

* **Kiracı**: Bu varsayılan ayardır. Bu ayarı kullanarak B2C kiracınıza aynı kullanıcı oturumuna paylaşmak için birden çok uygulama ve ilkeleri sağlar. Bir uygulamaya bir kullanıcı oturum açtıktan sonra Örneğin, Contoso alışveriş, isterse de sorunsuz bir şekilde başka bir Contoso eriştiği üzerine ilaç, içine oturum açabilirsiniz.
* **Uygulama**: Bu özel diğer uygulamaları bağımsız bir uygulama için bir kullanıcı oturumu tutmanıza olanak sağlar. Örneğin, Contoso ilaç için (aynı kimlik bilgileri ile), oturum açmak için kullanıcının isterse zaten Contoso alışveriş imzalansa bile isterseniz, başka bir uygulama aynı B2C Kiracı. 
* **İlke**: Bu şemayı kullanan uygulamaların bağımsız bir ilke için özel olarak bir kullanıcı oturumu tutmanıza olanak sağlar. Kullanıcı zaten açık ve çok faktörlü kimlik doğrulaması (MFA) adım tamamlandı, ilkeye bağlı oturumu süresi dolmadığı sürece Örneğin, isterse erişim daha yüksek güvenlik için birden çok uygulama bölümlerini verilebilir.
* **Devre dışı bırakılmış**: Bu ilkenin her yürütme tüm kullanıcı gezintisinde çalıştırmak için kullanıcı zorlar. Örneğin, böylece uygulamanızda (paylaşılan bir masaüstü senaryo) oturum açmak birden fazla kullanıcı, tek bir kullanıcı çalışırken bile tüm süre boyunca oturum açmış durumda kalır.

    > [!NOTE]
    > Bu ayarlar, parola sıfırlama ilkeleri için kullanılabilir değildir.
    > 
    > 

