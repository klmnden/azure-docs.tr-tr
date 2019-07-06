---
title: "Hızlı Başlangıç: CURL ve Azure REST API'si ile bir görüntüdeki yüzleri algılayın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir resimdeki yüz algılama için Azure yüz REST API ile cURL kullanır.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 68cdd147977294954051735d70307305aa5dc0cb
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603321"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-curl"></a>Hızlı Başlangıç: CURL ve yüz tanıma REST API'si ile bir resimdeki yüz algılama

Bu hızlı başlangıçta, bir resimdeki İnsan yüzlerini algılamak için cURL ile Azure yüz REST API'sini kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.

## <a name="write-the-command"></a>Komut yazma
 
Yüz tanıma API'sini çağırmak ve bir görüntüden yüz özniteliği veri almak için aşağıdakine benzer bir komut kullanırsınız. İlk olarak, kodu bir metin düzenleyicisine kopyalayın&mdash;çalışabilmesi için önce belirli bölümlerini komutu için değişiklik yapmanız.

```shell
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Content-Type: application/json" --data-ascii "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}"
```

### <a name="subscription-key"></a>Abonelik anahtarı
Değiştirin `<Subscription Key>` geçerli yüz abonelik anahtarınız ile.

### <a name="face-endpoint-url"></a>Yüz tanıma uç nokta URL'si

URL `https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect` Azure yüz uç noktaya sorgu gösterir. Bu URL, abonelik anahtarınızı karşılık gelen bölge eşleştirilecek ilk bölümünü değiştirmeniz gerekebilir. Bkz: [yüz tanıma API'si belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) tüm bölge uç noktaları listesi.

### <a name="url-query-string"></a>URL sorgu dizesi

Yüz tanıma uç nokta URL'sinin sorgu dizesini almak için hangi yüz öznitelikleri belirtir. Öngörülen kullanımınıza bağlı olarak bu dize değiştirmek isteyebilirsiniz.

```
?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise
```

### <a name="image-source-url"></a>Görüntü kaynağı URL'si
Kaynak URL, giriş olarak kullanılacak görüntüyü gösterir. Bu, çözümlemek istediğiniz herhangi bir görüntüye işaret edecek şekilde değiştirebilirsiniz.

```
https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg
``` 

## <a name="run-the-command"></a>Yeni bir “kurtarma VM’si” oluşturmak ve sorunlu VM’nin işletim sistemi diskini kurtarma VM’sine veri diski olarak takmak için

Değişikliklerinizi yaptıktan sonra bir komut istemi açın ve yeni bir komut girin. Konsol penceresinde JASON verileri olarak görüntülenen yüz bilgileri görmeniz gerekir. Örneğin:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir resimdeki yüz algılama ve onların öznitelikleri döndürmek için Azure yüz tanıma API'si çağıran bir cURL komutu yazıldı. Ardından, daha fazla bilgi için yüz API başvuru belgeleri keşfedin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API’si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
