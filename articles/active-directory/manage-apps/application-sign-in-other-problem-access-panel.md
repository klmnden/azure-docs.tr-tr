---
title: Bir uygulamaya erişim panelinden oturum açmada sorun | Microsoft Docs
description: Uygulamanın myapps.microsoft.com adresindeki Microsoft Azure AD erişim paneli erişme sorunları nasıl giderilir
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
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51e486e8eb2fef04c1b876dff3de644dda4aaf2e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65781128"
---
# <a name="problems-signing-in-to-an-application-from-the-access-panel"></a>Bir uygulamaya erişim panelinden oturum açmada sorun

Erişim paneli için bir kullanıcı Azure Active Directory (görüntüleyin ve bulut tabanlı uygulamalar Azure AD Yöneticisi bunları erişim izni vermiştir başlatmak için Azure AD) bir iş veya Okul hesabıyla sağlayan bir web tabanlı bir portaldır. 

Bu uygulamalar kullanıcılar Azure AD portalında yapılandırılır. Uygulama düzgün yapılandırılmış ve kullanıcı veya kullanıcı, uygulamayı erişim panelinde görmek için üyesi olduğu gruba atandı.

Bir kullanıcı görüyor olabilirsiniz uygulamaları türü aşağıdaki kategorilere ayrılır:

-   Office 365 uygulamaları

-   Federasyon tabanlı SSO ile yapılandırılan, Microsoft ve üçüncü taraf uygulamalar

-   Parola tabanlı SSO uygulamaları

-   Mevcut SSO çözümleriyle uygulamalar

## <a name="general-issues-to-check-first"></a>İlk denetlenecek genel sorunlar

-   Kullanarak emin bir **tarayıcı** erişim panelinde en düşük gereksinimlerini karşılayan.

-   Kullanıcının tarayıcı, uygulamaya URL'sini ekledi emin olun, **Güvenilen siteler**.

-   Uygulama denetlediğinizden emin olun **yapılandırılmış** doğru.

-   Kullanıcı hesabı olduğundan emin olun **etkin** için oturum açma işlemleri.

-   Kullanıcı hesabı olduğundan emin olun **kilitli değil.**

-   Kullanıcının emin **parola süresi değil veya unutulursa.**

-   Emin **multi-Factor Authentication** kullanıcı erişimi engellemediğini.

-   Emin bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi, kullanıcı erişimini engellemediğinden.

-   Emin olun kullanıcının **kimlik doğrulaması iletişim bilgileri** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkelerinin izin güncel olduğundan.

-   Tarayıcınızın tanımlama bilgilerini temizleyip tekrar oturum açmaya çalışırken de deneyin için emin olun.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Toplantı için erişim paneli tarayıcı gereksinimleri

Erişim paneli JavaScript destekleyen bir tarayıcı gerektirir ve CSS etkinleştirdi. Parola tabanlı çoklu oturum açma (SSO) erişim Paneli'nde kullanmak için erişim paneli uzantısı kullanıcının tarayıcısında yüklenmesi gerekir. Bu uzantı, bir kullanıcı, parola tabanlı SSO için yapılandırılmış bir uygulama seçtiğinde otomatik olarak indirilir.

Parola tabanlı SSO için son kullanıcının tarayıcılar olabilir:

-   Internet Explorer 8, 9, 10, 11 – Windows 7 veya üzeri

-   Windows 10 Anniversary Edition veya sonrası, Microsoft Edge

-   Chrome--Üzerinde Windows 7 ve daha sonra ve MacOS x veya sonrası

-   Firefox 26,0 veya daha sonra--Windows XP SP2 veya üstü ve Mac OS X 10,6 veya sonraki bir sürümü üzerinde

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  ' a tıklayın bir **parola SSO uygulama** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınız indirme bağlantısını yönlendirildiği temel. **Ekleme** tarayıcınızı uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirme** veya **izin** uzantısı.

6.  Yüklendiğinde, **yeniden** , tarayıcı oturumu.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamalarınız

