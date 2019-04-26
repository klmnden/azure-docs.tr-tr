---
title: Federasyon için yapılandırılan bir galeri uygulamasına oturum açmada sorun çoklu oturum açma | Microsoft Docs
description: Yönergeler için yapılandırdığınız uygulamaya SAML tabanlı Federasyon çoklu oturum açma için Azure AD ile imzalarken belirli hataları
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: celested
ms.reviewer: luleon, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 623d684f701df8b1a7c4b84a2bd3840f039ad174
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60292223"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Federasyon çoklu oturum açma için yapılandırılmış bir galeri uygulamasında oturum açma sorunları

Oturum açma sorunları aşağıdaki sorun giderme için daha iyi tanılama almak ve çözüm adımlarını otomatik hale getirmek için bu öneriyi izleyin öneririz:

- Yükleme [My Apps güvenli tarayıcı uzantısı](access-panel-extension-problem-installing.md) Azure sağlamak için Active Directory'yi (Azure AD) yardımcı olmak için Azure portalında daha iyi tanılama ve test kullanırken çözümleri karşılaşırsınız.
- Azure portalında uygulama yapılandırma sayfasında test deneyimini kullanarak hatayı yeniden oluşturun. Daha fazla bilgi [hata ayıklama SAML tabanlı çoklu oturum açma uygulamaları](../develop/howto-v1-debug-saml-sso-issues.md)


## <a name="application-not-found-in-directory"></a>Uygulama dizininde bulunamadı

*Hata AADSTS70001: Uygulama tanımlayıcısı ile ' https:\//contoso.com' dizininde bulunamadı*.

**Olası nedeni**

`Issuer` Uygulamasından Azure AD'de SAML isteğinde gönderilen özniteliği Azure AD'de bir uygulama için yapılandırılan tanımlayıcı değerini eşleşmiyor.

**Çözümleme**

Emin `Issuer` Azure AD'de yapılandırılan tanımlayıcı değerini SAML isteğindeki daha fazla öznitelikle eşleşiyor. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez.

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

1.  Açık **Azure Active Directory uzantısını** seçerek **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

1.  Tür **"Azure Active Directory"** filtre arama kutusunu seçip **Azure Active Directory** öğesi.

1.  Seçin **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

1.  Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamaları**.

1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra açın **temel SAML yapılandırma**. Tanımlayıcı metin kutusundaki değeri hata görüntülenen tanımlayıcı değeri değeri eşleştiğini doğrulayın.



## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor

*Hata AADSTS50011: Yanıt adresi ' https:\//contoso.com' uygulaması için yapılandırılan yanıt adresleriyle eşleşmiyor*

**Olası nedeni**

`AssertionConsumerServiceURL` Değerini SAML isteğindeki yanıt URL'si değeri veya Azure AD'de yapılandırılmış deseni eşleşmiyor. `AssertionConsumerServiceURL` Değerini SAML isteğindeki hataya bakın URL'sidir.

**Çözümleme**

Emin `AssertionConsumerServiceURL` değerini SAML isteğindeki yapılandırılmış Azure AD'de yanıt URL'si değeri ile eşleşir. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez.

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

1.  Açık **Azure Active Directory uzantısını** seçerek **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

1.  Tür **"Azure Active Directory"** filtre arama kutusunu seçip **Azure Active Directory** öğesi.

1.  Seçin **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

1.  Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamaları**.

1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra açın **temel SAML yapılandırma**. Doğrulayın veya yanıt URL'si metin kutusuna, eşleştirilecek değer güncelleştirme `AssertionConsumerServiceURL` SAML isteğindeki değeri.    
    
Azure AD'de yanıt URL'si değeri güncelleştirdik ve SAML isteğindeki uygulama tarafından gönderilen değerle eşleşen sonra uygulamaya oturum açabilir.

## <a name="user-not-assigned-a-role"></a>Kullanıcı bir role atanmış olmamalıdır

*Hata AADSTS50105: Oturum açmış olan kullanıcının ' brian\@contoso.com' uygulaması için bir role atanmadı*.

**Olası nedeni**

Kullanıcının Azure AD'de uygulamaya erişim verilmedi.

**Çözümleme**

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez.

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici**.

1.  Açık **Azure Active Directory uzantısını** seçerek **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

1.  Tür **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

1.  Seçin **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

1.  Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamaları**.

1.  Uygulamalar listesinden bir kullanıcıya atamak istediğiniz birini seçin.

1.  Uygulama yüklendikten sonra seçin **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

1.  Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

1.  Seçin **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

1. İçinde **adına veya e-posta adresine göre arama** arama kutusuna tam adını yazın veya e-posta adresi eklemek istediğiniz kullanıcının.

1. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı ya da kullanıcı eklemek için logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

1. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tam adı veya e-posta adresi içine **adına veya e-posta adresine göre arama** arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili**  listesi.

1. Kullanıcılar seçmeyi tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

1. **İsteğe bağlı:** Tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

1. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatması mümkün olacaktır.

## <a name="not-a-valid-saml-request"></a>Geçerli bir SAML isteğinde değil

*Hata AADSTS75005: İstek bir geçerli olduğu saml2 tabanlı protokol iletisi değil.*

**Olası nedeni**

Azure AD çoklu oturum açma için uygulama tarafından gönderilen SAML isteğini desteklemiyor. Bazı yaygın sorunlar şunlardır:

-   SAML isteğinde gerekli alanlar eksik
-   SAML isteği kodlama yöntemi

**Çözümleme**

