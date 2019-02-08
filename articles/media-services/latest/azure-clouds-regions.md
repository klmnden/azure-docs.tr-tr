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
ms.date: 02/07/2019
ms.author: juliako
ms.openlocfilehash: cadf4c564ab8600094bc545ae7270eb9c0642e1e
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55890969"
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

## <a name="regionsgeographieslocations"></a>Bölge/coğrafyalar/konumları

* [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)
* [Ürün bölgeye göre](https://azure.microsoft.com/global-infrastructure/services/)
* [Azure coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/)

### <a name="region-code-name"></a>Bölge kodu adı 

Sağlamanız gerektiğinde **konumu** parametresi, bölge kod adı olarak sağlamak için ihtiyacınız **konumu** değeri. Hesabınızın bulunduğu ve aramanız için yönlendirileceğini bölge kodu adını almak için aşağıdaki satırı çalıştırabilirsiniz [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

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

## <a name="endpoints"></a>Uç Noktalar  

| Uç Nokta|Küresel Azure | Azure Kamu|Azure Almanya|Azure Çin 21Vianet|
| --- | --- | --- | --- | --- |
| Azure Resource Manager |  `https://management.azure.com/` | `https://management.usgovcloudapi.net/` | `https://management.cloudapi.de/` | `https://management.chinacloudapi.cn/` |
| Authentication | `https://login.microsoftonline.com/` | `https://login.microsoftonline.us/` | `https://login.microsoftonline.de/` | `https://login.chinacloudapi.cn/` |
| Belirteç hedef kitlesi | `https://management.core.windows.net/` | `https://management.core.usgovcloudapi.net/` | `https://management.core.cloudapi.de/`| `https://management.core.chinacloudapi.cn/` |

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 genel bakış](media-services-overview.md)
