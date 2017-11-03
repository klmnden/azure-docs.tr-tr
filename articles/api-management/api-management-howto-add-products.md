---
title: "Oluşturma ve Azure API Management'te bir ürün yayımlama"
description: "Oluşturma ve Azure API Management'te ürünleri yayımlama öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 2cf905967f26de97613ff7d5fc495c9223e11390
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Oluşturma ve Azure API Management'te bir ürün yayımlama
Azure API Management'te bir ürün kullanım kotası ve kullanım koşullarını yanı sıra bir veya daha fazla API'leri içerir. Bir ürün yayımlandığında, geliştiriciler ürüne abone ve ürün API'lerine kullanmaya başlar. Konu Ürün oluşturma, bir API ekleme ve geliştiriciler için yayımlama için bir kılavuz sağlar.

## <a name="create-product"></a>Ürün oluşturma
Operations eklenir ve bir API yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda.

![Yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

Tıklayın **ürünleri** görüntülemek için soldaki menüde **ürünleri** sayfasında ve tıklayın **Ürün Ekle**.

![Ürünler][api-management-products]

![Yeni ürün][api-management-add-new-product]

Ürün için açıklayıcı bir ad girin **adı** alan ve ürün içinde açıklamasını **açıklama** alan.

API Management ürünleri olabilir **açık** veya **korumalı**. Korumalı ürünleri kullanabilmek için bunlara abone olmak gerekir, açık ürünler abonelik olmadan kullanılabilir. Denetleme **abonelik iste** bir abonelik gerektiren korumalı ürün oluşturmak için. Bu varsayılan ayardır.

Denetleme **abonelik onayı iste** gözden geçirin ve kabul edin ya da bu ürünün abonelik girişimleri reddetmek için yönetici istiyorsanız. Kutunun işaretli değilse abonelik girişimleri otomatik onaylı olacaktır. Abonelikler hakkında daha fazla bilgi için bkz: [bir ürüne abone olanları görüntüleme][View subscribers to a product].

Birden çok kez ürüne abone olmak Geliştirici hesaplarının izin verecek şekilde denetleyin **birden çok abonelik izin** onay kutusu. Her geliştirici hesabını, bu kutu işaretli değilse, yalnızca tek bir kez ürüne abone olabilirsiniz.

![Sınırsız birden çok abonelik][api-management-unlimited-multiple-subscriptions]

Birden çok aboneliğe sayısını sınırlamak için denetleme **aynı anda abonelik sayısını sınırlandır** onay kutusunu işaretleyin ve abonelik sınırı girin. Aşağıdaki örnekte, dört geliştirici her hesap için aynı anda abonelik sınırlıdır.

![Dört birden çok abonelik][api-management-four-multiple-subscriptions]

Tüm yeni ürün seçenekleri yapılandırıldıktan sonra tıklatın **kaydetmek** yeni ürün oluşturmak için.

![Ürünler][api-management-products-page]

> Varsayılan olarak yeni ürünler yayımdan ve yalnızca görünür **Yöneticiler** grubu.
> 
> 

Bir ürün yapılandırmak için ürün adı tıklayın **ürünleri** sekmesi.

## <a name="add-apis"></a>Bir ürüne API ekleme
**Ürünleri** sayfa yapılandırması için dört bağlantılarını içerir: **Özet**, **ayarları**, **görünürlük**, ve  **Aboneler**. **Özet** sekme, burada API'leri ekleyebilir ve yayımlama veya yayımdan bir ürün kullanılabilir.

![Özet][api-management-new-product-summary]

Ürünü yayımlamadan önce bir veya daha fazla API'leri eklemeniz gerekir. Bunu yapmak için tıklatın **ürüne API Ekle**.

![API ekleme][api-management-add-apis-to-product]

İstenen API'leri tıklatıp **kaydetmek**.

## <a name="add-description"></a>Açıklayıcı bilgiler için ürün ekleme
**Ayarları** sekme amacı, erişim sağladığı API'ları ve diğer yararlı bilgiler gibi ürün hakkında ayrıntılı bilgi sağlamak sağlar. İçerik, API çağırmayı ve düz metin veya HTML biçimlendirmesi yazılabilir geliştiriciler hedefleyen.

![Ürün ayarları][api-management-product-settings]

Denetleme **abonelik iste** abonelik olmadan çağrılabilir açık bir ürün oluşturmak için onay kutusunun işaretini kaldırın veya kullanılmak üzere bir abonelik gerektiren korumalı ürün oluşturmak için.

Seçin **abonelik onayı iste** el ile tüm ürün abonelik isteklerini onaylama istiyorsanız. Varsayılan olarak tüm ürün abonelikler otomatik olarak verilir.

Birden çok kez ürüne abone olmak Geliştirici hesaplarının izin verecek şekilde denetleyin **birden çok abonelik izin** onay kutusunu işaretleyin ve isteğe bağlı olarak bir sınırı belirtin. Her geliştirici hesabını, bu kutu işaretli değilse, yalnızca tek bir kez ürüne abone olabilirsiniz.

İsteğe bağlı olarak doldurmak **kullanım koşulları** hangi aboneleri ürünü kullanmak için kabul etmelisiniz ürün için kullanım koşullarını açıklayan alan.

## <a name="publish-product"></a>Bir ürün yayımlama
Bir ürün API'lerinde çağrılabilir önce ürünün yayımlanması gerekir. Üzerinde **Özet** ürün için sekmesini tıklatın, **Yayımla**ve ardından **Evet, Yayımla** onaylamak için. Daha önce yayımlanmış bir ürüne özel yapmak için tıklatın **yayımdan**.

![Ürünü yayımlama][api-management-publish-product]

## <a name="make-visible"></a>Bir ürün geliştiriciler tarafından görülebilir kılma
**Görünürlük** sekme hangi rollerin Geliştirici portalında ürün görebilecek ve ürüne abone seçmenize olanak sağlar.

![Ürün görünürlüğü][api-management-product-visiblity]

Etkinleştirmek veya bir grup geliştiriciler için bir ürün görünürlüğünü devre dışı bırakmak için denetleyin veya grubun yanındaki onay kutusunun işaretini kaldırın ve ardından **kaydetmek**.

> Daha fazla bilgi için bkz: [oluşturma ve Azure API Management'ta Geliştirici hesaplarını yönetmek için grupları kullanma][How to create and use groups to manage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Bir ürüne abone olanları görüntüleme
**Aboneleri** sekmesi ürüne abone olan geliştiricilere listeler. Her geliştirici ayarlarını ve Ayrıntılar Geliştirici adına tıklayarak görüntülenebilir. Bu örnekte hiçbir geliştiriciler henüz ürüne abone oldunuz.

![Geliştiriciler][api-management-developer-list]

## <a name="next-steps"> </a>Sonraki adımlar
İstenen API'ler eklendiğine ve ürün yayımlanan sonra geliştiriciler ürüne abone ve API'leri çağırmak başlayın. Bu öğeleri yanı sıra Gelişmiş Ürün yapılandırma gösteren bir öğretici için bkz: [oluşturma ve Gelişmiş Ürün ayarları Azure API Management'te yapılandırma][How create and configure advanced product settings in Azure API Management].

Aşağıdaki videoda ürünleri ile çalışma hakkında daha fazla bilgi için bkz.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
