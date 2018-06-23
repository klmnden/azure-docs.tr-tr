---
title: Python hızlı başlangıç projesi URL önizleme - Microsoft Bilişsel hizmetler için | Microsoft Docs
description: Hızlı bir şekilde proje URL önizleme Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki kullanmaya başlamak için komut dosyası örneği.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 78b2d83b02aa9ea32509029c7456e04e420b8572
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354016"
---
# <a name="url-preview-python-quickstart"></a>URL önizleme Python hızlı başlangıç

Aşağıdaki Python örnek SwiftKey Web sitesi için Url Önizleme oluşturur: https://swiftkey.com/en.

## <a name="prerequisites"></a>Önkoşullar

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

Bu örnek, Python 3.6 kullanır.

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
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
````
## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Düğüm URL Hızlı Başlangıç](node-quickstart.md)