---
title: Video Indexer sahneler, görüntüleri ve ana kareleri - Azure
titlesuffix: Azure Media Services
description: Bu konu, Video Indexer sahneler, görüntüleri ve ana kareler hakkında genel bir bakış sağlar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: d312a93f83ef38fa1ae855a1e313280fc608948d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799395"
---
# <a name="scenes-shots-and-keyframes"></a>Sahneler, çekimler, ana kareler

Video Indexer, zamana bağlı birimleri yapısal ve anlamsal özelliklerine bağlı olarak ayırarak videoları destekler. Bu özellik, müşterilerin kolayca bulun, yönetin ve düzenleyin üzerinde çeşitli ayrıntı düzeylerinde temel video içeriklerinin sağlar. Örneğin, görünümler, görüntüleri ve ana kareleri, bu konuda açıklanan temel. **Sahne algılama** özelliği şu anda Önizleme aşamasındadır.   

![Sahneler, çekimler, ana kareler](./media/scenes-shots-keyframes/scenes-shots-keyframes.png)

## <a name="scene-detection-preview"></a>Sahne algılama (Önizleme)

Video Indexer, videoda üzerinde görsel ipuçları Sahne değiştiğinde belirler. Tek bir olayı Sahne gösterir ve anlamsal olarak ilişkili ardışık görüntüleri bir dizi oluşur. Sahne küçük resim, kendi temel görüntüsü ilk karedir. Video Indexer, video arasında ardışık görüntüleri renk modellenmiş üzerinde temel planda içine kesim ve başlangıç ve bitiş zamanı her sahnenin alır. Videoları anlam yönlerini niceleme içermesidir Sahne algılama zor bir görev olarak kabul edilir.

> [!NOTE]
> En az 3 içeren videolar için geçerlidir.

## <a name="shot-detection"></a>Kare algılama

Video Indexer bir kare değiştirildiğinde üzerinde görsel ipuçları, bitişik çerçeve ani ve aşamalı geçişler, renk düzenini izleyerek tabanlı video belirler. Görüntüsü ait meta verileri, bir başlangıç ve bitiş zamanı yanı sıra, bu görüntüsünde dahil ana kareleri listesini içerir. Aynı anda aynı kameradan alınan ardışık kare görüntülerini var.

## <a name="keyframe-detection"></a>Ana kare algılama

En iyi görüntüsü temsil eden çerçeve(leri) seçer. Ana kareleri estetik özelliklerine (örneğin, karşıtlık ve stableness) bağlı tüm video seçildiği temsili karelerdir. Video Indexer, ana kare kimlikleri görüntüsü ait meta verileri, hangi müşteriler ana kare küçük ayıklayabilmeniz için temel bir parçası olarak bir listesini alır. 

Ana kareleri JSON çıkışında görüntüleri ile ilişkilidir. 

## <a name="next-steps"></a>Sonraki adımlar

[API'sı tarafından üretilen Video dizinleyici çıktısını İnceleme](video-indexer-output-json-v2.md#scenes)