---
title: Fiyatlandırma ve limitler - özel görüntü işleme hizmeti
titlesuffix: Azure Cognitive Services
description: Custom Vision Service için limitler ve kotalar hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: a3fdd39cdbd4204fece145bde23b23e155500bdb
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351397"
---
# <a name="pricing-and-limits"></a>Fiyatlandırma ve limitler

Özel görüntü işleme hizmeti için anahtarların üç katmanı vardır. Sınırlı deneme proje kaynakları, özel görüntü işleme oturum açma (diğer bir deyişle, bir Azure Active Directory hesabı veya MSA hesabı) eklenir. Kısa deneme hizmeti için kullanılmak üzere tasarlanmıştır. F0 (ücretsiz) veya Azure portalı üzerinden S0 (standart) abonelik için kaydolabilirsiniz. Buna karşılık gelen bkz [Bilişsel hizmetler fiyatlandırması sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) fiyatlandırma ve işlemler hakkında ayrıntı için.

Azure Önizleme (1 Mart 2018), giriş önce erken ücretsiz önizleme sırasında oluşturulan hesaplar için sınırlı deneme önceki kotalarını korur.

Eğitim resmi her proje ve proje başına etiket sayısı zamanla S0 projeleri için artması beklenmektedir.

||**Sınırlı deneme**|**F0**|**S0**|
|-----|-----|-----|-----|
|Projeler|2|2|100|
|Her proje, sınıflandırma eğitim resmi|5.000|5.000|50,000|
|Her proje, nesne algılama eğitim resmi|5.000|5.000|10,000|
|Öngörüler / ay|10,000 |10,000|Sınırsız|
|Etiketler / project|50|50|250|
|Yinelemeler |10|10|10|
|Etiketlenmiş resimleri etiketleme, sınıflandırma (50 önerilir ve üzeri) başına en az |5|5|5|
|Etiketlenmiş resimleri etiketi, nesne algılama (50 önerilir ve üzeri) başına en az|15|15|15|
|Öngörü görüntülerini saklama|30 gün|30 gün|30 gün|
|[Tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleriyle depolama (işlem / saniye)|2|2|10|
|[Tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleri olmadan depolama (işlem / saniye)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API çağrıları / saniye)|2|2|10|
|[Diğer API çağrıları](https://go.microsoft.com/fwlink/?linkid=865446) (işlem / saniye)|10|10|10|
|En yüksek görüntü boyutu (eğitim resmi karşıya yükleme) |6 MB|6 MB|6 MB|
|En yüksek görüntü boyutu (tahmini)|4 MB|4 MB|4 MB|