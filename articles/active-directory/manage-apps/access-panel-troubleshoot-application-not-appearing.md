---
title: Atanan uygulamanın erişim panelinde görünmeyen | Microsoft Docs
description: Bir uygulamanın erişim panelinde görünmeyen neden sorunlarını giderme
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
ms.topic: article
ms.date: 09/09/2018
ms.author: mimart
ms.reviwer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10dfcf337dc75a202e781e931f38783291a72fe7
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272750"
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a>Atanan uygulamanın erişim panelinde görünmüyor

Erişim paneli için bir kullanıcı Azure Active Directory (görüntüleyin ve bulut tabanlı uygulamalar Azure AD Yöneticisi bunları erişim izni vermiştir başlatmak için Azure AD) bir iş veya Okul hesabıyla sağlayan bir web tabanlı, portalıdır. Bu uygulamalar kullanıcılar Azure AD portalında yapılandırılır. Uygulama düzgün yapılandırılmış ve kullanıcı veya kullanıcı, uygulamayı erişim panelinde görmek için üyesi olduğu gruba atandı.

Bir kullanıcı görüyor olabilirsiniz uygulamaları türü aşağıdaki kategorilere ayrılır:

-   Office 365 uygulamaları

-   Federasyon tabanlı SSO ile yapılandırılan, Microsoft ve üçüncü taraf uygulamalar

-   Parola tabanlı SSO uygulamaları

-   Mevcut SSO çözümleriyle uygulamalar

## <a name="general-issues-to-check-first"></a>İlk denetlenecek genel sorunlar

-   Bir uygulama bir kullanıcıya yalnızca eklendiyse, giriş ve çıkış yeniden kullanıcının erişim paneline birkaç dakika sonra uygulama eklenip eklenmediğini görmek için oturum açmayı deneyin.

-   Bir lisansı yalnızca bir kullanıcı veya grup kaldırılmışsa bu üye, boyutu ve karmaşıklığı yapılacak değişiklikler grubuna bağlı olarak uzun zaman alabilir kullanıcıdır. Erişim paneline oturum açmadan önce ek süre izin verir.

## <a name="problems-related-to-application-configuration"></a>Uygulama yapılandırması ile ilgili sorunlar

Düzgün yapılandırılmadığından, uygulamanın bir kullanıcının erişim panelinde görünmesini değil. Aşağıda uygulama yapılandırması ile ilgili sorunları giderebilirsiniz bazı yollar şunlardır:

