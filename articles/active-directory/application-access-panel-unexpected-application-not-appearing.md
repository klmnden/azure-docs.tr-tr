---
title: Atanmış bir uygulama erişim Paneli'ne üzerinde görünmeyen | Microsoft Docs
description: Bir uygulama erişim panelinde görünmeyen neden sorun giderme
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
ms.reviwer: japere
ms.openlocfilehash: 5cb8b600f77c5b7dae91204126e64ec7b9a861ae
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29384125"
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a>Atanmış bir uygulama erişim Paneli'ne görünmüyor

Erişim için bir iş veya Okul hesabı Azure Active Directory'de (görüntülemek ve Azure AD Yöneticisi bunları erişim izni bulut tabanlı uygulamalar başlatmak için Azure AD) olan bir kullanıcı sağlayan bir web tabanlı portal, panosudur. Bu uygulamalar, Azure AD portalında kullanıcı adına yapılandırılır. Uygulama düzgün şekilde yapılandırılıp yapılandırılmadığını ve kullanıcı veya kullanıcı erişim paneli uygulamasında görmek için bir üyesi olan bir gruba atanmış.

Bir kullanıcı için görüyor olabilirsiniz uygulamalarının türü aşağıdaki kategorilere ayrılır:

-   Office 365 uygulamaları

-   Federasyon tabanlı SSO ile yapılandırılan Microsoft ve üçüncü taraf uygulamalar

-   Parola tabanlı SSO uygulamaları

-   Varolan SSO çözümleriyle uygulamaları

## <a name="general-issues-to-check-first"></a>İlk denetlemek için genel sorunlar

-   Bir uygulamayı yalnızca bir kullanıcıya eklendiyse, içeri ve dışarı kullanıcının erişim paneline birkaç dakika sonra uygulama eklediyseniz görmek için oturum yeniden deneyin.

-   Bir lisans yalnızca bir kullanıcı veya grup kaldırıldıysa bu üyesi boyutu ve karmaşıklığı grubunun yapılacak değişiklikler için bağlı olarak uzun bir süre devam edebilir kullanıcıdır. Erişim paneline oturum açmadan önce ek süresi için izin verin.

## <a name="problems-related-to-application-configuration"></a>Uygulama yapılandırması ile ilgili sorunlar

Düzgün yapılandırılmadığından, bir uygulama kullanıcının erişim panelinde görünüyor değil. Uygulama yapılandırması ile ilgili sorunları gidermek bazı yollar aşağıda verilmiştir:

