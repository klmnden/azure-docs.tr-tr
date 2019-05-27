---
title: Azure Active Directory - Azure API Management'ı kullanarak Geliştirici hesaplarını yetkilendirme | Microsoft Docs
description: API Yönetimi'nde Azure Active Directory kullanarak kullanıcıları yetkilendirme konusunda bilgi edinin.
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
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: d267ff3a43438d9fe6e4e21f0ac023cfa6675f19
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956312"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Azure Active Directory kullanarak Azure API Management'ta Geliştirici hesaplarını yetkilendirme

Bu makalede Azure Active Directory'den (Azure AD) kullanıcıları için geliştirici portalına erişim sağlamak nasıl gösterir. Bu kılavuzda ayrıca kullanıcıları içeren dış grupları ekleyerek Azure AD kullanıcı gruplarını yönetme işlemini gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- İçeri aktarma ve Azure API Management örneği yayımlama. Daha fazla bilgi için [içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Azure AD'yi kullanarak Geliştirici hesaplarını yetkilendirme

1. [Azure Portal](https://portal.azure.com) oturum açın. 
2. Seç ![oku seçin](./media/api-management-howto-aad/arrow.png):
3. Tür **API** arama kutusuna.
4. Seçin **API Management Hizmetleri**.
5. API Management hizmet örneğinizi seçin.
6. Altında **güvenlik**seçin **kimlikleri**.
7. Seçin **+ Ekle** üst.

    **Ekle kimlik sağlayıcısı** bölmesi sağ tarafta görüntülenir.
8. Altında **sağlayıcı türü**seçin **Azure Active Directory**.

    Diğer gerekli bilgileri girmenize olanak tanır denetimleri bölmesinde görünür. Denetimler içeren **istemci kimliği** ve **gizli**. (Makalenin sonraki bölümlerinde bu denetimleri hakkında bilgi edinin.)
9. İçeriği Not **tekrar yönlendirme URL'sini**.
    
   ![Azure portalında bir kimlik sağlayıcısı ekleme adımları](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. Tarayıcınızda, farklı bir sekme açın. 
11. Gidin [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) Active Directory'de bir uygulamayı kaydedin.
12. Altında **Yönet**seçin **uygulama kayıtları**.
13. Seçin **yeni kayıt**. Üzerinde **bir uygulamayı kaydetme** sayfasında, değerleri aşağıdaki gibi ayarlayın:
    
* Ayarlama **adı** için anlamlı bir ad. Örneğin, *Geliştirici Portalı*
* Ayarlama **desteklenen hesap türleri** için **hesapları yalnızca kuruluş bu dizinde**. 
* Ayarlama **yeniden yönlendirme URI'si** 9. adımdan aldığınız değeri. 
* Seçin **kaydetme**. 

14.  Uygulama kaydedildikten sonra kopyalama **uygulama (istemci) kimliği** gelen **genel bakış** sayfası. 
15. API Management örneğinizin geri dönün. İçinde **Ekle kimlik sağlayıcısı** penceresinde Yapıştır **uygulama (istemci) kimliği** içine değer **istemci kimliği** kutusu.
16. Geçiş geri Azure AD'ye yapılandırmaya, Select **sertifikaları ve parolaları** altında **Yönet**. Seçin **yeni gizli** düğmesi. Bir değer girin **açıklama**, tüm seçeneğini **Expires** ve **Ekle**. Bu sayfadan ayrılmadan önce istemci gizli değer kopyalayın. Bu sonraki adımda gerekecektir. 
17. Altında **Yönet**seçin **kimlik doğrulaması** seçip **kimlik belirteçlerini** altında **örtülü izin**
18. API Management örneğinizin geri dönün, gizli dizi içine yapıştırın **gizli** kutusu.

    > [!IMPORTANT]
    > Lütfen güncelleştirdiğinizden emin olun **gizli** anahtarın süresi dolmadan önce. 
    >  
    >

19. **Ekle kimlik sağlayıcısı** penceresi de içeren **izin verilen kiracılar** metin kutusu. Burada, API Management hizmet örneği API'ler için erişim vermek istediğiniz Azure AD örneğinde etki alanları belirtin. Birden çok etki alanı, satır başı, boşluk veya virgül ile ayırabilirsiniz.

> [!NOTE]
> Birden çok etki alanında belirtebilirsiniz **izin verilen kiracılar** bölümü. Farklı bir etki alanı genel Yöneticisi, uygulamayı kaydedildiği özgün etki alanı farklı bir etki alanından herhangi bir kullanıcı oturum açabilmek uygulama dizini verilere erişmek için izin vermeniz gerekir. Genel yönetici izinleri vermeniz gerekir: bir. Git `https://<URL of your developer portal>/aadadminconsent` (örneğin, https://contoso.portal.azure-api.net/aadadminconsent).
> b. Bunlar erişim vermek istediğiniz Azure AD kiracısı etki alanı adını yazın.
> c. Seçin **gönderme**. 

20.  İstenen yapılandırmayı belirtin, sonra seçin **Ekle**.

Değişiklikler kaydedildikten sonra kullanıcılar belirtilen Azure AD'de örneği Geliştirici portalında içindeki adımları izleyerek oturum [ve geliştirici portalında bir Azure AD hesabı kullanarak oturum açın](#log_in_to_dev_portal).

## <a name="add-an-external-azure-ad-group"></a>Bir dış ekleme Azure AD grubu

Bir Azure AD örneğinde kullanıcılar için erişimi etkinleştirdikten sonra API Yönetimi'nde Azure AD grupları ekleyebilirsiniz. Ardından, istenen ürünleri grubunda geliştiricilerinin ilişkilendirmesini daha kolay yönetebilirsiniz.

 > [!IMPORTANT]
 > Bir dış eklemek için Azure AD grubu yapılandırmalısınız Azure AD örneği üzerinde **kimlikleri** önceki bölümdeki yordamı izleyerek sekmesi. Ayrıca, uygulama erişim için Azure AD Graph API ile verilmelidir `Directory.Read.All` izni. 

Eklediğiniz dış Azure AD grupları gelen **grupları** sekmesi, API Management örneğinizin.

1. **Groups (Gruplar)** sekmesini seçin.
2. Seçin **ekleme AAD grubu** düğmesi.
   !["AAD grubu Ekle" düğmesi](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Eklemek istediğiniz grubu seçin.
4. Tuşuna **seçin** düğmesi.

Dış Azure AD'yi ekledikten sonra Grup gözden geçirebilir ve özelliklerini yapılandırın. Gruptan adını seçin **grupları** sekmesi. Buradan, düzenleyebileceğiniz **adı** ve **açıklama** grubu için bilgileri.
 
Kullanıcıların yapılandırılmış Azure AD örneği artık uygulamasında oturum açabilir ve geliştirici portalında. Görüntüleyebilir ve görünürlük sahip oldukları herhangi bir gruba abone olun.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Bir Azure AD hesabı kullanarak, geliştirici portalında oturum açın

Önceki bölümlerde yapılandırılmış bir Azure AD hesabı kullanarak Geliştirici portalında oturum açmak için:

1. Active Directory Uygulama yapılandırmasından oturum açma URL'sini kullanarak yeni bir tarayıcı penceresi açın ve seçin **Azure Active Directory**.

   ![Oturum açma sayfası][api-management-dev-portal-signin]

1. Azure AD'de bir kullanıcı kimlik bilgilerini girin ve seçin **oturum**.

   ![Kullanıcı adı ve parolanızla oturum açma][api-management-aad-signin]

1. Herhangi bir ek bilgi gerekiyorsa bir kayıt formu ile istenebilir. Kayıt formunu tamamladıktan ve seçin **kaydolun**.

   ![Kayıt formu üzerinde "Kaydolma" düğmesi][api-management-complete-registration]

Kullanıcı artık API Management hizmet örneğinizin Geliştirici portalında oturum açmış.

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

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: https://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
