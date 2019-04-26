---
title: Federasyon çoklu oturum açma galeri dışı bir uygulama yapılandırma sorunu | Microsoft Docs
description: Bazı Federasyon çoklu oturum açma Azure AD uygulama galerisinde bulunan listelenmeyen özel SAML uygulamanız için yapılandırma sırasında karşılaşabileceğiniz genel sorunları adresi
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
ms.openlocfilehash: 6ecbb097dd3cb3e3fdd6b365b059f7703668f07e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60291917"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon çoklu oturum açma galeri dışı bir uygulama yapılandırma sorunu

Bir uygulama yapılandırma sırasında bir sorunla karşılaşırsanız. Ardından, bu makaledeki tüm adımları doğrulayın [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırılıyor.](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery)

## <a name="cant-add-another-instance-of-the-application"></a>Uygulamanın başka bir örneği eklenemiyor

Uygulamanın ikinci bir örneğini eklemek için şunları yapması gerekir:

-   İkinci örneğini benzersiz bir tanımlayıcı yapılandırın. İlk örnek için kullanılan aynı tanımlayıcıya yapılandıramazsınız.

-   İlk örnek için kullanılan olandan farklı bir sertifika yapılandırın.

Uygulamanın önceki hiçbirini desteklemiyorsa, ikinci bir örneğini yapılandıramazsınız.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Entityıd (kullanıcı tanımlayıcısı) biçimi nereden ayarlayabilirim

Azure AD kullanıcı kimlik doğrulamasından sonra yanıtı uygulamaya gönderir (kullanıcı tanımlayıcısı) Entityıd biçimi seçemezsiniz.

Azure AD, seçili değerine göre biçimi Nameıd özniteliği (kullanıcı tanımlayıcısı) veya SAML AuthRequest uygulama tarafından istenen biçimi seçer. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy, bölümünün altında

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Uygulama meta verileri veya sertifika Azure AD'den nereden bulabilirim

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
