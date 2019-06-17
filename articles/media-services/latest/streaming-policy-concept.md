---
title: Azure Media Services ilkeleri akış | Microsoft Docs
description: Bu makalede, akış ilkeleri nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: a813c77e81e51bfe13e75ed6c8d0e24b4d0fa645
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66392919"
---
# <a name="streaming-policies"></a>Akış İlkeleri

Azure Media Services v3 içinde [akış ilkeleri](https://docs.microsoft.com/rest/api/media/streamingpolicies) , akış protokolleri ve şifreleme seçeneklerini tanımlamanıza olanak sağlar, [akış bulucuları](streaming-locators-concept.md). Media Services v3, böylece bunları doğrudan deneme veya üretim için kullanabileceğiniz bazı önceden tanımlanmış akış ilkeleri sağlar. 

Şu anda kullanılabilir akış ilkeleri önceden tanımlanmış:<br/>
* 'Predefined_DownloadOnly'
* 'Predefined_ClearStreamingOnly'
* 'Predefined_DownloadAndClearStreaming'
* 'Predefined_ClearKey'
* 'Predefined_MultiDrmCencStreaming' 
* 'Predefined_MultiDrmStreaming'

Aşağıdaki "karar ağacı" senaryonuz için önceden tanımlanmış bir akış ilkesini seçmenize yardımcı olur.

> [!IMPORTANT]
> * Özelliklerini **akış ilkeleri** DateTime türü her zaman UTC biçiminde olan.
> * Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarlayın ve aynı seçeneklere gerektiğinde bunları yeniden için akış Bulucular gerekir. Daha fazla bilgi için [kotaları ve sınırlamaları](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Karar ağacı

Resmi tam boyutlu görüntülemek için tıklayın.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

İçeriğinizi şifreleme oluşturmak gerekirse bir [içerik anahtarı ilke](content-key-policy-concept.md), **içerik anahtarı ilke** Temizle akış veya yükleme için gerekli değildir. 

(Örneğin, farklı protokollere belirtmek istiyorsanız bir özel anahtar dağıtımı hizmetiyle kullanmanız gerekir veya bir Temizle ses kaydı kullanmak ihtiyacınız varsa) özel gereksinimleriniz varsa [oluşturma](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) akış özel bir ilke. 

## <a name="get-a-streaming-policy-definition"></a>Bir akış ilke tanımı Al  

Akış bir ilke tanımı'nı görmek istiyorsanız, kullanın [alma](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) ve ilke adı belirtin. Örneğin:

### <a name="rest"></a>REST

İstek:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Yanıt:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslimat hizmeti kullanın](protect-with-drm.md)
