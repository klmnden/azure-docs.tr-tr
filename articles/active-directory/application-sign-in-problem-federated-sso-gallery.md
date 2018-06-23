---
title: Federasyon için yapılandırılan bir galeri uygulamaya oturumu açmada sorun çoklu oturum açma | Microsoft Docs
description: Yönergeler için yapılandırdığınız bir uygulamaya SAML tabanlı Federasyon tek oturum açma için Azure AD ile imzalarken belirli hataları
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: f8c17b8c14b63007c3b623e5ffb60c0a2567cb72
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333658"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Federasyon çoklu oturum açma için yapılandırılmış bir galeri uygulaması için oturum açma sorunları

Sorunu gidermek için izleme olarak Azure AD uygulama yapılandırmasını doğrulayın gerekir:

-   Azure AD galeri uygulama için tüm yapılandırma adımları izlediğinizi.

-   Kimlik ve yanıt AAD'de yapılandırılmış URL'yi eşleşen bunlar uygulamadaki beklenen değerler

-   Uygulamayı kullanıcılara atanan

## <a name="application-not-found-in-directory"></a>Uygulama dizinde bulunamadı

*Hata AADSTS70001: Uygulama, tanımlayıcısı 'https://contoso.com' dizininde bulunamadı*.

**Olası neden**

Azure ad SAML isteğinde uygulamadan özniteliği gönderir veren Azure AD uygulamada yapılandırılan tanımlayıcı değeri eşleşmiyor.

**Çözümleme**

Bu tanımlayıcı eşleştirme SAML isteğinde veren özniteliğinde emin Azure AD içinde yapılandırılan değeri:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **etki alanı ve URL'leri** bölümü. Tanımlayıcı metin değerinde hata görüntülenen tanımlayıcı değeri değeri eşleşen doğrulayın.

Azure AD'de veya güncelleştirilen tanımlayıcı değerine ve bu değeri gönderir SAML isteğinde uygulama tarafından eşleşen sonra uygulamaya oturum açabilir.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresini uygulama için yapılandırılan yanıt adresleri eşleşmiyor.

*Hata AADSTS50011: Yanıt adresinihttps://contoso.com' uygulaması için yapılandırılmış yanıt adresleri eşleşmiyor.*

**Olası neden**

SAML isteğinde AssertionConsumerServiceURL değerinde yanıt URL'si değer veya desen Azure AD içinde yapılandırılmış eşleşmiyor. Hatayı görmek URL SAML isteğinde AssertionConsumerServiceURL değerdir.

**Çözümleme**

Bu yanıt URL'si eşleşen SAML isteğinde AssertionConsumerServiceURL değerinde emin Azure AD içinde yapılandırılan değeri.

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **etki alanı ve URL'leri** bölümü. Doğrulamak veya SAML isteğinde AssertionConsumerServiceURL değerle eşleşecek şekilde yanıt URL'si textbox değeri güncelleştirin.  
    * Yanıt URL'si metin kutusuna görmüyorsanız seçin **Göster Gelişmiş URL ayarları** onay kutusu.

Azure AD'de veya güncelleştirilen yanıt URL'si değerine ve bu değeri gönderir SAML isteğinde uygulama tarafından eşleşen sonra uygulamaya oturum açabilir.

## <a name="user-not-assigned-a-role"></a>Kullanıcı bir role atanmış olmamalıdır

*Hata AADSTS50105: Oturum açmış olan kullanıcının 'brian@contoso.com' uygulaması için bir rol atanmamış*.

**Olası neden**

Kullanıcı Azure AD'de uygulama erişim izni yok.

**Çözümleme**

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** bölmesi.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama ilgilenen kullanıcının **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcının profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına göre arama veya e-posta adresi** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilir.

## <a name="not-a-valid-saml-request"></a>Bir geçerli SAML istekte

*Hata AADSTS75005: İstek bir geçerli Saml2 protokol iletisi değil.*

**Olası neden**

Azure AD, uygulama tarafından Çoklu oturum açma için gönderilen SAML İsteğini desteklemiyor. Bazı yaygın sorunlar şunlardır:

-   SAML isteğinde gerekli alanlar eksik

-   SAML kodlanmış isteği yöntemi

**Çözümleme**

1.  SAML isteğinde yakalayın. öğreticiyi izleyin [SAML tabanlı çoklu oturum açma uygulamaları için Azure AD içinde hata ayıklamak nasıl](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) SAML isteğinde yakalama öğrenin.

2.  Paylaşım ve uygulamanın satıcısına başvurun:

   -   SAML isteği

   -   [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Bunlar doğrulamalıdır çoklu oturum açma için Azure AD SAML uygulama destekledikleri.

## <a name="no-resource-in-requiredresourceaccess-list"></a>RequiredResourceAccess listesinde kaynak yok

*Hata AADSTS65005: istemci uygulama kaynağa erişim isteğinde bulundu ' 00000002-0000-0000-c000-000000000000'. İstemci, kendi requiredResourceAccess listesinde bu kaynak belirtilmedi bu isteği başarısız oldu*.

**Olası neden**

Uygulama nesnesi bozuk.

**Çözüm: seçeneği 1**

Sorunu çözmek için Azure AD yapılandırmasında benzersiz tanımlayıcısı değeri ekleyin. Tanımlayıcı değeri eklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde

8.  Altında **etki alanı ve URL** bölümünde, denetleme **Göster Gelişmiş URL ayarları**.

9.  içinde **tanımlayıcısı** metin kutusuna uygulama için benzersiz bir tanımlayıcı yazın.

10. **Kaydet** yapılandırma.


**Çözüm seçeneği 2**

Yukarıdaki 1. seçenek sizin için olmadıysa dizinden uygulamayı kaldırmayı deneyin. Ardından, ekleyin ve uygulamayı yeniden yapılandırmak, aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Tıklatın **silmek** uygulamanın üst sol **genel bakış** bölmesi.

8.  Azure AD yenileyin ve Azure AD Galeriden uygulama ekleyin. Ardından, uygulamayı yapılandırma

<span id="_Hlk477190176" class="anchor"></span>Uygulama yeniden yapılandırmadan sonra uygulamaya oturum açabilir olması gerekir.

## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı

*Hata AADSTS50003: yapılandırılmış hiçbir imzalama anahtarı.*

**Olası neden**

Uygulama nesnesi bozuk ve Azure AD uygulama için yapılandırılan sertifika tanımıyor.

**Çözümleme**

Silin ve yeni bir sertifika oluşturmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

 * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  tıklatın **yeni sertifika oluştur** altında **imzalama sertifikası SAML** bölümü.

9.  Sona erme tarihini seçin. Ardından **kaydedin.**

10. Denetleme **yeni sertifika etkin hale getirin** etkin sertifikanın geçersiz kılmak için. Ardından **kaydetmek** bölmesinin üst ve geçiş sertifikası etkinleştirmek için kabul edin.

11. Altında **SAML imzalama sertifikası** 'yi tıklatın **kaldırmak** kaldırmak için **kullanılmayan** sertifika.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Uygulamaya gönderilen SAML talep özelleştirirken sorunu

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmek öğrenmek için bkz: [talep eşleme Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[SAML tabanlı çoklu oturum açma Azure AD uygulamalarında hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
