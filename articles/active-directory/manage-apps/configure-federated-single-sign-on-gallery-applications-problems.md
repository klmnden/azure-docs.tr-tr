---
title: Federasyon çoklu oturum açma Azure AD galeri uygulaması için yapılandırma sorunu | Microsoft Docs
description: Bazı yaygın sorunları karşılaşabileceğiniz tek Federasyon yapılandırma, adresi Azure AD uygulama galerisinde listelenmiş olan uygulamalar için SAML kullanarak oturum açmayı
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
ms.openlocfilehash: 31e9746c0739a2ddd6267428f428e977151077b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60292122"
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federasyon çoklu oturum açma Azure AD galeri uygulaması için yapılandırma sorunu

Bir uygulama yapılandırma sırasında bir sorunla karşılaşırsanız. Uygulama için öğreticideki tüm adımları uyguladıysanız doğrulayın. Uygulama yapılandırması, satır içi belgeler uygulama yapılandırma konusunda sahip. Ayrıca, erişebileceğiniz [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) yönelik ayrıntılı adım adım yönergeler.

## <a name="cant-add-another-instance-of-the-application"></a>Uygulamanın başka bir örneği eklenemiyor

Uygulamanın ikinci bir örneğini eklemek için şunları yapması gerekir:

-   İkinci örneğini benzersiz bir tanımlayıcı yapılandırın. İlk örnek için kullanılan aynı tanımlayıcıyı yapılandırmak mümkün olmayacaktır.

-   İlk örnek için kullanılan olandan farklı bir sertifika yapılandırın.

Yukarıdakilerden herhangi biri uygulama desteklemiyorsa. Ardından, ikinci bir örneğini yapılandırmak mümkün olmayacaktır.

## <a name="cant-add-the-identifier-or-the-reply-url"></a>Tanımlayıcı veya yanıt URL'si eklenemiyor

Tanımlayıcı veya yanıt URL'si yapılandırabilirsiniz değilseniz, eşleşen uygulama için önceden yapılandırılmış desenleri tanımlayıcısı ve yanıt URL'si değerleri doğrulayın.

Uygulama için önceden yapılandırılmış desenleri öğrenmek için:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici** 7. adıma gidin. Azure AD uygulama yapılandırması dikey penceresinde zaten varsa.

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Seçin **SAML tabanlı oturum açma** gelen **modu** açılır.

9. Git **tanımlayıcı** veya **yanıt URL'si** metin kutusu altında **etki alanı ve URL'ler bölümü.**

10. Desteklenen desenler uygulama için bilmeniz gereken üç yolu vardır:

    * Metin kutusuna bir yer tutucu olarak desteklenen desenler bkz *örnek:* <https://contoso.com>.

    * Desen desteklenmiyorsa, metin kutusuna bir değer girin çalıştığınızda, kırmızı bir ünlem işareti görürsünüz. Kırmızı ünlem işareti üzerinde fareyi üzerine gelirseniz, desteklenen desenler bakın.

    * Uygulama için bir öğreticide, desteklenen desenler hakkında bilgi alabilirsiniz. Altında **yapılandırma Azure AD çoklu oturum açma** bölümü. Değerlerin altında yapılandırılmış adıma Git **etki alanı ve URL'ler** bölümü.

Azure AD üzerinde önceden yapılandırılmış desenlerle değerler eşleşmiyorsa. Şunları yapabilirsiniz:

-   Azure AD üzerinde önceden yapılandırılmış bir desenle eşleşen değerleri almak için uygulama satıcısına ile çalışma

-   Veya Azure AD ekibi sizinle <aadapprequest@microsoft.com> veya uygulama için desteklenen desenlerinin güncelleştirme isteği için öğreticinin bir yorum yazın

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Entityıd (kullanıcı tanımlayıcısı) biçimi nereden ayarlayabilirim

Azure AD kullanıcı kimlik doğrulamasından sonra yanıtı uygulamaya gönderir (kullanıcı tanımlayıcısı) Entityıd biçimini seçin mümkün olmayacaktır.

Azure AD seçin (kullanıcı tanımlayıcısı) Nameıd öznitelik biçimi değerine göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy, bölümünün altında

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a>Uygulama yapılandırmasını tamamlamak için Azure AD meta verileri bulamıyor

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

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Bir uygulama için gönderilen SAML talep özelleştirme nasıl yapılandıracağımı bilmiyorum

Uygulamanıza gönderilen SAML özniteliği talep özelleştirme öğrenmek için bkz. [talep eşlemesi, Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
