---
title: Azure portalı kullanarak SOAP API’sini içeri aktarma | Microsoft Docs
description: API Management ile SOAP API’sini içeri aktarmayı öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 7f7a203495e35d9e4143fe247a55adaa43bc5d2d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817025"
---
# <a name="import-soap-api"></a>SOAP API’sini içeri aktarma

Bu makalede, bir SOAP API’sinin standart bir XML gösteriminin nasıl içeri aktarılacağı gösterilir. Makale, APIM API’sinin nasıl test edileceğini de göstermektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * SOAP API’sini içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Önkoşullar

Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Arka uç API’sini içeri aktarma ve yayımlama

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Yeni API ekleyin** listesinden **WSDL**’yi seçin.

    ![SOAP API’si](./media/import-soap-api/wsdl-api.png)
3. **WSDL belirtimi**’nde, SOAP API’nizin bulunduğu URL'yi girin.
4. **SOAP geçişi** radyo düğmesi, varsayılan olarak seçilir. Bu seçimle API, SOAP olarak sunulur. Tüketicinin SOAP kurallarını kullanması gerekir. API’yi REST’e dönüştürmek istiyorsanız, [SOAP API'yi içeri aktarma ve REST’e dönüştürme](restify-soap-api.md) konusundaki adımları izleyin.

    ![Geçiş](./media/import-soap-api/pass-through.png)
5. Sekme tuşuna basın.

    Aşağıdaki alanlar SOAP API'si bilgilerle doldurulur: Görünen ad, ad, açıklama.
6. API URL'si soneki ekleyin. Sonek, bu belirli API’yi bu APIM örneğinde tanımlayan bir addır. Son ekin bu APIM örneğinde benzersiz olması gerekir.
9. API’yi bir ürünle ilişkilendirerek yayımlayın. Bu durumda, "*Sınırsız*" ürünü kullanılır.  API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız, bir ürüne ekleyin. Bunu API oluşturması sırasında yapabilir ya da daha sonra ayarlayabilirsiniz.

    Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    * **Başlangıç**
    * **Sınırsız**   
10. **Oluştur**’u seçin.

### <a name="test-the-new-apim-api-in-the-administrative-portal"></a>Yönetim portalında yeni APIM API’sini test etme

İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan yönetim portalından çağrılabilir.  

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **Test** sekmesine basın.
3. Bir işlem seçin.

    Sayfa, sorgu parametrelerinin ve üst bilgilerin alanlarını görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur. 
1. **Gönder**’e basın.

    Arka uç, **200 OK** ve bazı verilerle yanıt verir.

### <a name="call-operation"> </a>Geliştirici portalından işlem çağırma

API’leri test etmek için **Geliştirici portalından** da işlemler çağrılabilir. 

1. "Arka uç API’sini içeri aktarma ve yayımlama" adımında oluşturduğunuz API’yi seçin.
2. **Geliştirici portalı** düğmesine basın.

    "Geliştirici portalı" sitesi açılır.
3. Oluşturduğunuz **API**’yi seçin.
4. Test etmek istediğiniz işleme tıklayın.
5. **Deneyin**’e basın.
6. **Gönder**’e basın.
    
    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)