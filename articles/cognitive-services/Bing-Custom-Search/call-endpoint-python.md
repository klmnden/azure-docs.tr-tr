---
title: 'Hızlı başlangıç: Python kullanarak uç nokta çağırma - Bing Özel Arama'
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Bing Özel Arama uç noktasını çağırmak için Python kullanarak özel arama örneğinizden arama sonuçlarını isteme adımları gösterilmektedir.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: d16b7354b0ef727ba3c670b97105e1bd6f99034e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815469"
---
# <a name="quickstart-call-bing-custom-search-endpoint-python"></a>Hızlı başlangıç: Bing Özel Arama uç noktasını çağırma (Python)

Bu hızlı başlangıçta Bing Özel Arama uç noktasını çağırmak için Python kullanarak özel arama örneğinizden arama sonuçlarını isteme adımları gösterilmektedir. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Kullanıma hazır özel arama örneği. Bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).
- [Python](https://www.python.org/) uygulamasının yüklenmiş olması.
- Abonelik anahtarı. Abonelik anahtarını [ücretsiz denemenizi](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) etkinleştirdikten sonra alabilir veya Azure panonuzdan ücretli abonelik anahtarı (bkz. [Bilişsel Hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)) kullanabilirsiniz.    


## <a name="run-the-code"></a>Kodu çalıştırma

Bu örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Kodunuz için bir klasör oluşturun.  
  
2. Bir yönetici komut isteminden veya terminalden az önce oluşturduğunuz klasöre gidin.  
  
3. **requests** Python modülünü yükleyin:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Oluşturduğunuz klasörde BingCustomSearch.py adlı bir dosya oluşturun ve aşağıdaki kodu içine kopyalayın. **YOUR-SUBSCRIPTION-KEY** ve **YOUR-CUSTOM-CONFIG-ID** yerine abonelik anahtarınızı ve yapılandırma kimliğinizi yazın.  
  
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
  
7. Aşağıdaki komutu kullanarak kodu çalıştırın.  
  
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma](./hosted-ui.md)
- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