Ayrıca uzantısı Chrome ve Microsoft Edge için aşağıdaki doğrudan bağlantılardan indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Microsoft Edge erişim paneli uzantısı](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon çoklu oturum açma Azure AD galeri uygulaması için yapılandırma

Tüm Kurumsal çoklu oturum açma özelliğine sahip etkin bir Azure AD Galerisi uygulamasında kullanılabilen bir adım adım öğretici sahiptir. Erişebildiğiniz [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) yönelik ayrıntılı adım adım yönergeler.

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   Uygulama Azure AD galeri ekleme

-   [(Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [(Oturum açma URL'si, veren, oturum kapatma URL'si ve sertifikasını'de) uygulamasında Azure AD meta verileri değerlerini yapılandırma](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   Uygulamaya kullanıcı atama

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Uygulama Azure AD galeri ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** TextBox'dan **Galeriden Ekle** bölümünde, uygulama adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin.

9.  Tıklayın **Ekle** düğme, uygulamayı eklemek için.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD galeri dosyasından bir uygulama için çoklu oturum açmayı yapılandırın

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. SP tarafından başlatılan SSO'yu uygulamasını yapılandırmak için oturum açma URL'si gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

   2. IDP tarafından başlatılan SSO'yu uygulamasını yapılandırmak için yanıt URL'si gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** tıklayın **Gelişmiş URL ayarlarını göster** gerekli olmayan değerleri görmek istiyorsanız.

11. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

12. **İsteğe bağlı:** tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

    Bir öznitelik eklemek için:

    1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

    2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

13. tıklayın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine uygulamada çoklu oturum açmayı yapılandırma. Ayrıca, uygulama ile SSO kurulum için gerekli sertifika ve meta verileri URL'leri vardır.

14. Tıklayın **Kaydet** yapılandırmayı kaydetmek için.

15. Uygulamaya kullanıcı atama.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme

Kullanıcı tanımlayıcısı seçin veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Uygulama görmüyorsanız, istediğiniz kullanın, burada görünmesi **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** seçeneğini  **Tüm uygulamalar.**

6. Çoklu oturum açma yapılandırmış olduğunuz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Altında **kullanıcı öznitelikleri** bölümünde, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır. Seçeneği, kullanıcının kimliğini doğrulamak için uygulamada beklenen değer ile eşleşmesi gerekiyor.

   >[!NOTE]
   >Azure AD seçin (kullanıcı tanımlayıcısı) Nameıd öznitelik biçimi değerine göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünün altında.
   >
   >

9. Kullanıcı öznitelikleri eklemek için tıklatın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

   2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

Azure AD uygulama meta verileri veya sertifika indirmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açma yapılandırmış olduğunuz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Git **SAML imzalama sertifikası** bölümünde'a tıklayın **indirme** sütun değeri. Hangi uygulamanın çoklu oturum açmayı yapılandırma gerektirir bağlı olarak, meta veri XML yükleme seçeneği ya da sertifika bakın.

   Azure AD meta verilerini almak için bir URL sağlamaz. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Galeri dışı uygulama yapılandırmak için Azure AD Premium'a sahip gerekir ve uygulamanın SAML 2.0 destekler. Azure AD sürümleri hakkında daha fazla bilgi için ziyaret [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

-   (Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma

-   [Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   (Oturum açma URL'si, veren, oturum kapatma URL'si ve sertifikasını'de) uygulamasında Azure AD meta verileri değerlerini yapılandırma

### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>(Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma

Azure AD Galerisi'nde yer almayan bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6. tıklayın **galeri dışı uygulama** içinde **kendi uygulamanızı ekleyin** bölümü.

7. Uygulama adını girin **adı** metin.

8. Tıklayın **Ekle** düğme, uygulamayı eklemek için.

9. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

10. Seçin **SAML tabanlı oturum açma** içinde **modu** açılan kutusu

11. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

    1. IDP tarafından başlatılan SSO'yu uygulamasını yapılandırmak için yanıt URL'si ve tanımlayıcı girin.

    2. **İsteğe bağlı:** SP tarafından başlatılan SSO'yu uygulamasını yapılandırmak için oturum açma URL'si gerekli bir değerdir.

12. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

13. **İsteğe bağlı:** tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

    Bir öznitelik eklemek için:

    1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

    2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

14. tıklayın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine uygulamada çoklu oturum açmayı yapılandırma. Ayrıca, Azure AD URL'leri ve uygulama için gerekli sertifika vardır.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme

Kullanıcı tanımlayıcısı seçin veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açma yapılandırmış olduğunuz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Altında **kullanıcı öznitelikleri** bölümünde, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır. Seçeneği, kullanıcının kimliğini doğrulamak için uygulamada beklenen değer ile eşleşmesi gerekiyor.

   >[!NOTE]
   >Azure AD seçin (kullanıcı tanımlayıcısı) Nameıd öznitelik biçimi değerine göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünün altında.
   >
   >

9. Kullanıcı öznitelikleri eklemek için tıklatın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1'ı **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

   2 tıklatın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

Azure AD uygulama meta verileri veya sertifika indirmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açma yapılandırmış olduğunuz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Git **SAML imzalama sertifikası** bölümünde'a tıklayın **indirme** sütun değeri. Hangi uygulamanın çoklu oturum açmayı yapılandırma gerektirir bağlı olarak, meta veri XML yükleme seçeneği ya da sertifika bakın.

   Azure AD meta verilerini almak için bir URL sağlamaz. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   Uygulama Azure AD galeri ekleme

-   Parola çoklu oturum açma için uygulamayı yapılandırma

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Uygulama Azure AD galeri ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  İçinde **bir ad girin** TextBox'dan **Galeriden Ekle** bölümünde, uygulama adını yazın

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin

8.  Uygulama eklemeden önce adını değiştirebilirsiniz **adı** metin.

9.  Tıklayın **Ekle** düğme, uygulamayı eklemek için.

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. Uygulamaya kullanıcı atama.

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   [Galeri dışı bir uygulama ekleyin](#add-a-non-gallery-application)

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleyin

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  tıklayın **galeri dışı uygulama.**

7.  Uygulamanızda adını **adı** metin. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. Girin **oturum açma URL'si**. Bu URL, burada kullanıcıların kullanıcı adını ve oturum açmak için parola girin kullanılır. Oturum açma alanlarını URL'SİNDE görünür olduğundan emin olun.

10. Uygulamaya kullanıcı atama.

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcının uygulamaya doğrudan atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

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

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatması mümkün.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımlarını sorunu çözümleme durumunda

Aşağıdaki bilgilerle varsa bir destek bileti açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesi sırasında hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

