---
title: Azure Media Services Livestream gecikme | Microsoft Docs
description: Bu konuda Livestream gecikme genel bir bakış sağlar ve düşük gecikme süresi ayarlama işlemi gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: b167d3424d520cf8be515eec9d495164dd9785ab
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682104"
---
# <a name="liveevent-latency-in-media-services"></a>Media Services Livestream gecikme süresi

Bu makalede, düşük gecikme süresi ayarlamak gösterilmektedir bir **Livestream**. Ayrıca, düşük gecikme süresi ayarları çeşitli oynatıcılarda kullanırken göreceğiniz tipik sonuçları anlatılmaktadır. Sonuçları, CDN ve ağ gecikmesi göre değişir. 

Yeni **LowLatency** özelliği, ayarladığınız **StreamOptionsFlag** için **LowLatency** üzerinde **Livestream**. Akış, çalışır duruma geldikten sonra kullanabileceğiniz [Azure Media Player](http://ampdemo.azureedge.net/) (AMP) tanıtım sayfasını ve "Düşük gecikme süresi buluşsal yöntemler profili" kullanmak için kayıttan yürütme seçeneklerini ayarlayın.

Aşağıdaki .NET örnek nasıl ayarlanacağını gösterir **LowLatency** üzerinde **Livestream**:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second "Group Of Pictures" (GOP) length instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Tam örneğe bakın: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="liveevents-latency"></a>LiveEvents gecikme süresi

Aşağıdaki tablolarda, Media Services, bir Görüntüleyici oynatıcıda kayıttan yürütme gördüğünde hizmetinin katkı akış ulaştığında zamandan itibaren ölçülür (LowLatency bayrağı etkinleştirildiğinde) gecikme süresi için tipik sonuçları gösterir. Düşük gecikme süresi en iyi şekilde kullanmak için Kodlayıcı ayarlarınızı aşağı 1 saniye "Grubu, resimleri" (GOP) uzunluğunu ayarlayın. Daha yüksek bir GOP uzunluğu kullanırken, bant genişliği kullanımını en aza indirmek ve altında aynı kare hızı bit hızı azaltma. Daha az hareket videoları bu durum özellikle yararlıdır.

### <a name="pass-through"></a>Geçiş 

||2S GOP düşük gecikme süresi etkin|1s GOP düşük gecikme süresi etkin|
|---|---|---|
|AMP tire|10'luk bloklar|8s|
|HLS üzerinde yerel iOS player|14s|10'luk bloklar|

### <a name="live-encoding"></a>Live encoding

||2S GOP düşük gecikme süresi etkin|1s GOP düşük gecikme süresi etkin|
|---|---|---|
|AMP tire|14s|10'luk bloklar|
|HLS üzerinde yerel iOS player|18s|13s|

> [!NOTE]
> Uçtan uca gecikme süresi, yerel ağ koşullara bağlı olarak ya da bir CDN önbelleğe alma katmanı ile tanışın farklılık gösterebilir. Tam yapılandırmalarınızı test etmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)

