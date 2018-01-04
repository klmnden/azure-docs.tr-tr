---
title: "Federasyon tek oturum açma için Azure AD galeri uygulamanın yapılandırma | Microsoft Docs"
description: "Yapılandırmak için Federasyon nasıl çoklu oturum açmayı olan bir Azure AD galeri uygulamanın ve öğreticiler hızlı bir şekilde kullanmaya başlamak için kullanın"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: d8c22d20151fa70ba33ef017ab24c787f71fe6e0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon tek oturum açma için Azure AD galeri uygulama yapılandırma

Tüm uygulamaları ile Kurumsal çoklu oturum açma özelliği etkinleştirilmiş Azure AD galerisinde kullanılabilir adım adım öğretici sahiptir. Erişebileceğiniz [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) ayrıntılı adım adım yönergeler için.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulama (oturum açma URL'si, veren, oturum kapatma URL'si ve sertifika) Azure AD meta veri değerlerini yapılandırın](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Uygulamaya kullanıcılar atama](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure Portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** dikey.

6.  İçinde **bir ad girin** metin **galerisinden Ekle** bölümünde, uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin kutusu.

9.  Tıklatın **Ekle** düğmesi, bir uygulama eklemek için.

Bir kısa süre sonra uygulamanın yapılandırma dikey görüyor.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama için çoklu oturum açmayı yapılandırın

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9.  Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. SP tarafından başlatılan SSO'yu, oturum açma URL'si olarak uygulamayı yapılandırmak için bu gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

   2. IDP tarafından başlatılan SSO'yu, yanıt URL'si uygulamayı yapılandırmak için bu gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** tıklatın **Göster Gelişmiş URL ayarları** gerekli olmayan değerleri görmek istiyorsanız.

11. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

12. **İsteğe bağlı:** tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcı oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

  Bir öznitelik eklemek için:
   
   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   1. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

13. tıklatın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine çoklu oturum açma uygulamada yapılandırma. Ayrıca, meta verileri URL'leri ve SSO uygulama ile kurulum için gerekli sertifika sahiptir.

14. tıklatın **kaydetmek** yapılandırmayı kaydetmek için.

15. Kullanıcılar uygulamayı atayın.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin

Kullanıcı kimliği seçin veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Altında **kullanıcı öznitelikleri** bölümünde, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır. Seçili seçeneği kullanıcının kimliğini doğrulamak için uygulamada beklenen değer ile eşleşmesi gerekir.

  >[!NOTE] 
  >Azure AD seçin (kullanıcı tanımlayıcısı) NameID özniteliğin biçimini değere göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.
  >
  >

9.  Kullanıcı öznitelikleri eklemek için tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcı oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:
  
   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. **Kaydet** düğmesine tıklayın. Yeni öznitelik tablosundaki bakın.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

Uygulama meta verileri veya sertifika Azure AD'den karşıdan yüklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  *  Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamaları**.

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **SAML imzalama sertifikası** bölümünde ve ardından **karşıdan** sütun değeri. Çoklu oturum açma yapılandırma hangi uygulama gerektiriyor bağlı olarak, meta veri XML yükleme seçeneği ya da sertifika bakın.

Azure AD meta verilerini almak için bir URL sağlamaz. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcılar atama

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

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Uygulamaya gönderilen SAML talepler özelleştiriliyor

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmek öğrenmek için bkz: [talep eşleme Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](active-directory-application-proxy-sso-using-kcd.md)



