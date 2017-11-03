---
title: Bir API Azure API Management alma | Microsoft Docs
description: "Bir API ve işlemlerini Azure API Management içeri aktarmayı öğrenin."
services: api-management
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 40398b0a-ac2c-43f0-89e1-07e4abbf502f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: cc56c9a91979ec2ec06b2d63a22b2ded68c0dce1
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Azure API Management işlemleri olan bir API tanımını içeri aktarma
API Management, yeni API'leri oluşturulabilir ve el ile eklenmiş operations veya API işlemlerini tek bir adımda birlikte içeri aktarılabilir.

API'ler ve bunların işlemler aşağıdaki biçimlerden kullanılarak alınabilir.

* WADL
* Swagger

Bu kılavuz gösterir yeni bir API oluşturma ve işlemlerini tek bir adımda içeri aktarın. El ile bir API oluşturma ve işlemleri ekleme hakkında daha fazla bilgi için bkz: [API oluşturma] [ How to create APIs] ve [API'ye işlem ekleme][How to add operations to an API].

## <a name="import-api"> </a>Bir API'yi içeri aktarma
API oluşturulur ve yayımcı portalında yapılandırılır. Yayımcı portalına erişmek için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.

![Yayımcı portalı][api-management-management-console]

Tıklatın **API'leri** gelen **API Management** sol menüsünde ve ardından **API'yi içeri aktarma**.

![İçeri aktarma API'si][api-management-import-apis]

**İçeri aktarma API'si** penceresinde API belirtimine sağlamak için üç yol karşılık gelen üç sekme bulunur.

* **Pano'dan** API belirtimine belirlenen metin kutusuna yapıştırın olanak tanır.
* **Dosyadan** göz atın ve API belirtimine içeren dosyayı seçin olanak tanır.
* **URL'den** API belirtimine URL sağlamanız olanak tanır.

![API'yi İçeri Aktar biçimi][api-management-import-api-clipboard]

API belirtimine sağladıktan sonra radyo düğmelerinin sağ tarafta belirtimi biçimini belirtmek için kullanın. Aşağıdaki biçimleri desteklenir.

* WADL
* Swagger

Ardından, girin bir **Web API'si URL soneki**. Bu API management hizmetiniz için temel URL eklenir. Temel URL her bir API Management hizmet örneği üzerinde barındırılan tüm API'leri yaygındır. API Management API'leri kendi soneki ayırır ve bu nedenle soneki belirli bir API management hizmet örneğindeki her API için benzersiz olmalıdır.

Tüm değerleri girdikten sonra tıklayın **kaydetmek** API ve ilişkili operations oluşturmak için. 

> [!NOTE]
> Swagger biçiminde temel hesaplayıcı API'sini içeri aktarma bir öğretici için bkz: [ilk API'nizi Azure API Management'te yönetme](api-management-get-started.md).
> 
> 

## <a name="export-api"></a> Bir API dışarı aktarma
Yeni API'ları alma yanı sıra Apı'lerinizi tanımlarını yayımcı Portalı'ndan dışarı aktarabilirsiniz. Bunu yapmak için tıklatın **verme API** gelen **Özet sekmesi** biri, **API**.

![API dışarı aktarma][api-management-export-api]

API WADL veya Swagger kullanılarak verilebilir. İstenen biçim seçin, **kaydetmek**ve dosyanın kaydedileceği konumu seçin.

![Dışa aktarma API biçimi][api-management-export-api-format]

## <a name="next-steps"> </a>Sonraki adımlar
Bir API oluşturulur ve içe işlemleri, gözden geçirin ve yapılandırabilirsiniz sonra herhangi bir ek ayarı bir ürüne API ekleme ve böylece geliştiriciler için kullanılabilir yayımlayın. Daha fazla bilgi için aşağıdaki kılavuzlara bakın.

* [API ayarlarını yapılandırma][How to configure API settings]
* [Oluşturma ve bir ürün yayımlama][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create APIs]: api-management-howto-create-apis.md
[How to configure API settings]: api-management-howto-create-apis.md#configure-api-settings
