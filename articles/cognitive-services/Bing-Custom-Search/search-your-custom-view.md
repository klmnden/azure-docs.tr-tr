---
title: Özel Görünüm - Bing özel arama arama
titlesuffix: Azure Cognitive Services
description: Özel bir web görünümü arama açıklar.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 235062c1b3e54843b5e64f4ef16091ae5d630894
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814364"
---
# <a name="call-your-custom-search"></a>Özel arama çağırın

Örneğiniz için arama sonuçları elde etmek için özel arama API'si için ilk çağrı yapmadan önce bir Bilişsel hizmetler abonelik anahtarı almanız gerekir. Özel arama API'si için bir anahtarı almak için bkz. [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).


## <a name="try-it-out"></a>Deneyin

Özel arama deneyiminizi yapılandırdıktan sonra özel arama portalındaki yapılandırmasından test edebilirsiniz. 

1. Oturum [özel arama](https://customsearch.ai).
2. Örnek bir özel arama örneği listenizden tıklayın.
3. Tıklayın **üretim** sekmesi. 
4. Altında **uç noktaları** sekmesinde, bir uç nokta (örneğin, Web API'si) seçin. Aboneliğiniz, hangi uç noktaları gösterileceğini belirler (bkz [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) abonelik seçenekleri için). 
5. Parametre değerlerini belirtin. 

    Ayarlayabileceğiniz olası Parametreler aşağıda verilmiştir (gerçek listeden seçilen uç noktada bağlıdır). Bu parametreler hakkında ek bilgi için bkz: [özel arama API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) başvuru.

    - **Sorgu**: aranacak arama terimi. Yalnızca, Web, resim, Video ve otomatik öneri uç noktalar için de kullanılabilir.
    - **Özel yapılandırma kimliği**: seçilen özel arama örneği yapılandırma kimliği. Bu alan salt okunur.
    - **Pazar**: sonuçları nereden geldiğini Pazar. Yalnızca, Web, resim, Video ve UI barındırılan uç noktalar için de kullanılabilir.
    - **Abonelik anahtarı**: test etmek için bir abonelik anahtarı. Açılır listeden bir anahtar seçin veya el ile girin.  
      
    Tıklayarak **ek parametreler** aşağıdaki parametreleri gösterir:  
      
    - **Güvenli arama**: Web sayfalarında yetişkinlere yönelik içeriğe filtrelemek için kullanılan bir filtre. Yalnızca Web, resim, Video ve UI barındırılan uç noktaları için kullanılabilir.
    - **Kullanıcı arabirimi dili**: kullanıcı arabirimi dizeleri için kullanılan dili. Örneğin, görüntü ve video kullanıcı arabiriminde, barındırılan etkinleştirirseniz **görüntü** ve **Video** belirtilen dil sekmelerini kullanın.
    - **Sayısı**: yanıtta döndürülecek arama sonuçlarının sayısı. Yalnızca Web, görüntü ve Video uç noktaları için kullanılabilir.
    - **Uzaklık**: sonuç döndürülmeden önce atlamak için arama sonuçlarının sayısı. Yalnızca Web, görüntü ve Video uç noktaları için kullanılabilir.

6. Gerekli tüm seçenekler belirttikten sonra tıklayın **çağrı** JSON yanıtı sağ bölmede görüntülemek için. 

UI barındırılan uç noktayı seçin, arama deneyimini alt bölmede test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Çağrı C# ile özel görünümü](./call-endpoint-csharp.md)
- [Java ile özel görünümünüzü çağırın](./call-endpoint-java.md)
- [Çağrı, özel bir görünümü ile NodeJs](./call-endpoint-nodejs.md)
- [Python ile özel görünümünüzü çağırın](./call-endpoint-python.md)

- [C# SDK'sı ile özel görünümünüzü çağırın](./sdk-csharp-quick-start.md)