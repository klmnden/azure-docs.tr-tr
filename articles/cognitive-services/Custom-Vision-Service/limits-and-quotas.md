---
title: Limitler ve kotalar - özel görüntü işleme hizmeti
titlesuffix: Azure Cognitive Services
description: Custom Vision Service için limitler ve kotalar hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: ce06effbce12abb6271e050829d3218f4fbbfbf4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902872"
---
# <a name="limits-and-quotas"></a>Limitler ve kotalar

Özel görüntü işleme hizmeti için anahtarların üç katmanı vardır. Azure portalı üzerinden F0 ve S0 kaynakları elde edilir. Fiyatlandırma ve işlemlerin tanımlarını ayrıntıları olan [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  S0 projelerine F0 projeleri yükseltilebilir.

Sınırlı deneme proje kaynaklarını ekli, özel görüntü işleme oturum açma (diğer bir deyişle, bir AAD hesabı veya MSA hesabı.) Kısa deneme hizmeti için kullanılmak üzere tasarlanmıştır.  Azure Önizleme (1 Mart 2018) giriş önce erken ücretsiz önizleme sırasında oluşturulan hesaplar için sınırlı deneme önceki kotalarını korur. 

||**Sınırlı deneme**|**F0 (Azure)**|**S0 (Azure)**|
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
|En yüksek görüntü boyutu (eğitim resmi karşıya yükleme) |6MB|6MB|6MB|
|En yüksek görüntü boyutu (tahmini)|4MB|4MB|4MB|

Sınırlamalar *# eğitim resmi proje başına* ve *# etiketleri/proje* S0 projeleri için zaman içinde artırılması planlanmaktadır. 
