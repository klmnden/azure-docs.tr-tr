---
title: Sınırları ve kotalar özel görme hizmeti - Azure Bilişsel hizmetler için | Microsoft Docs
description: Azure Cognitives özel görme Hizmetleri için kota ve sınırlar hakkında bilgi edinin.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 44666d5d7f2a51e4017c704205d21b1f6d06908c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355054"
---
# <a name="limits-and-quotas"></a>Limitler ve kotalar

Özel görme hizmeti için anahtarların üç katmanı vardır. F0 ve S0 kaynakları Azure portalı üzerinden elde edilir. Fiyatlandırma ve işlemleri tanımları hakkında ayrıntılar olan [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  F0 projeleri S0 projelerine yükseltilebilir.

Sınırlı deneme proje kaynakları, özel görme oturum açma (diğer bir deyişle, bir AAD hesabıyla veya MSA hesabı) bağlı Hizmet kısa denemeler için kullanılmak üzere tasarlanmıştır.  Azure Önizleme sürümleri (1 Mart 2018) giriş önce erken ücretsiz önizleme sırasında oluşturulan hesapların kendi önceki kotaları için sınırlı denemeler korur. 

||**Sınırlı deneme**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projeler|2|2|100|
|Her proje eğitimi görüntüleri|5.000|5.000|50,000|
|Tahminleri / ay|10,000 |10,000|Sınırsız|
|Etiketler / project|50|50|250|
|Yineleme |10|10|10|
|Etiketli Görüntü etiketi, sınıflandırma (50 + önerilen) başına en az |5|5|5|
|Etiketli Görüntü etiketi, nesne algılama (50 + önerilen) başına en az|15|15|15|
|Tahmin görüntüleri ne kadar süreyle depolanır|30 gün|30 gün|30 gün|
|[Tahmin](https://go.microsoft.com/fwlink/?linkid=865445) işlemleri depolama (işlemleri / saniye)|2|2|10|
|[Tahmin](https://go.microsoft.com/fwlink/?linkid=865445) operations olmadan depolama (işlemleri / saniye)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (API çağrılarının / saniye)|2|2|10|
|[Diğer API çağrıları](https://go.microsoft.com/fwlink/?linkid=865446) (işlemleri / saniye)|10|10|10|
|En büyük görüntü boyutu (eğitim görüntüyü karşıya yükleme) |6MB|6MB|6MB|
|En büyük görüntü boyutu (tahmin)|4MB|4MB|4MB|

Sınırlamalar *# eğitim görüntüleri her proje* ve *# etiketleri/project* S0 projeleri için zamanla artırılacak beklenir. 
