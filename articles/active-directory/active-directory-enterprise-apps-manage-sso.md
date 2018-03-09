---
title: "Çoklu oturum açma Yönetimi Azure Active Directory'de Kurumsal uygulamaları için | Microsoft Docs"
description: "Azure Active Directory Uygulama Galerisi'nden kuruluşunuzdaki kurumsal uygulamalar için çoklu oturum açma ayarları yönetme"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: markvi
ms.reviewer: asmalser
ms.openlocfilehash: bb9c2e1fdf392e234e6c72e0728ab04d2fd88a81
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Kurumsal uygulamaları için çoklu oturum açmayı yönetme

Bu makalede nasıl kullanılacağını açıklar [Azure portal](https://portal.azure.com) kurumsal uygulamalar için çoklu oturum açma ayarları yönetmek için. Kurumsal uygulamalar dağıtılan ve kuruluşunuzda kullanılan uygulamalardır. Bu makale özellikle alanından eklenen uygulamalar için geçerlidir [Azure Active Directory Uygulama galerisinde](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-the-portal"></a>Portalda uygulamalarınızı bulma
Çoklu oturum açma için ayarlanan tüm kurumsal uygulamaları görüntülenebilir ve Azure portalında yönetilebilir. Uygulamaları bulunabilir **tüm hizmetleri** &gt; **kurumsal uygulamalar** portalı bölümü. 

![Kurumsal uygulamalar dikey penceresi][1]

Seçin **tüm uygulamaları** yapılandırılmış tüm uygulamaların bir listesini görüntülemek için. Bir uygulamayı seçerek burada bu uygulama için raporlar görüntülenebilir ve çeşitli ayarları yönetilebilmesi için bu uygulama için kaynakları görüntüler.

Çoklu oturum açma ayarları yönetmek için seçin **çoklu oturum açma**.

![Uygulama kaynağı dikey][2]

## <a name="single-sign-on-modes"></a>Çoklu oturum açma modları
**Çoklu oturum açma** ile başlayan bir **modu** yapılandırılması çoklu oturum açma modu sağlar menüsü. Mevcut seçenekler şunlardır:

* **SAML tabanlı oturum açma** -bu seçenek Azure WS-Federation, SAML 2.0 protokolü kullanılarak Active Directory'ye tam Federasyon çoklu oturum açma uygulamasının desteklediği veya Openıd connect protokolleri kullanılabilir.
* **Parola tabanlı oturum açma** -bu seçenek Azure AD parola form bu uygulama için doldurma destekliyorsa kullanılabilir.
* **Bağlantılı oturum açma** -"Varolan çoklu oturum açma" eski adıyla, bu seçenek, kullanıcının Azure AD erişim paneli veya Office 365 uygulama Başlatıcı bu uygulama ile bir bağlantı yerleştirmek Yöneticiler sağlar.

Bu modları hakkında daha fazla bilgi için bkz: [nasıl çoklu oturum açmayı Azure Active Directory iş](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML tabanlı oturum açma
**SAML tabanlı oturum açma** seçeneği, dört bölümlerde ayrılmıştır:

### <a name="domains-and-urls"></a>Etki alanları ve URL'ler
Uygulama etki alanı ve URL'ler ile ilgili tüm ayrıntıları Azure AD dizininiz için burada eklenen budur. Tüm isteğe bağlı girdilerin işaretleyerek görüntülenebilir ancak tek oturum açma iş uygulama yapmak için gerekli tüm girişleri doğrudan ekranda görüntülenen **Göster Gelişmiş URL ayarları** onay kutusu. Desteklenen girişleri tam listesini içerir:

* **URL oturum açma** – uygulamaya oturum açmak için kullanıcının gider burada. Uygulama hizmet sağlayıcı başlatılan çoklu oturum açma, bu URL'yi bir kullanıcı oturum açtığında gerçekleştirmek için yapılandırılmışsa, kimlik doğrulaması ve kullanıcı oturum açmak için Azure ad hizmet sağlayıcısı yönlendirir. 
  * Bu alan doldurulursa, Azure AD Office 365 ve Azure AD erişim paneli uygulamayı başlatmak için URL'yi kullanır.
  * Bu alan atlanırsa, ardından Azure AD yerine Azure AD erişim paneli Office 365'ten uygulama başlatıldığında kimlik sağlayıcısı başlatılan oturum açma veya gerçekleştirir tek Azure AD'den URL oturum.
* **Tanımlayıcı** -bu URI için çoklu oturum açma yapılandırılmış uygulama benzersiz şekilde tanımlamak. Bu geri uygulamaya SAML belirteci İzleyici parametresi olarak Azure AD gönderdiği değerdir ve uygulama doğrulamak beklenir. Bu değer, uygulama tarafından sağlanan herhangi bir SAML meta veri varlık kimliği olarak görünür.
* **Yanıt URL'si** -burada SAML belirteci almak uygulama bekler yanıt URL'dir. Bu aynı zamanda onaylama işlemi tüketici Hizmeti'ni (ACS) URL'si olarak adlandırılır. Bunlar girdikten sonra sonraki ekrana devam etmek için İleri'yi tıklatın. Bu ekran Azure AD'den SAML belirteci kabul etmek üzere etkinleştirmek için uygulama taraftaki yapılandırılması gerekenler hakkında bilgi sağlar.
* **Geçiş durumunu** -geçiş durumunu uygulamaya kimlik doğrulaması tamamlandıktan sonra kullanıcı yeniden yönlendirileceği bildirmek yardımcı olabilecek bir isteğe bağlı bir parametredir. Bazı uygulamalar bu alan farklı kullanır ancak genellikle uygulama geçerli bir URL değerdir (Ayrıntılar için uygulamanın tek oturum açma belgelere bakın). Geçiş durumunu ayarlama özelliği, yeni Azure portalına benzersiz olan yeni bir özelliktir.

### <a name="user-attributes"></a>Kullanıcı Öznitelikleri
Burada admins görüntüleyebilir ve Azure AD uygulama için her verir SAML belirteci zaman kullanıcılar oturum gönderilir öznitelikleri düzenlemenize budur.

Desteklenen yalnızca düzenlenebilir özniteliği **kullanıcı tanımlayıcısı** özniteliği. Bu özniteliğin değeri uygulamadaki her bir kullanıcı olarak tanıtan Azure ad alanıdır. Uygulama "e-posta adresi" kullanıcı adı ve benzersiz tanımlayıcı kullanarak dağıtılmışsa, örneğin, ardından değeri "user.mail" alanına Azure AD'de ayarlanır.

### <a name="saml-signing-certificate"></a>SAML İmzalama Sertifikası
Bu bölümde Azure AD her zaman kullanıcının kimliğini doğrular uygulamaya verilen SAML belirteçleri imzalamak için kullanılan sertifika ayrıntılarını gösterir. Denetlenecek, geçerli sertifikanın özelliklerini sona erme tarihi dahil olmak üzere sağlar.

### <a name="application-configuration"></a>Uygulama yapılandırması
Son bölümü, belgelerine ve/veya Azure Active Directory kimlik sağlayıcısı olarak kullanmak için uygulamanın kendisinin yapılandırmak için gerekli denetimleri sağlar.

**Uygulama Yapılandır** uçarak çıkış menü uygulamayı yapılandırmak için yeni kısa, katıştırılmış yönergeler sağlar. Yeni Azure portalına benzersiz bir başka yeni özellik budur.

> [!NOTE]
> Salesforce.com uygulama embedded belgeler tam bir örnek için bkz. Ek uygulamalar için belge sürekli olarak ekleniyor.
> 
> 

![Katıştırılmış belgeleri][3]

## <a name="password-based-sign-on"></a>Parola tabanlı oturum açma
Uygulama için destekleniyorsa, parola tabanlı SSO modu ve seçerek **kaydetmek** anında parola tabanlı SSO yapmak için yapılandırır. Parola tabanlı SSO dağıtma hakkında daha fazla bilgi için bkz: [nasıl çoklu oturum açmayı Azure Active Directory iş](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Parola tabanlı oturum açma][4]

## <a name="linked-sign-on"></a>Bağlantılı oturum açma
Uygulama için destekleniyorsa, bağlantılı SSO modu seçme Azure AD erişim paneli veya Office 365 kullanıcılar bu uygulamayı üzerinde tıklattığında yönlendirileceği istediğiniz URL'yi girmenizi sağlar. Bağlantılı SSO (eski adıyla "mevcut SSO") hakkında daha fazla bilgi için bkz: [nasıl çoklu oturum açmayı Azure Active Directory iş](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Bağlantılı oturum açma][5]

##<a name="feedback"></a>Geri Bildirim

Geliştirilmiş kullanarak gibi umuyoruz Azure AD deneyimi. Gelen geri bildirim unutmayın! Geri bildirim ve fikir geliştirme için post **Yönetici portalı** bölümünü bizim [geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Biz, her gün harika yeni hizmetler oluşturma hakkında heyecan ve şekil, kılavuzlar kullanabilir ve sonraki geliştirmemiz ne tanımlayın.

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
