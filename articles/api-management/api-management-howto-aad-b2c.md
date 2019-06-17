---
title: Azure Active Directory B2C - Azure API Management'ı kullanarak Geliştirici hesaplarını yetkilendirme | Microsoft Docs
description: API Yönetimi'nde Azure Active Directory B2C'yi kullanarak kullanıcıları yetkilendirme konusunda bilgi edinin.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 644cc2a4175043b523d53b39f17483c6f3acfe96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64696742"
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Azure API Yönetimi'nde Azure Active Directory B2C kullanarak Geliştirici hesaplarını yetkilendirme nasıl

## <a name="overview"></a>Genel Bakış

Azure Active Directory B2C, tüketicilere yönelik web ve mobil uygulamalar için bir bulut kimlik yönetimi çözümü ' dir. Geliştirici portalınızın erişimi yönetmek için kullanabilirsiniz. Bu kılavuz API Management hizmetinizdeki Azure Active Directory B2C ile tümleştirmek için gerekli yapılandırmayı gösterir. Klasik Azure Active Directory kullanarak Geliştirici portal erişimini etkinleştirme hakkında daha fazla bilgi için bkz: [Azure Active Directory kullanarak Geliştirici hesaplarını yetkilendirme nasıl].

> [!NOTE]
> Bu kılavuzdaki adımları tamamlamak için önce bir uygulamayı oluşturmak için bir Azure Active Directory B2C kiracısı olmalıdır. Ayrıca, kaydolma ve oturum açma ilkeleri hazır olması gerekir. Daha fazla bilgi için [Azure Active Directory B2C genel bakış].

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak Geliştirici hesaplarını yetkilendirme

