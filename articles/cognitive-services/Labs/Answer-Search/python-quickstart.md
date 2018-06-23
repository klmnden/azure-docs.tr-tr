---
title: Microsoft Bilişsel hizmetler, proje yanıt arama için Python hızlı başlangıç | Microsoft Docs
description: Python örnek proje yanıt arama, Microsoft Azure'da Bilişsel hizmetler kullanımına başlamanıza.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9cb5406c616ed8e96d73c00c788a0d20f66dcabd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353932"
---
# <a name="project-answer-search-python-quickstart"></a>Proje yanıt arama Python hızlı başlangıç

Aşağıdaki Python örneği oluşturur ve "Cebelitarık Rock" hakkında bilgi için bir istek gönderir.

## <a name="prerequisites"></a>Önkoşullar

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

Bu örnek, Python 3.6.4 kullanır.

## <a name="code-scenario"></a>Kod senaryosu 

Aşağıdaki kod, bir URL önizleme oluşturur.
Aşağıdaki adımlarda uygulanır:
1. Bitiş noktası tarafından konak ve yol belirtmek için değişkenleri bildirin.
2. Önizleme için sorgu URL'sini belirtin ve sorgu parametresi ekleyin.  
3. Sorgu parametresini ayarlayın.
4. İsteği oluşturur ve ekleyen arama işlevini tanımlayan *Apim abonelik anahtar Ocp* üstbilgi.
5. Ayarlama *Apim abonelik anahtar Ocp* üstbilgi. 
6. Bağlantıyı kurmak ve isteği gönderin.
7. JSON sonuçlarını yazdırın.

Bu Tanıtım için tam kod aşağıdaki gibidir:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'https://api.labs.cognitive.microsoft.com'
path = '/answerSearch/v7.0/search '

query = 'Rock of Gibraltar'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

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
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)