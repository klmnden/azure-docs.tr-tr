---
title: Federasyon için yapılandırılmış galeri dışındaki bir uygulamada oturum açarken sorun çoklu oturum açma | Microsoft Docs
description: SAML tabanlı Federasyon çoklu oturum açma için Azure AD ile yapılandırılmış bir uygulamada oturum açarken karşılaşabilecekleri belirli sorunlar için yönergeler
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38decf98707231c21427f7a22dd4d12adb41852b
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825433"
---
# <a name="problems-signing-in-to-a-non-gallery-application-configured-for-federated-single-sign-on"></a>Federasyon çoklu oturum açma için yapılandırılmış galeri dışındaki bir uygulamada oturum açma sorunları

Oturum açma sorunları aşağıdaki sorun giderme için daha iyi tanılama almak ve çözüm adımlarını otomatik hale getirmek için bu öneriyi izleyin öneririz:

- Yükleme [My Apps güvenli tarayıcı uzantısı](access-panel-extension-problem-installing.md) Azure sağlamak için Active Directory'yi (Azure AD) yardımcı olmak için Azure portalında daha iyi tanılama ve test kullanırken çözümleri karşılaşırsınız.
- Azure portalında uygulama yapılandırma sayfasında test deneyimini kullanarak hatayı yeniden oluşturun. Daha fazla bilgi [hata ayıklama SAML tabanlı çoklu oturum açma uygulamaları](../develop/howto-v1-debug-saml-sso-issues.md)

## <a name="application-not-found-in-directory"></a>Uygulama dizininde bulunamadı

*Hata AADSTS70001: Uygulama tanımlayıcısı ile `https://contoso.com` dizinde bulunamadı*.

**Olası nedeni**

Azure AD uygulama yapılandırılan tanımlayıcı değerini özniteliği Azure AD'de SAML isteğini uygulamasından gönderdiği veren eşleşmiyor.

**Çözümleme**

Emin `Issuer` Azure AD'de yapılandırılan tanımlayıcı değerini SAML isteğindeki daha fazla öznitelikle eşleşiyor. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez.

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Tanımlayıcı metin kutusundaki değeri hata görüntülenen tanımlayıcı değeri değeri eşleştiğini doğrulayın.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor. 

*Hata AADSTS50011: Yanıt adresi `https://contoso.com` uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor* 

**Olası nedeni** 

SAML isteğinde AssertionConsumerServiceURL değeri yanıt URL'si değeri veya Azure AD'de yapılandırılmış desen ile eşleşmiyor. SAML isteğinde AssertionConsumerServiceURL hata gördüğünüz URL'de değerdir. 

**Çözümleme** 

Emin `Issuer` Azure AD'de yapılandırılan tanımlayıcı değerini SAML isteğindeki daha fazla öznitelikle eşleşiyor. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez.
 
1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici** 

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde. 

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi. 

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde. 

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için. 

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**
  
6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Doğrulayın veya yanıt URL'si metin kutusuna, eşleştirilecek değer güncelleştirme `AssertionConsumerServiceURL` SAML isteğindeki değeri.    
    
Azure AD'de yanıt URL'si değeri güncelleştirdik ve SAML isteğindeki uygulama tarafından gönderilen değerle eşleşen sonra uygulamaya oturum açabilir.

## <a name="user-not-assigned-a-role"></a>Kullanıcı bir role atanmış olmamalıdır

*Hata AADSTS50105: Oturum açmış olan kullanıcının `brian\@contoso.com` uygulama için bir role atanmış olmamalıdır*

**Olası nedeni**

Kullanıcının Azure AD'de uygulamaya erişim verilmedi.

**Çözümleme**

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin. Kullanırsanız [deneyimi test](../develop/howto-v1-debug-saml-sso-issues.md) My Apps güvenli tarayıcı uzantısı ile Azure portalında aşağıdaki adımları el ile gerekmez.

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına veya e-posta adresine göre arama** Arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatması mümkün.

## <a name="not-a-valid-saml-request"></a>Olmayan bir geçerli SAML isteği

*Hata AADSTS75005: İstek bir geçerli olduğu saml2 tabanlı protokol iletisi değil.*

**Olası nedeni**

Azure AD, uygulama tarafından Çoklu oturum açma için gönderilen SAML İsteğini desteklemiyor. Bazı yaygın sorunlar şunlardır:

-   SAML isteğinde gerekli alanlar eksik

-   SAML isteği kodlama yöntemi

**Çözümleme**

1.  SAML isteğinde yakalayın. öğreticiyi takip edin [Azure AD'de SAML tabanlı çoklu oturum açma uygulamaları için hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) SAML isteğini yakalama hakkında bilgi edinmek için.

2.  Paylaşımı ve uygulamanın satıcısına başvurun:

    -   SAML isteği

    -   [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

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

1.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. Tanımlayıcı metin kutusundaki değeri hata görüntülenen tanımlayıcı değeri değeri eşleştiğini doğrulayın.

## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı

Hata AADSTS50003: İmzalama anahtarı yapılandırılmış.

**Olası nedeni**

Uygulama nesnesi bozuk ve Azure AD uygulaması için yapılandırılan sertifikaya tanımaz.

**Çözümleme**

Silin ve yeni bir sertifika oluşturmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. tıklayın **yeni sertifika oluştur** altında **SAML imzalama sertifikası** bölümü.

9. Sona erme tarihi seçin. ' A tıklayarak **kaydedin.**

10. Denetleme **yeni sertifikayı etkin hale getirin** etkin sertifikayı geçersiz kılmak için. Ardından bölmenin üstündeki **Kaydet**’e tıklayın ve geçiş sertifikasını etkinleştirmeyi kabul edin.

11. Altında **SAML imzalama sertifikası** bölümünde **Kaldır** kaldırmak için **kullanılmayan** sertifika.

## <a name="saml-request-not-present-in-the-request"></a>SAML isteği istek yok

*Hata AADSTS750054: SAMLRequest veya SAMLResponse sorgu dizesi parametreleri bağlama SAML yönlendirmek için HTTP isteği olarak mevcut olmalıdır.*

**Olası nedeni**

Azure AD SAML isteğini HTTP isteği URL'si parametrelerinde içinde belirlemek mümkün değildi. Uygulamayı Azure AD'ye SAML isteğini gönderirken, bağlama HTTP yeniden yönlendirme kullanmıyorsa bu durum oluşabilir.

**Çözümleme**

HTTP kullanarak konum üst bilgisi içinde kodlanmış SAML isteği göndermek uygulaması gereken bağlama yeniden yönlendirme. Bunun nasıl gerçekleştirileceği hakkında daha fazla bilgi için [SAML protokolü belirtimi belgesinde](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf) HTTP Yeniden Yönlendirme Bağlaması bölümünü okuyun.

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

7.  Uygulama yüklendikten sonra **Temel SAML yapılandırması**'nı açın. İçinde **yanıt URL'si (onay belgesi tüketici hizmeti URL'si)**, kullanılmayan Sil veya varsayılan yanıt URL'leri, sistem tarafından oluşturulan. Örneğin, `https://127.0.0.1:444/applications/default.aspx`.



## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulama için gönderilen SAML talepleri özelleştirme sırasında sorun

Uygulamanıza gönderilen SAML özniteliği talep özelleştirme öğrenmek için bkz. [talep eşlemesi, Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
