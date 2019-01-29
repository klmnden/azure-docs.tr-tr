---
title: Federasyon için yapılandırılmış galeri dışındaki bir uygulamada oturum açarken sorun çoklu oturum açma | Microsoft Docs
description: SAML tabanlı Federasyon çoklu oturum açma için Azure AD ile yapılandırılmış bir uygulamada oturum açarken karşılaşabilecekleri belirli sorunlar için yönergeler
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 279f0290bb873e06146ec83d921caa50d482f924
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55182889"
---
# <a name="problems-signing-in-to-a-non-gallery-application-configured-for-federated-single-sign-on"></a>Federasyon çoklu oturum açma için yapılandırılmış galeri dışındaki bir uygulamada oturum açma sorunları

Sorunu gidermek için izleme olarak Azure AD'de uygulama yapılandırmasını doğrulayın gerekir:

-   Azure AD galeri uygulaması için tüm yapılandırma adımları izlediyseniz.

-   Tanımlayıcı ve yanıt AAD içinde yapılandırılan URL ile bunlar uygulamada beklenen değerler

-   Uygulamaya veya atanan kullanıcılar

## <a name="application-not-found-in-directory"></a>Uygulama dizininde bulunamadı

*Hata AADSTS70001: Uygulama tanımlayıcısı ile 'https://contoso.com' dizininde bulunamadı*.

**Olası nedeni**

Azure AD uygulama yapılandırılan tanımlayıcı değerini özniteliği Azure AD'de SAML isteğini uygulamasından gönderdiği veren eşleşmiyor.

**Çözümleme**

Bu tanımlayıcı eşleştirme SAML isteğindeki veren özniteliği olduğundan emin olun Azure AD'de yapılandırılmış değer:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  <span id="_Hlk477190042" class="anchor"></span>Git **etki alanı ve URL'ler** bölümü. Tanımlayıcı metin kutusundaki değeri hata görüntülenen tanımlayıcı değeri değeri eşleşen doğrulayın.

Azure AD'de tanımlayıcı değerini güncelleştirdikten sonra bu değeri gönderir SAML isteğindeki uygulamayla eşleşen, uygulamaya oturum açabilir.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresi, uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor. 

*Hata AADSTS50011: Yanıt adresi https://contoso.com' uygulaması için yapılandırılan yanıt adresleriyle eşleşmiyor* 

**Olası nedeni** 

SAML isteğinde AssertionConsumerServiceURL değeri yanıt URL'si değeri veya Azure AD'de yapılandırılmış desen ile eşleşmiyor. SAML isteğinde AssertionConsumerServiceURL hata gördüğünüz URL'de değerdir. 

**Çözümleme** 

Bu yanıt URL'si eşleşen SAML isteğindeki AssertionConsumerServiceURL değeri olduğundan emin olun Azure AD'de yapılandırılmış değer. 
 
1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici** 

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde. 

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi. 

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde. 

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için. 

  * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**
  
6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **etki alanı ve URL'ler** bölümü. Doğrulamanız ya da değeri yanıt URL'si metin kutusuna, SAML isteğindeki AssertionConsumerServiceURL değerle eşleşecek şekilde güncelleştirin.

  * Yanıt URL'si metin kutusuna görmüyorsanız seçin **Gelişmiş URL ayarlarını göster** onay kutusu. 

Azure AD'de yanıt URL'si değeri güncelleştirdikten sonra değer gönderir SAML isteğindeki uygulamayla eşleşen, uygulamaya oturum açabilir.

## <a name="user-not-assigned-a-role"></a>Kullanıcı bir role atanmış olmamalıdır

*Hata AADSTS50105: Oturum açmış olan kullanıcının 'brian@contoso.com' uygulaması için bir role atanmış olmamalıdır*

**Olası nedeni**

Kullanıcının Azure AD'de uygulamaya erişim verilmedi.

**Çözümleme**

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9.  tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

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

-   SAML isteği kodlanmış yöntem

**Çözümleme**

1.  SAML isteğinde yakalayın. öğreticiyi takip edin [Azure AD'de SAML tabanlı çoklu oturum açma uygulamaları için hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) SAML isteğini yakalama hakkında bilgi edinmek için.

2.  Paylaşımı ve uygulamanın satıcısına başvurun:

    -   SAML isteği

    -   [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Bunlar doğrulamalıdır bunlar Azure AD SAML uygulaması için çoklu oturum açmayı destekler.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Hiçbir kaynak requiredResourceAccess listesi

*Hata AADSTS65005: İstemci uygulama, kaynağa erişim isteğinde bulundu ' 00000002-0000-0000-c000-000000000000'. İstemci, kendi requiredResourceAccess listesinde bu kaynak tanımlanmamış çünkü bu isteği başarısız oldu*.

**Olası nedeni**

Uygulama nesnesi bozuk.

**Çözümleme**

Sorunu çözmek için uygulama dizinden kaldırın. Ardından ekleyin ve uygulamayı yeniden, aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Tıklayın **Sil** uygulamasının üst sol **genel bakış** bölmesi.

8.  Azure AD yenileyin ve Azure AD galeri uygulaması ekleyin. Ardından, uygulamayı yeniden yapılandırın.

Uygulamayı yeniden sonra uygulamaya oturum açabilir olmalıdır.

## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı

Hata AADSTS50003: İmzalama anahtarı yapılandırılmış.

**Olası nedeni**

Uygulama nesnesi bozuk ve Azure AD uygulaması için yapılandırılan sertifikaya tanımaz.

**Çözümleme**

Silin ve yeni bir sertifika oluşturmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  tıklayın **yeni sertifika oluştur** altında **SAML imzalama sertifikası** bölümü.

9.  Sona erme tarihi seçin. ' A tıklayarak **kaydedin.**

10. Denetleme **yeni sertifikayı etkin hale getirin** etkin sertifikayı geçersiz kılmak için. ' A tıklayarak **Kaydet** Bölmenin üst kısmındaki ve geçiş sertifikasını etkinleştirmek için kabul edin.

11. Altında **SAML imzalama sertifikası** bölümünde **Kaldır** kaldırmak için **kullanılmayan** sertifika.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulama için gönderilen SAML talepleri özelleştirme sırasında sorun

Uygulamanıza gönderilen SAML özniteliği talep özelleştirme öğrenmek için bkz. [talep eşlemesi, Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
