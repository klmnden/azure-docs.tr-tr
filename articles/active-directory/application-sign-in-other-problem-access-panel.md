---
title: Bir uygulamaya erişim panelinde oturum açmada sorun | Microsoft Docs
description: Sorunları gidermek nasıl uygulama myapps.microsoft.com adresindeki Microsoft Azure AD erişim paneli erişme
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 5765d64fccba69edd0ebe91a6c34694763e6c539
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34070293"
---
# <a name="problems-signing-in-to-an-application-from-the-access-panel"></a>Bir uygulamaya erişim panelinde oturum açma sorunları

Erişim için bir iş veya Okul hesabı Azure Active Directory'de (görüntülemek ve Azure AD Yöneticisi bunları erişim izni bulut tabanlı uygulamalar başlatmak için Azure AD) olan bir kullanıcı sağlayan bir web tabanlı portal panosudur. 

Bu uygulamalar, Azure AD portalında kullanıcı adına yapılandırılır. Uygulama düzgün şekilde yapılandırılıp yapılandırılmadığını ve kullanıcı veya kullanıcı erişim paneli uygulamasında görmek için bir üyesi olan bir gruba atanmış.

Bir kullanıcı için görüyor olabilirsiniz uygulamalarının türü aşağıdaki kategorilere ayrılır:

-   Office 365 uygulamaları

-   Federasyon tabanlı SSO ile yapılandırılan Microsoft ve üçüncü taraf uygulamalar

-   Parola tabanlı SSO uygulamaları

-   Varolan SSO çözümleriyle uygulamaları

## <a name="general-issues-to-check-first"></a>İlk denetlemek için genel sorunlar

-   Kullanarak emin bir **tarayıcı** erişim paneli için minimum gereksinimleri karşılayan.

-   Kullanıcının tarayıcısı için uygulama URL'sini ekledi emin olun, **Güvenilen siteler**.

-   Uygulama denetlediğinizden emin olun **yapılandırılmış** doğru.

-   Kullanıcının hesabı olduğundan emin olun **etkin** oturum açma işlemleri için.

-   Kullanıcının hesabı olduğundan emin olun **kilitli değil.**

-   Kullanıcının emin olun **parola süresi değil veya unutulursa.**

-   Emin olun **çok faktörlü kimlik doğrulaması** kullanıcı erişimini engellemediğinden.

-   Emin olun bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi kullanıcı erişimini engellemediğinden.

-   Olduğundan emin olun kullanıcının **kimlik doğrulaması kişi bilgilerini** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkeleri izin güncel değil.

-   Tarayıcınızın tanımlama bilgilerini temizleyip ve yeniden oturum açmaya de deneyin emin olun.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim paneli toplantı tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma (SSO) erişimi Masası'nda kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde bu uzantıyı otomatik olarak yüklenir.

Parola tabanlı, SSO için son kullanıcının tarayıcılar olabilir:

-   Internet Explorer 8, 9, 10, 11--Windows 7 veya üzeri

-   Edge Windows 10 Anniversary Edition veya daha yenisi

-   Chrome--Windows 7 veya daha sonra ve MacOS x veya sonraki sürümlerde

-   Firefox 26,0 veya daha sonra--Windows XP SP2 veya sonraki ve Mac OS X 10,6 veya üzeri

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleme

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  tıklatın bir **parola SSO uygulaması** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınız için karşıdan yükleme bağlantısı yönlendirilir temel. **Ekleme** tarayıcınız uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirmek** veya **izin** uzantısı.

6.  Bir kez yüklenir, **yeniden** tarayıcı oturumunda.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamaları

Ayrıca uzantısı Chrome ve kenar için aşağıdaki doğrudan bağlantılarından yükleyebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Edge erişim paneli uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon tek oturum açma için Azure AD galeri uygulaması yapılandırma

Kurumsal çoklu oturum açma özelliğine sahip etkin Azure AD galerisinde tüm uygulama kullanılabilir bir adım adım öğretici sahiptir. Erişebileceğiniz [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) ayrıntılı adım adım yönergeler için.

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application)

