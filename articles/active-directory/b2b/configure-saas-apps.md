---
title: SaaS uygulamalarını B2B işbirliği - Azure Active Directory için yapılandırma | Microsoft Docs
description: Azure Active Directory B2B işbirliği için kod ve PowerShell örnekleri
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35dad420aa004e27ec974c494dc66e9b8e13c733
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65811937"
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>SaaS uygulamalarını B2B işbirliği için yapılandırma

Azure Active Directory (Azure AD) B2B işbirliği, Azure AD ile tümleştirme, çoğu uygulamalarla çalışır. Bu bölümde, bazı popüler SaaS uygulamalarını kullanmak için Azure AD B2B ile yapılandırmaya yönelik yönergeler size rehberlik eder.

Uygulamaya özgü yönergeleri göz atmadan önce bazı kurallar karşısında şunlardır:

* Uygulamaların çoğu için Kullanıcı Kurulumu el ile gerçekleştirilmesi gerekir. Diğer bir deyişle, kullanıcılar uygulamada el ile oluşturulması gerekir.

* Dropbox gibi otomatik kurulum destekleyen uygulamalar için ayrı davetleri uygulamalardan oluşturulur. Kullanıcıların her davetiyeyi kabul etmek emin olması gerekir.

* Kullanıcı öznitelikleri, karıştırılmış kullanıcı profili diski (UPD), Konuk kullanıcılar ile ilgili tüm sorunları gidermek için her zaman ayarlamak **kullanıcı tanımlayıcısı** için **user.mail**.


## <a name="dropbox-business"></a>Dropbox Business

Kendi kuruluş hesabını kullanarak oturum açmasına etkinleştirmek için bir güvenlik onaylama işlemi biçimlendirme dili (SAML) kimlik sağlayıcısı olarak Azure AD'yi kullanacak şekilde Dropbox iş el ile yapılandırmanız gerekir. Bunu yapmak için Dropbox iş yapılandırılmadı, bu isteyebilir veya aksi halde Azure AD kullanarak oturum açmasına imkan tanıyın.

1. Azure AD ile Dropbox iş uygulaması eklemek için seçin **kurumsal uygulamalar** sol bölmesi ve ardından **Ekle**.

   ![Kurumsal uygulamalar sayfasında "Ekle" düğmesi](media/configure-saas-apps/add-dropbox.png)

2. İçinde **uygulama ekleme** penceresinde girin **dropbox** arama kutusuna ve ardından **iş için Dropbox** sonuç listesinde.

   ![Uygulama Sayfası Ekle "dropbox" arayın](media/configure-saas-apps/add-app-dialog.png)

3. Üzerinde **çoklu oturum açma** sayfasında **çoklu oturum açma** sol bölmedeki ve enter **user.mail** içinde **kullanıcı tanımlayıcısı** kutusu. (Varsayılan olarak UPN ayarlanır.)

   ![Uygulama için çoklu oturum açmayı yapılandırma](media/configure-saas-apps/configure-app-sso.png)

4. Dropbox yapılandırmasında kullanılacak bir sertifika yüklemek için seçin **yapılandırma DropBox**ve ardından **SAML çoklu oturum üzerinde hizmet URL'si** listesinde.

   ![Dropbox yapılandırması için sertifika indirme](media/configure-saas-apps/download-certificate.png)

5. Oturum açmak için Dropbox oturum açma URL'si ile **çoklu oturum açma** sayfası.

   ![Dropbox oturum açma sayfasını gösteren ekran görüntüsü](media/configure-saas-apps/sign-in-to-dropbox.png)

6. Menüsünde **Yönetici Konsolu**.

   ![Dropbox menüsündeki "Yönetici konsolunda" bağlantısı](media/configure-saas-apps/dropbox-menu.png)

7. İçinde **kimlik doğrulaması** iletişim kutusunda **daha fazla**, sertifikayı karşıya yüklemek ve ardından **URL'de oturum** kutusuna, SAML çoklu oturum açma URL'si girin.

   ![Daraltılmış kimlik doğrulaması iletişim kutusunda "Daha fazla" bağlantısı](media/configure-saas-apps/dropbox-auth-01.png)

   !["Oturum URL'de" genişletilmiş kimlik doğrulama iletişim kutusunda](media/configure-saas-apps/paste-single-sign-on-URL.png)

8. Azure portalında otomatik Kullanıcı Kurulumu yapılandırmak için seçin **sağlama** seçin sol bölmede **otomatik** içinde **sağlama modu** kutusuna ve ardından seçin **Yetkilendirme**.

   ![Azure portalında otomatik kullanıcı sağlamayı yapılandırma](media/configure-saas-apps/set-up-automatic-provisioning.png)

Dropbox uygulamada konuk veya üye kullanıcı ayarlanan sonra Dropbox'tan ayrı bir davet alırsınız. Dropbox çoklu oturum açmayı kullanmak için davetlilerin daveti bir bağlantıya tıklayarak kabul etmeniz gerekir.

## <a name="box"></a>Box
SAML Protokolü temelinde Federasyon kutusuna Konuk kullanıcıları Azure AD hesabıyla kimliğini açabileceğinizi bilirsiniz. Bu yordamda, meta verileri için Box.com yükleyin.

1. Box uygulamasına Kurumsal uygulamaları ekleyin.

2. Çoklu oturum açma şu sırayla yapılandırın:

   ![Çoklu oturum açma yapılandırması ayarları gösteren ekran görüntüsü](media/configure-saas-apps/configure-box-sso.png)

   a. İçinde **oturum açma URL'si** kutusuna, Azure portalında kutusunu, oturum açma URL'si uygun şekilde ayarlandığından emin olun. Bu URL'yi Box.com kiracınızın URL'dir. Bu adlandırma kuralını uygulamalıdır *https://.box.com* .  
   **Tanımlayıcı** bu uygulama için geçerli değildir ancak yine de zorunlu bir alan olarak görünür.

   b. İçinde **kullanıcı tanımlayıcısı** kutusuna **user.mail** (için SSO Konuk hesapları için).

   c. Altında **SAML imzalama sertifikası**, tıklayın **yeni sertifika oluştur**.

   d. Kimlik sağlayıcısı olarak Azure AD'yi kullanacak şekilde Box.com kiracınızı yapılandırmaya başlamak için meta veri dosyası indirin ve yerel sürücünüze kaydedin.

   e. İleri kutusuna meta veri dosyası, sizin için çoklu oturum açmayı yapılandıran takım destekler.

3. Sol bölmede, Azure AD'ye otomatik kullanıcı kurulum seçin **sağlama**ve ardından **Authorize**.

   ![Azure AD kutusuna bağlanmak için yetki](media/configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Dropbox davetlilerin gibi kutusu katılımcıların kendi kutusu uygulamasından davetini gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Dinamik gruplar ve B2B işbirliği](use-dynamic-groups.md)
- [B2B işbirliği kullanıcı taleplerini eşleme](claims-mapping.md)
- [Office 365 dış paylaşım](o365-external-user.md)