1. SAML isteğini yakalayın. öğreticiyi izleyin [Azure AD'de SAML tabanlı çoklu oturum açma uygulamaları için hata ayıklama](../develop/howto-v1-debug-saml-sso-issues.md) SAML isteğini yakalama hakkında bilgi edinmek için.

1. Uygulama satıcısıyla iletişime geçin ve aşağıdaki bilgileri paylaşın:

   -   SAML isteği

   -   [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](../develop/single-sign-on-saml-protocol.md)

Uygulama satıcısıyla, Azure AD SAML uygulaması için çoklu oturum açmayı destekledikleri olduğunu doğrulamalıdır.

## <a name="misconfigured-application"></a>Yanlış yapılandırılmış bir uygulama

*Hata AADSTS650056: Yanlış yapılandırılmış bir uygulama. Bunun nedeni, aşağıdakilerden biri olabilir: İstemci, istemcinin uygulama kaydında istenen izinleri 'AAD Graph' için herhangi bir izni listelenen değil. Veya, yönetici kiracıda değil. Veya, uygulama tanımlayıcısı istek yapılandırılmış istemci uygulama tanımlayıcısı eşleştiğinden emin olmak için kontrol edin. Lütfen yapılandırmayı düzeltmenin veya Kiracı adına onay için yöneticinize başvurun.* .

**Olası nedeni**

`Issuer` Uygulamasından Azure AD'de SAML isteğinde gönderilen özniteliği Azure AD'de uygulama için yapılandırılan tanımlayıcı değerini eşleşmiyor.

**Çözümleme**

Emin `Issuer` Azure AD'de yapılandırılan tanımlayıcı değerini SAML isteğindeki daha fazla öznitelikle eşleşiyor. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

1.  Açık **Azure Active Directory uzantısını** seçerek **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

1.  Tür **"Azure Active Directory"** filtre arama kutusunu seçip **Azure Active Directory** öğesi.

1.  Seçin **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

1.  Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamaları**.

1.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

1.  Uygulama yüklendikten sonra açın **temel SAML yapılandırma**. Tanımlayıcı metin kutusundaki değeri hata görüntülenen tanımlayıcı değeri değeri eşleştiğini doğrulayın.


## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı

*Hata AADSTS50003: İmzalama anahtarı yapılandırılmış.*

**Olası nedeni**

Uygulama nesnesi bozuk ve Azure AD uygulaması için yapılandırılan sertifikaya tanımaz.

**Çözümleme**

Silin ve yeni bir sertifika oluşturmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

1. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

1. Tür **"Azure Active Directory"** filtre arama kutusunu seçip **Azure Active Directory** öğesi.

1. Seçin **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

1. Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamaları**.

1. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

1. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

1. Seçin **yeni sertifika oluştur** altında **SAML imzalama sertifikası** bölümü.

1. Sona erme tarihi seçin ve ardından **Kaydet**.

1. Denetleme **yeni sertifikayı etkin hale getirin** etkin sertifikayı geçersiz kılmak için. ' A tıklayarak **Kaydet** Bölmenin üst kısmındaki ve geçiş sertifikasını etkinleştirmek için kabul edin.

1. Altında **SAML imzalama sertifikası** bölümünde **Kaldır** kaldırmak için **kullanılmayan** sertifika.

## <a name="saml-request-not-present-in-the-request"></a>SAML isteği istek yok

*Hata AADSTS750054: SAMLRequest veya SAMLResponse sorgu dizesi parametreleri bağlama SAML yönlendirmek için HTTP isteği olarak mevcut olmalıdır.*

**Olası nedeni**

Azure AD SAML isteğini HTTP isteği URL'si parametrelerinde içinde belirlemek mümkün değildi. Uygulamayı Azure AD'ye SAML isteğini gönderirken, bağlama HTTP yeniden yönlendirme kullanmıyorsa bu durum oluşabilir.

**Çözümleme**

HTTP kullanarak konum üst bilgisi içinde kodlanmış SAML isteği göndermek uygulaması gereken bağlama yeniden yönlendirme. Bunu gerçekleştirme hakkında daha fazla bilgi için HTTP yeniden yönlendirme bağlama bölümü içinde [SAML protokolü belirtimi belgesi](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf).

## <a name="azure-ad-is-sending-the-token-to-an-incorrect-endpoint"></a>Azure AD belirteç yanlış bir uç noktaya gönderme

**Olası nedeni**

Çoklu oturum açma sırasında Azure AD yapılandırılmış birini seçip ardından oturum açma isteği bir açık bir yanıt URL'si (onay belgesi tüketici hizmeti URL'si) içermiyorsa, bu uygulama için URL kullanır. Uygulama bir açık yanıt URL'si yapılandırılmış olsa bile, kullanıcı yeniden yönlendirilen olabilir https://127.0.0.1:444. 

Uygulama galeriden olmayan bir uygulama olarak eklendiğinde, Azure Active Directory bu yanıt URL'sini bir varsayılan değer olarak oluşturuldu. Bu davranış değiştirildi ve Azure Active Directory artık varsayılan olarak bu URL'yi eklemez. 

**Çözümleme**

Uygulama için yapılandırılan kullanılmayan yanıt URL'lerinden silin.

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** seçerek **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Tür **"Azure Active Directory"** filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Seçin **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamaları**.

6.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra açın **temel SAML yapılandırma**. İçinde **yanıt URL'si (onay belgesi tüketici hizmeti URL'si)**, kullanılmayan Sil veya varsayılan yanıt URL'leri, sistem tarafından oluşturulan. Örneğin, `https://127.0.0.1:444/applications/default.aspx`.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulama için gönderilen SAML talepleri özelleştirme sırasında sorun

Uygulamanıza gönderilen SAML özniteliği talep özelleştirme öğrenmek için bkz. [talep eşlemesi, Azure Active Directory'de](../develop/active-directory-claims-mapping.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD uygulamaları için hata ayıklama SAML tabanlı çoklu oturum açma](../develop/howto-v1-debug-saml-sso-issues.md)
