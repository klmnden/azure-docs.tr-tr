---
title: Görüntü İşleme Hızlı Başlangıç Özeti
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçlarda Görüntü İşleme API’sini kullanarak bir görüntüyü analiz edecek, küçük resim oluşturacak, basılı ve el yazısı metinleri ayıklayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.openlocfilehash: e44eb52323a93bddd629f3591cdbfbf021768629
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344107"
---
# <a name="quickstart-summary"></a>Hızlı başlangıç: Özet

Bu hızlı başlangıçlar Görüntü İşleme hizmetini hızlıca kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri sunar.

Örnekler, Görüntü İşleme API'sine doğrudan HTTP çağrıları gönderir. HTTP çağrılarını sarmalayan kolaylık yöntemleri sunan Görüntü İşleme istemci kitaplıklarını kullanan örnekler için *SDK Hızlı Başlangıçları* bölümüne bakın.

Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

Görüntü İşleme hizmetini kullanarak aşağıdaki görevleri tamamlayabilirsiniz:

* Uzak resmi çözümleme
* Yerel resmi çözümleme
* Ünlüleri ve önemli yerleri belirleme (etki alanı modelleri)
* Akıllıca küçük resim oluşturma
* Görüntüdeki basılı metni (OCR) algılama ve ayıklama
* Görüntüdeki el yazısını algılama ve ayıklama

Örneklerdeki kodlar birbirine benzerdir. Ancak farklı Görüntü İşleme özelliklerini ve hizmetler veri alışverişi yapmak için farklı yöntemleri vurgular:

* _Küçük resim oluşturma_, bir görüntüyü yanıt gövdesinde bayt dizisi olarak döndürür.
* _Yerel resmi çözümleme_, görüntünün isteğe bayt dizisi olarak dahil edilmesini gerektirir.
* _El yazısı metni ayıklama_, metni almak için iki çağrı gerektirir.

## <a name="summary"></a>Özet

| Hızlı Başlangıç               | İstek Parametreleri                          | Yanıt          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Uzak resmi çözümleme   | visualFeatures=Categories,Description,Color | JSON dizesi       |
| Yerel resmi çözümleme    | data=image_data (byte array)                | JSON dizesi       |
| Ünlüleri belirleme       | model=celebrities                           | JSON dizesi       |
| Küçük resim oluşturma     | width=200&height=150&smartCropping=true     | bayt dizisi        |
| Yazdırılan metni ayıklama     | language=unk&detectOrientation=true         | JSON dizesi       |
| El yazısı metni ayıklama | handwriting=true                            | URL, JSON dizesi  |

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve önemli yerleri algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API'lerini keşfedin.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'lerini keşfedin](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
