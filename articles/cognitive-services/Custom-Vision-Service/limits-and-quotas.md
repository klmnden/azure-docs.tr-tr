---
title: Limitler ve kotalar - özel görüntü işleme hizmeti
titlesuffix: Azure Cognitive Services
description: Custom Vision Service için limitler ve kotalar hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: anroth
ms.openlocfilehash: 85abc4a50710629d3485d05115698e59a93fc96e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60995663"
---
# <a name="limits-and-quotas"></a>Limitler ve kotalar

Özel görüntü işleme hizmeti için anahtarların iki katmanı vardır. F0 (ücretsiz) veya Azure portalı üzerinden S0 (standart) abonelik için kaydolabilirsiniz. Buna karşılık gelen bkz [Bilişsel hizmetler fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) fiyatlandırma ve işlemler hakkında ayrıntı için.

Eğitim resmi her proje ve proje başına etiket sayısı zamanla S0 projeleri için artması beklenmektedir.

||**F0**|**S0**|
|-----|-----|-----|
|Projeler|2|100|
|Proje başına eğitim resmi |5,000|100,000|
|Öngörüler / ay|10,000 |Sınırsız|
|Etiketler / project|50|500|
|Yinelemeler |10|10|
|Etiketlenmiş resimleri etiketleme, sınıflandırma (50 önerilir ve üzeri) başına min |5|5|
|Etiketlenmiş resimleri etiketi, nesne algılama (50 önerilir ve üzeri) başına min|15|15|
|Öngörü görüntülerini saklama|30 gün|30 gün|
|[Tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleriyle depolama (işlem / saniye)|2|10|
|[Tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleri olmadan depolama (işlem / saniye)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API çağrıları / saniye)|2|10|
|[Diğer API çağrıları](https://go.microsoft.com/fwlink/?linkid=865446) (işlem / saniye)|10|10|
|En yüksek görüntü boyutu (eğitim resmi karşıya yükleme) |6 MB|6 MB|
|En yüksek görüntü boyutu (tahmini)|4 MB|4 MB|
