---
title: "Federasyon çoklu oturum açma galeri olmayan uygulama için yapılandırma | Microsoft Docs"
description: "Federasyon tek oturum açma için Azure AD ile tümleştirmek istediğiniz özel bir galeri olmayan uygulama yapılandırma"
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
ms.openlocfilehash: 3296587de5f89b7c96a24ec975249c4705b9f072
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri olmayan uygulama için yapılandırma

Bir galeri olmayan uygulama için çoklu oturum açmayı yapılandırmak için *kod yazma olmadan*, bir aboneliğe sahip olmanız gerekir veya Azure AD Premium ve uygulaması SAML 2.0 desteklemesi gerekir. Azure AD sürümleri hakkında daha fazla bilgi için ziyaret [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
SAML 2.0 ile Federasyon çoklu oturum açma için (örneğin özel) bir galeri olmayan uygulama yapılandırmak için gerekli olan adımları üst düzey bir özeti aşağıdadır.

-   [Azure AD (oturum açma URL'si, tanımlayıcı, yanıt URL'si) uygulamanın meta veri değerlerini yapılandırın](#_Configuring_single_sign-on)

-   [Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulama (oturum açma URL'si, veren, oturum kapatma URL'si ve sertifika) Azure AD meta veri değerlerini yapılandırın](#_Configuring_single_sign-on)

-   [Uygulamaya kullanıcılar atama](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a>Çoklu oturum açma galeri olmayan uygulamalar için yapılandırma

Azure AD Galerisi'nde olmayan bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **Ekle** düğmesine sağ üst köşede **kurumsal uygulamalar** bölmesi.

6.  tıklatın **olmayan galeri uygulama** içinde **kendi uygulama Ekle** bölümü

7.  Uygulama adını girin **adı** metin kutusu.

8.  Tıklatın **Ekle** düğmesi, bir uygulama eklemek için.

9.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

10. Seçin **SAML tabanlı oturum açma** içinde **modu** açılır.

11. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

   1. Uygulama IDP tarafından başlatılan SSO'yu yapılandırmak için yanıt URL'si ve tanımlayıcı girin.

   2. **İsteğe bağlı:** SP tarafından başlatılan SSO'yu, oturum açma URL'si uygulamayı yapılandırmak için bu gerekli bir değerdir.

12. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

13. **İsteğe bağlı:** tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

14. tıklatın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine çoklu oturum açma uygulamada yapılandırma. Ayrıca, Azure AD URL'leri ve uygulama için gerekli sertifika sahiptir.

15. [Kullanıcılar uygulamayı atayın.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı seçin ve uygulamaya gönderilmek üzere kullanıcı öznitelikleri ekleyin

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

 >[! Not} Azure AD, seçili değere göre NameID özniteliği (kullanıcı tanımlayıcısı) biçimi veya SAML AuthRequest uygulama tarafından istenen biçimi seçin. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.
 >
 >

9.  Kullanıcı öznitelikleri eklemek için tıklatın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** kullanıcılar oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için.

   Bir öznitelik eklemek için:

   1. tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   2. Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

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

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcılar atama

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

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Uygulamaya gönderilen SAML talepler özelleştiriliyor

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmek öğrenmek için bkz: [talep eşleme Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](active-directory-application-proxy-sso-using-kcd.md)
