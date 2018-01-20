---
title: "Azure portal ile bir API olarak bir mantıksal uygulama alma | Microsoft Docs"
description: "Bu öğreticide, API Management (APIM) mantıksal uygulama bir API olarak almak için nasıl kullanılacağını gösterir."
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
ms.openlocfilehash: 96ac8ce81087717f05ae6480a8f875079139b7b6
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="import-a-logic-app-as-an-api"></a>Bir mantıksal uygulama bir API'yi içeri aktarma

Bu makalede mantığını uygulamayı bir API olarak içeri aktarma gösterilmektedir. Makale ayrıca APIM API test etme gösterir.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir mantıksal uygulama bir API'yi içeri aktarma
> * Azure portalında API testi
> * Geliştirici Portalı'nda API testi

## <a name="prerequisites"></a>Önkoşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ Aboneliğinizde bir mantıksal uygulama olduğundan emin olun. Daha fazla bilgi için [ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Alma ve arka uç API'si yayımlama

1. Seçin **API'leri** gelen altında **API MANAGEMENT**.
2. Seçin **mantıksal uygulama** gelen **yeni bir API eklemek** listesi.

    ![Mantıksal uygulama](./media/import-logic-app-as-api/logic-app-api.png)
3. Tuşuna **Gözat** aboneliğinizde mantıksal uygulamalar listesini görmek için.
4. Uygulamayı seçin. Seçilen uygulama ile ilişkili swagger onu getirir ve bunu aktarır APIM bulur. 
5. Bir API'si URL soneki ekleyin. Bu APIM örnekte belirli bu API tanımlayan bir ad sonekidir. Bu APIM örneğinde benzersiz olması gerekir.
6. Bir ürün API ilişkilendirerek API yayımlayacak. Bu durumda, "*sınırsız*" Ürün kullanılır.  Yayımlanması ve geliştiricileri için kullanılabilir olması API istiyorsanız, bir ürün ekleyin. API oluşturma sırasında yapın ya da daha sonra ayarlayın.

    Ürün bir veya daha fazla API'leri ilişkilendirmelerini değil. Geliştiriciler Geliştirici Portalı aracılığıyla sunar ve API sayısını içerir. Geliştiriciler ilk API erişmek için bir ürüne abone olması gerekir. Bunlar abone olduğunuzda, bunlar herhangi bir API'yi bu ürün için iyi bir abonelik anahtarı alın. APIM örneği oluşturduysanız, varsayılan olarak her ürüne abone için zaten yönetici olduğunuz.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    * **Başlangıç**
    * **Sınırsız**   
7. **Oluştur**’u seçin.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Azure portalında yeni APIM API testi

İşlemleri görüntülemek ve bir API'nin işlemlerini test etmek için kullanışlı bir yol sağlayan doğrudan Azure portalından çağrılabilir.  

1. Önceki adımda oluşturduğunuz API seçin.
2. Tuşuna **Test** sekmesi.
3. Başka bir işlem seçin.

    Sorgu parametrelerinin ve üst bilgileri için alanları sayfasını görüntüler. Üst bilgilerinden biri "Ocp-Apim-Subscription-Key" Bu API ile ilişkili ürün abonelik anahtarı içindir. APIM örneği oluşturduysanız, anahtarı otomatik olarak doldurulur için zaten yönetici olduğunuz. 
1. **Gönder**’e basın.

    Arka uç yanıt ile **200 Tamam** ve bazı veriler.

## <a name="call-operation"> </a>Geliştirici portalından işlem çağırma

İşlemler de çağrılabilir **Geliştirici Portalı** API'leri test etmek için. 

1. Oluşturduğunuz API seçin "alma ve arka uç API'si yayımlama" adım.
2. Tuşuna **Geliştirici Portalı**.

    "Geliştirici Portalı" site açılır.
3. Seçin **API** oluşturduğunuz.
4. Test etmek istediğiniz işlemi seçin.
5. Tuşuna **deneyin**.
6. **Gönder**’e basın.
    
    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
> Her mantıksal uygulama sahip **el ile çağırma** işlemi. Birden çok mantıksal uygulamalar, çakışma, sahip değilse sırayla API'nizi oluşturan istiyorsanız işlevi yeniden adlandırmanız gerekir.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dönüştürme ve yayımlanan bir API koruyun](transform-api.md)