---
title: Microsoft Bilişsel hizmetler - Bing özel arama - Python kullanarak uç noktasını çağırmak
description: Bu hızlı başlangıçta, Bing özel arama uç noktasını çağırmak için Python'ı kullanarak özel arama örneğinizin arama sonuçları isteği gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 15bfce02b334b67aedd634fa864efb4849fc5ee2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "35355733"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Bing özel arama uç noktası (Python) çağırın

Bu hızlı başlangıçta, Bing özel arama uç noktasını çağırmak için Python'ı kullanarak özel arama örneğinizin arama sonuçları isteği gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir özel arama örneği. Bkz: [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).

-  [Python](https://www.python.org/) yüklü.

- A [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirin ya da Ücretli abonelik anahtarı, Azure panosundan kullanabilir sağlanan erişim anahtarı gerekir. 

## <a name="run-the-code"></a>Kodu çalıştırma

Bing özel arama uç noktasını çağırmak için bu adımları izleyin:

1. Kodunuz için bir klasör oluşturun.
2. Bir yönetici komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.
3. Yükleme **istekleri** python Modülü:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. ' % S'dosyası BingCustomSearch.py oluşturun ve aşağıdaki kodu kopyalayın.
8. Değiştirin **YOUR-SUBSCRIPTION-KEY** ve **YOUR-özel-CONFIG-ID** anahtarınızı ve yapılandırma kimliğinizle (1. adıma bakın).

    ``` Python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=' + searchTerm + '&customconfig=' + customConfigId
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```
9. Aşağıdaki komutu kullanarak kodu çalıştırın.
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)