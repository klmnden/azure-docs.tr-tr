---
title: Hızlı Başlangıç - sorgu python'da Bing yerel iş arama API'si için bir gönderme | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Python'da Bing yerel iş arama API'sini kullanmaya başlamak için bu makaleyi kullanın.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: ccac5f986b765e03caf939e28c5bcb7757e729b6
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852308"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Hızlı Başlangıç: Bing yerel iş arama API'si python'da bir sorgu gönderin

Bu hızlı başlangıçta, Azure Bilişsel hizmet olduğu Bing yerel iş arama API'si için istekleri göndermeye başlamak için kullanın. Bu basit uygulama Python'da yazılmıştır, ancak tüm programlama dillerini HTTP isteğinde bulunan ve JSON ayrıştırma özelliğine sahip uyumlu bir RESTful Web hizmeti API'dir.

Bu örnek uygulama, arama sorgusu için API'sinden yerel yanıt verilerini alır. `hotel in Bellevue`.

## <a name="prerequisites"></a>Önkoşullar

* [Python](https://www.python.org/) 2.x veya 3.x
 
Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Bing API'leri ile. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme tarafından sağlanan erişim anahtarı kullanın.

## <a name="run-the-complete-application"></a>Tam uygulama çalıştırma

Aşağıdaki kodu, yerelleştirilmiş sonuçlarını alır. Aşağıdaki adımları izler:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorgu parametresini belirtin. 
3. İsteği oluşturur ve Ocp-Apim-Subscription-Key üstbilgisi ekler arama işlevini tanımlar.
4. Ocp-Apim-Subscription-Key başlığı ayarlayın. 
5. Bağlantı kurmak ve istek gönderin.
6. JSON sonuçlarını yazdırma.

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com/bing'
path = '/v7.0/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&appid=' + subscriptionKey + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

````

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel işletme arama Java hızlı başlangıç](local-search-java-quickstart.md)
- [Yerel işletme arama C# hızlı başlangıç](local-quickstart.md)
- [Yerel işletme arama düğüm hızlı başlangıç](local-search-node-quickstart.md)