---
title: OAuth 2.0 kullanarak Azure API Management'ta Geliştirici hesaplarını yetkilendirme | Microsoft Docs
description: API Yönetimi'nde OAuth 2.0 kullanarak kullanıcılara nasıl yetki vereceğiniz öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.author: apimpm
ms.openlocfilehash: b195271edeea6cd5ea527454ad1615ac85a32138
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60658725"
---
# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>OAuth 2.0 kullanarak Azure API Management'ta Geliştirici hesaplarını yetkilendirme nasıl

Çok sayıda API desteği [OAuth 2.0](https://oauth.net/2/) API güvenliğini sağlama ve yalnızca geçerli kullanıcı erişimi vardır ve yalnızca bunlar başlıklı kaynaklarına erişebilmek emin olun. Böyle API'leri ile Azure API Management'ın etkileşimli Geliştirici konsolu kullanmak için hizmet ile OAuth 2.0 etkin API çalışması için hizmet örneği yapılandırmanızı sağlar.

## <a name="prerequisites"> </a>Önkoşulları

Bu kılavuz, OAuth 2.0 yetkilendirme için geliştirici hesapları kullanmak için API Management hizmet örneğinizin yapılandırma işlemi gösterilmektedir, ancak bir OAuth 2.0 sağlayıcısını yapılandırmak nasıl algılanacağını göstermez. Adımları da buradakilere benzer ve OAuth 2.0 API Management hizmet örneğinizin yapılandırılırken kullanılan bilgi gerekli parçalarını aynıdır ancak her bir OAuth 2.0 sağlayıcı yapılandırmasını farklıdır. Bu konuda bir OAuth 2.0 sağlayıcısı olarak Azure Active Directory kullanan örnekler gösterilmektedir.

> [!NOTE]
> OAuth 2.0 kullanarak Azure Active Directory yapılandırma hakkında daha fazla bilgi için bkz. [WebApp GraphAPI DotNet] [ WebApp-GraphAPI-DotNet] örnek.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="step1"> </a>API Yönetimi'nde bir OAuth 2.0 yetkilendirme sunucusu yapılandırma

> [!NOTE]
> Henüz bir API Management hizmet örneği oluşturmadıysanız bkz [bir API Management hizmet örneği oluşturma][Create an API Management service instance].

1. Sol taraftaki menüden OAuth 2.0 sekmesine tıklayın ve tıklayarak **+ Ekle**.

    ![OAuth 2.0 menüsü](./media/api-management-howto-oauth2/oauth-01.png)

2. Bir ad ve isteğe bağlı bir açıklama girin **adı** ve **açıklama** alanları.

    > [!NOTE]
    > Bu alanlar, OAuth 2.0 yetkilendirme sunucusu içinde geçerli API Management hizmet örneğini tanımlamak için kullanılır ve OAuth 2.0 sunucudan değerlerine gelmeyen.

3. Girin **istemci kayıt sayfası URL'si**. Bu sayfasıdır burada kullanıcılar oluşturabilir ve kendi hesaplarını yönetebilir ve kullanılan OAuth 2.0 sağlayıcıya bağlı olarak değişir. **İstemci kayıt sayfası URL'si** kullanıcılar oluşturma ve kullanıcı hesaplarının yönetimini desteklemek için OAuth 2.0 sağlayıcıları kendi hesaplarını yapılandırmak için kullanabileceğiniz sayfayı işaret eder. Bazı kuruluşların yapılandırmazsanız veya OAuth 2.0 sağlayıcının desteklediği olsa bile bu işlevi kullanın. OAuth 2.0 sağlayıcınız yapılandırılmış hesap için kullanıcı yönetimi yoksa, şirketinizin URL veya URL gibi burada yer tutucu URL'yi gibi girin `https://placeholder.contoso.com`.

    ![OAuth 2.0 yeni sunucu](./media/api-management-howto-oauth2/oauth-02.png)

4. Sonraki bölümde form içeren **yetkilendirme verme türleri**, **yetkilendirme uç noktası URL'si**, ve **yetkilendirme istek yöntemi** ayarları.

    Belirtin **yetkilendirme verme türleri** denetleyerek istenen türleri. **Yetkilendirme kodu** varsayılan olarak belirtilir.

    Girin **yetkilendirme uç noktası URL'si**. Azure Active Directory için bu URL'yi aşağıdaki URL'ye benzer olacaktır burada `<client_id>` uygulamanız OAuth 2.0 sunucusuna tanıtan istemci kimliği ile değiştirilir.

    `https://login.microsoftonline.com/<client_id>/oauth2/authorize`

    **Yetkilendirme istek yöntemi** yetkilendirme isteği için OAuth 2.0 sunucusu nasıl gönderileceğini belirtir. Varsayılan olarak **alma** seçilir.

5. Ardından, **belirteç uç noktası URL'si**, **istemci kimlik doğrulama yöntemleri**, **erişim belirteci gönderme yöntemi** ve **varsayılan kapsam** olması gerekir Belirtilen.

    ![OAuth 2.0 yeni sunucu](./media/api-management-howto-oauth2/oauth-03.png)

    Bir Azure Active Directory OAuth 2.0 sunucusu **belirteç uç noktası URL'si** aşağıdaki biçimde olan burada `<TenantID>` biçimi olan `yourapp.onmicrosoft.com`.

    `https://login.microsoftonline.com/<TenantID>/oauth2/token`

    Varsayılan ayarı **istemci kimlik doğrulama yöntemleri** olduğu **temel**, ve **erişim belirteci gönderme yöntemi** olduğu **yetkilendirme üst bilgisi**. Bu değerleri ile birlikte bu bölümünde bir form üzerinde yapılandırılan **varsayılan kapsam**.

6. **İstemci kimlik bilgileri** bölüm içeren **istemci kimliği** ve **gizli**, OAuth 2.0 sunucunuzu oluşturma ve yapılandırma işlemi sırasında alınan, . Bir kez **istemci kimliği** ve **gizli** belirtilir, **redirect_uri** için **yetkilendirme kodu** oluşturulur. Bu URI, OAuth 2.0 sunucusu yapılandırmanızda yanıt URL'si yapılandırmak için kullanılır.

    ![OAuth 2.0 yeni sunucu](./media/api-management-howto-oauth2/oauth-04.png)

    Varsa **yetkilendirme verme türleri** ayarlanır **kaynak sahibi parolası**, **kaynak sahibi parola kimlik bilgileri** bölümü, bu kimlik bilgilerini belirtmek için kullanılır; Aksi takdirde boş bırakabilirsiniz.

    Form tamamlandığında tıklayın **Oluştur** API Management OAuth 2.0 yetkilendirme sunucusu yapılandırmasını kaydetmek için. Sunucu yapılandırmasını kaydettikten sonra sonraki bölümde gösterildiği gibi bu yapılandırmayı kullanmak için API'ler yapılandırabilirsiniz.

## <a name="step2"> </a>OAuth 2.0 kullanıcı kimlik doğrulaması kullanmak için API'yi yapılandırma

1. Tıklayın **API'leri** gelen **API Management** sol taraftaki menüden.

    ![OAuth 2.0 API'leri](./media/api-management-howto-oauth2/oauth-05.png)

2. İstenen API ve tıklatın adına **ayarları**. Kaydırma **güvenlik** bölümüne ve ardından kutusunu işaretlemeniz **OAuth 2.0**.

    ![OAuth 2.0 ayarları](./media/api-management-howto-oauth2/oauth-06.png)

3. İstenen seçin **yetkilendirme sunucusu** açılır listede, ve **Kaydet**.

    ![OAuth 2.0 ayarları](./media/api-management-howto-oauth2/oauth-07.png)

## <a name="step3"> </a>OAuth 2.0 kullanıcı kimlik doğrulaması, geliştirici portalında test etme

OAuth 2.0 yetkilendirme sunucunuz yapılandırılmış ve yapılandırılmış sunucu kullanmak için API'nizi sonra Geliştirici Portalı'na giderek ve bir API'yi çağırıp sınayabilirsiniz.  Tıklayın **Geliştirici Portalı** üstteki menüden Azure API Management örneğinizin içinde **genel bakış** sayfası.

![Geliştirici portalı][api-management-developer-portal-menu]

Tıklayın **API'leri** üst menü seçip **Echo API'si**.

![Echo API’si][api-management-apis-echo-api]

> [!NOTE]
> Yapılandırılmış ya da hesabınıza görünen yalnızca bir API’niz varsa, API’lere tıklamak sizi doğrudan bu API’nin işlemlerine götürür.

Seçin **GET kaynağı** işlemi tıklayın **konsolu aç**ve ardından **yetkilendirme kodu** açılır listeden.

![Konsolu açma][api-management-open-console]

Zaman **yetkilendirme kodu** olduğu belirlenirse, bir açılır pencere ile OAuth 2.0 sağlayıcısının oturum açma formu görüntülenir. Bu örnekte oturum açma formu, Azure Active Directory tarafından sağlanır.

> [!NOTE]
> Açılır pencereler devre dışı varsa bunları tarayıcı tarafından etkinleştirmeniz istenir. Bunları etkinleştirdikten sonra Seç **yetkilendirme kodu** yeniden ve oturum açma formu görüntülenir.

![Oturum aç][api-management-oauth2-signin]

Oturum açtıktan sonra **istek üst** ile doldurulmuş bir `Authorization : Bearer` isteği yetkilendirir başlığı.

![İstek üst bilgisi belirteç][api-management-request-header-token]

Bu noktada veya kalan parametreler için istenen değerleri yapılandırın ve isteği gönderin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki video ve eşlik eden OAuth 2.0 ve API Management'ı kullanma hakkında daha fazla bilgi için bkz. [makale](api-management-howto-protect-backend-with-aad.md).

[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
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

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

