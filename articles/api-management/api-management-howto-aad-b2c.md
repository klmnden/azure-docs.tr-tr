---
title: "Azure Active Directory B2C - Azure API Management kullanarak Geliştirici hesaplarını yetkilendirmede | Microsoft Docs"
description: "API Management'te Azure Active Directory B2C kullanarak kullanıcıları yetkilendirmek öğrenin."
services: api-management
documentationcenter: API Management
author: vladvino
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: d99dbbd834cb8f067b88b765ccddcd7f4eb44a1f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
> [!WARNING]
> Azure Active Directory B2C tümleştirme sağlanmıştır [Geliştirici ve Premium](https://azure.microsoft.com/en-us/pricing/details/api-management/) yalnızca katmanlarını.

# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Azure API Management'te Azure Active Directory B2C kullanarak Geliştirici hesaplarını yetkilendirmede nasıl
## <a name="overview"></a>Genel Bakış
Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür. Geliştirici portalınızın erişimi yönetmek için kullanabilirsiniz. Bu kılavuz, Azure Active Directory B2C ile tümleştirmek için API Management hizmetiniz gerekli yapılandırmayla gösterir. Klasik Azure Active Directory'yi kullanarak Geliştirici portalına erişim etkinleştirme hakkında daha fazla bilgi için bkz: [Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl].

> [!NOTE]
> Bu kılavuzdaki adımları tamamlamak için ilk bir uygulama oluşturmak için bir Azure Active Directory B2C kiracısına sahip olmanız gerekir. Ayrıca, kaydolma ve oturum açma ilkeleri hazır olması gerekir. Daha fazla bilgi için bkz: [Azure Active Directory B2C genel bakış].

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak Geliştirici hesaplarını yetkilendirmede

1. Kullanmaya başlamak için tıklayın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Bu sizi API Management yayımcı portalına götürür.

   ![Yayımcı portalı][api-management-management-console]

   > [!NOTE]
   > Henüz bir API Management hizmeti örneği oluşturmadıysanız, bkz: [bir API Management hizmet örneği oluşturma] [ Create an API Management service instance] içinde [Azure API Management öğretici ile çalışmaya başlama][Get started with Azure API Management].

2. Üzerinde **API Management** menüsünde tıklatın **güvenlik**. Üzerinde **kimlikleri** sekmesinde, seçin **Azure Active Directory B2C**.

  ![Dış kimlikler 1][api-management-howto-aad-b2c-security-tab]

3. Not **tekrar yönlendirme URL'sini** ve anahtar Azure Active Directory B2C Azure portalında.

  ![Dış kimlikler 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. Tıklatın **uygulamaları** düğmesi.

  ![Yeni bir uygulama 1 kaydetme][api-management-howto-aad-b2c-portal-menu]

5. Tıklatın **Ekle** düğmesi yeni bir Azure Active Directory B2C uygulaması oluşturun.

  ![Yeni bir uygulama 2 kaydetme][api-management-howto-aad-b2c-add-button]

6. İçinde **yeni uygulama** dikey penceresinde, uygulama için bir ad girin. Seçin **Evet** altında **Web uygulaması/Web API**ve seçin **Evet** altında **örtük akışına izin**. Ardından kopyalama **tekrar yönlendirme URL'sini** gelen **Azure Active Directory B2C** bölümünü **kimlikleri** sekmesinde yayımcı portalında ve yapıştırın **yanıt URL'si** metin kutusu.

  ![Yeni bir uygulamayı 3 Kaydet][api-management-howto-aad-b2c-app-details]

7. **Oluştur** düğmesine tıklayın. Uygulama oluşturulduğunda görünür **uygulamaları** dikey. Ayrıntılarını görmek için uygulama adına tıklayın.

  ![Yeni bir uygulama 4 kaydetme][api-management-howto-aad-b2c-app-created]

8. Gelen **özellikleri** dikey penceresinde, kopya **uygulama kimliği** panoya.

  ![1 uygulama kimliği][api-management-howto-aad-b2c-app-id]

9. Geçiş yayımcı portalına dönün ve kimliği içine yapıştırma **istemci kimliği** metin kutusu.

  ![Uygulama kimliği 2][api-management-howto-aad-b2c-client-id]

10. Azure portalına anahtarı, tıklatın **anahtarları** düğmesine tıklayın ve ardından **anahtar üret**. Tıklatın **kaydetmek** yapılandırmasını kaydetmek ve görüntülemek için **uygulama anahtarı**. Anahtarı panoya kopyalayın.

  ![1 uygulama anahtarı][api-management-howto-aad-b2c-app-key]

11. Geçiş yayımcı portalına dönün ve içine anahtarını yapıştırın **gizli** metin kutusu.

  ![Uygulama anahtarı 2][api-management-howto-aad-b2c-client-secret]

12. Azure Active Directory B2C kiracısı'nda etki alanı adını belirtin **izin Kiracı**.

  ![İzin verilen Kiracı][api-management-howto-aad-b2c-allowed-tenant]

13. Belirtin **kaydolma İlkesi** ve **Signın İlkesi**. İsteğe bağlı olarak, ayrıca sağlayabilirsiniz **Profil Düzenleme İlkesi** ve **parola sıfırlama İlkesi**.

  ![İlkeler][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > İlkeler hakkında daha fazla bilgi için bkz: [Azure Active Directory B2C: Genişletilebilir ilke çerçevesini].

14. İstenen yapılandırma belirttikten sonra tıklatın **kaydetmek**.

  Değişiklikler kaydedildikten sonra geliştiricilerin yeni hesapları oluşturabilir ve Azure Active Directory B2C kullanarak oturum açın Geliştirici portalına olacaktır.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir geliştirici hesabı için kaydolun

1. Azure Active Directory B2C kullanarak bir geliştirici hesabı için kaydolmak için yeni bir tarayıcı penceresi açın ve geliştirici portalına gidin. Tıklatın **kaydolun** düğmesi.

   ![Geliştirici Portalı 1][api-management-howto-aad-b2c-dev-portal]

2. İle kaydolmak isterseniz **Azure Active Directory B2C**.

   ![Geliştirici Portalı 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Önceki bölümde yapılandırılmış kaydolma İlkesi yönlendirilirsiniz. E-posta adresinizi veya var olan sosyal hesaplarınızı birini kullanarak kaydolmak seçin.

   > [!NOTE]
   > Azure Active Directory B2C etkin tek seçenek ise **kimlikleri** sekmesini yayımcı Portalı'nda, kaydolma ilkeye doğrudan yönlendirilmesi.

   ![Geliştirici portalı][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Kayıt işlemi tamamlandıktan sonra Geliştirici portalına yönlendirilirsiniz. Artık Geliştirici portalına API Management hizmet örneği için oturum açtınız.

    ![Kayıt tamamlandı][api-management-registration-complete]

## <a name="next-steps"></a>Sonraki adımlar

*  [Azure Active Directory B2C genel bakış]
*  [Azure Active Directory B2C: Genişletilebilir ilke çerçevesini]
*  [Azure Active Directory B2C, kimlik sağlayıcısı bir Microsoft hesabı kullanın]
*  [Azure Active Directory B2C, kimlik sağlayıcısı bir Google hesabı kullan]
*  [Azure Active Directory B2C, kimlik sağlayıcısı LinkedIn hesabını kullan]
*  [Azure Active Directory B2C, kimlik sağlayıcısı Facebook hesabıyla kullanın]




[api-management-howto-aad-b2c-security-tab]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab.PNG
[api-management-howto-aad-b2c-security-tab-reply-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab-reply-url.PNG
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
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[Azure Active Directory B2C genel bakış]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: Genişletilebilir ilke çerçevesini]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Azure Active Directory B2C, kimlik sağlayıcısı bir Microsoft hesabı kullanın]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[Azure Active Directory B2C, kimlik sağlayıcısı bir Google hesabı kullan]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Azure Active Directory B2C, kimlik sağlayıcısı Facebook hesabıyla kullanın]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Azure Active Directory B2C, kimlik sağlayıcısı LinkedIn hesabını kullan]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
