---
title: Federasyon tek oturum açma için Galeri olmayan uygulama yapılandırma sorunu | Microsoft Docs
description: Bazı Federasyon çoklu oturum açma Azure AD uygulama galerisinde listelenmeyen özel SAML uygulamanız için yapılandırma sırasında karşılaşabileceğiniz yaygın sorunları ele
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 36262320a5a8457b22cbe9fe9d902fda26b6609c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331113"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Federasyon tek oturum açma için Galeri olmayan uygulama yapılandırma sorunu

Bir uygulama yapılandırırken bir sorunla karşılaşırsanız. İzlediğiniz makaledeki tüm adımları doğrulayın [çoklu oturum açma Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için yapılandırma.](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery)

## <a name="cant-add-another-instance-of-the-application"></a>Uygulama başka bir örneği eklenemiyor

Uygulamanın ikinci bir örneğini eklemek için şunları yapması gerekir:

-   İkinci örneği için benzersiz bir tanımlayıcı yapılandırın. İlk örnek için kullanılan aynı tanımlayıcısı yapılandıramazsınız.

-   İlk örnek için kullanılan olandan farklı bir sertifika yapılandırın.

Uygulamanın önceki hiçbirini desteklemiyorsa, ikinci bir örneğini yapılandıramazsınız.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Entityıd (kullanıcı tanımlayıcısı) biçimi belirlendiği

Azure AD kullanıcı kimlik doğrulamasının ardından yanıt uygulamaya gönderir Entityıd (kullanıcı tanımlayıcısı) biçimi seçemezsiniz.

Azure AD, seçili değere göre biçim NameID özniteliği (kullanıcı tanımlayıcısı) veya SAML AuthRequest uygulama tarafından istenen biçim seçer. Daha fazla bilgi için makaleyi ziyaret [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy, bölümünde

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Uygulama meta verileri veya sertifika Azure AD'den nereden bulabilirim

Uygulama meta verileri veya sertifika Azure AD'den indirmek için şu adımları izleyin:

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

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>Uygulamaya gönderilen SAML talep özelleştirmek nasıl bilinmiyor

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmek öğrenmek için bkz: [talep eşleme Azure Active Directory'de](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
