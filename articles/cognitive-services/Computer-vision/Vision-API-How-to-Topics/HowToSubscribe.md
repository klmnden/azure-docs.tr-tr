---
title: Abonelik anahtarları - görüntü işleme alın
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel hizmetler görüntü işleme API'si çağrılarını abonelik anahtarlarını almak öğrenin.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 820531cc2254d9cbc7aaf7e758dd0457b282d892
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580816"
---
# <a name="how-to-obtain-subscription-keys"></a>Abonelik anahtarları edinme

Bilgisayar işleme Hizmetleri özel Abonelik anahtarları gerektirir. Görüntü İşleme API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir.

Abonelik anahtarları için kaydolmak için bkz: [abonelikleri](https://azure.microsoft.com/try/cognitive-services/). Kaydolmak ücretsizdir. Bu hizmetler için fiyatlandırma tabi değişikliktir.

>[!NOTE]
Abonelik anahtarlarınızın bunlardan yalnızca biri için geçerli olan [Microsoft Azure bölgeleri](https://azure.microsoft.com/regions/). 

| Bölge | Adres |
|---|---|
| Batı ABD | westus.api.cognitive.microsoft.com |
| Doğu ABD 2 | eastus2.api.cognitive.microsoft.com |
| Batı Orta ABD | westcentralus.api.cognitive.microsoft.com |
| Batı Avrupa | westeurope.api.cognitive.microsoft.com |
| Güneydoğu Asya | southeastasia.api.cognitive.microsoft.com |

Görüntü işleme ücretsiz deneme sürümünü kullanmaya kaydolun, abonelik anahtarlarınızın geçerli **westcentral** bölge (`https://westcentralus.api.cognitive.microsoft.com/`). En yaygın bir durumdur. Ancak, Microsoft Azure ile görüntü işleme için aracılığıyla hesabı [ https://azure.microsoft.com/ ](https://azure.microsoft.com/) Web sitesi, bölgelerin önceki listede deneme sürenizi bölgeyi belirtin.

Örneğin, Microsoft Azure hesabınızı ve ile görüntü işleme, kaydolun, belirtin `westus` bölgeniz için kullanmalısınız `westus` bölge için REST API çağrılarınıza (`https://westus.api.cognitive.microsoft.com/`).

Deneme anahtarınızı aldıktan sonra bölge için abonelik anahtarınızı unutursanız, bölge düzeyinde bulabilirsiniz [ https://azure.microsoft.com/try/cognitive-services/my-apis/ ](https://azure.microsoft.com/try/cognitive-services/my-apis/).

### <a name="related-links"></a>İlgili bağlantılar:

* [Azure Bilişsel hizmetler API'leri için fiyatlandırma seçenekleri](https://azure.microsoft.com/pricing/details/cognitive-services/)
