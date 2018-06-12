---
title: Azure AD içinde Federasyon sertifikalarını yönetmek | Microsoft Docs
description: Federasyon sertifikalarınızı sona erme tarihini özelleştirmeyi ve süresi yakında dolacak sertifikaları yenilemek nasıl öğrenin.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: c4d812db6371a4cd1fcc701f7eae2c913c29fb6b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303902"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Federasyon tek oturum açma için Azure Active Directory'de sertifikaları yönetme
Bu makale ortak sorular ve Federasyon çoklu oturum açma (SSO) SaaS uygulamalarınıza oluşturmak için Azure Active Directory (Azure AD) oluşturur sertifikalar ilgili bilgiler yer almaktadır. Uygulamaları Azure AD uygulama galerisinde veya bir galeri olmayan uygulama şablonu kullanarak ekleyin. Uygulama, Federasyon SSO seçeneğini kullanarak yapılandırın.

Bu makalede, Azure AD SSO SAML Federasyon aracılığıyla kullanmak için aşağıdaki örnekte gösterildiği gibi yapılandırılan uygulamalar için geçerlidir:

![Azure AD çoklu oturum açma](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Galeri ve galeri olmayan uygulamalar için otomatik olarak oluşturulan sertifika
Galeriden yeni bir uygulama eklemek ve bir SAML tabanlı oturum açma yapılandırdığınızda Azure AD üç yıl geçerli olan uygulama için bir sertifika oluşturur. Bu sertifikayı indirebilirsiniz **SAML imzalama sertifikası** bölümü. Galeri uygulamalar için bu bölümde sertifika veya meta veri, uygulama gereksinim bağlı olarak indirmek için bir seçenek gösterebilir.

![Azure AD çoklu oturum açma](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Federasyon sertifikanızı sona erme tarihini özelleştirebilir ve yeni bir sertifika geçir
Varsayılan olarak, sertifikaları üç yıl sonra süresi dolacak şekilde ayarlanır. Aşağıdaki adımları izleyerek sertifikanızı farklı sona erme tarihini seçebilirsiniz.
Ekran görüntüleri Salesforce amacıyla örnek kullanın, ancak tüm Federasyon SaaS uygulamaları için bu adımları uygulayabilirsiniz.

1. İçinde [Azure portal](https://aad.portal.azure.com), tıklatın **Kurumsal uygulama** sol bölmesinde ve ardından **yeni uygulama** üzerinde **genel bakış** sayfa:

   ![SSO Yapılandırma Sihirbazı'nı açın](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. İçin Galeri uygulama arayın ve ardından eklemek istediğiniz uygulamayı seçin. Gerekli uygulama bulamazsanız, uygulamasını kullanarak eklemek **olmayan galeri uygulama** seçeneği. Bu özellik yalnızca Azure AD Premium (P1 ve P2) SKU'da kullanılabilir.

    ![Azure AD çoklu oturum açma](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Tıklatın **çoklu oturum açma** değiştirmek ve bağlama sol bölmede **tek oturum açma modu** için **SAML tabanlı oturum açma**. Bu adımı uygulamanız için üç yıllık sertifika oluşturur.

4. Yeni bir sertifika oluşturmak için tıklatın **yeni sertifika oluştur** bağlamak **SAML imzalama sertifikası** bölümü.

    ![Yeni bir sertifika oluşturma](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. **Yeni bir sertifika oluşturmak** bağlantı Takvim denetimi açar. Herhangi bir tarihi ayarlamak ve geçerli tarihten en fazla üç yıl saat. Seçilen tarih ve saat olan yeni sona erme tarihi ve yeni sertifikanızın süresi. **Kaydet**’e tıklayın.

    ![İndirme sonra sertifikasını karşıya yükle](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Şimdi yeni sertifikayı yüklemek kullanılabilir. Tıklatın **sertifika** indirmeleri için bağlantı. Bu noktada, sertifikanızı etkin değil. Bu sertifika için değil, UTC'ye istediğinizde seçin **yeni sertifika etkin hale getirin** onay kutusunu ve tıklatın **kaydetmek**. Bu noktadan itibaren Azure AD yanıt imzalama için yeni sertifikayı kullanarak başlatır.

7.  Sertifika belirli SaaS uygulamanızı karşıya yükleme konusunda bilgi almak için Yardım düğmesini tıklatın **görünüm uygulaması yapılandırma Öğreticisi** bağlantı.

## <a name="certificate-expiration-notification-email"></a>Sertifika süre sonu bildirim e-postası

Azure AD SAML sertifikanın süresi dolmadan önce bir e-posta bildirim 7, 30 ve 60 gün gönderir. Where bildirim göndermek için e-posta adresini belirtmek için:

- Azure Active Directory Uygulama tek oturum açma sayfasında, bildirim e-posta alanına gidin.
- Sertifika süre sonu bildirim e-posta alması gereken e-posta adresi girin. Varsayılan olarak, bu alan eklenen uygulama yönetici e-posta adresini kullanır.

Bildirim e-posta alacak aadnotification@microsoft.com. İstenmeyen posta konumunuza giden e-posta önlemek için bu e-posta kişilerinize eklediğinizden emin olun. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Süresi yakında dolacak bir sertifikayı yenileme
Aşağıdaki yenileme adımları sonucunda kullanıcılarınız için önemli kapalı kalma süresi. Bu bölümde özelliği Salesforce örneği, ancak bu adımların olarak görüntülerde tüm Federasyon SaaS uygulamaları için geçerli olabilir.

1. Üzerinde **Azure Active Directory** uygulama **çoklu oturum açma** sayfasında, uygulamanız için yeni sertifika oluşturun. Tıklayarak bunu yapabilirsiniz **yeni sertifika oluştur** bağlamak **SAML imzalama sertifikası** bölümü.

    ![Yeni bir sertifika oluşturma](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. İstenen sona erme tarihi ve saati, yeni sertifika seçin ve tıklatın **kaydetmek**.

3. Sertifikayı indirin **SAML imzalama sertifikası** seçeneği. Yeni sertifika SaaS uygulamanın tek oturum açma yapılandırma ekranına karşıya yükleyin. Belirli SaaS uygulamanız için bunu öğrenmek için Yardım düğmesini tıklatın **görünüm uygulaması yapılandırma Öğreticisi** bağlantı.
   
4. Azure AD'de yeni sertifikayı etkinleştirmek için seçin **yeni sertifika etkin hale getirin** onay kutusunu ve tıklatın **kaydetmek** sayfanın üstündeki düğmesi. Bu Azure AD tarafında yeni sertifika yapar. Sertifikanın durumunu değiştirir **yeni** için **etkin**. Bu noktadan itibaren Azure AD yanıt imzalama için yeni sertifikayı kullanarak başlatır. 
   
    ![Yeni bir sertifika oluşturma](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](../active-directory-saas-tutorial-list.md)
* [Azure Active Directory'de uygulama yönetimi için makale dizini](../active-directory-apps-index.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](what-is-single-sign-on.md)
* [Sorun giderme SAML tabanlı çoklu oturum açma](../develop/active-directory-saml-debugging.md)
