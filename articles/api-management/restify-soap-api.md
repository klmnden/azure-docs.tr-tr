---
title: "SOAP API içeri aktarın ve Azure portalını kullanarak REST Dönüştür | Microsoft Docs"
description: "SOAP API'yi içeri aktarma ve REST API Management ile dönüştürmek hakkında bilgi edinin."
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
ms.openlocfilehash: 74935f11d5bf3c83aef46c1d41fccd81ad312acb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="import-a-soap-api-and-convert-to-rest"></a>SOAP API'yi içeri aktarma ve REST için Dönüştür

Bu makalede, bir SOAP API'yi içeri aktarma ve KALANLAR Dönüştür gösterilmektedir. Makale ayrıca APIM API test etme gösterir.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * SOAP API'yi içeri aktarma ve REST için Dönüştür
> * Azure portalında API testi
> * Geliştirici Portalı'nda API testi

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Alma ve arka uç API'si yayımlama

1. Seçin **API'leri** gelen altında **API MANAGEMENT**.
2. Seçin **WSDL** gelen **yeni bir API eklemek** listesi.

    ![SOAP API](./media/restify-soap-api/wsdl-api.png)
3. İçinde **WSDL belirtimi**, SOAP API bulunduğu için URL'yi girin.
4. Tıklatın **REST SOAP** radyo düğmesi. Bu seçenek tıklatıldığında APIM XML ve JSON arasında otomatik bir dönüştürme yapmaya çalışır. Bu durumda tüketicileri API JSON döndüren bir restful API'si çağrılıyor. APIM her istek SOAP çağrısını dönüştürülüyor.

    ![SOAP'tan REST'e](./media/restify-soap-api/soap-to-rest.png)

5. Sekme tuşuna basın.

    Aşağıdaki alanları SOAP API bilgisi doldurulmuş: görünen adı, ad, açıklama.
6. Bir API'si URL soneki ekleyin. Bu APIM örnekte belirli bu API tanımlayan bir ad sonekidir. Bu APIM örneğinde benzersiz olması gerekir.
9. Bir ürün API ilişkilendirerek API yayımlayacak. Bu durumda, "*sınırsız*" Ürün kullanılır.  Yayımlanması ve geliştiricileri için kullanılabilir olması API istiyorsanız, bir ürün ekleyin. API oluşturma sırasında yapın ya da daha sonra ayarlayın.

    Ürün bir veya daha fazla API'leri ilişkilendirmelerini değil. Geliştiriciler Geliştirici Portalı aracılığıyla sunar ve API sayısını içerir. Geliştiriciler ilk API erişmek için bir ürüne abone olması gerekir. Bunlar abone olduğunuzda, bunlar herhangi bir API'yi bu ürün için iyi bir abonelik anahtarı alın. APIM örneği oluşturduysanız, varsayılan olarak her ürüne abone için zaten yönetici olduğunuz.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    * **Başlangıç**
    * **Sınırsız**   
10. **Oluştur**’u seçin.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Azure portalında yeni APIM API testi

İşlemleri görüntülemek ve bir API'nin işlemlerini test etmek için kullanışlı bir yol sağlayan doğrudan Azure portalından çağrılabilir.  

1. Önceki adımda oluşturduğunuz API seçin.
2. Tuşuna **Test** sekmesi.
3. Başka bir işlem seçin.

    Sorgu parametrelerinin ve üst bilgileri için alanları sayfasını görüntüler. Üst bilgilerinden biri "Ocp-Apim-Subscription-Key" Bu API ile ilişkili ürün abonelik anahtarı içindir. APIM örneği oluşturduysanız, anahtarı otomatik olarak doldurulur için zaten yönetici olduğunuz. 
1. Tuşuna **Gönder**.

    Arka uç yanıt ile **200 Tamam** ve bazı veriler.

## <a name="call-operation"> </a>Geliştirici portalından işlem çağırma

İşlemler de çağrılabilir **Geliştirici Portalı** API'leri test etmek için. 

1. Oluşturduğunuz API seçin "alma ve arka uç API'si yayımlama" adım.
2. Tuşuna **Geliştirici Portalı**.

    "Geliştirici Portalı" site açılır.
3. Seçin **API** oluşturduğunuz.
4. Test etmek istediğiniz işlemi seçin.
5. Tuşuna **deneyin**.
6. Tuşuna **Gönder**.
    
    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dönüştürme ve yayımlanan bir API koruyun](transform-api.md)