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
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 3faa6c1867808436a66a2b33ea1a9d79ede2c8fb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
> [!WARNING]
> Azure Active Directory Tümleştirme sağlanmıştır [Geliştirici ve Premium](https://azure.microsoft.com/en-us/pricing/details/api-management/) yalnızca katmanlarını.

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Azure API Management'te Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl
## <a name="overview"></a>Genel Bakış
Bu kılavuz Geliştirici portalına kullanıcılar için Azure Active Directory'den erişmesini gösterilmiştir. Bu kılavuz ayrıca bir Azure Active Directory Kullanıcıları içeren dış grupları ekleyerek Azure Active Directory kullanıcı gruplarını yönetme gösterir.

> Bu kılavuzdaki adımları tamamlamak için önce bir uygulama oluşturmak Azure Active Directory olması gerekir.
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl
Kullanmaya başlamak için tıklayın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Bu sizi API Management yayımcı portalına götürür.

![Yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

Tıklatın **güvenlik** gelen **API Management** menüsünü tıklatın ve sol **dış kimlikler**.

![Dış kimlikler][api-management-security-external-identities]

Tıklatın **Azure Active Directory**. Not **tekrar yönlendirme URL'sini** ve klasik Azure portalı, Azure Active Directory'de geçebilir.

![Dış kimlikler][api-management-security-aad-new]

Tıklatın **Ekle** yeni bir Azure Active Directory uygulaması oluşturmak için düğmesine tıklayın ve seçin **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**.

![Yeni bir Azure Active Directory uygulaması ekleyin][api-management-new-aad-application-menu]

Select uygulama için bir ad girin **Web uygulaması ve/veya Web API**ve İleri düğmesine tıklayın.

![Yeni Azure Active Directory uygulaması][api-management-new-aad-application-1]

İçin **oturum açma URL'si**, Geliştirici Portalı oturum açma URL'sini girin. Bu örnekte, **oturum açma URL'si** olan `https://aad03.portal.current.int-azure-api.net/signin`. 

İçin **uygulama kimliği URL'si**, varsayılan etki alanı ya da özel bir etki alanı için Azure Active Directory girin ve benzersiz bir dize ekler. Bu örnekte, varsayılan etki alanı **https://contoso5api.onmicrosoft.com** sonek ile kullanılır **/api** belirtilen.

![Yeni Azure Active Directory uygulama özellikleri][api-management-new-aad-application-2]

Kaydetme ve uygulama oluşturma ve geçmek için onay düğmesini **yapılandırma** yeni uygulama yapılandırma sekmesi.

![Oluşturulan yeni Azure Active Directory uygulaması][api-management-new-aad-app-created]

Birden çok Azure Active dizinleri, bu uygulama için kullanılacak kullanacaksanız, tıklatın **Evet** için **uygulamasıdır çok kiracılı**. Varsayılan değer **Hayır**.

![Çok kiracılı uygulama][api-management-aad-app-multi-tenant]

Kopya **tekrar yönlendirme URL'sini** gelen **Azure Active Directory** bölümünü **dış kimlikler** sekmesinde yayımcı portalında ve yapıştırın **yanıt URL'si** metin kutusu. 

![Yanıt URL'si][api-management-aad-reply-url]

Yapılandırma sekmesinde sonuna kaydırın **uygulama izinleri** aşağı açılır ve denetleme **dizin verilerini okuma**.

![Uygulama İzinleri][api-management-aad-app-permissions]

Seçin **temsilci izinleri** aşağı açılır ve denetleme **oturum açmayı etkinleştir ve kullanıcıların profilleri okuma**.

![Temsilcili İzinler][api-management-aad-delegated-permissions]

> Uygulama ve temsilci izinleri hakkında daha fazla bilgi için bkz: [grafik API'sine erişim][Accessing the Graph API].
> 
> 

Kopya **istemci kimliği** panoya.

![İstemci Kimliği][api-management-aad-app-client-id]

Geçiş yayımcı portalına dönün ve yapıştırın **istemci kimliği** Azure Active Directory Uygulama yapılandırmasından kopyalanır.

![İstemci Kimliği][api-management-client-id]

Azure Active Directory yapılandırmasına geçin ve tıklatın **seçin süresi** açılan **anahtarları** bölüm ve bir aralık belirtin. Bu örnekte, **1 yıl** kullanılır.

![Anahtar][api-management-aad-key-before-save]

Tıklatın **kaydetmek** yapılandırmayı kaydedin ve anahtarı görüntülemek için. Anahtarı panoya kopyalayın.

> Bu anahtarı not edin. Azure Active Directory yapılandırması penceresini kapattığınızda, anahtarı yeniden görüntülenemiyor.
> 
> 

![Anahtar][api-management-aad-key-after-save]

Geçiş yayımcı portalına dönün ve içine anahtarını yapıştırın **gizli** metin kutusu.

![İstemci Gizli Anahtarı][api-management-client-secret]

**Kiracılar izin** hangi dizinleri API Management hizmet örneği API erişimi belirtir. Azure Active Directory örnekleri için erişim vermek istediğiniz etki alanlarını belirtin. Birden çok etki alanı, satır başı, boşluk veya virgülle ayırabilirsiniz.

![İzin verilen kiracılar][api-management-client-allowed-tenants]


İstenen yapılandırma belirlendikten sonra tıklatın **kaydetmek**.

![Kaydet][api-management-client-allowed-tenants-save]

Değişiklikler kaydedildikten sonra kullanıcılar belirtilen Azure Active Directory Geliştirici Portalı'ndaki adımları izleyerek oturum [bir Azure Active Directory hesabı kullanarak Geliştirici Portalı oturum açma][Log in to the Developer portal using an Azure Active Directory account].

Birden çok etki alanı içinde belirtilen **izin kiracılar** bölümü. Herhangi bir kullanıcı uygulama kaydedildiği özgün etki alanı farklı bir etki alanından oturum önce farklı etki alanının genel yönetici uygulama dizini verilere erişmek için izni vermesi gerekir. İzin vermek için genel yönetici için tamamlamalıdır `https://<URL of your developer portal>/aadadminconsent` (örneğin, https://contoso.portal.azure-api.net/aadadminconsent) erişimi verin ve Gönder'i istedikleri Active Directory Kiracı etki alanı adı yazın. Aşağıdaki örnekte, bir genel yönetici `miaoaad.onmicrosoft.com` bu belirli Geliştirici Portalı izni vermek çalışıyor. 

![İzinler][api-management-aad-consent]

Sonraki ekranda, genel yönetici izni vermiş onaylamanız istenir. 

![İzinler][api-management-permissions-form]

> Genel olmayan bir yönetici bir genel yönetici tarafından izin verilmeden önce oturum açmak çalışırsa, oturum açma denemesi başarısız olur ve hata ekranı görüntülenir.
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Bir dış Azure Active Directory grubu ekleme
Kullanıcılar Azure Active Directory erişimi etkinleştirdikten sonra API yönetime daha kolay grubundaki geliştiriciler ilişkilendirme istenen ürünleri ile yönetmek için Azure Active Directory grupları ekleyebilirsiniz.

> Dış bir Azure Active Directory grubunu yapılandırmak için Azure Active Directory ilk kimlikleri sekmesinde önceki bölümdeki yordamı izleyerek yapılandırılması gerekir. 
> 
> 

Dış Azure Active Directory grupları eklendi **görünürlük** grubuna erişim vermek istediğiniz ürün sekmesinde. Tıklatın **ürünleri**ve ardından istenen ürün adını tıklatın.

![Ürünü yapılandırma][api-management-configure-product]

Geçiş **görünürlük** sekmesine ve tıklayın **Azure Active Directory grupları Ekle**.

![Grupları Ekle][api-management-add-groups]

Seçin **Azure Active Directory Kiracı** gelen aşağı açılan listesinde ve ardından istediğiniz grubun adını yazın **grupları** metin kutusu eklenecek.

![Grup seçin][api-management-select-group]

Bu grup adı bulunabilir **grupları** aşağıdaki örnekte gösterildiği gibi Azure Active Directory için liste.

![Azure Active Directory grupları listesi][api-management-aad-groups-list]

Tıklatın **Ekle** grup adını doğrulamak ve grubunu ekleyin. Bu örnekte, **Contoso 5 geliştiriciler** dış grubuna eklenir. 

![Eklenen grubu][api-management-aad-group-added]

Tıklatın **kaydetmek** yeni Grup Seçimi kaydetmek için.

Bir Azure Active Directory grubu bir üründen yapılandırıldıktan sonra üzerinde denetlenecek kullanılabilir **görünürlük** API Management hizmet örneğindeki diğer ürünleri sekmesi.

Gözden geçirmek ve bunlar eklendikten sonra dış grupları özelliklerini yapılandırmak için grubun adına tıklayın **grupları** sekmesi.

![Grupları yönetme][api-management-groups]

Buradan düzenleyebilirsiniz **adı** ve **açıklama** grubunun.

![Grubu düzenle][api-management-edit-group]

Yapılandırılmış Azure Active Directory'den kullanıcıları görebilir ve Geliştirici Portalı oturum açın ve görünürlük aşağıdaki bölümünde yer alan yönergeleri izleyerek sahip oldukları herhangi bir grup için abone olabilirsiniz.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Bir Azure Active Directory hesabı kullanarak Geliştirici portalında oturum açmak nasıl
Geliştirici portalında önceki bölümlerde yapılandırılmış bir Azure Active Directory hesabı kullanarak oturum açın, kullanarak yeni bir tarayıcı penceresi açık **oturum açma URL'si** Active Directory Uygulama Yapılandırması ve tıklatın **Azure Active Directory**.

![Geliştirici Portalı][api-management-dev-portal-signin]

Azure Active Directory'de bir kullanıcı kimlik bilgilerini girin ve tıklayın **oturum**.

![Oturum aç][api-management-aad-signin]

Ek bilgileri gerekiyorsa, Kayıt formuyla istenebilir. Kayıt formunu tamamladıktan ve tıklatın **kaydolun**.

![Kayıt][api-management-complete-registration]

Kullanıcı artık, API Management hizmet örneğinizin Geliştirici Portalı içine kaydedilir.

![Kayıt Tamamlandı][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
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
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
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

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

