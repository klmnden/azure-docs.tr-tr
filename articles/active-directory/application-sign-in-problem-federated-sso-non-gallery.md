---
title: "Federasyon için yapılandırılan bir galeri olmayan uygulamaya oturumu açmada sorun çoklu oturum açma | Microsoft Docs"
description: "SAML tabanlı Federasyon tek oturum açma için Azure AD ile yapılandırılmış bir uygulama için oturum açarken karşılaşabilecekleri belirli sorunları için yönergeler"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 3afc7bca878caef424d3fa3c64aa17df0fda7de5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-a-non-gallery-application-configured-for-federated-single-sign-on"></a>Federasyon çoklu oturum açma için yapılandırılmış bir galeri olmayan uygulama oturum açma sorunları

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

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  <span id="_Hlk477190042" class="anchor"></span>Git **etki alanı ve URL'leri** bölümü. Tanımlayıcı metin değerinde hata görüntülenen tanımlayıcı değeri değeri eşleşen doğrulayın.

Azure AD'de veya güncelleştirilen tanımlayıcı değerine ve bu değeri gönderir SAML isteğinde uygulama tarafından eşleşen sonra uygulamaya oturum açabilir.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Yanıt adresini uygulama için yapılandırılan yanıt adresleri eşleşmiyor. 

*Hata AADSTS50011: Yanıt adresini 'https://contoso.com' uygulaması için yapılandırılmış yanıt adresleri eşleşmiyor.* 

**Olası neden** 

SAML isteğinde AssertionConsumerServiceURL değerinde yanıt URL'si değer veya desen Azure AD içinde yapılandırılmış eşleşmiyor. Hatayı görmek URL SAML isteğinde AssertionConsumerServiceURL değerdir. 

**Çözümleme** 

Bu yanıt URL'si eşleşen SAML isteğinde AssertionConsumerServiceURL değerinde emin Azure AD içinde yapılandırılan değeri. 
 
1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici** 

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki. 

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

*Hata AADSTS50105: Oturum açmış olan kullanıcının 'brian@contoso.com' uygulama için bir role atanmış olmamalıdır*

**Olası neden**

Kullanıcı Azure AD'de uygulama erişim izni yok.

**Çözümleme**

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** dikey.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** dikey.

10. Yazın **tam adı** veya **e-posta adresi** içine atama ilgilenen kullanıcının **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcının profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **ad veya e-posta adresine göre arama** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** seçtiğiniz kullanıcılara atamak için bir rol seçin dikey.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilir.

## <a name="not-a-valid-saml-request"></a>Bir geçerli SAML istekte

*Hata AADSTS75005: İstek bir geçerli Saml2 protokol iletisi değil.*

**Olası neden**

Azure AD çoklu oturum açma için uygulama tarafından gönderilen isteği SAML desteklemiyor. Bazı yaygın sorunlar şunlardır:

-   SAML isteğinde gerekli alanlar eksik

-   SAML kodlanmış isteği yöntemi

**Çözümleme**

1.  SAML isteğinde yakalayın. öğreticiyi izleyin [SAML tabanlı çoklu oturum açma uygulamaları için Azure AD içinde hata ayıklamak nasıl](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) SAML isteğinde yakalama öğrenin.

2.  Paylaşım ve uygulamanın satıcısına başvurun:

    -   SAML isteği

    -   [Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Bunlar doğrulamalıdır çoklu oturum açma için Azure AD SAML uygulama destekledikleri.

## <a name="no-resource-in-requiredresourceaccess-list"></a>RequiredResourceAccess listesinde kaynak yok

*Hata AADSTS65005: İstemci uygulama kaynağa erişim isteğinde bulundu ' 00000002-0000-0000-c000-000000000000'. İstemci, kendi requiredResourceAccess listesinde bu kaynak belirtilmedi bu isteği başarısız oldu*.

**Olası neden**

Uygulama nesnesi bozuk.

**Çözümleme**

Sorunu çözmek için uygulama dizininden kaldırın. Ardından, ekleyin ve uygulamayı yeniden yapılandırmak, aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Tıklatın **silmek** uygulamanın üst sol **genel bakış** dikey.

8.  Azure AD yenileyin ve Azure AD Galeriden uygulama ekleyin. Ardından, uygulamayı yeniden yapılandırın.

Uygulama yeniden yapılandırmadan sonra uygulamaya oturum açabilir olması gerekir.

## <a name="certificate-or-key-not-configured"></a>Sertifika veya anahtar yapılandırılmadı

Hata AADSTS50003: yapılandırılmış hiçbir imzalama anahtarı.

**Olası neden**

Uygulama nesnesi bozuk ve Azure AD uygulama için yapılandırılan sertifika tanımıyor.

**Çözümleme**

Silin ve yeni bir sertifika oluşturmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  tıklatın **yeni sertifika oluştur** altında **imzalama sertifikası SAML** bölümü.

9.  Sona erme tarihini seçin. Ardından **kaydedin.**

10. Denetleme **yeni sertifika etkin hale getirin** etkin sertifikanın geçersiz kılmak için. Ardından **kaydetmek** dikey pencerenin üstündeki ve geçiş sertifikası etkinleştirmek için kabul edin.

11. Altında **SAML imzalama sertifikası** 'yi tıklatın **kaldırmak** kaldırmak için **kullanılmayan** sertifika.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Uygulamaya gönderilen SAML talep özelleştirirken sorunu

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmek öğrenmek için bkz: [talep eşleme Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD çoklu oturum açma SAML protokolü gereksinimleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
