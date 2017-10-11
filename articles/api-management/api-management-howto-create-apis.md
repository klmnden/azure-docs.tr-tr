---
title: "Azure API Management'te API oluşturma"
description: "Oluşturma ve Azure API Management'te API'leri yapılandırmak hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ab08256fbc3caca05bf23a12016ad2acf4fc7412
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a>Azure API Management'te API oluşturma
API Management bir API İstemci uygulamaları tarafından çağrılabilen işlemler kümesini temsil eder. Yeni API'leri yayımcı portalında oluşturulur ve ardından istenen işlemleri eklenir. Operations eklendikten sonra API bir ürüne eklenebilir ve yayımlanabilir. Bir API yayımlandığında abone ve geliştiriciler tarafından kullanılır.

Bu kılavuz, işlemde ilk adımı gösterir: oluşturma ve yeni bir API API Management içinde yapılandırın. Operations ekleme ve bir ürün yayımlama hakkında daha fazla bilgi için bkz: [API'ye işlem ekleme] [ How to add operations to an API] ve [nasıl oluşturulacağı ve bir ürün yayımlama][How to create and publish a product].

## <a name="create-new-api"></a>Yeni bir API oluşturma
API oluşturulur ve yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda.

![Yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

Tıklatın **API'leri** gelen **API Management** sol menüsünde ve ardından **API ekleme**.

![API oluşturma][api-management-create-api]

Kullanım **yeni API ekleme** yeni API yapılandırmak için penceresi.

![Yeni API ekle][api-management-add-new-api]

Aşağıdaki alanları yeni API yapılandırmak için kullanılır.

* **Web API adı** API için benzersiz ve açıklayıcı bir ad sağlar. Geliştirici ve yayımcı portallarında görüntülenir.
* **Web hizmeti URL'si** API uygulama HTTP hizmeti başvuruyor. API management bu adrese isteklerini iletir.
* **Web API'si URL soneki** API management hizmeti temel URL'si eklenir. Temel URL bir API Management hizmet örneği tarafından barındırılan tüm API'leri yaygındır. API Management API'leri kendi soneki ayırır ve bu nedenle soneki her API için belirtilen yayımcı için benzersiz olmalıdır.
* **Web API'si URL şeması** API erişmek için hangi protokollerin kullanılabileceğini belirler. HTTPs varsayılan olarak belirtilir.
* İsteğe bağlı olarak bu yeni API ürün eklemek için tıklatın **ürünler (isteğe bağlı)** açılır ve bir ürün seçin. Bu adım, birden fazla ürün için API eklemek için birden çok kez yinelenebilir.

İstenen değerleri yapılandırıldıktan sonra tıklatın **kaydetmek**. Yeni API oluşturulduktan sonra API için Özet sayfasında yayımcı portalında görüntülenir.

![API özeti][api-management-api-summary]

## <a name="configure-api-settings"></a>API yapılandırma ayarları
Kullanabileceğiniz **ayarları** doğrulayın ve API yapılandırmasını düzenlemek için sekmesini. **Web API adı**, **Web hizmeti URL'si**, ve **Web API'si URL soneki** API oluşturulduğunda ve değiştirilebilir başlangıçta burada ayarlanır. **Açıklama** isteğe bağlı bir açıklama sağlar ve **Web API'si URL şeması** API erişmek için hangi protokollerin kullanılabileceğini belirler.

![API ayarları][api-management-api-settings]

API uygulama arka uç hizmetine ağ geçidi kimlik doğrulamasını yapılandırmak için seçin **güvenlik** sekmesi. **Kimlik bilgileriyle** açılan yapılandırmak için kullanılabilir **HTTP temel** veya **istemci sertifikalarını** kimlik doğrulaması. HTTP temel kimlik doğrulaması kullanmak için istenen kimlik bilgilerini girmeniz yeterlidir. İstemci sertifikası kimlik doğrulamasını kullanma hakkında daha fazla bilgi için bkz: [arka uç hizmetlerini kullanan istemci sertifikası kimlik doğrulaması Azure API Management'te güvenliğini sağlamak nasıl][How to secure back-end services using client certificate authentication in Azure API Management].

**Güvenlik** sekmesinde de kullanılabilir yapılandırmak için **kullanıcı yetkilendirme** OAuth 2.0 kullanan. Daha fazla bilgi için bkz: [Azure API Management'te OAuth 2.0 kullanan Geliştirici hesaplarını yetkilendirmede nasıl][How to authorize developer accounts using OAuth 2.0 in Azure API Management].

![Temel kimlik doğrulama ayarları][api-management-api-settings-credentials]

Tıklatın **kaydetmek** API ayarları yaptığınız değişiklikleri kaydetmek için.

## <a name="next-steps"> </a>Sonraki adımlar
Bir API oluşturup yapılandırılan ayarları, sonraki adımlar için API işlemleri eklemek üzeresiniz sonra bir ürüne API ekleme ve böylece geliştiriciler için kullanılabilir yayımlayın. Daha fazla bilgi için aşağıdaki makalelere bakın.

* [API'ye işlem ekleme][How to add operations to an API]
* [Oluşturma ve bir ürün yayımlama][How to create and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
