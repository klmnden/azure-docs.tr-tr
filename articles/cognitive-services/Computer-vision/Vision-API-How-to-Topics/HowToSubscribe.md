---
title: Görüntü işleme API'si için Abonelik anahtarları alma | Microsoft Docs
description: Abonelik anahtarları çağrılar için görüntü işleme API'si Bilişsel hizmetler almayı öğrenin.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.openlocfilehash: 682266df9e1a1f8209904697358d10bbaa633cf1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760276"
---
# <a name="how-to-obtain-subscription-keys"></a>Abonelik anahtarları edinme

Bilgisayar işleme Hizmetleri özel Abonelik anahtarları gerektirir. Görüntü işleme API'si yapılan her çağrı bir abonelik anahtarı gerektirir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir.

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

* [Microsoft Bilişsel API'ler için fiyatlandırma seçenekleri](https://azure.microsoft.com/pricing/details/cognitive-services/)
