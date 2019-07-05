---
title: 'Hızlı Başlangıç: Python – Form tanıyıcı kullanarak giriş verilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, satış makbuzuna görüntülerden verileri ayıklamak için Form tanıyıcı REST API ile Python kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: 8bd4d441859df6dbb36f594d8423eefd84274ec4
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592538"
---
# <a name="quickstart-extract-receipt-data-using-the-form-recognizer-rest-api-with-python"></a>Hızlı Başlangıç: Python ile Form tanıyıcı REST API kullanarak giriş verilerini ayıklama

Bu hızlı başlangıçta, ayıklayın ve satış makbuzuna ilgili bilgileri tanımlamak için Python ile Azure Form tanıyıcı REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlara sahip olmalısınız:
- Form tanıyıcı sınırlı erişim önizlemesine erişebilirsiniz. Önizleme erişim elde etmek için doldurun ve gönderme [Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu.
- [Python](https://www.python.org/downloads/) (örnek yerel olarak çalıştırmak istiyorsanız) yüklü.
- Görüntüyü bir giriş için bir URL. Kullanabileceğiniz bir [örnek görüntü](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/contoso-receipt.png?raw=true) Bu Hızlı Başlangıç için.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynak oluştur

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-receipt"></a>Bir giriş analiz edin

Bir giriş analiz etmeye başlamak için çağrı **analiz giriş** Python komut dosyası kullanarak API. Betiği çalıştırmadan önce şu değişiklikleri yapın:

1. Değiştirin `<Endpoint>` Form tanıyıcı abonelik anahtarınızı aldığınız uç noktası ile. Form tanıyıcı kaynağınızda bulabilirsiniz **genel bakış** sekmesi.
1. Değiştirin `<your receipt URL>` Giriş resminin URL adresine sahip.
1. Değiştirin `<subscription key>` önceki adımda kopyaladığınız abonelik anahtarı.

    ```python
    import http.client, urllib.request, urllib.parse, urllib.error, base64

    source = r"<your receipt URL>"
    body = {"url":source}
    body = json.dumps(body)

    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': '<subscription key>',
    }

    try:
        conn = http.client.HTTPSConnection('<Endpoint>')
        conn.request("POST", "/formrecognizer/v1.0-preview/prebuilt/receipt/asyncBatchAnalyze", body, headers)
        response = conn.getresponse()
        data = response.read()
        operationURL = "" + response.getheader("Operation-Location")
        print ("Operation-Location header: " + operationURL)
        conn.close()
    except Exception as e:
        print(e)
        exit()
    ```

1. Kod bir dosyayı .py uzantısıyla kaydedin. Örneğin, *form tanıyıcı receipts.py*.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python form-recognizer-receipts.py`.

Size gönderilecektir bir `202 (Success)` içeren yanıt bir **işlemi konumu** üst bilgi betik konsola yazdırır. Bu üst bilgi işlemin durumunu sorgulamak ve analiz sonuçları almak için kullanabileceğiniz bir işlem kimliği içeriyor. Aşağıdaki örnek değeri, sonra gelen dize içindeki `operations/` işlem kimliğidir.

```console
https://cognitiveservice/formrecognizer/v1.0-preview/prebuilt/receipt/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-receipt-results"></a>Giriş sonuçlar elde edin

Çağırdıktan sonra **analiz giriş** API'sini çağırırsınız **giriş sonuç alma** API işlemi ve Ayıklanan veriler durumunu almak için. Python betiğinizi altına aşağıdaki kodu ekleyin. Bu işlem kimlik değerini ayıklar ve yeni bir API çağrısına geçirir. Bu betik sonuçlarını kullanılabilir olana kadar düzenli aralıklarla API'yi çağıran için zaman uyumsuz bir işlemdir. Bir saniye veya daha fazla zaman aralığı öneririz.

```python
operationId = operationURL.split("operations/")[1]

conn = http.client.HTTPSConnection('<Endpoint>')
while True:
    try:
        conn.request("GET", f"/formrecognizer/v1.0-preview/prebuilt/receipt/operations/{operationId}", "", headers)
        responseString = conn.getresponse().read().decode('utf-8')
        responseDict = json.loads(responseString)
        conn.close()
        print(responseString)
        if 'status' in responseDict and responseDict['status'] not in ['NotStarted','Running']:
            break
        time.sleep(1)
    except Exception as e:
        print(e)
        exit()
```

1. Komut dosyasını kaydedin.
1. Yeniden kullanmak `python` örneği çalıştırmak için komutu. Örneğin, `python form-recognize-analyze.py`.

### <a name="examine-the-response"></a>Yanıtı inceleme

Analiz işlemi tamamlanana kadar komut yanıtlarını konsola yazdırır. Ardından, ayıklanan metin verileri JSON biçiminde yazdırılır. `"recognitionResults"` Alan alınmasından ayıklanan metin her satırı içerir ve `"understandingResults"` alan kaydınızda en uygun bölümleri için anahtar/değer bilgilerini içerir.

Aşağıdaki giriş resmi ve karşılık gelen kendi JSON çıkışa bakın. Çıkış okunabilir olması için kısaltıldı.

![Contoso Mağazası'ndan bir giriş](../media/contoso-receipt.png)

```json
{
  "status": "Succeeded",
  "recognitionResults": [{
    "page": 1,
    "clockwiseOrientation": 0.36,
    "width": 1688,
    "height": 3000,
    "unit": "pixel",
    "lines": [{
      "boundingBox": [616, 291, 1050, 278, 1053, 384, 620, 397],
      "text": "Contoso",
      "words": [{
        "boundingBox": [619, 292, 1051, 284, 1052, 382, 620, 398],
        "text": "Contoso"
      }]
    }, {
      "boundingBox": [322, 588, 501, 600, 497, 655, 318, 643],
      "text": "Contoso",
      "words": [{
        "boundingBox": [330, 590, 501, 602, 499, 654, 326, 644],
        "text": "Contoso"
      }]
    },
    ...
    ]
  }],
  "understandingResults": [{
    "pages": [1],
    "fields": {
      "Subtotal": {
        "valueType": "numberValue",
        "value": 1098.99,
        "text": "1098.99",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/14/words/1"
        }]
      },
      "Total": {
        "valueType": "numberValue",
        "value": 1203.39,
        "text": "1203.39",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/18/words/1"
        }]
      },
      "Tax": {
        "valueType": "numberValue",
        "value": 104.4,
        "text": "$104.40",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/16/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/16/words/1"
        }]
      },
      "MerchantAddress": {
        "valueType": "stringValue",
        "value": "123 Main Street Redmond, WA 98052",
        "text": "123 Main Street Redmond, WA 98052",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/2/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/2/words/1"
        }, {
          "$ref": "#/recognitionResults/0/lines/2/words/2"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/1"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/2"
        }]
      },
      "MerchantName": {
        "valueType": "stringValue",
        "value": "Contoso",
        "text": "Contoso",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/1/words/0"
        }]
      },
      "MerchantPhoneNumber": {
        "valueType": "stringValue",
        "value": null,
        "text": "123-456-7890",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/4/words/0"
        }]
      },
      "TransactionDate": {
        "valueType": "stringValue",
        "value": "2019-06-10",
        "text": "6/10/2019",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/5/words/0"
        }]
      },
      "TransactionTime": {
        "valueType": "stringValue",
        "value": "13:59:00",
        "text": "13:59",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/5/words/1"
        }]
      }
    }
  }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Form tanıyıcı REST API ile Python bir modeli eğitmek ve bir örnek senaryosunda çalıştırmak için kullanılır. Ardından, daha fazla ayrıntılı Form tanıyıcı API'sini keşfetmek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt)
