---
title: Bir Azure kapsayıcı görüntüsü için SKU'ları | Azure Market
description: Bir Azure kapsayıcı için SKU'ları yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 6953329bfabe99fc4bb28f2494cb412ba9cbbba0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942924"
---
# <a name="container-skus-tab"></a>Kapsayıcı SKU'ları sekmesi

**SKU'ları** sekmesinde **yeni teklif** sayfası, bir veya daha fazla SKU'ları oluşturma ve bunları yeni teklifinizi ilişkilendirme olanak tanır.  Bir çözüm özellik kümeleri, faturalandırma modelleri veya diğer özelliği ayırt etmek için farklı SKU'ları kullanabilirsiniz.

## <a name="sku-settings"></a>SKU ayarları

Yeni bir teklif oluşturma başlattığınızda, bir teklifle ilgili SKU'ları yok. Yeni bir SKU'ya oluşturmak için aşağıdaki adımları izleyin:

1. SKU'ları sekmesinde seçin **yeni SKU**

   ![Yeni SKU istemi](./media/containers-sku-settings.png)

2. Gerekli SKU ve kapsayıcı bilgileri sağlayın. Her SKU, bir kapsayıcı görüntüsüne karşılık gelir. Bir SKU iki bölümü vardır:

    -   SKU meta verileri
    -   Kapsayıcı meta verileri


### <a name="sku-metadata"></a>SKU meta verileri

SKU kapsayıcı listeleme için mağaza görüntüleme bilgileri içerir.

![SKU meta verileri](./media/containers-sku-details.png)


### <a name="container-metadata"></a>Kapsayıcı meta verileri

Kapsayıcı meta verileri, görüntü deposu ayrıntılarının içinde Azure Container Registry (ACR) başvuru bilgilerini içerir. Azure Market Market özel, genel bir kayıt defterine bu görüntüyü kopyalar ve ardından görüntüyü müşteriler için sonra sertifika kullanımına. Azure Market kapsayıcı görüntüsü kullanmak için Azure kullanıcı gelen tüm isteklerin Market'ın genel kayıt defterinden değil ACR sunulur.

![Kapsayıcı meta verileri](./media/containers-image-repository.png)
    
**Görüntü deposu ayrıntıları** önceki ekranda yakalama aşağıdaki alanları içerir.  Gerekli alanlar yıldız (*) indicted.

-   **Abonelik kimliği\***  -ACR olduğu mevcut Azure abonelik kimliği.
-   **Kaynak grubu adı\***  -ACR kaynak grubu adı.
-   **Kayıt defteri adı\***  -ACR adı.
-   **Depo adı\***  -depo adı. Bu değer, bu ad ayarlandıktan sonra değiştirilemez. Hesabınızdaki diğer hizmetlerle çakışma olmasını önlemek için benzersiz bir ad kullanın.
-   **Kullanıcı adı\***  -ACR görüntü ile ilişkili kullanıcı adını (yönetici kullanıcı adı).
-   **Parola\***  -ACR görüntüsüyle ilişkili parola.

    >[!NOTE]
    >Kullanıcı adı ve parola iş ortakları yayımlama işleminde belirtilen ACR erişiminiz olduğundan emin olmak için gereklidir.


### <a name="image-version"></a>Görüntü Sürümü

Bir kapsayıcı görüntüsü yayımlama sırasında bir veya daha fazla görüntü etiketleri sağlayabilir ve SHA özetleyen.

**Görüntü etiketi\* veya Özet**
 
- Bu etiket veya Özet içermelidir bir `latest` etiketi ve sürüm etiketi (örneğin, başlayarak `xx.xx.xx-` burada xx, bir sayı). Olmaları gerektiği [bildirim etiketleri](https://github.com/estesp/manifest-tool) birden çok platformu hedefleyecek şekilde. Biz bunları yüklemek için bir bildirim etiketi tarafından başvurulan tüm etiketleri de eklenmelidir. 
- Kapsayıcı etiketleri kullanarak çeşitli sürümlerini ekleyebilirsiniz. Tüm etiketleri bildirim (dışında `latest`) ile başlamalıdır `X.Y-` veya `X.Y.Z-` X, Y, Z tamsayılar olduğu. <br/> Örneğin, bir `latest` etiketi noktalarına `1.0.1-linux-x64`, `1.0.1-linux-arm32`, ve `1.0.1-windows-arm32`, bu etiketler burada eklenmesi gerekir.

>[!NOTE]
>Eklemeyi unutmayın bir **test etiketi** görüntünüzü test sırasında görüntünün tanımlayabilmeniz için.


## <a name="next-steps"></a>Sonraki adımlar

Kullanım [Marketi sekmesinden](./cpp-marketplace-tab.md) teklifiniz için bir Market açıklama oluşturmak için. 