-   [Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulama (oturum açma URL'si, veren, oturum kapatma URL'si ve sertifika) Azure AD meta veri değerlerini yapılandırın](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Uygulamaya kullanıcılar atama](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** metin **galerisinden Ekle** bölümünde, uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin kutusu.

9.  Tıklatın **Ekle** düğmesi, bir uygulama eklemek için.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama için çoklu oturum açmayı yapılandırın

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9.  Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. Uygulama SP tarafından başlatılan SSO'yu yapılandırmak için oturum açma URL'si gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

   2. Uygulama IDP tarafından başlatılan SSO'yu yapılandırmak için yanıt URL'si gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** tıklatın **Göster Gelişmiş URL ayarları** gerekli olmayan değerleri görmek istiyorsanız.

11. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

12. **İsteğe bağlı:** tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

13. tıklatın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine çoklu oturum açma uygulamada yapılandırma. Ayrıca, uygulama ile SSO kurulum için gerekli sertifika ve meta verileri URL'leri vardır.

14. tıklatın **kaydetmek** yapılandırmayı kaydetmek için.

15. Kullanıcılar uygulamayı atayın.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin

Kullanıcı kimliği seçin veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Uygulama görmüyorsanız, istediğiniz burada gösterilecek kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek  **Tüm uygulamalar.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Altında **kullanıcı öznitelikleri** bölümünde, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır. Seçili seçeneği kullanıcının kimliğini doğrulamak için uygulamada beklenen değer ile eşleşmesi gerekir.

    >[!NOTE]
    >Azure AD seçin (kullanıcı tanımlayıcısı) NameID özniteliğin biçimini değere göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.
    >
    >

9.  Kullanıcı öznitelikleri eklemek için tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

Uygulama meta verileri veya sertifika Azure AD'den karşıdan yüklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **SAML imzalama sertifikası** bölümünde ve ardından **karşıdan** sütun değeri. Çoklu oturum açma yapılandırma hangi uygulama gerektiriyor bağlı olarak, meta veri XML yükleme seçeneği ya da sertifika bakın.

    Azure AD meta verilerini almak için bir URL sağlamaz. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri olmayan uygulama için yapılandırma

Bir galeri olmayan uygulama yapılandırmak için Azure AD premium olması gerekir ve uygulama SAML 2.0 destekler. Azure AD sürümleri hakkında daha fazla bilgi için ziyaret [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın](#configuring-single-sign-on)

-   [Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulama (oturum açma URL'si, veren, oturum kapatma URL'si ve sertifika) Azure AD meta veri değerlerini yapılandırın](#configuring-single-sign-on)

### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın

Azure AD Galerisi'nde olmayan bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  tıklatın **olmayan galeri uygulama** içinde **kendi uygulama Ekle** bölümü.

7.  Uygulama adını girin **adı** metin kutusu.

8.  Tıklatın **Ekle** düğmesi, bir uygulama eklemek için.

9.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

10. Seçin **SAML tabanlı oturum açma** içinde **modu** açılır

11. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

  1. Uygulama IDP tarafından başlatılan SSO'yu yapılandırmak için yanıt URL'si ve tanımlayıcı girin.

  2. **İsteğe bağlı:** uygulama SP tarafından başlatılan SSO'yu yapılandırmak için oturum açma URL'si gerekli bir değerdir.

12. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

13. **İsteğe bağlı:** tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

14. tıklatın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine çoklu oturum açma uygulamada yapılandırma. Ayrıca, Azure AD URL'leri ve uygulama için gerekli sertifika vardır.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin

Kullanıcı kimliği seçin veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Altında **kullanıcı öznitelikleri** bölümünde, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır. Seçili seçeneği kullanıcının kimliğini doğrulamak için uygulamada beklenen değer ile eşleşmesi gerekir.

   >[!NOTE]
   >Azure AD seçin (kullanıcı tanımlayıcısı) NameID özniteliğin biçimini değere göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.
   >
   >

9.  Kullanıcı öznitelikleri eklemek için tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1'ı **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2 tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

Uygulama meta verileri veya sertifika Azure AD'den karşıdan yüklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **SAML imzalama sertifikası** bölümünde ve ardından **karşıdan** sütun değeri. Çoklu oturum açma yapılandırma hangi uygulama gerektiriyor bağlı olarak, meta veri XML yükleme seçeneği ya da sertifika bakın.

    Azure AD meta verilerini almak için bir URL sağlamaz. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola tek oturum açma için Azure AD galeri uygulaması yapılandırma

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** metin **galerisinden Ekle** bölümünde, uygulamanın adını yazın

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin kutusu.

9.  Tıklatın **Ekle** düğmesi, bir uygulama eklemek için.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

 * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Kullanıcılar uygulamayı atayın.

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri olmayan uygulama için yapılandırma

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Bir galeri olmayan uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Bir galeri olmayan uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  tıklatın **olmayan galeri uygulaması.**

7.  Uygulamanızda adını girin **adı** metin kutusu. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesi görüyor.

### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

 * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu kullanıcılar, kullanıcı adını ve oturum açmak için parola girdiğiniz yere URL'dir. Oturum açma alanları URL'de göründüğünden emin olun.

10. Kullanıcılar uygulamayı atayın.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Kullanıcının uygulamaya doğrudan atama

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

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözümleme yaparsanız

bir destek bileti aşağıdaki bilgilerle varsa açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Tenantıd

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesine hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

