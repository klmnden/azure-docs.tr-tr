---
title: "Federasyon tek oturum açma için Azure AD galeri uygulamanın yapılandırma sorunu | Microsoft Docs"
description: "Bazı yapılandırma tek federe olduğunda karşılaşabilir yaygın sorunları ele Azure AD uygulama galerisinde listelenen uygulamalar için SAML kullanarak oturum"
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
ms.openlocfilehash: 290ca66048281de5e031b0404919bed84ab19ffa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon tek oturum açma için Azure AD galeri uygulamanın yapılandırma sorunu

Bir uygulama yapılandırırken bir sorunla karşılaşırsanız. Uygulama için öğreticideki tüm adımları izlediğinizden emin olun. Uygulamanın yapılandırma dosyasındaki satır içi belgeleri uygulama yapılandırma konusunda sahip. Ayrıca, erişim [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) ayrıntılı adım adım yönergeler için.

## <a name="cant-add-another-instance-of-the-application"></a>Uygulama başka bir örneği eklenemiyor

Uygulamanın ikinci bir örneğini eklemek için şunları yapması gerekir:

-   İkinci örneği için benzersiz bir tanımlayıcı yapılandırın. İlk örnek için kullanılan aynı tanımlayıcısı yapılandırmanız mümkün olmayacaktır.

-   İlk örnek için kullanılan olandan farklı bir sertifika yapılandırın.

Yukarıdakilerin tümü uygulamayı desteklemiyorsa. Ardından, ikinci bir örneğini yapılandırmanız mümkün olmayacaktır.

## <a name="cant-add-the-identifier-or-the-reply-url"></a>Tanımlayıcı veya yanıt URL'si eklenemiyor

Tanımlayıcı veya yanıt URL'si yapılandırmadı değilseniz, uygulama için önceden yapılandırılmış desenleri tanımlayıcısı ve yanıt URL'si değerleri eşleşen onaylayın.

Uygulama için önceden yapılandırılmış desenleri öğrenmek için:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici** 7. adıma gidin. Üzerinde Azure AD uygulama yapılandırma dikey penceresinde zaten varsa.

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9.  Git **tanımlayıcısı** veya **yanıt URL'si** metin kutusu altında **etki alanı ve URL'ler bölümü.**

10. Uygulama için desteklenen desenleri bilmeniz üç yolu vardır:

   * Metin kutusuna bir yer tutucu olarak desteklenen desenler bkz *örnek:* <https://contoso.com>.

   * Desen desteklenmiyorsa, metin kutusuna bir değer girmenizi çalıştığınızda, kırmızı bir ünlem işareti görürsünüz. Farenizi kırmızı bir ünlem işareti getirirseniz, desteklenen desenleri bakın.

   * Uygulama için öğreticide desteklenen desenleri hakkında bilgi alabilirsiniz. Altında **yapılandırma Azure AD çoklu oturum açma** bölümü. Altındaki değerleri yapılandırılmış adıma Git **etki alanı ve URL'leri** bölümü.

Değerler üzerinde Azure AD önceden yapılandırılmış desenlerle eşleşmiyorsa. Şunları yapabilirsiniz:

-   Üzerinde Azure AD önceden yapılandırılmış bir desenle eşleşen değerleri almak için uygulama satıcısına ile çalışma

-   Veya, Azure AD ekibi sizinle < aadapprequest@microsoft.com > veya uygulama için desteklenen desenleri güncelleştirmesi istemek için öğreticide bir yorum yazın

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Entityıd (kullanıcı tanımlayıcısı) biçimi belirlendiği

Azure AD kullanıcı kimlik doğrulamasının ardından yanıt uygulamaya gönderir Entityıd (kullanıcı tanımlayıcısı) biçimi seçin mümkün olmayacaktır.

Azure AD seçin (kullanıcı tanımlayıcısı) NameID özniteliğin biçimini değere göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy, bölümünde

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a>Uygulama ile yapılandırmayı tamamlamak için Azure AD meta verileri bulamıyor

Uygulama meta verileri veya sertifika Azure AD'den karşıdan yüklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırdığınız uygulaması'nı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Git **SAML imzalama sertifikası** bölümünde ve ardından **karşıdan** sütun değeri. Çoklu oturum açma yapılandırma hangi uygulama gerektiriyor bağlı olarak, meta veri XML yükleme seçeneği ya da sertifika bakın.

Azure AD meta verilerini almak için bir URL sağlamaz. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Uygulamaya gönderilen SAML talep özelleştirmek nasıl bilinmiyor

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmek öğrenmek için bkz: [talep eşleme Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulamaları yönetme](active-directory-enable-sso-scenario.md)
