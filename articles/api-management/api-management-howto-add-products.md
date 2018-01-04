---
title: "Oluşturma ve Azure API Management'te bir ürün yayımlama"
description: "Oluşturma ve Azure API Management'te ürünleri yayımlama öğrenin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: e6b11145506780f9a08799c4c9daf55ba17b366d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-publish-a-product"></a>Oluşturma ve bir ürün yayımlama  

Azure API Management'te bir ürün kullanım kotası ve kullanım koşullarını yanı sıra bir veya daha fazla API'leri içerir. Bir ürün yayımlandığında, geliştiriciler ürüne abone ve ürün API'lerine kullanmaya başlar.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Oluşturma ve bir ürün yayımlama
> * Ürüne bir API ekleme

![eklenen ürün](media/api-management-howto-add-products/added-product.png)

## <a name="prerequisites"></a>Önkoşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-and-publish-a-product"></a>Oluşturma ve bir ürün yayımlama

1. Tıklayın **ürünleri** görüntülemek için soldaki menüde **ürünleri** sayfası.
2. Tıklatın **+ ürün**.

    ![eklenen ürün](media/api-management-howto-add-products/add-product.png)

    Bir ürün eklediğinizde aşağıdaki bilgileri sağlamanız gerekir: 

    |Ad|Açıklama|
    |---|---|
    |Görünen ad|Bunu gösterilmesini istediğiniz adı **Geliştirici Portalı**.|
    |Ad|Ürünün açıklayıcı bir ad.|
    |Açıklama|**Açıklama** alan amacı, erişim sağladığı API'ları ve diğer yararlı bilgiler gibi ürün hakkında ayrıntılı bilgi sağlamak sağlar.|
    |Durum|Tuşuna **yayımlanan** ürün yayımlama istiyorsanız. Bir ürün API'lerinde çağrılabilir önce ürünün yayımlanması gerekir. Varsayılan olarak yeni ürünler yayımdan ve yalnızca görünür **Yöneticiler** grubu.|
    |Onay gerektiriyor|Denetleme **abonelik onayı iste** gözden geçirin ve kabul edin ya da bu ürünün abonelik girişimleri reddetmek için yönetici istiyorsanız. Kutunun işaretli değilse abonelik girişimleri otomatik olarak onaylanır. |
    |Abonelik sayısı limiti|Birden çok aboneliğe sayısını sınırlamak için abonelik sınırı girin. |
    |Yasal koşullar|Ürünü kullanmak için hangi aboneleri kabul etmeniz gerekir ürün için kullanım koşullarını içerebilir.|
    |API'ler|Ürün bir veya daha fazla API'leri ilişkilendirmelerini değil. Geliştiriciler Geliştirici Portalı aracılığıyla sunar ve API sayısını içerir. <br/> Ürün oluşturma sırasında var olan bir API ekleyebilirsiniz. Ürünün daha sonra ürünlerden ya da bir API ekleyebilirsiniz **ayarları** sayfasında veya oluşturulurken bir API.|<br/>Geliştiriciler ilk API erişmek için bir ürüne abone olması gerekir. Bunlar abone olduğunuzda, bunlar herhangi bir API'yi bu ürün için iyi bir abonelik anahtarı alın.<br/> APIM örneği oluşturduysanız, varsayılan olarak her ürüne abone için zaten yönetici olduğunuz.|

3. Tıklatın **oluşturma** yeni ürün oluşturmak için.

### <a name="add-more-configurations"></a>Daha fazla yapılandırmalarını ekleyin

Ürün seçerek kaydedildikten sonra yapılandırmaya devam etmek **ayarları** sekmesi. 

Görünümü/aboneleri ürün eklemek **abonelikleri** sekmesi.

Geliştiriciler için bir ürün görünürlüğünü ayarlamak veya gelen Konuk **erişim denetimi** sekmesi.

## <a name="add-apis"></a>Bir ürüne API ekleme

Ürün bir veya daha fazla API'leri ilişkilendirmelerini değil. Geliştiriciler Geliştirici Portalı aracılığıyla sunar ve API sayısını içerir. Ürün oluşturma sırasında var olan bir API ekleyebilirsiniz. Ürünün daha sonra ürünlerden ya da bir API ekleyebilirsiniz **ayarları** sayfasında veya oluşturulurken bir API.

Geliştiriciler ilk API erişmek için bir ürüne abone olması gerekir. Bunlar abone olduğunuzda, bunlar herhangi bir API'yi bu ürün için iyi bir abonelik anahtarı alın. APIM örneği oluşturduysanız, varsayılan olarak her ürüne abone için zaten yönetici olduğunuz.

### <a name="add-an-api-to-an-existing-product"></a>Varolan bir ürüne bir API ekleme

1. Bir ürün seçin.
2. API sekmesini seçin.
3. Tıklatın **+ API**.
4. Bir API seçin ve tıklatın **oluşturma**.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Oluşturma ve bir ürün yayımlama
> * Ürüne bir API ekleme

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [Boş API'si oluşturma ve API yanıtları mock](mock-api-responses.md)