-   [Federasyon tek oturum açma için Azure AD galeri uygulaması yapılandırma](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Federasyon çoklu oturum açma galeri olmayan uygulama için yapılandırma](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Parola tek bir oturum açma uygulama için Azure AD galeri uygulaması yapılandırma](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Parola tek bir oturum açma uygulama Galerisi olmayan uygulama için yapılandırma](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon tek oturum açma için Azure AD galeri uygulaması yapılandırma

Kurumsal çoklu oturum açma özelliğine sahip etkin Azure AD galerisinde tüm uygulamaları kullanılabilir adım adım öğretici sahiptir. Erişebileceğiniz [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) ayrıntılı adım adım yönergeler için.

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulama (oturum açma URL'si, veren, oturum kapatma URL'si ve sertifika) Azure AD meta veri değerlerini yapılandırın](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

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

Kısa bir süre sonra uygulamanın yapılandırma bölmesi görüyor.

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama için çoklu oturum açmayı yapılandırın

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9.  Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. SP tarafından başlatılan SSO'yu, oturum açma URL'si olarak uygulamayı yapılandırmak için bu gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

   2. IDP tarafından başlatılan SSO'yu, yanıt URL'si uygulamayı yapılandırmak için bu gerekli bir değerdir. Bazı uygulamalar için tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** tıklatın **Göster Gelişmiş URL ayarları** gerekli olmayan değerleri görmek istiyorsanız.

11. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

12. **İsteğe bağlı:** tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

13. tıklatın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine çoklu oturum açma uygulamada yapılandırma. Ayrıca, uygulama ile SSO'yu ayarlamak için gerekli sertifika ve meta verileri URL'leri vardır.

14. tıklatın **kaydetmek** yapılandırmayı kaydetmek için.

15. Kullanıcılar uygulamayı atayın.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin

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
   >Azure AD, seçili değere göre biçim NameID özniteliği (kullanıcı tanımlayıcısı) veya SAML AuthRequest uygulama tarafından istenen biçim seçer. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.
   >
   >

9.  Kullanıcı öznitelikleri eklemek için tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki görürsünüz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

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

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri olmayan uygulama için yapılandırma

Bir galeri olmayan uygulama yapılandırmak için Azure AD premium olması gerekir ve uygulama SAML 2.0 destekler. Azure AD sürümleri hakkında daha fazla bilgi için ziyaret [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın](#configuring-single-sign-on)

-   [Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulama (oturum açma URL'si, veren, oturum kapatma URL'si ve sertifika) Azure AD meta veri değerlerini yapılandırın](#configuring-single-sign-on)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın

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

10. Seçin **SAML tabanlı oturum açma** içinde **modu** açılır.

11. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. Uygulama IDP tarafından başlatılan SSO'yu yapılandırmak için yanıt URL'si ve tanımlayıcı girin.

   2.  **İsteğe bağlı:** SP tarafından başlatılan SSO'yu, oturum açma URL'si olarak uygulamayı yapılandırmak için bu gerekli bir değerdir.

12. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

13. **İsteğe bağlı:** tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

14. tıklatın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine çoklu oturum açma uygulamada yapılandırma. Ayrıca, Azure AD URL'leri ve uygulama için gerekli sertifikaları vardır.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin

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
   >Azure AD, seçili değere göre biçim NameID özniteliği (kullanıcı tanımlayıcısı) veya SAML AuthRequest uygulama tarafından istenen biçim seçer. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.
   >
   >

9.  Kullanıcı öznitelikleri eklemek için tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

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

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Parola tek oturum açma için Azure AD galeri uygulaması yapılandırma

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Azure AD Galeriden bir uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galeriden bir uygulama ekleme

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

#### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

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

9.  [Uygulamaya kullanıcılar atama](#how-to-assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri olmayan uygulama için yapılandırma

Bir uygulama için gereken Azure AD galerisinden yapılandırmak için:

-   [Bir galeri olmayan uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulaması parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Bir galeri olmayan uygulama ekleme

Azure AD Galeriden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Açık [Azure portal](https://portal.azure.com) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  tıklatın **olmayan galeri uygulaması.**

7.  Uygulamanızda adını girin **adı** metin kutusu. Seçin **ekleyin.**

Kısa bir süre sonra uygulamanın yapılandırma bölmesi görüyor.

#### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulaması parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    1.  Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu kullanıcılar, kullanıcı adını ve oturum açmak için parola girdiğiniz yere URL'dir. Oturum açma alanları URL'de göründüğünden emin olun.

10. [Uygulamaya kullanıcılar atama](#how-to-assign-a-user-to-an-application-directly).

11. Ayrıca, kullanıcı adına kimlik bilgilerini kullanıcıları satırlarını seçerek ve tıklayarak sağlayabilirsiniz **güncelleştirme kimlik bilgileri** ve kullanıcılar adına kullanıcı adı ve parola girme. Aksi takdirde, kullanıcılar başlatma sırasında kimlik kendilerini girmeniz istenir.

## <a name="problems-related-to-assigning-applications-to-users"></a>Uygulamaları kullanıcılara atamak için ilgili sorunlar

Uygulamaya atanmadıkları bir kullanıcı bir uygulama kullanıcıların erişim panelinde görüyor olabilirsiniz değil. Aşağıda denetlemek için bazı yöntemler şunlardır:

-   [Kullanıcı uygulamaya atandığından emin olun](#check-if-a-user-is-assigned-to-the-application)

-   [Kullanıcının uygulamaya doğrudan atama](#how-to-assign-a-user-to-an-application-directly)

-   [Bir kullanıcının uygulamayla ilgili bir lisansa atanıp atanmadığını denetleyin](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Bir kullanıcıya bir lisans atama](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a>Kullanıcı uygulamaya atandığından emin olun

Kullanıcı uygulamaya atanıp atanmadığını denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6.  **Arama** için söz konusu uygulamanın adı.

7.  tıklatın **kullanıcılar ve gruplar**.

8.  Kullanıcı uygulamaya atanıp atanmadığını denetleyin.

   * Aksi halde "bir kullanıcının uygulamaya doğrudan atama" adımları Bunu yapmak için.

### <a name="how-to-assign-a-user-to-an-application-directly"></a>Kullanıcının uygulamaya doğrudan atama

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici**.

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

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Bir kullanıcının uygulamayla ilgili bir lisansı altında olup olmadığını denetleyin

Bir kullanıcının atanan lisansları denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

  * Kullanıcı bu kullanıcının erişim panelinde için birinci taraf Office uygulamalarını sağlayan bir Office lisansı atanırsa.

### <a name="how-to-assign-a-user-a-license"></a>Bir kullanıcı bir lisans atama 

Bir kullanıcıya bir lisans atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

8.  tıklatın **atamak** düğmesi.

9.  Seçin **bir veya daha fazla ürün** mevcut ürünler listesinden.

10. **İsteğe bağlı** tıklatın **atama seçenekleri** ürünleri granularly atanacak öğe. Tıklatın **Tamam** ne zaman bu tamamlandı.

11. Tıklatın **atamak** bu lisans bu kullanıcıya atamak için düğmesi.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplarına atamayla ilgili sorunlar

Uygulama atanmış bir grubun parçası olmadığından bir kullanıcı bir uygulama kullanıcıların erişim panelinde görüyor olabilirsiniz. Aşağıda denetlemek için bazı yöntemler şunlardır:

-   [Bir kullanıcı grup üyeliklerini denetleyin](#check-a-users-group-memberships)

-   [Bir uygulama grubu için doğrudan atama](#how-to-assign-an-application-to-a-group-directly)

-   [Bir kullanıcı için bir lisans atanması grubunun parçası olup olmadığını denetleyin](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Bir gruba bir lisans atama](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Bir kullanıcı grup üyeliklerini denetleyin

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **grupları**.

8.  Kullanıcı uygulamaya atanan bir grubun parçası olup olmadığını denetleyin.

  * Kullanıcıyı gruptan kaldırmak istiyorsanız, **satıra tıklayın** seçin ve Grup DELETE.

### <a name="how-to-assign-an-application-to-a-group-directly"></a>Bir uygulama grubu için doğrudan atama

Bir veya daha fazla grupları doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici**.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** bölmesi.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **grup** ortaya çıkarmak için listedeki bir **onay kutusunu**. Grubun profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla grubu Ekle**, başka bir tür **tam grup adı** içine **ad veya e-posta adresine göre arama** arama kutusuna ve Bu gruba eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Grupları seçmek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** bölmesinde seçtiğiniz gruplarına atamak için bir rol seçin.

15. Tıklatın **atamak** seçilen grupları uygulamaya atamak için düğmesi.

Kısa bir süre sonra seçtiğiniz kullanıcıların erişim panelinde bu uygulamaları başlatabilir.

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a>Bir kullanıcı için bir lisans atanması grubunun parçası olup olmadığını denetleyin

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **grupları**.

8.  belirli bir grup satıra tıklayın.

9.  tıklatın **lisansları** hangi grubun lisansları görmek için atanır.

   * Bu kullanıcının erişim panelinde için belirli birinci taraf Office uygulamalarını etkinleştirebilir bir Office lisansı için Grup atanmışsa.

### <a name="how-to-assign-a-license-to-a-group"></a>Bir gruba bir lisans atama

Bir gruba bir lisans atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubu için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** hangi Grup şu anda lisansları görmek için atanır.

8.  tıklatın **atamak** düğmesi.

9.  Seçin **bir veya daha fazla ürün** mevcut ürünler listesinden.

10. **İsteğe bağlı** tıklatın **atama seçenekleri** ürünleri granularly atanacak öğe. Tıklatın **Tamam** ne zaman bu tamamlandı.

11. Tıklatın **atamak** bu gruba bu lisansları atamak için düğmesi. Bu, boyut ve Grup karmaşıklığına bağlı olarak uzun bir süre devam edebilir.

>[!NOTE]
>Bu daha hızlı yapmak için kullanıcıya bir lisans'geçici olarak doğrudan atama göz önünde bulundurun. 
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory'ye yeni kullanıcı ekleme](active-directory-users-create-azure-portal.md)

