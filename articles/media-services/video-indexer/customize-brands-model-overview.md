---
title: Video Indexer - Azure markaları modelinde özelleştirme
titlesuffix: Azure Media Services
description: Bu makalede, bir Video Indexer markaları modeli nedir ve nasıl özelleştireceğinizi genel bir bakış sağlar.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: 15f4b8b6d4a27e721dfcaed8b9dd700e87e3a257
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459014"
---
# <a name="customize-a-brands-model-in-video-indexer"></a>Video Indexer markaları modelinde özelleştirme

Video Indexer, video ve ses içeriği ölçeklemek ve dizin oluşturma sırasında markaya algılama konuşma ve görsel metin destekler. Bahsetme ürünleri, hizmetleri, marka algılama özelliğini tanımlar ve şirketler Bing'in markaları veritabanı tarafından önerilen. Örneğin, Microsoft bir video veya ses içeriğini belirtilen veya bir video visual metinde görünür gerekiyorsa, Video Indexer, bir marka içerik olarak algılar. Markaları bağlamını kullanan diğer terimlerden disambiguated.

Marka algılama çok çeşitli içeriği Arşiv ve bulma, bağlamsal reklam, sosyal medya analizi gibi iş senaryosu için kullanışlıdır, perakende analizi ve daha birçok rekabet. Video Indexer marka algılama, konuşma ve Bing'e markaları veritabanını kullanarak görsel metin özelleştirme yanı sıra dizin marka bahsetmeleri her Video Indexer hesabınız için özel bir markaları modeli oluşturarak sağlar. Özel markaları model özellik Video Indexer veritabanından Bing markaları hariç engeller markaları algılandı (aslında markaları, bloke liste oluşturmaya) belirli markaları algılar ve parçası olması gereken markaları dahil et kullanılıp kullanılmayacağını seçmenize olanak sağlar, (aslında markaları beyaz listesi oluşturma) Bing'e markaları veritabanında olmayabilir modeli. Oluşturduğunuz özel markaları model yalnızca model oluşturulduğu hesapta kullanılabilir olacak.

## <a name="out-of-the-box-detection-example"></a>Dışında kutusu algılama örneği

İçinde [Microsoft Build 2017 2. gün](https://www.videoindexer.ai/media/ed6ede78ad/) sunumunu ve marka "Microsoft Windows" birden çok kez görünür. Bazen transkripti bazen görsel metin ve hiçbir zaman olarak aynen. Video Indexer ile yüksek düzeyde hassasiyet bir terimi aslında marka bağlamı, üzerinde 90 k hazır markaları kapsayan ve sürekli güncelleştirme dayalı olduğunu algılar. 02:25, Video Indexer, konuşma gelen marka algılar ve daha sonra tekrar 02:40 gelen görsel metin, hangi windows logo parçasıdır.

![Markaları genel bakış](./media/content-model-customization/brands-overview.png)

Yapı bağlamında windows hakkında konuşmak bağlamından belirsizliğinin ortadan kaldırılmasını ne yapılacağını bildiğiniz Gelişmiş Machine Learning algoritmaları temel kutu, Apple, Fox, vs. için bir marka ve aynı olarak "Windows" sözcüğü algılamaz. Marka algılaması bizim desteklenen tüm dillerde çalışır. İçin burayı tıklatın [tam Microsoft Build 2017 2. gün açılış konuşması videosu ve dizin](https://www.videoindexer.ai/media/ed6ede78ad/).

Kendi markaları getirmek için sonraki adımları denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Markaları modeli API'lerini kullanarak özelleştirme](customize-brands-model-with-api.md)

[Web sitesini kullanarak markaları modeli özelleştirme](customize-brands-model-with-website.md)
