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
ms.date: 05/07/2019
ms.author: juliako
ms.openlocfilehash: 7b2691f543cf38a56eefb1e8521169aeccbf3221
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65409277"
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

[Azure Media Services hizmeti dağıtıldığı bölgeler](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)

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

Aşağıdaki uç noktaların Media Services hesapları için farklı Ulusal Azure bulutlarına bağlanırken bilmek önemlidir.

### <a name="global-azure"></a>Genel Azure

|Uç Noktalar ||
| --- | --- | 
| Azure Resource Manager |  `https://management.azure.com/` |
| Kimlik Doğrulaması | `https://login.microsoftonline.com/` | 
| Belirteç hedef kitlesi | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Kamu

|Uç Noktalar||
| --- | --- | 
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| Kimlik Doğrulaması | `https://login.microsoftonline.us/` | 
| Belirteç hedef kitlesi | `https://management.core.usgovcloudapi.net/` |

### <a name="azure-germany"></a>Azure Almanya

| Uç Noktalar ||
| --- | --- |  
| Azure Resource Manager | `https://management.cloudapi.de/` |
| Kimlik Doğrulaması | `https://login.microsoftonline.de/` |
| Belirteç hedef kitlesi | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure Çin 21Vianet

|Uç Noktalar||
| --- | --- | 
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| Kimlik Doğrulaması | `https://login.chinacloudapi.cn/` |
| Belirteç hedef kitlesi |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>Ayrıca bkz.

* [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions/)
* [Azure coğrafyaları](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 genel bakış](media-services-overview.md)
