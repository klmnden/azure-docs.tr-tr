---
title: Azure AD'de Federasyon sertifikalarını yönetme | Microsoft Docs
description: Federasyon sertifikalarınızı sona erme tarihini özelleştirme ve yakında sona erecek sertifikaları yenile öğrenin.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/09/2017
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 7bae891bd16ecd3fbbad88022fbbffd32ff41eae
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577558"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Federasyon çoklu oturum açma için Azure Active Directory'de sertifikaları yönetme
Bu makalede, yaygın soruları ve Azure Active Directory (Azure AD), SaaS uygulamaları için Federasyon çoklu oturum açma (SSO) kurmaya oluşturur sertifikalarla ilgili bilgiler yer almaktadır. Uygulamaları Azure AD uygulama galerisinde veya galeri dışı uygulama şablonu kullanarak ekleyin. Federasyon SSO seçeneği kullanarak uygulamayı yapılandırın.

Bu makalede, aşağıdaki örnekte gösterildiği gibi SAML Federasyon aracılığıyla Azure AD SSO kullanılacak sonuna yapılandırılan uygulamalar için geçerlidir:

![Azure AD çoklu oturum açma](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Galeri ve galeri dışı uygulamalar için otomatik olarak oluşturulan sertifika
Galeriden yeni bir uygulama eklemek ve bir SAML tabanlı oturum açmayı yapılandırma, Azure AD'ye üç yıl boyunca geçerli uygulama için bir sertifika oluşturur. Bu sertifika indirebileceğiniz **SAML imzalama sertifikası** bölümü. Galeri uygulamalar için bu bölümde sertifika veya meta veri, uygulamanın gereksinim bağlı olarak indirmek için bir seçenek olarak gösterebilir.

![Azure AD çoklu oturum açma](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Federasyon sertifikanız için sona erme tarihi özelleştirebilir ve yeni bir sertifika atla
Varsayılan olarak, sertifikaları, üç yıl sonra süresi dolacak şekilde ayarlanır. Bir farklı sona erme tarihi sertifikanız için aşağıdaki adımları tamamlayarak seçebilirsiniz.
Ekran görüntüleri örnek için Salesforce kullanın, ancak tüm Federasyon SaaS uygulamaları için bu adımları uygulayabilirsiniz.

1. İçinde [Azure portalında](https://aad.portal.azure.com), tıklayın **Kurumsal uygulama** sol bölmesi ve ardından **yeni uygulama** üzerinde **genel bakış** sayfası:

   ![SSO Yapılandırma Sihirbazı'nı açın](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. Galeri uygulaması için arama yapın ve ardından eklemek istediğiniz uygulamayı seçin. Gerekli uygulama bulamazsanız uygulamasını kullanarak eklemek **galeri dışı uygulama** seçeneği. Bu özellik yalnızca Azure AD Premium (P1 ve P2) SKU'da kullanılabilir.

    ![Azure AD çoklu oturum açma](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Tıklayın **çoklu oturum açma** sol bölmede bağlantı ve değiştirme **çoklu oturum açma modunu** için **SAML tabanlı oturum açma**. Bu adım, uygulamanız için üç yıllık sertifika oluşturur.

4. Yeni bir sertifika oluşturmak için tıklayın **yeni sertifika oluştur** bağlantısını **SAML imzalama sertifikası** bölümü.

    ![Yeni Sertifika Oluştur](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. **Yeni bir sertifika oluşturmak** Takvim denetimi bağlantı açar. Herhangi bir tarihi ayarlama ve üç yıl geçerli tarihten itibaren en fazla saat. Seçilen tarih ve saat olan yeni bir sona erme tarihi ve yeni sertifikanızın süresi. **Kaydet**’e tıklayın.

    ![İndir sonra sertifikayı karşıya yüklemek](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Artık yeni sertifikayı yüklemek kullanılabilir. Tıklayın **sertifika** indirmek için bağlantı. Bu noktada, sertifikanızı etkin değil. Bu sertifika için Atla istediğinizde, seçin **yeni sertifikayı etkin hale getirin** onay kutusunu ve tıklatın **Kaydet**. Bu noktadan itibaren yeni sertifikayı yanıt imzalama için kullanan Azure AD başlatır.

7.  Sertifikayı karşıya yüklemek, belirli bir SaaS uygulaması için öğrenmek için tıklayın **görünümü uygulaması yapılandırma Öğreticisi** bağlantı.

## <a name="certificate-expiration-notification-email"></a>Sertifika süre sonu bildirimi e-posta

Azure AD, SAML sertifikanın süresi dolmadan önce bir e-posta bildirimi 60, 30 ve 7 gün gönderir. Bildirimin gönderileceği e-posta adresini belirtmek için:

- Azure Active Directory Uygulama tek oturum açma sayfasında, bildirim e-postası alanına gidin.
- Sertifika süre sonu bildirimi e-posta gönderileceği e-posta adresi girin. Varsayılan olarak, uygulama eklenen yönetici e-posta adresi bu alanı kullanır.

Bildirim e-postası alırsınız aadnotification@microsoft.com. İstenmeyen posta konumunuza giden e-posta önlemek için bu e-posta kişilerinize eklediğinizden emin olun. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Süresi yakında dolacak bir sertifikayı yenileme
Aşağıdaki yenileme adımları sonucunda, kullanıcılarınızın önemli kapalı kalma süresi. Bu bölümde özelliği Salesforce örneği, ancak bu adımları olarak ekran görüntüleri, tüm Federasyon SaaS uygulamaları için uygulayabilirsiniz.

1. Üzerinde **Azure Active Directory** uygulama **çoklu oturum açma** sayfasında, uygulamanız için yeni bir sertifika oluşturur. Tıklayarak bunu yapabilirsiniz **yeni sertifika oluştur** bağlantısını **SAML imzalama sertifikası** bölümü.

    ![Yeni Sertifika Oluştur](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. Yeni sertifikanız için saat ve istenen sona erme tarihi seçin ve tıklayın **Kaydet**.

3. Sertifikayı indirin **SAML imzalama sertifikası** seçeneği. SaaS uygulamanın çoklu oturum açma yapılandırması ekranına yeni sertifikayı yükleyin. Bu, belirli bir SaaS uygulamanız için yapma hakkında bilgi için tıklayın **görünümü uygulaması yapılandırma Öğreticisi** bağlantı.
   
4. Azure AD'de yeni sertifikayı etkinleştirmek için işaretleyin **yeni sertifikayı etkin hale getirin** onay kutusunu ve tıklatın **Kaydet** sayfanın üstünde düğme. Bu, Azure AD tarafında yeni sertifikayı yapar. Sertifika durumu değişir **yeni** için **etkin**. Bu noktadan itibaren yeni sertifikayı yanıt imzalama için kullanan Azure AD başlatır. 
   
    ![Yeni Sertifika Oluştur](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>İlgili makaleler
* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
* [Azure Active Directory'de uygulama yönetimi için makale dizini](../active-directory-apps-index.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](what-is-single-sign-on.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](../develop/howto-v1-debug-saml-sso-issues.md)
