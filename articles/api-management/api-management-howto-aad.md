---
title: "Azure Active Directory - Azure API Management kullanarak Geliştirici hesaplarını yetkilendirmede | Microsoft Docs"
description: "API Management'te Azure Active Directory'yi kullanarak kullanıcıların nasıl yetkilendirileceğini öğrenin."
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
ms.openlocfilehash: c9d99d6f7c2777c8e68f5db50b402280377d88e9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Azure API Management'te Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl

Bu kılavuz Geliştirici portalına kullanıcılar için Azure Active Directory'den erişmesini gösterilmiştir. Bu kılavuz ayrıca bir Azure Active Directory Kullanıcıları içeren dış grupları ekleyerek Azure Active Directory kullanıcı gruplarını yönetme gösterir.

> [!WARNING]
> Azure Active Directory Tümleştirme sağlanmıştır [geliştirici, standart ve Premium](https://azure.microsoft.com/pricing/details/api-management/) yalnızca katmanlarını.

## <a name="prerequisites"></a>Önkoşullar

- Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- İçeri aktarma ve API Management örneği yayımlayın. Daha fazla bilgi için bkz: [alma ve yayımlama](import-and-publish.md).

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
3. Arama kutusuna "API" yazın.
4. Tıklatın **API Management services**.
5. APIM hizmet örneği seçin.
6. Altında **güvenlik**seçin **kimlikleri**.

    ![Dış kimlikler](./media/api-management-howto-aad/api-management-with-aad001.png)
7. Tıklatın **+ Ekle** üstten.

    **Ekle kimlik sağlayıcısı** penceresi sağ tarafta görüntülenir.
8. Altında **sağlayıcı türü**seçin **Azure Active Directory**.

    Gerekli diğer bilgileri girmeye olanak denetimleri penceresinde görünür. Denetimleri içerir: **istemci kimliği**, **gizli** (daha sonra öğreticide bilgi al).
9. Not **tekrar yönlendirme URL'sini**.  
10. Tarayıcınızda, farklı bir sekme açın. 
11. [Azure portalını](https://portal.azure.com) açın.
12. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
13. Türü "active" **Azure Active Directory** görüntülenir.
14. Seçin **Azure Active Directory**.
15. Altında **Yönet**seçin **uygulama kaydı**.

    ![Uygulama kaydı](./media/api-management-howto-aad/api-management-with-aad002.png)
16. **Yeni uygulama kaydı**’na tıklayın.

    **Oluşturma** penceresi sağ tarafta görüntülenir. AAD uygulama relavent bilgileri girdiğiniz olmasıdır.
17. Eneter uygulama için bir ad.
18. Uygulama türü için **Web app/API**.
19. Oturum açma URL'si için Geliştirici Portalı oturum açma URL'sini girin. Bu örnekte, oturum açma URL'si şöyledir: https://apimwithaad.portal.azure-api.net/signin.
20. Tıklatın **oluşturma** uygulaması oluşturmak için.
21. Uygulamanızı bulmak için seçin **uygulama kayıtlar** ve adına göre arama.

    ![Uygulama kaydı](./media/api-management-howto-aad/find-your-app.png)
22. Uygulama kaydedildikten sonra Git **yanıt URL'si** ve "Yeniden yönlendirme URL'si" adım 9 ' aldığınız değere ayarlandığından emin olun. 
23. Uygulamanızı yapılandırmak istiyorsanız (örneğin, değiştirme **uygulama kimliği URL'si**) seçin **özellikleri**.

    ![Uygulama kaydı](./media/api-management-howto-aad/api-management-with-aad004.png)

    Birden çok Azure Active dizinleri, bu uygulama için kullanılacak kullanacaksanız, tıklatın **Evet** için **uygulamasıdır çok kiracılı**. Varsayılan değer **Hayır**.
24. Uygulama izinleri ayarla seçerek **gerekli izinleri**.
25. Seçin, uygulama ve onay **dizin verilerini okuma** ve **oturum açın ve kullanıcı profilini okuma**.

    ![Uygulama kaydı](./media/api-management-howto-aad/api-management-with-aad005.png)

    Uygulama ve temsilci izinleri hakkında daha fazla bilgi için bkz: [grafik API'sine erişim][Accessing the Graph API].
26. Sol penceresinde kopyalama **uygulama kimliği** değeri.

    ![Uygulama kaydı](./media/api-management-howto-aad/application-id.png)
27. API Management uygulamanıza geri çevirin. **Ekle kimlik sağlayıcısı** penceresinde görüntülenmesi. <br/>Yapıştır **uygulama kimliği** değeri **istemci kimliği** kutusu.
28. Geçiş Azure Active Directory yapılandırmasına ve tıklayın **anahtarları**.
29. Yeni bir anahtar tarafından speicifying bir ad ve süre oluşturun. 
30. **Kaydet**’e tıklayın. Anahtar oluşturulan.

    Anahtarı panoya kopyalayın.

    ![Uygulama kaydı](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Bu anahtarı not edin. Azure Active Directory yapılandırması penceresini kapattığınızda, anahtarı yeniden görüntülenemiyor.
    > 
    > 
31. API Management uygulamanıza geri çevirin. <br/>İçinde **Ekle kimlik sağlayıcısı** penceresinin içine anahtarını yapıştırın **gizli** metin kutusu.
32. **Ekle kimlik sağlayıcısı** penceresinde içeren **izin kiracılar** metin kutusunda, hangi dizinleri API Management hizmet örneği API erişimi belirtin. <br/>Azure Active Directory örnekleri için erişim vermek istediğiniz etki alanlarını belirtin. Birden çok etki alanı, satır başı, boşluk veya virgülle ayırabilirsiniz.

    Birden çok etki alanı içinde belirtilen **izin kiracılar** bölümü. Herhangi bir kullanıcı uygulama kaydedildiği özgün etki alanı farklı bir etki alanından oturum önce farklı etki alanının genel yönetici uygulama dizini verilere erişmek için izni vermesi gerekir. İzin vermek için genel yönetici için tamamlamalıdır `https://<URL of your developer portal>/aadadminconsent` (örneğin, https://contoso.portal.azure-api.net/aadadminconsent) erişimi verin ve Gönder'i istedikleri Active Directory Kiracı etki alanı adı yazın. Aşağıdaki örnekte, bir genel yönetici `miaoaad.onmicrosoft.com` bu belirli Geliştirici Portalı izni vermek çalışıyor. 

33. İstenen yapılandırma belirlendikten sonra tıklatın **Ekle**.

    ![Uygulama kaydı](./media/api-management-howto-aad/api-management-with-aad007.png)

Değişiklikler kaydedildikten sonra kullanıcılar belirtilen Azure Active Directory Geliştirici Portalı'ndaki adımları izleyerek oturum [bir Azure Active Directory hesabı kullanarak Geliştirici Portalı oturum açma](#log_in_to_dev_portal).

![İzinler](./media/api-management-howto-aad/api-management-aad-consent.png)

Sonraki ekranda, genel yönetici izni vermiş onaylamanız istenir. 

![İzinler](./media/api-management-howto-aad/api-management-permissions-form.png)

Genel olmayan bir yönetici bir genel yönetici tarafından izin verilmeden önce oturum açmak çalışırsa, oturum açma denemesi başarısız olur ve hata ekranı görüntülenir.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Bir dış Azure Active Directory grubu ekleme

Kullanıcılar Azure Active Directory erişimi etkinleştirdikten sonra API yönetime daha kolay grubundaki geliştiriciler ilişkilendirme istenen ürünleri ile yönetmek için Azure Active Directory grupları ekleyebilirsiniz.

Dış bir Azure Active Directory grubunu yapılandırmak için Azure Active Directory ilk kimlikleri sekmesinde önceki bölümdeki yordamı izleyerek yapılandırılması gerekir. 

Dış Azure Active Directory grupları eklendi **grupları** sekmesi, API Management örneğinin.

![Gruplar](./media/api-management-howto-aad/api-management-with-aad008.png)

1. **Groups (Gruplar)** sekmesini seçin.
2. Tıklatın **ekleme AAD grup** düğmesi.
3. Eklemek istediğiniz grubu seçin.
4. Tuşuna **seçin** düğmesi.

Bir Azure Active Directory grubu oluşturulduktan sonra gözden geçirin ve bunlar eklendikten sonra dış Gruplar'ı tıklatın grubundan adını özelliklerini yapılandıramadı **grupları** sekmesi.

Buradan düzenleyebilirsiniz **adı** ve **açıklama** grubunun.
 
Yapılandırılmış Azure Active Directory'den kullanıcıları görebilir ve Geliştirici Portalı oturum açın ve görünürlük aşağıdaki bölümünde yer alan yönergeleri izleyerek sahip oldukları herhangi bir grup için abone olabilirsiniz.

## <a name="a-idlogintodevportalhow-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><a id="log_in_to_dev_portal"/>Bir Azure Active Directory hesabı kullanarak Geliştirici portalında oturum açmak nasıl

Geliştirici portalında önceki bölümlerde yapılandırılmış bir Azure Active Directory hesabı kullanarak oturum açın, kullanarak yeni bir tarayıcı penceresi açık **oturum açma URL'si** Active Directory Uygulama Yapılandırması ve tıklatın **Azure Active Directory**.

![Geliştirici Portalı][api-management-dev-portal-signin]

Azure Active Directory'de bir kullanıcı kimlik bilgilerini girin ve tıklayın **oturum**.

![Oturum aç][api-management-aad-signin]

Ek bilgileri gerekiyorsa, Kayıt formuyla istenebilir. Kayıt formunu tamamladıktan ve tıklatın **kaydolun**.

![Kayıt][api-management-complete-registration]

Kullanıcı artık, API Management hizmet örneğinizin Geliştirici Portalı içine kaydedilir.

![Kayıt Tamamlandı][api-management-registration-complete]

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

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
