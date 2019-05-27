---
title: Bing Yazım Denetimi API’si nedir?
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi bağlamsal yazım denetimi için makine öğrenimi ve istatistiksel makine çevirisi kullanan API'si hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 22f75efb3cb4baa645030e7ad64072674de662ed
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "60593200"
---
# <a name="what-is-the-bing-spell-check-api"></a>Bing Yazım Denetimi API’si nedir?

Bing yazım denetimi API'si, bağlamsal gramer ve yazım metin denetimi gerçekleştirmenizi sağlar. Sözlüğe dayalı kural kümeleri üzerinde yazım denetleyicileri kullanır, ancak makine öğrenimi ve istatistiksel makine çevirisi doğru ve bağlamsal düzeltmeleri sağlamak için Bing yazım denetleyicisi yararlanır. 

## <a name="features"></a>Özellikler


|  |  |
|---------|---------|
|Birden çok yazım denetimi modları     | Yazım denetimi modları birden çok dil bilgisi ve/veya yazım odaklanmış düzeltmeleri yararlanmanıza olanak tanıyacak. |
|Argo ve resmi olmayan dil tanıma     | Yaygın ifadeler ve metinde kullanılan resmi olmayan terimler tanır.         |
|Benzer kelimeler arasındaki ayırt     | Sözcükler arasındaki doğru kullanım ilgili ses benzer Bul ancak anlamını (örneğin, "see" ve "sea") içinde farklı        |
|Marka, başlık ve popüler kullanım desteği     | Yeni markalar, başlıklar ve diğer popüler ifadeler çıkan tanıması |

## <a name="workflow"></a>İş Akışı

Bing yazım denetimi API'si, HTTP istekleri ve JSON yanıtlarını ayrıştırabilen herhangi programlama dilinden çağırmak kolay bir işlemdir. Hizmet REST API'si veya Bing yazım denetleme SDK'lar kullanarak erişilebilir. 

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa, ücretsiz bir hesap oluşturabilirsiniz. 
2. Bing Web araması API'si için bir istek gönderin.
3. JSON yanıtı ayrıştırılamadı

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing yazım denetimi arama API'sini deneyin [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/spell-check/) metinleri çeşitli nasıl hızlı bir şekilde denetleyebilirsiniz görmek için.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](../../cognitive-services/cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
