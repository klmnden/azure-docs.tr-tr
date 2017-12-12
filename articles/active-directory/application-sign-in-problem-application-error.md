---
title: "Oturum açtıktan sonra bir uygulamanın sayfada hata | Microsoft Docs"
description: "Uygulama bir hata olduğunda yayar Azure AD oturum ile ilgili sorunları gidermek nasıl"
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
ms.openlocfilehash: bd86d4b13c8f61f278589e5c1d705ad91b3e3d4c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Oturum açtıktan sonra bir uygulamanın sayfada hata

Bu senaryoda, Azure AD kullanıcı oturumu, ancak uygulama başarıyla oturum açma akışını son kullanıcıya izin hata görüntüleme. Bu senaryoda, uygulamanın yanıt sorunu Azure AD tarafından kabul etmiyor.

Uygulama Azure AD'den yanıt neden kabul etmediğiniz bazı olası nedenleri vardır. Uygulamada hata ne sonra yanıtta eksik olduğunu bilmeniz açık değilse:

-   Azure AD galeri uygulamasıysa, makaledeki tüm adımları doğrulayın [SAML tabanlı çoklu oturum açma Azure Active Directory'de uygulamalar için hata ayıklama](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler) SAML isteği SAML yanıtını ve SAML belirteci yakalamak için.

-   SAML yanıtını eksik biliyor için uygulama satıcısına ile paylaşır.

## <a name="missing-attributes-in-the-saml-response"></a>SAML yanıtını eksik öznitelikleri

Azure AD yanıtta gönderilecek Azure AD yapılandırmasında bir öznitelik eklemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  tıklatın **görüntüleme ve düzenleme diğer tüm kullanıcı özniteliklerini altında** **kullanıcı öznitelikleri** kullanıcı oturum açtığında SAML belirteci uygulamada gönderilmesini öznitelikleri düzenlemek için bölüm.

   Bir öznitelik eklemek için:

   * tıklatın **Ekle özniteliği**. Girin **adı** seçip **değeri** gelen açılır.

   * Tıklatın **kaydedin.** Yeni öznitelik tablosundaki bakın.

9.  Yapılandırmayı kaydedin.

Uygulama için kullanıcı oturum açtığında sonraki zaman Azure AD yeni öznitelik SAML yanıt olarak gönderin.

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a>Farklı kullanıcı tanımlayıcısı value veya format uygulama bekliyor

Oturum açma uygulamaya SAML yanıtını rolleri gibi öznitelikleri eksik veya uygulama Entityıd özniteliği için farklı bir biçim bekleniyordu nedeniyle başarısız oluyor.

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a>Azure AD uygulama yapılandırmasında bir özniteliği ekleyin:

Kullanıcı tanımlayıcısı değeri değiştirmek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Altında **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

## <a name="change-entityid-user-identifier-format"></a>Entityıd (kullanıcı tanımlayıcısı) biçimini değiştirme

Uygulama Entityıd özniteliği için başka bir biçime görüyorsa. Ardından, Azure AD kullanıcı kimlik doğrulamasının ardından yanıt uygulamaya gönderir Entityıd (kullanıcı tanımlayıcısı) biçimi seçin mümkün olmayacaktır.

Azure AD seçin (kullanıcı tanımlayıcısı) NameID özniteliğin biçimini değere göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünde.

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a>SAML yanıtını için farklı imza yöntemi uygulama bekliyor

SAML belirteci hangi kısımlarının Azure Active Directory tarafından dijital olarak imzalanmış değiştirmek için. Aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  tıklatın **Göster gelişmiş sertifika imzalama ayarları** altında **SAML imzalama sertifikası** bölümü.

9.  Uygun seçin **imzalama seçeneği** uygulama tarafından beklenen:

  * Oturum SAML yanıtını

  * Oturum açma SAML yanıtını ve onaylama

  * Oturum SAML onayı

Uygulama için kullanıcı oturum açtığında sonraki zaman Azure AD oturum seçili SAML yanıtını parçası.

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a>SHA-1 olması için imzalama algoritmasını uygulama bekliyor

Varsayılan olarak, Azure AD çoğu güvenlik algoritmasını kullanarak SAML belirteci imzalar. Oturum algoritması SHA-1'den değiştirilmesi uygulama tarafından gerekli kılınmadıkça hiçbir önerilmez.

İmza algoritmasını değiştirmek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  tıklatın **Göster gelişmiş sertifika imzalama ayarları** altında **SAML imzalama sertifikası** bölümü.

9.  SHA-1, seçin **imza algoritması**.

Sonraki sefer uygulama için kullanıcı oturum açtığında, Azure AD'sha-1 algoritmasını kullanarak SAML belirteci oturum açın.

## <a name="next-steps"></a>Sonraki adımlar
[SAML tabanlı çoklu oturum açma Azure Active Directory uygulamalarında hata ayıklama](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