1. Başlamak için oturum açın [Azure portalında](https://portal.azure.com) ve API Management örneğinizin bulun.

   > [!NOTE]
   > Henüz bir API Management hizmet örneği oluşturmadıysanız, bkz. [bir API Management hizmet örneği oluşturma] [ Create an API Management service instance] içinde [Azure API Yönetimi Öğreticisi ile çalışmaya başlama] [Get started with Azure API Management].

2. Altında **kimlikleri**. Tıklayın **+ Ekle** en üstünde.

   **Ekle kimlik sağlayıcısı** bölmesi sağ tarafta görüntülenir. Seçin **Azure Active Directory B2C**.
    
   ![AAD B2C kimlik sağlayıcısı olarak ekleme][api-management-howto-add-b2c-identity-provider]

3. Kopyalama **yeniden yönlendirme URL'si**.

   ![AAD B2C kimlik sağlayıcısına yeniden yönlendirme URL'si][api-management-howto-copy-b2c-identity-provider-redirect-url]

4. Yeni bir sekmede, Azure Active Directory B2C kiracınızda Azure portal ve açık erişim **uygulamaları** dikey penceresi.

   ![Yeni bir uygulamayı 1 kaydetme][api-management-howto-aad-b2c-portal-menu]

5. Tıklayın **Ekle** yeni bir Azure Active Directory B2C uygulaması oluşturmak için.

   ![Yeni bir uygulamayı 2 kaydetme][api-management-howto-aad-b2c-add-button]

6. İçinde **yeni uygulama** dikey penceresinde, uygulama için bir ad girin. Seçin **Evet** altında **Web uygulaması/Web API'sini**ve **Evet** altında **örtük akışa izin ver**. Yapıştırın **tekrar yönlendirme URL'sini** 3. adımda kopyaladığınız **yanıt URL'si** metin kutusu.

   ![Yeni bir uygulamayı 3 kaydetme][api-management-howto-aad-b2c-app-details]

7. **Oluştur** düğmesine tıklayın. Uygulama oluşturulduğunda görünür **uygulamaları** dikey penceresi. Ayrıntılarını görmek için uygulamanın adına tıklayın.

   ![Yeni bir uygulamayı 4 kaydetme][api-management-howto-aad-b2c-app-created]

8. Gelen **özellikleri** dikey penceresinde kopyalama **uygulama kimliği** panoya.

   ![Uygulama kimliği 1][api-management-howto-aad-b2c-app-id]

9. Geçiş API Management'a **Ekle kimlik sağlayıcısı** bölmesi ve içine yapıştırma kimliği **istemci kimliği** metin kutusu.
    
10. B2C uygulaması kayıt anahtarı, tıklatın **anahtarları** düğmesine ve ardından **anahtar üret**. Tıklayın **Kaydet** yapılandırmasını kaydetmek ve görüntülemek için **uygulama anahtarı**. Anahtar, panoya kopyalayın.

    ![1 uygulama anahtarı][api-management-howto-aad-b2c-app-key]

11. Geçiş API Management'a **Ekle kimlik sağlayıcısı** bölmesi ve anahtarını yapıştırın **gizli** metin kutusu.
    
12. Azure Active Directory B2C kiracısında etki alanı adını **Signın Kiracı**.

13. **Yetkilisi** alanı kullanmak için Azure AD B2C oturum açma URL'sini denetlemenize olanak tanır. Değerine **< your_b2c_tenant_name >. b2clogin.com**.

14. Belirtin **kaydolma İlkesi** ve **Signın ilke** B2C Kiracısı ilkelerden. İsteğe bağlı olarak, sağlayabilirsiniz **Profil Düzenleme İlkesi** ve **parolası sıfırlama İlkesi**.

15. İstenen yapılandırmayı belirledikten sonra tıklayın **Kaydet**.

    Değişiklikler kaydedildikten sonra geliştiricilerin yeni hesapları oluşturmanız ve Azure Active Directory B2C kullanarak oturum açın ve geliştirici portalında mümkün olacaktır.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir geliştirici hesabı açın

1. Azure Active Directory B2C kullanarak bir geliştirici hesabı kaydolmak için yeni bir tarayıcı penceresi açın ve Geliştirici Portalı'na gidin. Tıklayın **kaydolun** düğmesi.

   ![Geliştirici Portalı 1][api-management-howto-aad-b2c-dev-portal]

2. Kaydolmak için istediğinize **Azure Active Directory B2C**.

   ![Geliştirici portalını Yapılandır 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Önceki bölümde yapılandırdığınız kaydolma İlkesi yönlendirilirsiniz. E-posta adresinizi veya biri, mevcut sosyal hesaplarını kullanarak oturum açmak seçin.

   > [!NOTE]
   > Azure Active Directory B2C etkin tek seçenek ise **kimlikleri** sekmesi publisher Portalı'nda, kaydolma ilkeye doğrudan yönlendirilirsiniz.

   ![Geliştirici portalı][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Kayıt işlemi tamamlandığında, geliştirici portalına yönlendirilirsiniz. Şimdi Geliştirici Portalı, API Management hizmet örneği için oturum açmadıysanız.

    ![Kayıt tamamlandı][api-management-registration-complete]

## <a name="next-steps"></a>Sonraki adımlar

*  [Azure Active Directory B2C genel bakış]
*  [Azure Active Directory B2C: Genişletilebilir ilke çerçevesi]
*  [Azure Active Directory B2C'de kimlik sağlayıcısı olarak bir Microsoft hesabı kullanın]
*  [Bir Google hesabı Azure Active Directory B2C'de kimlik sağlayıcısı kullanın]
*  [Bir LinkedIn hesabıyla bir kimlik sağlayıcısı olarak Azure Active Directory B2C'yi kullanın]
*  [Bir Facebook hesabı Azure Active Directory B2C'de kimlik sağlayıcısı kullanın]



[api-management-howto-add-b2c-identity-provider]: ./media/api-management-howto-aad-b2c/api-management-add-b2c-identity-provider.PNG
[api-management-howto-copy-b2c-identity-provider-redirect-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-identity-provider-redirect-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: https://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[Azure Active Directory B2C genel bakış]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[Azure Active Directory kullanarak Geliştirici hesaplarını yetkilendirme nasıl]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: Genişletilebilir ilke çerçevesi]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Azure Active Directory B2C'de kimlik sağlayıcısı olarak bir Microsoft hesabı kullanın]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[Bir Google hesabı Azure Active Directory B2C'de kimlik sağlayıcısı kullanın]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Bir Facebook hesabı Azure Active Directory B2C'de kimlik sağlayıcısı kullanın]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Bir LinkedIn hesabıyla bir kimlik sağlayıcısı olarak Azure Active Directory B2C'yi kullanın]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
