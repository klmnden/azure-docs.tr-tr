---
title: "Azure Active Directory'de SaaS uygulamaları B2B işbirliği için yapılandırma | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği için PowerShell ve kod örnekleri"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 149a493f7b369415f0a2726dd6a576f0195c13d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>SaaS uygulamaları B2B işbirliği için yapılandırma

Azure AD ile tümleştirmek çoğu uygulamaları ile Azure Active Directory (Azure AD) B2B işbirliği çalışır. Bu bölümde, Azure AD B2B ile kullanmak için bazı yaygın SaaS uygulamaları yapılandırmaya yönelik yönergeler size rehberlik eder.

Uygulamaya özgü yönergeleri göz önünde bulundurmanız önce bazı kurallar altın şunlardır:

* Uygulamaların çoğu için Kullanıcı Kurulumu el ile gerçekleştirilmesi gerekir. Diğer bir deyişle, kullanıcı uygulamada el ile oluşturulması gerekir.

* Dropbox gibi otomatik kurulum destekleyen uygulamalar için ayrı davetleri uygulamalardan oluşturulur. Kullanıcıların her daveti kabul etmek emin olması gerekir.

* Her zaman kullanıcı öznitelikleri karıştırılmış kullanıcı profili diski (UPD) Konuk kullanıcılar ile ilgili tüm sorunları hafifletmek için ayarlanmış **kullanıcı tanımlayıcısı** için **user.mail**.


## <a name="dropbox-business"></a>Dropbox iş

Kullanıcıların kendi kuruluş hesabını kullanarak oturum sağlamak için Dropbox Azure AD güvenlik onaylama işlemi biçimlendirme dili (SAML) kimlik sağlayıcısı kullanmak için iş el ile yapılandırmanız gerekir. Bunu yapmak için Dropbox iş yapılandırılmamış, bu isteyebilir veya aksi takdirde Azure AD kullanarak kullanıcıların oturum açmasını sağlar.

1. Azure AD ile Dropbox iş uygulama eklemek için seçin **kurumsal uygulamalar** sol bölmesinde ve ardından **Ekle**.

  ![Kuruluş uygulamaları sayfasında "Ekle" düğmesi](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. İçinde **bir uygulama eklemek** penceresinde girin **dropbox** arama kutusuna ve ardından **iş için Dropbox** sonuçlar listesinde.

  ![Uygulama Sayfası Ekle "dropbox" arayın](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. Üzerinde **çoklu oturum açma** sayfasında, **çoklu oturum açma** sol bölmede ve ardından girin **user.mail** içinde **kullanıcı tanımlayıcısı** kutusu. (Bunu UPN varsayılan olarak ayarlanır.)

  ![Uygulama için çoklu oturum açmayı yapılandırma](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. Dropbox yapılandırması için kullanılacak sertifikayı yüklemek üzere seçin **yapılandırma DropBox**ve ardından **SAML çoklu oturum üzerinde hizmet URL'si** listesinde.

  ![Dropbox yapılandırması için sertifika indirme](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. Oturum açtığınızda Dropbox oturum açma URL'si ile **çoklu oturum açma** sayfası.

  ![Dropbox oturum açma sayfası](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. Menüsünde seçin **Yönetici Konsolu**.

  ![Dropbox menüsünde "Yönetici Konsolu" bağlantısı](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. İçinde **kimlik doğrulaması** iletişim kutusunda **daha fazla**, sertifikayı karşıya yüklemek ve ardından **URL'de oturum** kutusuna, SAML çoklu oturum açma URL'si girin.

  ![Daraltılmış kimlik doğrulama iletişim kutusunda "Daha fazla" bağlantı](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  !["Oturum URL'de" genişletilmiş kimlik doğrulama iletişim kutusunda](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. Otomatik kullanıcı kurulum Azure portalında yapılandırmak için seçin **sağlama** sol bölmesinde seçin **otomatik** içinde **sağlama modu** kutusuna ve ardından **Authorize**.

  ![Azure portalında otomatik kullanıcı sağlamayı yapılandırma](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

Konuk veya üye kullanıcı Dropbox uygulamada ayarlanan sonra Dropbox'tan ayrı bir davet alırsınız. Dropbox çoklu oturum açma kullanmak için davetlilerin daveti bir bağlantıya tıklayarak kabul etmelisiniz.

## <a name="box"></a>Box
SAML protokolünü temel Federasyon kullanarak Azure AD hesabıyla kutusunu konuk kullanıcıların kimliklerini doğrulamak kullanıcıların sağlayabilirsiniz. Bu yordamda, meta veriler için Box.com yükleyin.

1. Box uygulamasına Kurumsal uygulamalardan ekleyin.

2. Çoklu oturum açma şu sırayla yapılandırın:

  ![Kutusunu çoklu oturum açmayı yapılandırın](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 a. İçinde **URL üzerinde oturum** kutusunda, Azure portalında kutusunu, oturum açma URL'si uygun şekilde ayarlandığından emin olun. Bu Box.com kiracınızın URL'dir. Adlandırma kuralına uymalı *https://.box.com*.  
 **Tanımlayıcısı** bu uygulama için geçerli değildir, ancak bu zorunlu bir alan görünmeye devam eder.

 b. İçinde **kullanıcı tanımlayıcısı** kutusuna **user.mail** (için SSO Konuk hesapları için).

 c. Altında **SAML imzalama sertifikası**, tıklatın **yeni sertifika oluştur**.

 d. Azure AD kimlik sağlayıcısı olarak kullanmak için Box.com Kiracı yapılandırmaya başlamak için meta veri dosyası karşıdan yükle ve yerel diskinize kaydedin.

 e. İleri kutusuna meta veri dosyası, çoklu oturum açma sizin için yapılandırır takım destekler.

3. Sol bölmede, Azure AD otomatik kullanıcı kurulumu seçin **sağlama**ve ardından **Authorize**.

  ![Azure AD kutusuna bağlanmak için yetki](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Dropbox davetlilerin gibi kutusunu davetlilerin Box uygulamasına kendi davetini almak gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği aşağıdaki makalelere bakın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [B2B işbirliği davetleri temsilci seçme](active-directory-b2b-delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](active-directory-b2b-dynamic-groups.md)
* [B2B işbirliği kodu ve PowerShell örnekleri](active-directory-b2b-code-samples.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
* [Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
* [B2B işbirliği geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
