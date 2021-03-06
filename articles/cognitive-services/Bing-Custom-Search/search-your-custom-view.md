---
title: Özel Görünüm - Bing özel arama arama
titlesuffix: Azure Cognitive Services
description: Özel bir web görünümü arama açıklar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: maheshb
ms.openlocfilehash: 9c0c4fc21f6f4b23b14c8020ecd7e95c63dedcbf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66388543"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Bing özel arama örneğinizin portalından çağırın

Özel arama deneyiminizi yapılandırdıktan sonra ondan içinde Bing özel arama sınayabilirsiniz [portalı](https://customsearch.ai). 

![Bing özel arama Portalı'nın bir ekran görüntüsü](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Bir arama sorgusu oluşturun 

Bing özel arama açtıktan sonra [portalı](https://customsearch.ai), arama örneğinizi seçin ve tıklayın **üretim** sekmesi. Altında **uç noktaları**, bir API uç noktası (örneğin, Web API'si) seçin. Aboneliğiniz, hangi uç noktaları gösterileceğini belirler.

Bir arama sorgusu oluşturmak için uç noktanız için parametre değerlerini girin. Seçtiğiniz uç nokta bağlı olarak gösterilmesi parametreleri değişebileceğini unutmayın. Bkz: [özel arama API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) daha fazla bilgi için. İçin arama örneğinizin kullandığı aboneliğinizi değiştirmek, uygun bir abonelik anahtarı ekleyin ve uygun dil ve/veya market parametreleri güncelleştirin.

Önemli bazı parametreler aşağıda verilmiştir:


|Parametre  |Açıklama  |
|---------|---------|
|Sorgu     | Aranacak arama terimi. Yalnızca Web, resim, Video ve otomatik öneri uç noktaları için kullanılabilir |
|Özel yapılandırma kimliği | Seçili özel arama örneği yapılandırma kimliği. Bu alan salt okunur. |
|Market     | Sonuçları Pazar kaynaklanan. Yalnızca, Web, resim, Video ve UI barındırılan uç noktalar için de kullanılabilir.        |
|Abonelik anahtarı | Test etmek için abonelik anahtarı. Açılır listeden bir anahtar seçin veya el ile girin.          |

Tıklayarak **ek parametreler** aşağıdaki parametreleri gösterir:  

|Parametre  |Açıklama  |
|---------|---------|
|Safe Search     | Web sayfaları için yetişkinlere yönelik içeriğe filtrelemek için kullanılan bir filtre. Yalnızca, Web, resim, Video ve UI barındırılan uç noktalar için de kullanılabilir.        |
|Kullanıcı arabirimi dili    | Kullanıcı arabirimi dizeleri için kullanılan dil. Örneğin, görüntü ve video kullanıcı arabiriminde, barındırılan etkinleştirirseniz **görüntü** ve **Video** belirtilen dil sekmelerini kullanın.        |
|Sayı     | Yanıtta döndürülecek arama sonuçlarının sayısı. Yalnızca Web, görüntü ve Video uç noktaları için kullanılabilir.         |
|Offset    | Sonuç döndürülmeden önce atlamak için arama sonuçları sayısı. Yalnızca Web, görüntü ve Video uç noktaları için kullanılabilir.        |
    
Gerekli tüm seçenekler belirttikten sonra tıklayın **çağrı** JSON yanıtı sağ bölmede görüntülemek için. UI barındırılan uç noktayı seçin, arama deneyimini alt bölmede test edebilirsiniz.

## <a name="change-your-bing-custom-search-subscription"></a>Bing özel arama aboneliğinizi değiştirme

Yeni bir örneği oluşturulmadan Bing özel arama örneğinizin ile ilişkili aboneliği değiştirebilirsiniz. Gönderilen ve yeni bir abonelik için ücret API çağrıları için Azure portalında yeni bir Bing özel arama kaynağı oluşturun. Yeni bir abonelik anahtarı API isteklerinizin örneğinizin özel yapılandırma kimliği ile birlikte kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Çağrı C# ile özel görünümü](./call-endpoint-csharp.md)
- [Java ile özel görünümünüzü çağırın](./call-endpoint-java.md)
- [Çağrı, özel bir görünümü ile NodeJs](./call-endpoint-nodejs.md)
- [Python ile özel görünümünüzü çağırın](./call-endpoint-python.md)

- [C# SDK'sı ile özel görünümünüzü çağırın](./sdk-csharp-quick-start.md)
