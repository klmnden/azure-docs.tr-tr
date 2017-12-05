---
title: "Azure Portalı'nı kullanarak bir OpenAPI belirtimi alma | Microsoft Docs"
description: "API Management OpenAPI belirtimiyle içeri aktarmayı öğrenin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: f0c77c6e959ca99698b3ea704756a6abf36147f3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-openapi-specification"></a>Alma OpenAPI belirtimi

Bu makalede, http://conferenceapi.azurewebsites.net?format=json bulunan bir "OpenAPI belirtimi" arka uç API'yi içeri aktarma gösterilmektedir. Bu arka uç API Microsoft tarafından sağlanan ve Azure üzerinde barındırılan. Makale ayrıca APIM API test etme gösterir.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir "OpenAPI belirtimi" arka uç API'yi içeri aktarma
> * Azure portalında API testi
> * Geliştirici Portalı'nda API testi

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Alma ve arka uç API'si yayımlama

1. Seçin **API'leri** gelen altında **API MANAGEMENT**.
2. Seçin **OpenAPI belirtimi** gelen **yeni bir API eklemek** listesi.
    ![OpenAPI belirtimi](./media/import-api-from-oas/oas-api.png)
3. Uygun ayarları girin. Oluşturma sırasında tüm API değerlerini ayarlayabilirsiniz. Alternatif olarak, bazıları daha sonra giderek ayarlayabileceğiniz **ayarları** sekmesi. <br/> Basarsanız **sekmesini** alanları bazı (veya tüm) ile belirtilen arka uç hizmetinden bilgi doldurulan.

    ![Bir API oluşturma](./media/api-management-get-started/create-api.png)

    |Ayar|Değer|Açıklama|
    |---|---|---|
    |**OpenAPI belirtimi**|http://conferenceapi.azurewebsites.NET?Format=JSON|API uygulama hizmeti başvurur. API management bu adrese isteklerini iletir.|
    |**Görünen ad**|*Tanıtım konferans API*|Hizmet URL'si girdikten sonra SEKME tuşuna basın, APIM ne json'da olduğuna bağlı olarak bu alanı doldurur. <br/>Bu ad, Geliştirici Portalı'nda görüntülenir.|
    |**Ad**|*Tanıtım konferans API*|API için benzersiz bir ad sağlar. <br/>Hizmet URL'si girdikten sonra SEKME tuşuna basın, APIM ne json'da olduğuna bağlı olarak bu alanı doldurur.|
    |**Açıklama**|API isteğe bağlı bir açıklama sağlayın.|Hizmet URL'si girdikten sonra SEKME tuşuna basın, APIM ne json'da olduğuna bağlı olarak bu alanı doldurur.|
    |**API'si URL soneki**|*konferans*|API management hizmeti için temel URL soneki eklenir. API Management API'leri kendi soneki ayırır ve bu nedenle soneki her API için belirtilen yayımcı için benzersiz olmalıdır.|
    |**URL şeması**|*HTTPS*|API erişmek için hangi protokollerin kullanılabileceğini belirler. |
    |**Ürünler**|*Sınırsız*| Bir ürün API ilişkilendirerek API yayımlayacak. İsteğe bağlı olarak bu yeni API ürün eklemek için ürün adı yazın. Bu adım, birden fazla ürün için API eklemek için birden çok kez yinelenebilir.<br/>Ürün bir veya daha fazla API'leri ilişkilendirmelerini değil. Geliştiriciler Geliştirici Portalı aracılığıyla sunar ve API sayısını içerir. Geliştiriciler ilk API erişmek için bir ürüne abone olması gerekir. Bunlar abone olduğunuzda, bunlar herhangi bir API'yi bu ürün için iyi bir abonelik anahtarı alın. APIM örneği oluşturduysanız, varsayılan olarak her ürüne abone için zaten yönetici olduğunuz.<br/> Varsayılan olarak, her API Management örneği iki örnek ürün ile gelir: **Starter** ve **sınırsız**. |

4. **Oluştur**’u seçin.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Azure portalında yeni APIM API testi

İşlemleri görüntülemek ve bir API'nin işlemlerini test etmek için kullanışlı bir yol sağlayan doğrudan Azure portalından çağrılabilir.  

1. Önceki adımda oluşturduğunuz API seçin.
2. Tuşuna **Test** sekmesi.

    ![Test API'si](./media/api-management-get-started/test-api.png)
1. Tıklayın **GetSpeakers**.

    Alanlar sorgu parametreleri için sayfanın görüntüler, ancak bu durumda biz yok. Sayfa üstbilgileri alanları de görüntüler. Üst bilgilerinden biri "Ocp-Apim-Subscription-Key" Bu API ile ilişkili ürün abonelik anahtarı içindir. APIM örneği oluşturduysanız, anahtarı otomatik olarak doldurulur için zaten yönetici olduğunuz. 
4. Tuşuna **Gönder**.

    Arka uç yanıt ile **200 Tamam** ve bazı veriler.

## <a name="call-operation"></a>Geliştirici portalından bir işlem çağırma

İşlemler de çağrılabilir **Geliştirici Portalı** API'leri test etmek için. 

1. Oluşturduğunuz API seçin "alma ve arka uç API'si yayımlama" adım.
2. Tuşuna **Geliştirici Portalı**.

    ![Geliştirici Portalı'nda test](./media/api-management-get-started/developer-portal.png)

    "Geliştirici Portalı" site açılır.
3. Seçin **API**.
4. Seçin **gösteri konferans API**.
5. Tıklatın **GetSpeakers**.
    
    Alanlar sorgu parametreleri için sayfanın görüntüler, ancak bu durumda biz yok. Sayfa üstbilgileri alanları de görüntüler. Üst bilgilerinden biri "Ocp-Apim-Subscription-Key" Bu API ile ilişkili ürün abonelik anahtarı içindir. APIM örneği oluşturduysanız, anahtarı otomatik olarak doldurulur için zaten yönetici olduğunuz.
6. Tuşuna **deneyin**.
7. Tuşuna **Gönder**.
    
    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dönüştürme ve yayımlanan bir API koruyun](transform-api.md)
