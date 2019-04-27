---
title: 'Hızlı Başlangıç: Python kullanarak Bing özel arama uç noktanızı arayın | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Python kullanarak Bing özel arama örneğinizin arama sonuçlarını talep başlamak için kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: a6f8d3964c71d036053fe0d4136468fde690f872
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60593149"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Hızlı Başlangıç: Python kullanarak Bing özel arama uç noktanızı arayın

Bing özel arama örneğinizin arama sonuçlarını talep başlamak için bu Hızlı Başlangıç'ı kullanın. Bu uygulama Python'da yazılmıştır, ancak Bing özel arama API'si bir RESTful web çoğu programlama dilleri ile uyumlu hizmetidir. Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py)’da mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

- Bing özel arama örneği için. Bkz: [hızlı başlangıç: İlk Bing özel arama örneğinizin oluşturma](quick-start.md) daha fazla bilgi için.
- [Python](https://www.python.org/) 2.x veya 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Sık kullandığınız IDE veya düzenleyici yeni bir Python dosyası oluşturun ve aşağıdaki import deyimlerini girin. Abonelik anahtarınız ve özel yapılandırma kimliği bir arama terimi için değişkenler oluşturun. 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderip 

1. Arama teriminizi ekleyerek istek URL'si oluşturmak `q=` sorgu parametresi ve yapılandırma kimliği özel arama örneğinizin `customconfig=`. parametrelerle ayrı bir `&` karakter. 

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Bing özel arama örneğinizin isteği gönderir ve döndürülen arama sonuçlarını yazdırın.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](./tutorials/custom-search-web-page.md)
