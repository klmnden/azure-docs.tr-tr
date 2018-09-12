---
title: Bilgi Bankasını Güncelleştirme, Python Hızlı Başlangıcı - Azure Bilişsel Hizmetler | Microsoft Docs
description: Python’da Soru-Cevap Oluşturma için bilgi bankası nasıl güncelleştirilir?
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: quickstart
ms.date: 06/18/2018
ms.author: nolachar
ms.openlocfilehash: 79b2af150558aa5da8e060e1b5117b91ffd34387
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "43772532"
---
# <a name="update-a-knowledge-base-in-python"></a>Python’da bilgi bankası güncelleştirme

Aşağıdaki kod, [Güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yöntemini kullanarak mevcut bir bilgi bankasını güncelleştirir.

Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-python.md).

1. Sık kullandığınız IDE'de yeni bir Python projesi oluşturun.
1. Aşağıda sağlanan kodu ekleyin.
1. `subscriptionKey` değerini geçerli bir abonelik anahtarı ile değiştirin.
1. `kb` değerini geçerli bir bilgi bankası kimliği ile değiştirin. [Soru-Cevap Oluşturma bilgi bankalarınızdan](https://www.qnamaker.ai/Home/MyServices) birine giderek bu değeri bulun. Güncelleştirmek istediğiniz bilgi bankasını seçin. Söz konusu sayfaya geldiğinizde, aşağıda gösterildiği gibi URL’de 'kdid=' öğesini bulun. Kod örneğiniz için değerini kullanın.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

1. Programı çalıştırın.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, json, time

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace this with a valid subscription key.
subscriptionKey = 'ADD KEY HERE'

# Replace this with a valid knowledge base ID.
kb = 'ADD ID HERE';

# Represents the various elements used to create HTTP request path
# for QnA Maker operations.
host = 'westus.api.cognitive.microsoft.com'
service = '/qnamaker/v4.0'
method = '/knowledgebases/'

'''
Formats and indents JSON for display.
:param content: The JSON to format and indent.
:type: string
:return: Formatted and indented JSON.
:rtype: string
'''
def pretty_print(content):
    # Note: We convert content to and from an object so we can pretty-print it.
    return json.dumps(json.loads(content), indent=4)

'''
Sends a PATCH HTTP request.
:param path: The URL path of your request.
:param content: The contents of your PATCH request.
:type: string
:return: URL with status of PATCH request in updating the kb, actual response.
:rtype: string, string
'''
def update_kb(path, content):
    print('Calling ' + host + path + '.')
    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-Type': 'application/json',
        'Content-Length': len (content)
    }
    conn = http.client.HTTPSConnection(host)
    conn.request ("PATCH", path, content, headers)
    response = conn.getresponse ()
    return response.getheader('Location'), response.read ()

'''
Gets the status of the specified QnA Maker operation.
:param path: The URL of the request.
:type: string
:return: Header from retrying of the request (if retry is needed), response of the retry.
:rtype: string, string
'''
def check_status (path):
    print('Calling ' + host + path + '.')
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection(host)
    conn.request("GET", path, None, headers)
    response = conn.getresponse ()
    # If the operation is not finished, /operations returns an HTTP header named
    # 'Retry-After' with the number of seconds to wait before querying the operation again.
    return response.getheader('Retry-After'), response.read ()

'''
Dictionary that holds the knowledge base.
Modifications to the knowledge base are made here, using 'update', 'delete' and so on.
'''
req = {
    'add': {
        'qnaList': [
            {
              'id': 1,
              'answer': 'You can change the default message if you use the QnAMakerDialog. '
                      + 'See this for details:  https://docs.botframework.com/en-us'
                      + '/azure-bot-service/templates/qnamaker/#navtitle',
              'source': 'Custom Editorial',
              'questions': [
                  'How can I change the default message from QnA Maker?'
              ],
              'metadata': []
            }
        ],
        'urls': []
    },
    'update' : {
        'name' : 'New KB Name'
    },
    'delete': {
        'ids': [
            0
        ]
    }
}

# Builds the path URL.
path = service + method + kb
# Convert the request to a string.
content = json.dumps(req)
# Retrieve the operation ID to check status, and JSON result.
operation, result = update_kb(path, content)
# Print request response in JSON with presentable formatting.
print(pretty_print(result))

'''
Iteratively gets the operation state, updating the knowledge base.
Once state is no longer "Running" or "NotStarted", the loop ends.
'''
done = False
while False == done:
    path = service + operation
    # Gets the status of the operation.
    wait, status = check_status(path)
    # Print status checks in JSON with presentable formatting
    print(pretty_print(status))

    # Convert the JSON response into an object and get the value of the operationState field.
    state = json.loads(status)['operationState']
    # If the operation isn't finished, wait and query again.
    if state == 'Running' or state == 'NotStarted':
        print('Waiting ' + wait + ' seconds...')
        time.sleep (int(wait))
    else:
        done = True # request has been processed, if successful, knowledge base is updated
```

## <a name="understand-what-qna-maker-returns"></a>Soru-Cevap Oluşturma’nın ne döndürdüğünü anlama

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: Sonuçlarınız biraz farklılık gösterebilir. Son çağrı "Başarılı" durumunu döndürürse, bilgi bankanız başarıyla güncelleştirilmiştir. Sorun gidermek için Soru-Cevap Oluşturma API'sine ait [Bilgi Bankasını Güncelleştir](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yanıt kodlarına başvurun.

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:48Z",
  "userId": "2280ef5917rt4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:50Z",
  "resourceLocation": "/knowledgebases/140a46f3-b248-4f1b-9349-614bfd6e5563",
  "userId": "2280ef5917rt4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
Press any key to continue.
```

Bilgi bankanız güncelleştirildikten sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. Yeni bir Soru-Cevap çiftinin eklendiğini ve veritabanınızın artık 'New KB Name' olarak adlandırıldığını görebilirsiniz.

Bilgi bankanızın diğer öğelerini değiştirmek için Soru-Cevap Oluşturma [JSON şeması](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600)’na başvurun ve `req` dizesini değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
