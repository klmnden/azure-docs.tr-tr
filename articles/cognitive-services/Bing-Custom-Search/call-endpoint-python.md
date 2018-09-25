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
ms.openlocfilehash: 3666e92372e9bed80e5c0c7991dcac730cebb588
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967602"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Bing özel arama uç noktası (Python) çağırın

Bu hızlı başlangıçta, arama sonuçlarını Bing özel arama uç noktasını çağırmak için Python'ı kullanarak özel arama örneğinizin isteği gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir kullanıma hazır özel arama örneği. Bkz: [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).
- [Python](https://www.python.org/) yüklü.
- Bir abonelik anahtarı. Etkinleştirme sırasında bir abonelik anahtarı edinirler, [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), ya da Ücretli abonelik anahtarı, Azure panosundan kullanabilirsiniz (bkz [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    


## <a name="run-the-code"></a>Kodu çalıştırma

Bu örneği çalıştırmak için aşağıdaki adımları izleyin:

1. Kodunuz için bir klasör oluşturun.  
  
2. Bir yönetici komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.  
  
3. Yükleme **istekleri** python Modülü:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Oluşturduğunuz klasördeki BingCustomSearch.py adlı bir dosya oluşturun ve içine aşağıdaki kodu kopyalayın. Değiştirin **YOUR-SUBSCRIPTION-KEY** ve **YOUR-özel-CONFIG-ID** yapılandırma ve subscriptioin anahtarı ile kimliği  
  
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
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)