-   [Federasyon çoklu oturum açma Azure AD galeri uygulaması için yapılandırma](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Federasyon çoklu oturum açma galeri dışı bir uygulama için yapılandırma](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Parola tek bir oturum açma uygulamanın Azure AD galeri uygulaması için yapılandırma](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Parola tek bir oturum açma uygulama için bir galeri dışı uygulama yapılandırma](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon çoklu oturum açma Azure AD galeri uygulaması için yapılandırma

Adım adım öğretici kullanılabilir tüm uygulamalarda Kurumsal çoklu oturum açma özelliğine sahip etkin Azure AD galeri var. Erişebildiğiniz [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) yönelik ayrıntılı adım adım yönergeler.

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   [Uygulama Azure AD galeri ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [(Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [(Oturum açma URL'si, veren, oturum kapatma URL'si ve sertifikasını'de) uygulamasında Azure AD meta verileri değerlerini yapılandırma](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Uygulama Azure AD galeri ekleme

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

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD galeri dosyasından bir uygulama için çoklu oturum açmayı yapılandırın

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. SP tarafından başlatılan SSO'yu, oturum açma URL'si olarak uygulamayı yapılandırmak için bu gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

   2. IDP tarafından başlatılan SSO'yu, yanıt URL'si uygulamanın yapılandırmak için bu gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** tıklayın **Gelişmiş URL ayarlarını göster** gerekli olmayan değerleri görmek istiyorsanız.

11. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

12. **İsteğe bağlı:** tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

    Bir öznitelik eklemek için:

    1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

    2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

13. tıklayın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine uygulamada çoklu oturum açmayı yapılandırma. Ayrıca, uygulama ile SSO'yu ayarlamak için gerekli sertifika ve meta verileri URL'leri vardır.

14. Tıklayın **Kaydet** yapılandırmayı kaydetmek için.

15. Uygulamaya kullanıcı atama.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme

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
   >Azure AD, seçili değerine göre biçimi Nameıd özniteliği (kullanıcı tanımlayıcısı) veya SAML AuthRequest uygulama tarafından istenen biçimi seçer. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünün altında.
   >
   >

9. Kullanıcı öznitelikleri eklemek için tıklatın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

   2. Tıklayın **kaydedin.** Yeni öznitelik tabloda görürsünüz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

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

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Galeri dışı uygulama yapılandırmak için Azure AD Premium'a sahip gerekir ve uygulamanın SAML 2.0 destekler. Azure AD sürümleri hakkında daha fazla bilgi için ziyaret [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

-   [(Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [(Oturum açma URL'si, veren, oturum kapatma URL'si ve sertifikasını'de) uygulamasında Azure AD meta verileri değerlerini yapılandırma](#configure-the-application-for-password-single-sign-on-1)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>(Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma

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

10. Seçin **SAML tabanlı oturum açma** içinde **modu** açılır.

11. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

    1. IDP tarafından başlatılan SSO'yu uygulamasını yapılandırmak için yanıt URL'si ve tanımlayıcı girin.

    2.  **İsteğe bağlı:** SP tarafından başlatılan SSO'yu, oturum açma URL'si olarak uygulamayı yapılandırmak için bu gerekli bir değerdir.

12. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

13. **İsteğe bağlı:** tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

    Bir öznitelik eklemek için:

    1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

    2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

14. tıklayın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine uygulamada çoklu oturum açmayı yapılandırma. Ayrıca, Azure AD URL'leri ve uygulama için gerekli sertifikaları vardır.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme

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
   >Azure AD, seçili değerine göre biçimi Nameıd özniteliği (kullanıcı tanımlayıcısı) veya SAML AuthRequest uygulama tarafından istenen biçimi seçer. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünün altında.
   >
   >

9. Kullanıcı öznitelikleri eklemek için tıklatın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

   2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

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

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   [Uygulama Azure AD galeri ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Uygulama Azure AD galeri ekleme

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

#### <a name="configure-the-application-for-password-single-sign-on"></a>Parola çoklu oturum açma için uygulamayı yapılandırma

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

9. [Uygulamaya kullanıcı atama](#how-to-assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Bir uygulama için gereken Azure AD Galerisi yapılandırmak için:

-   [Galeri dışı bir uygulama ekleyin](#add-a-non-gallery-application)

-   [Parola çoklu oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleyin

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portalında](https://portal.azure.com) ve oturum açma bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6.  tıklayın **galeri dışı uygulama.**

7.  Uygulamanızda adını **adı** metin. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesinde görebilirsiniz.

#### <a name="configure-the-application-for-password-single-sign-on-1"></a> Parola çoklu oturum açma için uygulamayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    1.  Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modu **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu URL, burada kullanıcıların kullanıcı adını ve oturum açmak için parola girin kullanılır. Oturum açma alanlarını URL'SİNDE görünür olduğundan emin olun.

10. [Uygulamaya kullanıcı atama](#how-to-assign-a-user-to-an-application-directly).

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçip tıklayarak sağlayabilirsiniz **kimlik bilgilerini güncelleştirme** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="problems-related-to-assigning-applications-to-users"></a>Uygulamaları kullanıcılara atamak için ilgili sorunlar

Uygulamaya atanmamış olduğundan bir kullanıcı bir uygulamanın erişim panelinde görüyor olabilirsiniz değil. Aşağıda denetlemek için bazı yollar şunlardır:

-   [Bir kullanıcı uygulamaya atanıp atanmadığını kontrol edin](#check-if-a-user-is-assigned-to-the-application)

-   [Bir kullanıcının uygulamaya doğrudan atama](#how-to-assign-a-user-to-an-application-directly)

-   [Bir kullanıcı, uygulamayla ilgili bir lisansı atanıp atanmadığını kontrol edin](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Bir kullanıcıya lisans atama](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a>Bir kullanıcı uygulamaya atanıp atanmadığını kontrol edin

Bir kullanıcı uygulamaya atanıp atanmadığını kontrol etmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6. **Arama** söz konusu uygulamanın adı.

7. tıklayın **kullanıcılar ve gruplar**.

8. Kullanıcı uygulamaya atanıp atanmadığını görmek için kontrol edin.

   * Aksi halde bir kullanıcının uygulamaya doğrudan atamak "nasıl" adımları Bunu yapmak için.

### <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcının uygulamaya doğrudan atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici**.

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

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Bir kullanıcının uygulamayla ilgili bir lisans olup olmadığını denetleyin

Bir kullanıcının lisans atanmış denetlemek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7. tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

   * Kullanıcı bu kullanıcının erişim panelinde görünmesini birinci taraf Office uygulamaları etkinleştiren bir Office lisansı atanmışsa.

### <a name="how-to-assign-a-user-a-license"></a>Bir kullanıcı bir lisans atama 

Bir kullanıcıya bir lisans atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

8.  Tıklayın **atama** düğmesi.

9.  Seçin **bir veya daha çok ürünlerin** kullanılabilir ürünler listesinden.

10. **İsteğe bağlı** tıklayın **atama seçenekleri** hedefle ürünleri atamak için öğesi. Tıklayın **Tamam** bu ne zaman tamamlanır.

11. Tıklayın **atama** bu lisans bu kullanıcıya atamak için düğme.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Uygulamaları gruplara atama için ilgili sorunlar

Atanan uygulama bir grubun parçası olduklarından bir kullanıcı bir uygulamanın erişim panelinde görüyor olabilirsiniz. Aşağıda denetlemek için bazı yollar şunlardır:

-   [Bir kullanıcının grup üyeliklerini denetle](#check-a-users-group-memberships)

-   [Bir grubun uygulamaya doğrudan atama](#how-to-assign-an-application-to-a-group-directly)

-   [Bir kullanıcı grubu için bir lisans atanmış bir parçası olup olmadığını denetleyin](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Bir gruba lisans atama](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Bir kullanıcının grup üyeliklerini denetle

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7. tıklayın **grupları**.

8. Kullanıcı uygulamaya atanmış bir gruba ait olup olmadığını denetleyin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız **satıra tıklayın** seçin ve Grup DELETE.

### <a name="how-to-assign-an-application-to-a-group-directly"></a>Bir grubun uygulamaya doğrudan atama

Bir veya daha fazla grup, doğrudan uygulamaya atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici**.

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **grubu** göstermek için listedeki bir **onay kutusu**. Grubun profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla Grup Ekle**, başka bir tür **tam grup adı** içine **adına veya e-posta adresine göre arama** arama kutusuna ve bu gruba eklemek için onay kutusuna tıklayın için **seçili** listesi.

13. Grupları seçme işiniz bittiğinde, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz gruplara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi seçili gruplara uygulama atama.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatması mümkün.

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a>Bir kullanıcı grubu için bir lisans atanmış bir parçası olup olmadığını denetleyin

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7. tıklayın **grupları**.

8. belirli bir grubun satıra tıklayın.

9. tıklayın **lisansları** hangi Grup lisansları görmek için atanmış durumda.

   * Grup, bu belirli birinci taraf Office uygulamaları, kullanıcının erişim panelinde görünmesini sağlayabilir bir Office lisansı atanmışsa.

### <a name="how-to-assign-a-license-to-a-group"></a>Bir gruba lisans atama

Bir gruba lisans atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubunun ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda Grup lisansları görmek üzere atanır.

8.  Tıklayın **atama** düğmesi.

9.  Seçin **bir veya daha çok ürünlerin** kullanılabilir ürünler listesinden.

10. **İsteğe bağlı** tıklayın **atama seçenekleri** hedefle ürünleri atamak için öğesi. Tıklayın **Tamam** bu ne zaman tamamlanır.

11. Tıklayın **atama** bu gruba bu lisansları atamak için düğme. Bu, boyutu ve karmaşıklığı grubunun bağlı olarak uzun zaman alabilir.

>[!NOTE]
>Daha hızlı bir şekilde bunun için geçici olarak bir lisans kullanıcıya doğrudan atamayı göz önünde bulundurun. 
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory'ye yeni kullanıcı ekleme](./../fundamentals/add-users-azure-active-directory.md)

