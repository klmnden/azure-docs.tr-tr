---
title: Azure API Management'ta ürün oluşturma ve yayımlama
description: Azure API Management'ta ürün oluşturmayı ve yayımlamayı öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
origin.date: 08/10/2018
ms.date: 12/03/2018
ms.author: apimpm
ms.openlocfilehash: 0f2b45685d2976c567c16666e2ca89d334914b63
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62112547"
---
# <a name="create-and-publish-a-product"></a>Ürün oluşturma ve yayımlama  

Azure API Management'ta, üründe bir veya birden çok API, ayrıca kullanım kotası ve kullanım koşulları bulunur. Ürün yayımlandığında, geliştiriciler ürüne abone olabilir ve ürünün API'lerini kullanmaya başlayabilir.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ürün oluşturma ve yayımlama
> * Ürüne bir API ekleme

![Ürün öğreticisi ekleme](media/api-management-howto-add-products/added-product.png)

## <a name="prerequisites"></a>Önkoşullar

+ [Azure API Management terminolojisini](api-management-terminology.md) öğrenin.
+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca şu öğreticiyi tamamlayın: [İçeri aktarma ve ilk API'nizi yayımlama](import-and-publish.md).

## <a name="create-and-publish-a-product"></a>Ürün oluşturma ve yayımlama

![Ürün ekleme](media/api-management-howto-add-products/02-create-publish-product-01.png)

1. **Ürünler** sayfasını görüntülemek için soldaki menüden **Ürünler**’e tıklayın.
2. **+ Ekle**'ye tıklayın.

    Ürün eklerken aşağıdaki bilgileri sağlamanız gerekir: 

    | Ad                     | Açıklama                                                                                                                                                                                                                                                                                                             |
    |--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Görünen ad             | **Geliştirici portalında** gösterilmesini istediğiniz ad.                                                                                                                                                                                                                                                        |
    | Ad                     | Ürün için açıklayıcı bir ad.                                                                                                                                                                                                                                                                                      |
    | Açıklama              | **Açıklama** alanı, ürünün amacı ve hangi API’ler için erişim sağladığı gibi konularda ayrıntılı, kullanışlı bilgiler sağlamanıza imkan tanır.                                                                                                                                               |
    | Durum                    | Ürünü yayımlamak istiyorsanız **Yayımlandı** seçeneğine basın. Bir üründeki API'lerin çağrılabilmesi için önce ürünün yayımlanması gerekir. Varsayılan olarak, yeni ürünler yayımlanmış durumda değildir ve yalnızca **Yöneticiler** grubundaki kullanıcılar tarafından görülebilir.                                                                                      |
    | Abonelik gerektirir    | Kullanıcının ürünü kullanmak için aboneliğe sahip olması gerekiyorsa **Abonelik iste**'yi işaretleyin.                                                                                                                                                                                                                                   |
    | Onay gerekiyor        | Yöneticinin bu ürüne yönelik abonelik girişimlerini gözden geçirip kabul etmesini ya da reddetmesini istiyorsanız **Onay iste** seçeneğini işaretleyin. Kutu işaretsiz bırakılırsa abonelik girişimleri otomatik olarak onaylanır.                                                                                                                         |
    | Abonelik sayısı limiti | Aynı anda sahip olunabilecek abonelik sayısını kısıtlamak için abonelik limitini girin.                                                                                                                                                                                                                                |
    | Yasal koşullar              | Abonelerin ürünü kullanmak için kabul etmek zorunda olduğu ürün kullanım koşullarını ekleyebilirsiniz.                                                                                                                                                                                                             |
    | API'ler                     | Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. <br/> Ürün oluşturma sırasında mevcut bir API’yi ekleyebilirsiniz. Ürüne daha sonra Ürün **Ayarları** sayfasından veya bir API oluşturduğunuz sırada API ekleyebilirsiniz. |

3. Yeni ürünü oluşturmak için **Oluştur**’a tıklayın.

### <a name="add-more-configurations"></a>Daha fazla yapılandırma ekleme

Ürünü kaydettikten sonra **Ayarlar** sekmesini seçerek yapılandırmaya devam edebilirsiniz. 

**Abonelikler** sekmesinden aboneleri görüntüleyin/ürüne abone ekleyin.

**Erişim denetimi** sekmesinden geliştiriciler veya konuklar için bir ürünün görünürlük düzeyini ayarlayın.

## <a name="add-apis"> </a>Ürüne API ekleme

Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Ürün oluşturma sırasında mevcut bir API’yi ekleyebilirsiniz. Ürüne daha sonra Ürün **Ayarları** sayfasından veya bir API oluşturduğunuz sırada API ekleyebilirsiniz.

Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.

### <a name="add-an-api-to-an-existing-product"></a>Mevcut bir ürüne API ekleme

![ürün ekleme API'si](media/api-management-howto-add-products/02-create-publish-product-02.png)

1. **Ürünler** sekmesinden bir ürün seçin.
2. **API'ler** sekmesine gidin.
3. **+ Ekle**'ye tıklayın.
4. Bir API seçip **Seç**’e tıklayın.

> [!TIP]
> [REST API](https://docs.microsoft.com/rest/api/apimanagement/subscription/createorupdate) veya PowerShell komutu aracılığıyla özel abonelik anahtarları kullanarak kullanıcının *Ürün* aboneliğini oluşturabilir veya güncelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ürün oluşturma ve yayımlama
> * Ürüne bir API ekleme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Boş bir API ve sahte API yanıtları oluşturma](mock-api-responses.md)
