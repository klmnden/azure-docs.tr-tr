---
title: Video Indexer'ı ve Azure Media Services v3 hazır karşılaştırması | Microsoft Docs
description: Bu konuda, Video Indexer'ı ve Azure Media Services v3 hazır karşılaştırır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2019
ms.author: juliako
ms.openlocfilehash: 2c98f6d12f4868e5f90874fe3210fe5368f7ca2d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59270345"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Azure Media Services v3 hazır ve Video Indexer'ı karşılaştırma 

Bu makalede yeteneklerini karşılaştırır **Video Indexer API** ve **Media Services v3 API'ler**. 

Şu anda, sunduğu özellikler arasında bir çakışma var. [Video Indexer API](https://api-portal.videoindexer.ai/) ve [Media Services v3 API'ler](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). Aşağıdaki tabloda benzerlikleri ve farkları anlamak için geçerli bir kılavuz sağlar. 

## <a name="compare"></a>Karşılaştır

|Özellik|Video Indexer API |Video Çözümleyicisi ve ses Çözümleyicisi hazır<br/>Media Services v3 API'lerindeki|
|---|---|---|
|Media Insights|[Gelişmiş](video-indexer-output-json-v2.md) |[Temel Konular](../latest/intelligence-concept.md)|
|Deneyimleri|Desteklenen özelliklerin tam listesine bakın: <br/> [Genel Bakış](video-indexer-overview.md)|Video içgörüleri yalnızca döndürür|
|Faturalandırma|[Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Media Services fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Uyumluluk|[ISO 27001](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [ISO 27018](https://www.microsoft.com/trustcenter/Compliance/ISO-IEC-27018), [SOC 1,2,3](https://www.microsoft.com/TrustCenter/Compliance/SOC), [HIPAA](https://www.microsoft.com/trustcenter/compliance/hipaa), [FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/fedramp), [PCI](https://www.microsoft.com/trustcenter/compliance/pci)ve [ HITRUST](https://www.microsoft.com/TrustCenter/Compliance/hitrust) sertifikalı. En yeni güncelleştirmeler için ziyaret [Video Indexer'ın geçerli sertifikaları durumu](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).|Media Services, birçok sertifikaları ile uyumludur. Kullanıma [Azure uyumluluk Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) ve ilgilendiğiniz bir sertifika ile uyumlu olmadığını görmek "Media Services" için arama yapın.|
|Ücretsiz Deneme|Doğu ABD|Kullanılamaz|
|Bölge kullanılabilirliği|Doğu ABD 2, Orta Güney ABD, Batı ABD 2, Kuzey Avrupa, Batı Avrupa, Güneydoğu Asya, Güneydoğu Asya ve Avustralya Doğu.  En yeni güncelleştirmeler için ziyaret [bölgelere göre ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) sayfası.|Bkz: [Azure durumu](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Sonraki adımlar

[Video Indexer’a genel bakış](video-indexer-overview.md)

[Media Services v3 genel bakış](../latest/media-services-overview.md)
