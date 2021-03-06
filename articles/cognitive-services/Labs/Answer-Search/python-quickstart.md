---
title: 'Hızlı Başlangıç: Proje yanıt arama, Python'
titlesuffix: Azure Cognitive Services
description: Yanıt Arama Projesini kullanmaya başlamak için Python örneği.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 2eaf07e041998efade1091861144a2dc4d78c56d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60721547"
---
# <a name="quickstart-project-answer-search-with-python"></a>Hızlı başlangıç: Python ile Yanıt Arama Projesi

Aşağıdaki Python örneği "Rock of Gibraltar" hakkında bilgi almak için bir istek oluşturup gönderir.

## <a name="prerequisites"></a>Önkoşullar

[Bilişsel Hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription) ücretsiz denemesi için erişim anahtarı alın

Bu örnekte Python 3.6.4 kullanılmıştır

## <a name="code-scenario"></a>Kod senaryosu 

Aşağıdaki kod URL Önizlemesi oluşturur.
Aşağıdaki adımları izler:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Önizleme için sorgu URL'sini belirtme ve sorgu parametresini ekleme.  
3. Sorgu parametresini ayarlama.
4. İsteği oluşturan ve *Ocp-Apim-Subscription-Key* üst bilgisini ekleyen Search işlevini tanımlama.
5. *Ocp-Apim-Subscription-Key* üst bilgisini ayarlama. 
6. Bağlantı kurma ve isteği gönderme.
7. JSON sonuçlarını yazdırma.

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

```
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

```
## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıcı](c-sharp-quickstart.md)
- [Java hızlı başlangıcı](java-quickstart.md)
- [Node hızlı başlangıcı](node-quickstart.md)
