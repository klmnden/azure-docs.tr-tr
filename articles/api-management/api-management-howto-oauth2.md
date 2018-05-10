---
title: Azure API Management'te OAuth 2.0 kullanan Geliştirici hesaplarını yetkilendirmede | Microsoft Docs
description: API Management'te OAuth 2.0 kullanan kullanıcıların nasıl yetkilendirileceğini öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: f3611fa4da571dd74d844c7fad45788ece372be4
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Azure API Management'te OAuth 2.0 kullanan Geliştirici hesaplarını yetkilendirmede nasıl
Birçok API'lerini destekleyen [OAuth 2.0](http://oauth.net/2/) API güvenliğini sağlama ve yalnızca geçerli kullanıcıların erişimi vardır ve bunlar yalnızca bunlar yetkili kaynaklara erişebilir emin olun. Azure API Management'ın etkileşimli Geliştirici konsolu gibi API'leri ile kullanmak için hizmet, hizmet örneği, OAuth 2.0 etkin API çalışmak için yapılandırmanıza olanak sağlar.

## <a name="prerequisites"> </a>Önkoşulları
Bu kılavuz, API Management hizmeti örneği OAuth 2.0 yetkilendirme için geliştirici hesaplarını kullanacak şekilde yapılandırmak nasıl gösterir, ancak bir OAuth 2.0 sağlayıcı yapılandırma göstermez. Her bir OAuth 2.0 sağlayıcı için yapılandırma adımları benzerdir ve gerekli API Management hizmet örneği içinde OAuth 2.0 yapılandırmada kullanılan bilgi aynı parçalarıdır rağmen farklıdır. Bu konuda bir OAuth 2.0 sağlayıcısı olarak Azure Active Directory kullanan örnekler gösterilmektedir.

> [!NOTE]
> Azure Active Directory'yi kullanarak OAuth 2.0 yapılandırma hakkında daha fazla bilgi için bkz: [WebApp GraphAPI DotNet] [ WebApp-GraphAPI-DotNet] örnek.
> 
> 

## <a name="step1"> </a>Bir OAuth 2.0 yetkilendirme Sunucusu API yönetimini yapılandırma
Kullanmaya başlamak için API Management hizmetiniz için Azure Portal'da **Yayımcı portalı**’na tıklayın.

![Yayımcı portalı][api-management-management-console]

> [!NOTE]
> Henüz bir API Management hizmeti örneği oluşturmadıysanız, bkz: [bir API Management hizmet örneği oluşturma][Create an API Management service instance].
> 
> 

Tıklatın **güvenlik** gelen **API Management** sol, tıklatma menüsünden **OAuth 2.0**ve ardından **yetkilendirme Sunucusu Ekle**.

![OAuth 2.0][api-management-oauth2]

' I tıklattıktan sonra **yetkilendirme Sunucusu Ekle**, yeni yetkilendirme sunucusu form görüntülenir.

![Yeni sunucu][api-management-oauth2-server-1]

Bir ad ve isteğe bağlı bir açıklama girin **adı** ve **açıklama** alanları. 

> [!NOTE]
> Değerlerine OAuth 2.0 sunucudan gelen ve bu alanlar geçerli API Management hizmet örneği içinde OAuth 2.0 yetkilendirme sunucusu tanımlamak için kullanılır.
> 
> 

Girin **istemci kayıt sayfası URL'si**. Bu sayfa, kullanıcıların oluşturmak ve kendi hesaplarını yönetme içindir ve kullanılan OAuth 2.0 sağlayıcıya bağlı olarak değişir. **İstemci kayıt sayfası URL'si** kullanıcılar oluşturmak ve kullanıcı hesaplarının yönetimini desteklemek için OAuth 2.0 sağlayıcıları kendi hesaplarını yapılandırmak için kullanabileceğiniz sayfa işaret eder. Bazı kuruluşlar yapılandırmazsanız veya OAuth 2.0 sağlayıcının desteklediği olsa bile bu işlevi kullanın. OAuth 2.0 sağlayıcınız yapılandırılmış hesaplarının kullanıcı yönetimini yoksa, şirketinizin URL veya URL gibi burada yer tutucu URL gibi girin `https://placeholder.contoso.com`.

Formun sonraki bölümü içerir **yetki kodu izin türleri**, **yetkilendirme uç noktası URL'si**, ve **yetkilendirme isteği yöntemi** ayarlar.

![Yeni sunucu][api-management-oauth2-server-2]

Belirtin **yetki kodu izin türleri** istenen türleri denetleyerek. **Yetkilendirme kodu** varsayılan olarak belirtilir.

Girin **yetkilendirme uç noktası URL'si**. Azure Active Directory'de, bu URL aşağıdaki URL için benzer olacaktır nerede `<client_id>` uygulamanız OAuth 2.0 sunucuya tanımlayan istemci kimliği ile değiştirilir.

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

