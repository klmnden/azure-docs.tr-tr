---
title: 'Hızlı Başlangıç: CURL - Form tanıyıcı kullanarak giriş verilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, satış makbuzuna görüntülerden verileri ayıklamak için Form tanıyıcı REST API ile cURL kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: f9111dcb1f85b28a688282f792540128e7146a6b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566186"
---
# <a name="quickstart-extract-receipt-data-using-the-form-recognizer-rest-api-with-curl"></a>Hızlı Başlangıç: Form tanıyıcı REST API ile cURL kullanarak giriş verilerini ayıklama

Bu hızlı başlangıçta, ayıklayın ve satış makbuzuna ilgili bilgileri tanımlamak için Azure Form tanıyıcı REST API ile cURL kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlara sahip olmalısınız:
- Form tanıyıcı sınırlı erişim önizlemesine erişebilirsiniz. Önizleme erişim elde etmek için doldurun ve gönderme [Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu.
- [cURL](https://curl.haxx.se/windows/) yüklü.
- Görüntüyü bir giriş için bir URL. Kullanabileceğiniz bir [örnek görüntü](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/contoso-receipt.png?raw=true) Bu Hızlı Başlangıç için.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynak oluştur

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-receipt"></a>Bir giriş analiz edin

Bir giriş analiz etmeye başlamak için çağrı **analiz giriş** aşağıdaki cURL komutu kullanarak API. Komutu çalıştırmadan önce şu değişiklikleri yapın:

1. Değiştirin `<Endpoint>` Form tanıyıcı abonelik anahtarınızı aldığınız uç noktası ile. Form tanıyıcı kaynağınızda bulabilirsiniz **genel bakış** sekmesi.
1. Değiştirin `<your receipt URL>` Giriş resminin URL adresine sahip.
1. Değiştirin `<subscription key>` önceki adımda kopyaladığınız abonelik anahtarı.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/prebuilt/receipt/asyncBatchAnalyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"url\": \"<your receipt URL>\"}"
```

Size gönderilecektir bir `202 (Success)` içeren yanıt bir **işlemi konumu** başlığı. İşlemin durumunu sorgulamak ve sonuçları almak için kullanabileceğiniz bir işlem kimliği bu üstbilgisinin değerini içerir. Aşağıdaki örnekte, sonra dize `operations/` işlem kimliğidir.

```console
https://cognitiveservice/formrecognizer/v1.0-preview/prebuilt/receipt/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-receipt-results"></a>Giriş sonuçlar elde edin

Çağırdıktan sonra **analiz giriş** API'sini çağırırsınız **giriş sonuç alma** API işlemi ve Ayıklanan veriler durumunu almak için.

1. Değiştirin `<operationId>` önceki adımdan gelen işlem Kimliğine sahip.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v1.0-preview/prebuilt/receipt/operations/<operationId>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Yanıtı inceleme

Size gönderilecektir bir `200 (Success)` JSON çıkışını yanıtıyla. İlk alanı `"status"`, işlemin durumunu gösterir. İşlem tamamlandığında `"recognitionResults"` alan her alınmasından ayıklanan metin satırı içeriyor ve `"understandingResults"` alan kaydınızda en uygun bölümleri için anahtar/değer bilgilerini içerir. İşlemi tam değilse, değeri `"status"` olacaktır `"Running"` veya `"NotStarted"`, ve tekrar API'yi çağırması gerekir el ile veya bir komut dosyası aracılığıyla. Bir saniye veya daha fazla çağrıları arasında bir aralık öneririz.

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

Bu hızlı başlangıçta, Form tanıyıcı REST API ile cURL satış giriş içeriğini ayıklamak için kullanılır. Ardından, daha fazla ayrıntılı Form tanıyıcı API'sini keşfetmek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt)
