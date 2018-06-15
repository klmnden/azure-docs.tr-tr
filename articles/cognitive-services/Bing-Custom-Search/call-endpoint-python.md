---
title: Uç nokta Python - Bing özel arama - Microsoft Bilişsel hizmetler kullanarak çağırma
description: Bu hızlı başlangıç Bing özel arama uç noktasını çağırmak için Python kullanarak özel arama örneğinden arama sonuçlarında istek gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355733"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Çağrı Bing özel arama uç noktası (Python)

Bu hızlı başlangıç Bing özel arama uç noktasını çağırmak için Python kullanarak özel arama örneğinden arama sonuçlarında istek gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir özel arama örneği. Bkz: [ilk Bing özel arama örneğinizi oluşturmak](quick-start.md).

-  [Python](https://www.python.org/) yüklü.

- A [Bilişsel Hizmetleri API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir. 

## <a name="run-the-code"></a>Kodu çalıştırma

Bing özel arama uç noktasını çağırmak için aşağıdaki adımları izleyin:

1. Kodunuz için bir klasör oluşturun.
2. Bir yönetici komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.
3. Yükleme **istekleri** python Modülü:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. BingCustomSearch.py dosyası oluşturun ve aşağıdaki kodu kopyalayın.
8. Değiştir **YOUR ABONELİK anahtarı** ve **YOUR-özel-CONFIG-ID** anahtar ve yapılandırma Kimliğine sahip (1. adım bakın).

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
- [Metni vurgulama için decoration işaretlerini kullanın](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)