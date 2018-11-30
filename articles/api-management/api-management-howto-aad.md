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
ms.openlocfilehash: 33a5b6e894c9c2b74f4c85ee96d59c0313a4bbe2
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446985"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Azure Active Directory kullanarak Azure API Management'ta Geliştirici hesaplarını yetkilendirme

Bu makalede Azure Active Directory'den (Azure AD) kullanıcıları için geliştirici portalına erişim sağlamak nasıl gösterir. Bu kılavuzda ayrıca kullanıcıları içeren dış grupları ekleyerek Azure AD kullanıcı gruplarını yönetme işlemini gösterir.

> [!NOTE]
> Azure AD tümleştirmesi kullanılabilir [geliştirici, standart ve Premium](https://azure.microsoft.com/pricing/details/api-management/) yalnızca katmanları.

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- İçeri aktarma ve Azure API Management örneği yayımlama. Daha fazla bilgi için [içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Azure AD'yi kullanarak Geliştirici hesaplarını yetkilendirme

1. [Azure Portal](https://portal.azure.com) oturum açın. 
1. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
1. Tür **API** arama kutusuna.
1. Seçin **API Management Hizmetleri**.
1. API Management hizmet örneğinizi seçin.
1. Altında **güvenlik**seçin **kimlikleri**.

1. Seçin **+ Ekle** üst.

    **Ekle kimlik sağlayıcısı** bölmesi sağ tarafta görüntülenir.
1. Altında **sağlayıcı türü**seçin **Azure Active Directory**.

    Diğer gerekli bilgileri girmenize olanak tanır denetimleri bölmesinde görünür. Denetimler içeren **istemci kimliği** ve **gizli**. (Makalenin sonraki bölümlerinde bu denetimleri hakkında bilgi edinin.)
1. İçeriğini Not **tekrar yönlendirme URL'sini**.
    
   ![Azure portalında bir kimlik sağlayıcısı ekleme adımları](./media/api-management-howto-aad/api-management-with-aad001.png)  
1. Tarayıcınızda, farklı bir sekme açın. 
1. [Azure Portal](https://portal.azure.com) gidin.
1. Şunu seçin: ![oku seçin](./media/api-management-howto-aad/arrow.png).
1. Tür **etkin**. **Azure Active Directory** bölmesi görünür.
1. **Azure Active Directory**'yi seçin.
1. Altında **Yönet**seçin **uygulama kayıtları**.
1. **Yeni uygulama kaydı**’nı seçin.

    ![Yeni bir uygulama kaydı oluşturmak için seçim](./media/api-management-howto-aad/api-management-with-aad002.png)

    **Oluştur** bölmesi sağ tarafta görüntülenir. Azure AD uygulaması ilgili bilgileri girebileceğiniz olmasıdır.
1. Uygulama için bir ad girin.
1. Uygulama türü için **Web uygulaması/API'si**.
1. Oturum açma URL'si, geliştirici portalınızın oturum açma URL'sini girin. Bu örnekte oturum açma URL'si: `https://apimwithaad.portal.azure-api.net/signin`.
1. Seçin **Oluştur** uygulama oluşturmak için.
1. Uygulamanızı bulmak için seçin **uygulama kayıtları** ve ada göre arama.

    ![Burada, uygulama için arama kutusu](./media/api-management-howto-aad/find-your-app.png)
1. Uygulama kaydedildikten sonra Git **yanıt URL'si** emin **tekrar yönlendirme URL'sini** 9. adımdan aldığınız değerine ayarlanır. 
1. Uygulamanızı yapılandırmak istiyorsanız (örneğin, değiştirme **uygulama kimliği URL'si**) seçin **özellikleri**.

    !["Özellikler" bölmesini açma](./media/api-management-howto-aad/api-management-with-aad004.png)

    Bu uygulama için birden çok Azure AD örneğinde kullanılıp kullanılmayacağını seçin **Evet** için **çok kiracılı**. Varsayılan değer **Hayır**.
1. Seçerek uygulama izinleri ayarla **gerekli izinler**.
1. Uygulamanızı seçin ve ardından **dizin verilerini okuma** ve **oturum açın ve kullanıcı profilini okuma** onay kutuları.

    ![İzinler için onay kutularını](./media/api-management-howto-aad/api-management-with-aad005.png)

1. Seçin **izinleri verin** uygulama izinleri onay verme.

    Uygulama izinlerini ve temsilci izinleri hakkında daha fazla bilgi için bkz: [Graph API'sine erişim][Accessing the Graph API].
    
1. Sol bölmede, kopyalamak **uygulama kimliği** değeri.

    !["Uygulama kimliği" değeri](./media/api-management-howto-aad/application-id.png)
1. API Management uygulamanıza geri geçiş yapın. 

    İçinde **Ekle kimlik sağlayıcısı** penceresinde Yapıştır **uygulama kimliği** değerini **istemci kimliği** kutusu.
1. Azure AD yapılandırmasına geçin ve seçin **anahtarları**.
1. Bir ad ve süresi belirterek yeni bir anahtar oluşturun. 
1. **Kaydet**’i seçin. Anahtarı oluşturulur.

    Anahtar, panoya kopyalayın.

    ![Bir anahtar oluşturmak için seçim](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Bu anahtarı not edin. Azure AD yapılandırma bölmesinde kapattıktan sonra anahtarı yeniden görüntülenemiyor.
    > 
    > 

1. API Management uygulamanıza geri geçiş yapın. 

    İçinde **Ekle kimlik sağlayıcısı** penceresinde anahtar yapıştırın **gizli** metin kutusu.

    > [!IMPORTANT]
    > Lütfen güncelleştirdiğinizden emin olun **gizli** anahtarın süresi dolmadan önce. 
    >  
    >

1. **Ekle kimlik sağlayıcısı** penceresi de içeren **izin verilen kiracılar** metin kutusu. Burada, API Management hizmet örneği API'ler için erişim vermek istediğiniz Azure AD örneğinde etki alanları belirtin. Birden çok etki alanı, satır başı, boşluk veya virgül ile ayırabilirsiniz.

    Birden çok etki alanında belirtebilirsiniz **izin verilen kiracılar** bölümü. Farklı bir etki alanı genel Yöneticisi, uygulamayı kaydedildiği özgün etki alanı farklı bir etki alanından herhangi bir kullanıcı oturum açabilmek uygulama dizini verilere erişmek için izin vermeniz gerekir. İzin vermek için genel yönetici gerekir:
    
    a. Git `https://<URL of your developer portal>/aadadminconsent` (örneğin, https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Bunlar erişim vermek istediğiniz Azure AD kiracısı etki alanı adını yazın.
    
    c. Seçin **gönderme**. 
    
    Aşağıdaki örnekte, miaoaad.onmicrosoft.com genel Yöneticisi, bu belirli Geliştirici portalına izin vermek çalışıyor. 

1. İstenen yapılandırmayı belirtin, sonra seçin **Ekle**.

    !["Ekle"Kimlik sağlayıcısı ekle"Bölmesi'nde düğmesi"](./media/api-management-howto-aad/api-management-with-aad007.png)

Değişiklikler kaydedildikten sonra kullanıcılar belirtilen Azure AD'de örneği Geliştirici portalında içindeki adımları izleyerek oturum [ve geliştirici portalında bir Azure AD hesabı kullanarak oturum açın](#log_in_to_dev_portal).

![Azure AD kiracısı adını girmek](./media/api-management-howto-aad/api-management-aad-consent.png)

Sonraki ekranda, genel yönetici vermiş onaylamanız istenir. 

![Onay izinleri atama](./media/api-management-howto-aad/api-management-permissions-form.png)

Genel olmayan yönetici bir genel yönetici izinleri verir. önce oturum açmaları çalışırsa, oturum açma denemesi başarısız olur ve hata ekranı görüntülenir.

## <a name="add-an-external-azure-ad-group"></a>Bir dış ekleme Azure AD grubu

Bir Azure AD örneğinde kullanıcılar için erişimi etkinleştirdikten sonra API Yönetimi'nde Azure AD grupları ekleyebilirsiniz. Ardından, istenen ürünleri grubunda geliştiricilerinin ilişkilendirmesini daha kolay yönetebilirsiniz.

Bir dış yapılandırmak için Azure AD grubu yapılandırmalısınız Azure AD örneği üzerinde **kimlikleri** önceki bölümdeki yordamı izleyerek sekmesi. 

Eklediğiniz dış Azure AD grupları gelen **grupları** sekmesi, API Management örneğinizin.

1. **Groups (Gruplar)** sekmesini seçin.
1. Seçin **ekleme AAD grubu** düğmesi.
   !["AAD grubu Ekle" düğmesi](./media/api-management-howto-aad/api-management-with-aad008.png)
1. Eklemek istediğiniz grubu seçin.
1. Tuşuna **seçin** düğmesi.

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

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
