---
title: Oturum açtıktan sonra bir uygulamanın sayfada hata | Microsoft Docs
description: Uygulama bir hata olduğunda yayar Azure AD oturum açma ile ilgili sorunları gidermek nasıl
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
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 234fc94a0a2ad3d3bee49853cf1dfc6a805a0166
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65825399"
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Oturum açtıktan sonra bir uygulamanın sayfada hata

Bu senaryoda, Azure AD oturum açmış kullanıcı, ancak uygulamanın oturum açma akışını başarıyla tamamlamak kullanıcı sağlanmıyor hata görüntülüyor. Bu senaryoda, uygulamanın yanıt sorunu Azure AD tarafından kabul etmiyor.

Uygulamayı Azure AD'ye yanıtından neden kabul etmedi bazı olası nedeni vardır. Chyba v aplikaci ardından yanıt olarak, eksik olana bilmek açık değilse:

-   Azure AD galeri uygulaması ise, bu makaledeki tüm adımları uyguladığınız doğrulayın [SAML tabanlı çoklu oturum açma, Azure Active Directory'de uygulamalar için hata ayıklama](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Gibi bir araç kullanmak [Fiddler](https://www.telerik.com/fiddler) SAML isteği, SAML yanıtını ve SAML belirteci yakalamak için.

-   SAML yanıtını neyin eksik olduğunu öğrenmek için uygulama satıcısına ile paylaşın.

## <a name="missing-attributes-in-the-saml-response"></a>SAML yanıtını eksik öznitelikler

Azure AD yanıtta gönderilecek Azure AD yapılandırmasında bir öznitelik eklemek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. tıklayın **görüntüleme ve düzenleme diğer tüm kullanıcı özniteliklerini altında** **kullanıcı öznitelikleri** bölümü kullanıcılar oturum açtığında uygulamaya SAML belirtecindeki gönderilmek üzere özniteliklerini düzenleyin.

   Bir öznitelik eklemek için:

   * tıklayın **eklemek agentconfigutil**. Girin **adı** ve select **değer** açılır listeden.

   * Tıklayın **kaydedin.** Yeni özniteliği tabloda görürsünüz.

9. Yapılandırmayı kaydedin.

Kullanıcının uygulamaya sonraki oturum açışında Azure AD, SAML yanıt olarak yeni bir öznitelik gönderin.

## <a name="the-application-expects-a-different-user-identifier-value-or-format"></a>Uygulama bir farklı kullanıcı tanımlayıcısı value veya format bekliyor

Oturum açma uygulamaya SAML yanıtını rolleri gibi öznitelikleri eksik olduğundan veya uygulama, Entityıd özniteliği için farklı bir biçim bekleniyordu, çünkü başarısız oluyor.

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a>Azure AD uygulama yapılandırmasında bir özniteliği ekleyin:

Kullanıcı tanımlayıcısı değeri değiştirmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Altında **kullanıcı öznitelikleri**, kullanıcılarınız için benzersiz tanımlayıcı seçin **kullanıcı tanımlayıcısı** açılır.

## <a name="change-entityid-user-identifier-format"></a>Entityıd (kullanıcı tanımlayıcısı) biçimini değiştirme

Uygulama Entityıd özniteliği için başka bir biçime bekliyorsa. Ardından, Azure AD kullanıcı kimlik doğrulamasından sonra yanıtı uygulamaya gönderir (kullanıcı tanımlayıcısı) Entityıd biçimini seçmek mümkün olmayacaktır.

Azure AD seçin (kullanıcı tanımlayıcısı) Nameıd öznitelik biçimi değerine göre seçilen veya biçimi SAML AuthRequest uygulama tarafından istenen. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy bölümünün altında.

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a>Uygulama SAML yanıtını için farklı imza yöntemi bekliyor

SAML belirtecinin hangi bölümlerinin Azure Active Directory tarafından dijital olarak imzalandığını değiştirmek için. Şu adımları uygulayın:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. tıklayın **gelişmiş sertifika imzalama ayarlarını göster** altında **SAML imzalama sertifikası** bölümü.

9. Uygun seçin **imzalama seçeneği** uygulama tarafından beklenen:

   * SAML yanıtını imzala

   * SAML yanıtını ve onayını imzala

   * SAML onayını imzala

Kullanıcının uygulamaya sonraki oturum açışında Azure AD oturum seçili SAML yanıtın bir parçası.

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a>İmzalama Algoritması SHA-1'uygulama bekliyor

Varsayılan olarak, Azure AD çoğu güvenlik algoritması kullanılarak SAML belirteci imzalar. Oturum algoritması SHA-1 olarak değiştirilmesi, uygulama tarafından istenmediği sürece önerilmez.

İmzalama Algoritması değiştirmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. tıklayın **gelişmiş sertifika imzalama ayarlarını göster** altında **SAML imzalama sertifikası** bölümü.

9. SHA-1, seçin **imza algoritması**.

Kullanıcının uygulamaya sonraki oturum açışında, Azure AD'saml belirteci SHA-1 algoritmasını kullanarak oturum açın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory'de uygulamalar için SAML tabanlı çoklu oturum açma hata ayıklama](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
