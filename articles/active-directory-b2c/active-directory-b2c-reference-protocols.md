---
title: "Azure Active Directory B2C: Kimlik doğrulama protokolleri | Microsoft Docs"
description: "Doğrudan Azure Active Directory B2C tarafından desteklenen protokolleri kullanarak uygulamalar oluşturma"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5e407d0a-73a2-4d74-ac81-3aa6c31ddcee
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: 8e7e7bc7633370057f8dc596ad04a3f1d796a7d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Kimlik doğrulama protokolleri
Azure Active Directory B2C (Azure AD B2C) sağlayan kimlik uygulamalarınız için bir hizmet olarak iki endüstri standardı protokoller destekleyerek: Openıd Connect ve OAuth 2.0. Hizmet standartlarıyla uyumlu, ancak bu protokollerin iki belirtilmesinden küçük farklılıklar olabilir. 

Bu kılavuzda yer alan bilgiler, doğrudan göndererek ve HTTP isteklerini işlemek yerine bir açık kaynak kitaplığı kullanarak kodunuzu yazma yararlıdır. Bu sayfayı her belirli Protokolü ayrıntıları hemen önce okumanızı öneririz. Ancak, zaten Azure AD B2C ile konusunda bilginiz varsa, doğrudan gidebilirsiniz [Protokolü Başvuru Kılavuzu](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Temel bilgiler
Azure AD B2C kullanan her uygulamanın B2C dizininizde kaydedilmesi gerekiyor [Azure portal](https://portal.azure.com). Uygulama kayıt işlemi birkaç değer toplayarak uygulamanıza atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama Kimliği**.
* A **yeniden yönlendirme URI'si** veya **tanımlayıcısı paketini** yanıtları uygulamanıza geri yönlendirmek için kullanılabilir.
* Diğer birkaç senaryoya özel değerler. Daha fazla bilgi için bilgi [uygulamanızı kaydetme](active-directory-b2c-app-registration.md).

Uygulamanızı kaydettikten sonra Azure Active Directory (Azure AD) ile uç noktasına istek göndererek iletişim kurar:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Neredeyse tüm OAuth ve Openıd Connect akışlarında, dört tarafların Exchange'de söz konusu şunlardır:

![OAuth 2.0 rolleri](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

* **Yetkilendirme sunucusu** Azure AD uç noktadır. Güvenli bir şekilde kullanıcı bilgilerini ve erişim ile ilgili herhangi bir şey işler. Ayrıca, akış içindeki taraflar arasında güven ilişkilerini işler. Kullanıcının kimliğini doğrulamak, verme ve kaynaklara erişimi iptal etme ve belirteç için sorumlu değildir. Bu da kimlik sağlayıcısı denir.

* **Kaynak sahibi** genellikle son kullanıcısıdır. Verilerin sahibi taraf olur ve bu veri ya da kaynağa erişmek için üçüncü taraf izin vermek için güç vardır.

* **OAuth istemcisi** uygulamanızın. Kendi uygulama kimliği tarafından tanımlanır Genellikle, son kullanıcıların etkileşimde taraf olur. Ayrıca belirteçleri yetkilendirme sunucusundan gelen istekleri. Kaynak sahibi istemci kaynağa erişim izni vermeniz gerekir.

* **Kaynak sunucusu** kaynak veya veri bulunduğu değil. Güvenli bir şekilde kimlik doğrulaması ve OAuth istemcisi yetkilendirmek için yetkilendirme sunucusu güvenir. Ayrıca bir kaynağa erişim izni olduğundan emin olmak için taşıyıcı erişim belirteçlerini kullanır.

## <a name="policies"></a>İlkeler
Tartışmaya açık bir şekilde, hizmetin en önemli özelliklerini Azure AD B2C ilkelerdir. Azure AD B2C, ilkeleri sunarak standart OAuth 2.0 ve Openıd Connect protokollerini genişletir. Bunlar çok daha fazlasını Basit kimlik doğrulama ve yetkilendirme gerçekleştirmek Azure AD B2C izin verir. 

İlkeleri tam olarak tüketici kimlik deneyimi kaydolma, oturum açma dahil olmak üzere,'ı açıklamakta ve profil düzenleme. İlkeleri, bir yönetici kullanıcı Arabiriminde tanımlanabilir. HTTP kimlik doğrulama isteklerinin özel sorgu parametresi kullanılarak çalıştırılabilir. 

Bunları anlamak için zaman ayırmanız böylece ilkeleri OAuth 2.0 ve Openıd Connect, standart özelliklerini değildir. Daha fazla bilgi için bkz: [Azure AD B2C İlkesi Başvuru Kılavuzu](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Belirteçler
OAuth 2.0 ve Openıd Connect Azure AD B2C uygulaması JSON web belirteçleri (Jwt'ler) temsil edilir taşıyıcı belirteçlerini dahil olmak üzere, taşıyıcı belirteçlerini kapsamlı kullanımını sağlar. Bir taşıyıcı belirteci korunan bir kaynağa "bearer" erişim veren bir basit güvenlik belirtecidir.

Taşıyıcı belirteci sunabilir herhangi bir tarafa ' dir. Bir taşıyıcı belirteci almak için önce azure AD önce bir taraf kimlik doğrulaması gerekir. Ancak belirteç iletim ve depolama güvenli hale getirmek için gerekli adımları katılmaz varsa, onu kullanılabilir ele ve istenmeyen bir şirket tarafından kullanılan.

Bazı güvenlik belirteçleri yetkisiz tarafların onları kullanmasına engel olmak yerleşik mekanizmalar sahip, ancak taşıyıcı belirteçlerini bu düzenek sahip değil. Bunlar bir Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir. 

Bir taşıyıcı belirteci dışında güvenli bir kanal iletilirse, kötü amaçlı bir taraf belirtecini almak ve korunan bir kaynağa yetkisiz erişim kazanmak için kullanmak üzere bir adam-in--middle saldırı kullanabilirsiniz. Taşıyıcı belirteçlerini depolanan veya daha sonra kullanmak için önbelleğe alınmış, aynı güvenlik ilkelerini uygular. Her zaman uygulamanızı aktarır ve güvenli bir şekilde taşıyıcı belirteçleri depolar emin olun.

Ek taşıyıcı belirteci güvenlik konuları için bkz: [RFC 6750 bölüm 5](http://tools.ietf.org/html/rfc6750).

Azure AD B2C'de kullanılan belirteçleri farklı uygulama türleri hakkında daha fazla bilgi edinilebilir [Azure AD belirteç başvurusu](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokoller
Bazı örnek isteklerini gözden geçirme hazır olduğunuzda, aşağıdaki öğreticiler birini başlatabilirsiniz. Her bir belirli kimlik doğrulama senaryosu karşılık gelir. Hangi akış sizin için uygun olan saptarken yardıma gerek duyarsanız kullanıma [Azure AD B2C kullanarak oluşturabilir uygulama türlerini](active-directory-b2c-apps.md).

* [OAuth 2.0 kullanarak mobil ve yerel uygulamalar oluşturma](active-directory-b2c-reference-oauth-code.md)
* [Openıd Connect kullanarak Web uygulamaları oluşturma](active-directory-b2c-reference-oidc.md)
* [OAuth 2.0 örtük akışını kullanarak tek sayfalı uygulamalar oluşturma](active-directory-b2c-reference-spa.md)