**Yetkilendirme isteği yöntemi** yetkilendirme isteği OAuth 2.0 sunucunun nasıl gönderileceğini belirtir. Varsayılan olarak **almak** seçilir.

Sonraki bölümde yerdir **belirteç uç nokta URL'si**, **istemci kimlik doğrulama yöntemleri**, **yöntemi gönderme erişim belirteci**, ve **varsayılan kapsamı** belirtilir.

![Yeni sunucu][api-management-oauth2-server-3]

Bir Azure Active Directory OAuth 2.0 sunucusu için **belirteç uç nokta URL'si** aşağıdaki biçime sahip nerede `<APPID>` biçimi olan `yourapp.onmicrosoft.com`.

`https://login.microsoftonline.com/<APPID>/oauth2/token`

İçin varsayılan ayar **istemci kimlik doğrulama yöntemleri** olan **temel**, ve **yöntemi gönderme erişim belirteci** olan **Authorization Üstbilgisi**. Bu değerleri ile birlikte bu form bölüm üzerinde yapılandırılmış **varsayılan kapsamı**.

**İstemci kimlik bilgileri** bölüm içerir **istemci kimliği** ve **gizli**, OAuth 2.0 sunucunuzu oluşturma ve yapılandırma işlemi sırasında elde hangi. Bir kez **istemci kimliği** ve **gizli** belirtilir, **redirect_uri** için **yetkilendirme kodu** oluşturulur. Bu URI, OAuth 2.0 sunucusu yapılandırmanızda yanıt URL'si yapılandırmak için kullanılır.

![Yeni sunucu][api-management-oauth2-server-4]

Varsa **yetki kodu izin türleri** ayarlanır **kaynak sahibi parolası**, **kaynak sahibi parolası kimlik bilgileri** bölümü, bu kimlik bilgilerini belirtmek için kullanılır; Aksi halde boş bırakılabilir.

![Yeni sunucu][api-management-oauth2-server-5]

Formun tamamlandıktan sonra tıklatın **kaydetmek** API Management OAuth 2.0 yetkilendirme sunucusu yapılandırmasını kaydetmek için. Sunucu yapılandırmasını kaydedildikten sonra sonraki bölümde gösterildiği gibi bu yapılandırmayı kullanmak için API'ler yapılandırabilirsiniz.

## <a name="step2"> </a>Bir API OAuth 2.0 kullanıcı kimlik doğrulaması kullanacak şekilde yapılandırma
Tıklatın **API'leri** gelen **API Management** sol, tıklatma menüsünden istenen API adına tıklayın **güvenlik**ve ardından onay kutusunu için **OAuth 2.0**.

![Kullanıcı kimlik doğrulaması][api-management-user-authorization]

İstenen seçin **yetkilendirme sunucusu** aşağı açılan liste ve tıklatın **kaydetmek**.

![Kullanıcı kimlik doğrulaması][api-management-user-authorization-save]

## <a name="step3"> </a>OAuth 2.0 kullanıcı yetkilendirme Geliştirici Portalı'nda test etme
OAuth 2.0 yetkilendirme sunucunuz yapılandırılmış ve API'nizi sunucu kullanacak şekilde yapılandırılmış sonra Geliştirici portalına giderek ve bir API çağırma sınayabilirsiniz.  Sağ üstteki menüde **Geliştirici Portalı**’na tıklayın.

![Geliştirici portalı][api-management-developer-portal-menu]

Tıklatın **API'leri** üst menü seçip **Echo API'si**.

![Echo API’si][api-management-apis-echo-api]

> [!NOTE]
> Yapılandırılmış ya da hesabınıza görünen yalnızca bir API’niz varsa, API’lere tıklamak sizi doğrudan bu API’nin işlemlerine götürür.
> 
> 

Seçin **GET kaynağı** işlemi,'ı tıklatın **Konsolu'nu Aç**ve ardından **yetkilendirme kodu** gelen açılır.

![Konsolu açma][api-management-open-console]

Zaman **yetkilendirme kodu** olduğunu belirlenirse, bir açılır pencere ile OAuth 2.0 sağlayıcısının oturum açma formu görüntülenir. Bu örnekte oturum açma formu Azure Active Directory tarafından sağlanmıştır.

> [!NOTE]
> Devre dışı açılır pencereleri varsa tarayıcı tarafından etkinleştirmeniz istenir. Bunları etkinleştirdikten sonra Seç **yetkilendirme kodu** yeniden ve oturum açma formu görüntülenir.
> 
> 

![Oturum aç][api-management-oauth2-signin]

Olarak oturum açtıktan sonra **istek üst** ile doldurulan bir `Authorization : Bearer` isteği yetkilendirir üstbilgi.

![İstek üstbilgisi belirteci][api-management-request-header-token]

Bu noktada kalan parametreler için istenen değerleri yapılandırın ve isteği gönderin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki video ve eşlik eden OAuth 2.0 ve API Management kullanma hakkında daha fazla bilgi için bkz: [makale](api-management-howto-protect-backend-with-aad.md).


[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


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

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

