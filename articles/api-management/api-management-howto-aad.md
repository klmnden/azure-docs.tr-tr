---
title: "Azure Active Directory - Azure API Management kullanarak Geliştirici hesaplarını yetkilendirmede | Microsoft Docs"
description: "API Management'te Azure Active Directory kullanarak kullanıcıları yetkilendirmek öğrenin."
services: api-management
documentationcenter: API Management
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: d89257cba70fb82d56fb1beef8a8efe66a8af02d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Azure API Management'te Azure Active Directory kullanarak Geliştirici hesaplarını yetkilendirmede

Bu makalede Azure Active Directory (Azure AD) kullanıcılar için Geliştirici Portalı için erişim sağlamak nasıl gösterir. Bu kılavuz ayrıca kullanıcıları içeren dış grupları ekleyerek Azure AD kullanıcı gruplarını yönetme gösterir.

> [!NOTE]
> Azure AD tümleştirme sağlanmıştır [geliştirici, standart ve Premium](https://azure.microsoft.com/pricing/details/api-management/) yalnızca katmanlarını.

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- İçeri aktarma ve Azure API Management örneği yayımlayın. Daha fazla bilgi için bkz: [alma ve yayımlama](import-and-publish.md).

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Azure AD kullanarak Geliştirici hesaplarını yetkilendirmede

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
3. Tür **API** arama kutusuna.
4. Seçin **API Management services**.
5. API Management hizmet örneği seçin.
6. Altında **güvenlik**seçin **kimlikleri**.

7. Seçin **+ Ekle** üstten.

    **Ekle kimlik sağlayıcısı** bölmesi sağ tarafta görüntülenir.
8. Altında **sağlayıcı türü**seçin **Azure Active Directory**.

    Gerekli diğer bilgileri girmeye olanak denetimleri bölmesinde görünür. Denetimleri içerir **istemci kimliği** ve **gizli**. (Makalenin sonraki bölümlerinde bu denetimleri hakkında bilgi alın.)
9. İçeriğini Not **tekrar yönlendirme URL'sini**.
    
   ![Azure portalında bir kimlik sağlayıcısı ekleme adımları](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. Tarayıcınızda, farklı bir sekme açın. 
11. [Azure Portal](https://portal.azure.com) gidin.
12. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
13. Tür **etkin**. **Azure Active Directory** bölmesinde görünür.
14. Seçin **Azure Active Directory**.
15. Altında **Yönet**seçin **uygulama kayıtlar**.
16. Seçin **yeni uygulama kaydı**.

    ![Yeni bir uygulama kaydı oluşturmak için seçimleri](./media/api-management-howto-aad/api-management-with-aad002.png)

    **Oluşturma** bölmesi sağ tarafta görüntülenir. Azure AD uygulama ilgili bilgileri girdiğiniz olmasıdır.
17. Uygulama için bir ad girin.
18. Uygulama türü için **Web app/API**.
19. Oturum açma URL'sini, Geliştirici Portalı oturum açma URL'sini girin. Bu örnekte, oturum açma https://apimwithaad.portal.azure-api.net/signin URL'dir.
20. Seçin **oluşturma** uygulaması oluşturmak için.
21. Uygulamanızı bulmak için seçin **uygulama kayıtlar** ve adına göre arama.

    ![Burada, bir uygulama için arama kutusu](./media/api-management-howto-aad/find-your-app.png)
22. Uygulama kaydedildikten sonra Git **yanıt URL'si** ve emin olun **tekrar yönlendirme URL'sini** adım 9 ' aldığınız değerine ayarlanır. 
23. Uygulamanızı yapılandırmak istiyorsanız (örneğin, değiştirme **uygulama kimliği URL'si**), select **özellikleri**.

    !["Özellikler" bölmesini açın](./media/api-management-howto-aad/api-management-with-aad004.png)

    Bu uygulama için birden çok Azure AD örneğinde kullanılacaksa seçin **Evet** için **çoklu kiralanan**. Varsayılan değer **Hayır**.
24. Uygulama izinleri ayarla seçerek **gerekli izinleri**.
25. Uygulamanızı seçin ve ardından **dizin verilerini okuma** ve **oturum açın ve kullanıcı profilini okuma** onay kutuları.

    ![İzinler için onay kutularını](./media/api-management-howto-aad/api-management-with-aad005.png)

    Uygulama izinleri ve temsilci izinleri hakkında daha fazla bilgi için bkz: [grafik API'sine erişim][Accessing the Graph API].
26. Sol bölmede, kopyalama **uygulama kimliği** değeri.

    !["Uygulama kimliği" değeri](./media/api-management-howto-aad/application-id.png)
27. API Management uygulamanıza geri çevirin. 

    İçinde **Ekle kimlik sağlayıcısı** penceresinde, Yapıştır **uygulama kimliği** değeri **istemci kimliği** kutusu.
28. Azure AD yapılandırmasına geçin ve seçin **anahtarları**.
29. Bir ad ve süre belirterek yeni bir anahtar oluşturun. 
30. **Kaydet**’i seçin. Anahtarı oluşturulur.

    Anahtarı panoya kopyalayın.

    ![Bir anahtar oluşturmak için seçimleri](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Bu anahtarı not edin. Azure AD yapılandırma bölmesini kapattıktan sonra anahtarı yeniden görüntülenemiyor.
    > 
    > 
31. API Management uygulamanıza geri çevirin. 

    İçinde **Ekle kimlik sağlayıcısı** penceresinde anahtarı yapıştırın **gizli** metin kutusu.
32. **Ekle kimlik sağlayıcısı** penceresinde de bulunur **izin kiracılar** metin kutusu. Burada, API Management hizmet örneği API'lerine erişim vermek istediğiniz Azure AD örneklerinin etki alanlarını belirtin. Birden çok etki alanı, satır başı, boşluk veya virgülle ayırabilirsiniz.

    Birden çok etki alanında belirtebilirsiniz **izin kiracılar** bölümü. Herhangi bir kullanıcı uygulama kaydedildiği özgün etki alanı farklı bir etki alanından oturum açabilmeniz için önce farklı etki alanının genel yönetici uygulama dizini verilere erişmek için izni vermesi gerekir. İzin vermek için genel yönetici gerekir:
    
    a. Git `https://<URL of your developer portal>/aadadminconsent` (örneğin, https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Bunlar erişim vermek istediğiniz Azure AD Kiracı etki alanı adını yazın.
    
    c. Seçin **gönderme**. 
    
    Aşağıdaki örnekte, bu belirli Geliştirici Portalı izni vermek miaoaad.onmicrosoft.com bir genel yönetici çalışıyor. 

33. İstenen yapılandırma belirttikten sonra Seç **Ekle**.

    !["" Ekle kimlik sağlayıcısı"bölmesinde Ekle düğmesi"](./media/api-management-howto-aad/api-management-with-aad007.png)

Değişiklikler kaydedilir sonra kullanıcılar belirtilen Azure AD'de örnek Geliştirici Portalı'ndaki adımları izleyerek oturum açarak [Geliştirici Portalı bir Azure AD hesabı kullanarak oturum](#log_in_to_dev_portal).

![Azure AD kiracısı adını girme](./media/api-management-howto-aad/api-management-aad-consent.png)

Sonraki ekranda, genel yönetici izni vermiş onaylamanız istenir. 

![Onay izinler atama](./media/api-management-howto-aad/api-management-permissions-form.png)

Genel olmayan bir yönetici bir genel yönetici izinleri verir önce oturum açmak çalışırsa, oturum açma denemesi başarısız olur ve hata ekranı görüntülenir.

## <a name="add-an-external-azure-ad-group"></a>Bir dış eklemek Azure AD grubu

Azure AD örneğinde kullanıcıların erişim etkinleştirdikten sonra API Management'te Azure AD grupları ekleyebilirsiniz. Ardından, grubu geliştiriciler ilişkilendirme istenen ürünleri ile daha kolay yönetebilir.

Bir dış yapılandırmak için Azure AD grubundaki ilk yapılandırmanız gerekir Azure AD örneğinde üzerinde **kimlikleri** önceki bölümdeki yordamı izleyerek sekmesi. 

Dış Azure eklediğiniz AD grupları **grupları** sekmesi, API Management örneğinin.

1. **Groups (Gruplar)** sekmesini seçin.
2. Seçin **ekleme AAD grup** düğmesi.
   !["AAD Grup Ekle" düğmesi](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Eklemek istediğiniz grubu seçin.
4. Tuşuna **seçin** düğmesi.

Dış Azure AD ekledikten sonra Grup gözden geçirin ve özelliklerini yapılandırın. Gruptan adını seçin **grupları** sekmesi. Buradan, düzenleyebileceğiniz **adı** ve **açıklama** grubu için bilgileri.
 
Yapılandırılmış kullanıcıların Azure AD örneğinde şimdi uygulamasında oturum açabilir Geliştirici portalına. Görüntüleyin ve görünürlük sahip oldukları herhangi bir gruba abone olun.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Geliştirici Portalı bir Azure AD hesabı kullanarak oturum açın

Geliştirici Portalı önceki bölümlerde yapılandırılmış bir Azure AD hesabı kullanarak oturum açmak için:

1. Active Directory Uygulama yapılandırması oturum açma URL'SİNDEN kullanarak yeni bir tarayıcı penceresi açın ve seçin **Azure Active Directory**.

   ![Oturum açma sayfası][api-management-dev-portal-signin]

2. Azure AD içinde bir kullanıcı kimlik bilgilerini girin ve seçin **oturum**.

   ![Kullanıcı adı ve parola ile imzalama][api-management-aad-signin]

3. Gerekirse ek bilgileri bir kayıt formu istenebilir. Kayıt formunu tamamladıktan ve seçin **kaydolun**.

   ![Kayıt formundaki "Kaydolun" düğmesi][api-management-complete-registration]

Kullanıcı artık, API Management hizmet örneğinizin Geliştirici Portalı oturum.

![Kayıt tamamlandıktan sonra Geliştirici Portalı][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
