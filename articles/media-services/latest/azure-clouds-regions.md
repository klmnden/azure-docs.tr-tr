---
title: Bulut ve hangi Azure Media Services v3 kullanılabilir bölgeleri | Microsoft Docs
description: Bu makalede, Azure bulutlarında ve hangi Azure Media Services v3 kullanılabilir bölgeler hakkında konuşuyor.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/11/2019
ms.author: juliako
ms.openlocfilehash: 8eb49010d89c3039f46e5c84cd305b7d0b5ca025
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54307014"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Bulut ve bölgelerde hangi Azure Media Services v3 var.

Azure Media Services v3, Azure Resource Manager bildiriminde genel Azure, Azure kamu, Azure Almanya, Azure Çin 21Vianet aracılığıyla kullanılabilir. Ancak, Azure bulutlarında Media Services özelliklerinin tamamı kullanılabilir. Bu belge, temel Media Services v3 bileşenlerinin desteklenmediği özetlenmektedir.

## <a name="feature-availability-in-azure-clouds"></a>Azure bulutlarında Özellik kullanılabilirliği

| Özellik|Küresel Azure bölgeleri | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Kullanılabilir | Kullanılamaz | Kullanılamaz | Kullanılamaz |
| [StandardEncoderPreset](encoding-concept.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| [LiveEvents](live-streaming-overview.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |
| [Akış](streaming-endpoint-concept.md) | Kullanılabilir | Kullanılabilir | Kullanılabilir | Kullanılabilir |

## <a name="regions"></a>Bölgeler 

Sağlamanız gerektiğinde **konumu** parametresi, bölge kod adı olarak sağlamak için ihtiyacınız **konumu** değeri. Hesabınızın bulunduğu ve aramanız için yönlendirileceğini bölge kodu adını almak için aşağıdaki satırı çalıştırabilirsiniz [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

```bash
az account list-locations
```

Bir kez yukarıda gösterilen tüm Azure bölgelerinde listesini alma satırı çalıştırın. Sahip Azure bölgesine gidin *displayName* aradığınız ve kullanmak, *adı* değerini **konumu** parametresi.

Örneğin, Azure bölgesinde Batı ABD 2 (aşağıda gösterilen), "westus2" için kullanacağınız **konumu** parametresi.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 genel bakış](media-services-overview.md)
