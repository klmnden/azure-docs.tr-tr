---
title: "Hızlı Başlangıç: CURL - duygu tanıma API'si, resimdeki yüz temel duyguları tanıma"
titlesuffix: Azure Cognitive Services
description: cURL ile Duygu Tanıma API'sini kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri edinin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd157bfeff8ceaa18a75f15895b7304385c5e522
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56232046"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Hızlı Başlangıç: Görüntüdeki yüzleri üzerinde duyguları tanıma için bir uygulama oluşturun.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur.

Bu makalede bir görüntüdeki bir veya daha fazla kişinin duygularını tanımak için cURL ile [Duygu Tanıma API'si Recognize metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri bulunmaktadır.

## <a name="prerequisite"></a>Önkoşul
* [Buradan](https://azure.microsoft.com/try/cognitive-services/) ücretsiz Abonelik Anahtarınızı alın

## <a name="recognize-emotions-curl-example-request"></a>Duygu Tanıma cURL Örnek İsteği

> [!NOTE]
> REST çağrınızda abonelik anahtarlarınızı almak için kullandığınız konumu kullanmanız gerekir. Örneğin, güvenlik anahtarlarınızı westcentralus konumundan aldıysanız, aşağıdaki URL’de "westus" değerini "westcentralus" olarak değiştirin.

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
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
