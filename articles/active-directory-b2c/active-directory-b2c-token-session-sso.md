---
title: Belirteç, oturum ve çoklu oturum açma yapılandırması - Azure AD B2C | Microsoft Docs
description: Belirteç, oturum ve Azure Active Directory B2C, tek oturum açma yapılandırması
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 08/16/2017
ms.author: davidmu
ms.openlocfilehash: 925313b6f2a00826f2ec8086457315c60f70b007
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Belirteci, oturum ve tek oturum açma yapılandırması

Bu özellik, hassas bir denetim üzerinde sunar bir [ilke başına temel](active-directory-b2c-reference-policies.md), biri:

1. Azure Active Directory (Azure AD) B2C tarafından gösterilen güvenlik belirteçlerinin yaşam süresi.
2. Azure AD B2C tarafından yönetilen web uygulama oturumları ömürleri.
3. Azure AD B2C tarafından gösterilen güvenlik belirteçleri önemli Taleplerde biçimleri.
4. Çoklu oturum açma (SSO) davranışı birden çok uygulamalar ve ilkeler B2C kiracınızda arasında.

Yerleşik ilkeleri için bu özellik Azure AD B2C dizininizde aşağıdaki gibi kullanabilirsiniz:

1. Aşağıdaki adımları izleyin [B2C özellikleri menüsüne gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. Tıklatın **oturum açma veya kaydolma ilkeleri**. * Not: Bu özellik tüm ilke türüne açık değil yalnızca kullanabileceğiniz ** oturum açma veya kaydolma ilkeleri ***.
3. Bir ilke tıklatarak açın. Örneğin, tıklayın **B2C_1_SiUpIn**.
4. Tıklatın **Düzenle** menüsünün üstünde.
5. Tıklatın **belirteci, oturum ve çoklu oturum açma config**.
6. İstediğiniz değişiklikleri yapın. Sonraki bölümlerde kullanılabilir özellikler hakkında bilgi edinin.
7. **Tamam**’a tıklayın.
8. Tıklatın **kaydetmek** menüsünün üst kısmında.

## <a name="token-lifetimes-configuration"></a>Belirteç yaşam süreleri yapılandırma

Azure AD B2C destekleyen [OAuth 2.0 yetkilendirme protokolünü](active-directory-b2c-reference-protocols.md) korumalı kaynaklara güvenli erişim için etkinleştirme. Bu destek uygulamak için Azure AD B2C çeşitli yayar [güvenlik belirteçleri](active-directory-b2c-reference-tokens.md). Azure AD B2C tarafından gösterilen güvenlik belirteçlerinin yaşam süreleri yönetmek için kullanabileceğiniz özellikleri şunlardır:

* **Erişim & kimliği belirteci yaşam süresi (dakika)**: OAuth 2.0 taşıyıcı belirteç ömrü korunan bir kaynağa erişmek için kullanılır.
  * Varsayılan = 60 dakika.
  * Minimum (dahil) = 5 dakika.
  * Maksimum (dahil) = 1440 dakika.
* **Belirteç ömrü Yenile (gün)**: önce bir yenileme belirteci kullanılabilecek yeni erişim veya kimliği belirteci almak için en fazla süre (ve uygulamanızı verilen, isteğe bağlı olarak, yeni bir yenileme belirteci, `offline_access` kapsam).
  * Varsayılan = 14 gün.
  * (Dahil) en az 1 gün =.
  * Maksimum (dahil) = 90 gün.
* **Yenileme belirteci kayan pencere yaşam süresi (gün)**: kullanıcı bu süre geçtikten sonra zorla yeniden kimlik doğrulaması, geçerlilik süresi en son bağımsız olarak uygulama tarafından alınan belirteci yenileyin. Anahtar ayarlanırsa yalnızca sağlanabilir **Bounded**. Büyük veya eşit olması gerekir **yenileme belirteci yaşam süresi (gün)** değeri. Anahtar ayarlanmışsa **Unbounded**, belirli bir değer sağlayamaz.
  * Varsayılan = 90 gün.
  * (Dahil) en az 1 gün =.
  * Maksimum (dahil) = 365 gün.

Bu, birkaç bu özellikleri kullanarak etkinleştirebilirsiniz kullanım örnekleri şunlardır:

* Çözemiyorsa uygulamanın sürekli etkin olduğu sürece, bir mobil uygulamasına kalmak bir kullanıcı izin verin. Bunu ayarlayarak yapabilirsiniz **yenileme belirteci kayan pencere yaşam süresi (gün)** geçiş **Unbounded** ilkenizde oturum açma.
* Uygun erişim belirteci yaşam süresi ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerini karşılar.

    > [!NOTE]
    > Bu ayarları ilkeleri parola sıfırlama için kullanılabilir değil.
    > 
    > 

## <a name="token-compatibility-settings"></a>Belirteç uyumluluk ayarları

Biz biçimlendirme Azure AD B2C tarafından gösterilen güvenlik belirteçleri önemli Taleplerde yapılan değişiklikler. Bu bizim standart protokol desteği geliştirmek amacıyla yapılmıştır ve üçüncü taraf kimlik kitaplıkları ile daha iyi birlikte çalışabilirlik. Ancak, var olan uygulamaları çiğnemekten önlemek için müşterilerin gerektiğinde katılımı izin vermek için aşağıdaki özellikleri oluşturduk:

* **Veren (ISS) talep**: Bu belirtecin Azure AD B2C kiracısı tanımlar.
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Bu varsayılan değerdir.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Bu değer kimlikleri B2C Kiracı ve belirteç istekte kullanılan ilke içerir. Uygulama veya kitaplık Azure AD B2C'ile uyumlu olması gerekip gerekmediğini [Openıd Connect bulma 1.0 belirtimi](http://openid.net/specs/openid-connect-discovery-1_0.html), bu değeri kullanın.
* **Konu (alt) talep**: Bu belirteci onaylar bilgileri başka bir deyişle, kullanıcı varlık tanımlar.
  * **ObjectID**: Bu varsayılan değerdir. Dizine kullanıcı nesne kimliği doldurur `sub` belirteç talep.
  * **Desteklenmeyen**: Bu, yalnızca geriye dönük uyumluluk için sağlanır ve için geçiş öneririz **objectID** yapabileceksiniz hemen sonra.
* **İlke kimliği temsil eden talep**: Bu belirteci istekte kullanılan ilke kimliği doldurulmuş talep türü tanımlar.
  * **tfp**: Bu varsayılan değerdir.
  * **ACR**: Bu, yalnızca geriye dönük uyumluluk için sağlanır ve için geçiş öneririz `tfp` yapabileceksiniz hemen sonra.

## <a name="session-behavior"></a>Oturum davranışı

Azure AD B2C destekleyen [Openıd Connect kimlik doğrulama protokolü](active-directory-b2c-reference-oidc.md) güvenli oturum açma web uygulamaları için etkinleştirme. Web uygulama oturumları yönetmek için kullanabileceğiniz özellikleri şunlardır:

* **Web uygulaması oturum yaşam süresi (dakika)**: Azure AD B2C'ın oturum tanımlama bilgisi kullanıcının tarayıcısına başarılı bir kimlik doğrulaması sırasında depolanan ömrü.
  * Varsayılan = 1440 dakika.
  * Minimum (dahil) = 15 dakika.
  * Maksimum (dahil) = 1440 dakika.
* **Web uygulaması oturum zaman aşımı**: Bu anahtar ayarlanırsa **mutlak**, kullanıcı tarafından belirtilen süre sonra yeniden kimlik doğrulaması için zorlanır **Web uygulaması oturum yaşam süresi (dakika)** sona erdiğinde. Bu anahtar ayarlanırsa **çalışırken** (varsayılan ayar), kullanıcı web uygulamanızda sürekli etkin olduğu sürece kullanıcı oturum açmış durumda kalır.

Bu, birkaç bu özellikleri kullanarak etkinleştirebilirsiniz kullanım örnekleri şunlardır:

* Uygun web uygulaması oturum ayarlayarak, sektörün güvenlik ve uyumluluk gereksinimlerini karşılayan yaşam süresi yok.
* Yeniden kimlik doğrulama işleminden sonra ayarlanmış bir süre içinde web uygulamanızı yüksek güvenlikli bir parçası olan bir kullanıcı etkileşimi zorlar. 

    > [!NOTE]
    > Bu ayarları ilkeleri parola sıfırlama için kullanılabilir değil.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Çoklu oturum açma (SSO) yapılandırma
B2C kiracınızda birden çok uygulama ve ilkeleri varsa, kullanıcı etkileşimleri arasında gezinmek yönetebileceğiniz kullanarak **tek oturum açma yapılandırması** özelliği. Aşağıdaki ayarlardan birini özelliği ayarlayabilirsiniz:

* **Kiracı**: varsayılan ayar budur. Bu ayarı kullanarak birden çok uygulama ve ilkeleri aynı kullanıcı oturumunu paylaşmak için B2C kiracınızda izin verir. Bir uygulamaya bir kullanıcı oturum açtığında sonra Örneğin, Contoso alışveriş buldukça, aynı zamanda sorunsuz bir şekilde başka bir, Contoso erişmekte bağlı ilaç, içine oturum açabilirsiniz.
* **Uygulama**: Bu özel olarak diğer uygulamaları bağımsız bir uygulama için bir kullanıcı oturumu korumanıza olanak sağlar. Örneğin, isterse zaten Contoso alışveriş imzalansa bile (aynı kimlik bilgileri ile), Contoso ilaç oturum açmak için kullanıcının istiyorsanız, başka bir uygulama aynı B2C Kiracı. 
* **İlke**: Bu özel olarak bunu kullanan uygulamalar bağımsız bir ilke için bir kullanıcı oturumu korumanıza olanak sağlar. Kullanıcı daha önce açtığınız ve çok faktörlü kimlik doğrulama (MFA) adımını tamamlamış, ilkeyi bağlı oturum süresi sona ermiyor sürece Örneğin, çözemiyorsa erişim birden çok uygulama daha yüksek güvenlik bölümlerine verilebilir.
* **Devre dışı**: Bu kullanıcıya, tüm kullanıcı gezisine her ilkenin üzerinde yürütülmesi zorlar. Örneğin, uygulamanızda (paylaşılan bir masaüstü senaryo) için kaydolmak birden çok kullanıcı böylece, tek bir kullanıcı sırasında bile tüm süre boyunca oturum açmış durumda kalır.

    > [!NOTE]
    > Bu ayarları ilkeleri parola sıfırlama için kullanılabilir değil.
    > 
    > 

