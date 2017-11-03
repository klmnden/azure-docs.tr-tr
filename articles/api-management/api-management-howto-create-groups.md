---
title: "Azure API Management'te grupları kullanılarak Geliştirici hesaplarını yönetme | Microsoft Docs"
description: "Geliştirici hesaplarının Azure API Management'te grupları kullanarak yönetmeyi öğrenin"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 00322340d67fd064a0dc9149790e031b94390709
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Oluşturma ve Azure API Management'ta Geliştirici hesaplarını yönetmek için grupları kullanma
API Management’te, ürünlerin geliştiricilere görünürlüğünü yönetmek için gruplar kullanılır. Ürünler ilk gruplar tarafından görünür yapılır ve sonra bu gruplara geliştiriciler görüntülemek ve gruplarıyla ilişkili ürünlere abone. 

API Management aşağıdaki sabit sistem gruplarına sahiptir.

* **Yöneticiler**: Azure aboneliği yöneticileri bu grubun üyesidir. Yöneticiler, geliştiriciler tarafından kullanılan API’leri, işlemleri ve ürünleri oluşturarak API Management hizmet örneklerini yönetir.
* **Geliştiriciler**: Kimliği doğrulanmış geliştirici portalı kullanıcıları bu gruba girer. Geliştiriciler, API'lerinizi kullanarak uygulama oluşturan müşterilerdir. Geliştiriciler, geliştirici portalına erişim iznine sahiptir ve bir API’nin işlemlerini çağıran uygulamalar oluşturur.
* **Konuklar**: Kimliği doğrulanmamış geliştirici portalı kullanıcıları (bir API Management örneğinin geliştirici portalını ziyaret eden olası müşteriler gibi) bu gruba girer. Bunlara API’leri görüntüleyebilme ancak çağıramama gibi bazı salt okunur erişimler verilebilir.

Bu sistem gruplarına ek olarak, yöneticiler özel gruplar oluşturabilir veya [ilişkili Azure Active Directory kiracılarındaki dış grupları yararlanan][leverage external groups in associated Azure Active Directory tenants]. Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir. Örneğin, belirli bir iş ortağı kuruluşla ilişkili geliştiriciler için özel bir grup oluşturabilir ve bunlara yalnızca ilgili API'leri içeren bir üründen API'lere erişim izni verebilirsiniz. Bir kullanıcı birden fazla grubun üyesi olabilir.

Bu kılavuz, nasıl bir API Management örneğinin yöneticileri yeni gruplar eklemek ve ürünleri ve geliştiricilerin ilişkilendirmek gösterir.

> [!NOTE]
> Grup oluşturma ve yayımcı portalında yönetme ek olarak, oluşturmak ve gruplarınızı API Management REST API kullanarak yönetmek [grup](https://msdn.microsoft.com/library/azure/dn776329.aspx) varlık.
> 
> 

## <a name="create-group"></a>Bir grup oluşturun
Yeni bir grup oluşturmak için tıklatın **yayımcı portalına** API Management hizmetiniz için Azure Portalı'nda. Bu sizi API Management yayımcı portalına götürür.

![Yayımcı portalı][api-management-management-console]

> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

Tıklatın **grupları** gelen **API Management** sol menüsünde ve ardından **Grup Ekle**.

![Yeni Grup Ekle][api-management-add-group]

Grup ve isteğe bağlı bir açıklama için benzersiz bir ad girin ve tıklayın **kaydetmek**.

![Yeni Grup Ekle][api-management-add-group-window]

Yeni Grup grupları sekmesinde görüntülenir. Düzenlenecek **adı** veya **açıklama** grubunu, listedeki grup adını tıklatın. Grubunu silmek için tıklatın **silmek**.

![Eklenen grubu][api-management-new-group]

Grubu oluşturulan, ürünleri ve geliştiriciler ile ilişkili olabilir.

## <a name="associate-group-product"></a>Bir grup ürünü ile ilişkilendirme
Bir grup olan bir ürün ilişkilendirmek için tıklatın **ürünleri** gelen **API Management** sol menüsünde ve istenen ürün adına tıklayın.

![Görünürlüğü Ayarla][api-management-add-group-to-product]

Seçin **görünürlük** sekme ekleme ve grupları kaldırmak için ve ürün için geçerli gruplarını görüntülemek için. Grupları ekleyebilir veya kaldırabilirsiniz için denetleyin veya istenen grupları için onay kutusunun işaretini kaldırın ve tıklatın **kaydetmek**.

![Görünürlüğü Ayarla][api-management-add-group-to-product-visibility]

> [!NOTE]
> Azure Active Directory grupları eklemek için bkz: [Azure API Management'te Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl](api-management-howto-aad.md).
> 
> Gruplardan yapılandırmak için **görünürlük** bir ürün sekmesini tıklatın, **grupları yönet**.
> 
> 

Bir ürün grubu ile ilişkili olduğunda, o gruptaki geliştiriciler görüntüleyebilir ve ürüne abone.

## <a name="associate-group-developer"></a>Grupları geliştiricilerle ilişkilendirme
Grupları geliştiricilerle ilişkilendirme için tıklatın **kullanıcılar** gelen **API Management** sol menüsünde ve bir grubu ile ilişkilendirmek istediğiniz geliştiriciler yanındaki kutuyu işaretleyin.

![Geliştirici grubuna Ekle][api-management-add-group-to-developer]

İstenen geliştiriciler işaretlendiğinde, istenen grubunda tıklatın **gruba ekleyin** açılır. Geliştiriciler kaldırılabilir gruplarından kullanarak **grubundan** açılır. 

![Geliştiriciler][api-management-add-group-to-developer-saved]

Geliştirici ve grubu ilişki eklendikten sonra içinde görüntüleyebilirsiniz **kullanıcılar** sekmesi.

## <a name="next-steps"> </a>Sonraki adımlar
* Bir geliştirici bir gruba eklendikten sonra görüntüleyin ve bu grupla ilişkili ürünlere abone. Daha fazla bilgi için bkz: [oluşturma ve Azure API Management'te bir ürün yayımlama][How create and publish a product in Azure API Management],
* Grup oluşturma ve yayımcı portalında yönetme ek olarak, oluşturmak ve gruplarınızı API Management REST API kullanarak yönetmek [grup](https://msdn.microsoft.com/library/azure/dn776329.aspx) varlık.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
