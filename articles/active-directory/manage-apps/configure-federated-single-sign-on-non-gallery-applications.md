---
title: Federasyon çoklu oturum açma galeri dışı bir uygulama için yapılandırma | Microsoft Docs
description: Federasyon çoklu oturum açma, Azure AD ile tümleştirmek istediğiniz özel bir galeri dışı uygulama için yapılandırma
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fcb77fe257a1b99525d009a1756a473e7e61a5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60291900"
---
# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri dışı bir uygulama için yapılandırma

Galeri dışı bir uygulama için çoklu oturum açmayı yapılandırmak için *kod yazmadan*, bir aboneliğe sahip olmanız gerekir veya SAML 2.0, Azure AD Premium ve uygulamayı desteklemesi gerekir. Azure AD sürümleri hakkında daha fazla bilgi için ziyaret [Azure AD fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
SAML 2.0 ile Federasyon çoklu oturum açma için (örneğin özel) bir galeri dışı uygulama yapılandırmak için gerekli adımları üst düzey bir genel bakış aşağıda verilmiştir.

-   (Oturum açma URL'si, tanımlayıcı, yanıt URL'si) Azure AD'de uygulama meta verileri değerlerini yapılandırma

-   [Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verileri ve sertifika alma](#download-the-azure-ad-metadata-or-certificate)

-   (Oturum açma URL'si, veren, oturum kapatma URL'si ve sertifikasını'de) uygulamasında Azure AD meta verileri değerlerini yapılandırma

-   Uygulamaya kullanıcı atama

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a>Galeri dışı uygulamalar için çoklu oturum açma yapılandırılıyor

Azure AD Galerisi'nde yer almayan bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **Ekle** sağ üst köşesindeki düğme **kurumsal uygulamalar** bölmesi.

6. tıklayın **galeri dışı uygulama** içinde **kendi uygulamanızı ekleyin** bölümü

7. Uygulama adını girin **adı** metin.

8. Tıklayın **Ekle** düğme, uygulamayı eklemek için.

9. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

10. Seçin **SAML tabanlı oturum açma** içinde **modu** açılır.

11. Gerekli değerleri girin **etki alanı ve URL'ler.** Bu değerleri uygulama satıcısından almanız gerekir.

    1. IDP tarafından başlatılan SSO'yu uygulamasını yapılandırmak için yanıt URL'si ve tanımlayıcı girin.

    2. **İsteğe bağlı:** SP tarafından başlatılan SSO'yu, oturum açma URL'si uygulamayı yapılandırmak için bu gerekli bir değerdir.

12. İçinde **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

13. **İsteğe bağlı:** tıklayın **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilecek öznitelikleri düzenlemek için.

    Bir öznitelik eklemek için:

    1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

    2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

14. tıklayın **yapılandırma &lt;uygulama adı&gt;**  erişim belgelerine uygulamada çoklu oturum açmayı yapılandırma. Ayrıca Azure AD URL'leri ve uygulama için gerekli sertifika sahiptir.

15. [Uygulamaya kullanıcı atama.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı'nı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleri ekleme

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

   1. tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

   2. Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verileri veya sertifika yükleme

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

Azure AD ayrıca meta verilerini almak için bir URL sağlar. Meta veri URL'si, uygulamaya özgü almak için bu deseni izler: `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`.

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

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

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatması mümkün.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulama için gönderilen SAML talepleri özelleştirme

Uygulamanıza gönderilen SAML özniteliği talep özelleştirme öğrenmek için bkz. [talep eşlemesi, Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
