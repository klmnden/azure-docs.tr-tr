---
title: "Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanıma - Duygu Tanıma API'si, Ruby"
titlesuffix: Azure Cognitive Services
description: Ruby ile Duygu Tanıma API'sini kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri edinin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: bcab24334c1ee4e47061ce6ea28bd60039e17b3f
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239038"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanımak için bir uygulama oluşturma.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Bu makalede bir görüntüdeki bir veya daha fazla kişinin duygularını tanımak için Ruby ile [Duygu Tanıma API'si Recognize metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri bulunmaktadır.

## <a name="prerequisite"></a>Önkoşul
* [Buradan](https://azure.microsoft.com/try/cognitive-services/) ücretsiz Abonelik Anahtarınızı alın

## <a name="recognize-emotions-ruby-example-request"></a>Duygu Tanıma Ruby Örnek İsteği

REST URL'sini abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin, "Ocp-Apim-Subscription-Key" değerinin yerine geçerli abonelik anahtarınızı yazın ve `body` değişkenine bir fotoğraf URL'si ekleyin.

```ruby
require 'net/http'

# NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
#   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
#   URL below with "westcentralus".
uri = URI('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize')
uri.query = URI.encode_www_form({
})

request = Net::HTTP::Post.new(uri.request_uri)
# Request headers
request['Content-Type'] = 'application/json'
# NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '13hc77781f7e4b19b5fcdd72a8df7156'
# Request body
request.body = "{\"url\":\"http://example.com/1.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts response.body

```

## <a name="recognize-emotions-sample-response"></a>Duygu Tanıma Örneği Yanıtı
Başarılı bir çağrı, yüz dikdörtgenine göre büyükten küçüğe doğru sıralanmış şekilde yüz girişlerinden ve ilgili duygu puanlarından oluşan bir dizi döndürür. Yanıtın boş olması hiç yüz algılanmadığını gösterir. Duygu girişi aşağıdaki alanları içerir:
* faceRectangle - Dikdörtgen şeklinde yüzün görüntü içindeki konumu.
* scores - Görüntüdeki her bir yüzün duygu puanı.

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
