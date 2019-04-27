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
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 30cd77799837f9b1ef08a9c609e518fd679b9b15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322310"
---
# <a name="live-event-latency-in-media-services"></a>Media Services canlı olay gecikme süresi

Bu makalede, düşük gecikme süresi ayarlamak gösterilmektedir bir [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents). Ayrıca, düşük gecikme süresi ayarları çeşitli oynatıcılarda kullanırken göreceğiniz tipik sonuçları anlatılmaktadır. Sonuçları, CDN ve ağ gecikmesi göre değişir.

Yeni **LowLatency** özelliği, ayarladığınız **StreamOptionsFlag** için **LowLatency** üzerinde **Livestream**. Oluştururken [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) HLS kayıttan yürütme için ayarlanmış [LiveOutput.Hls.fragmentsPerTsSegment](https://docs.microsoft.com/rest/api/media/liveoutputs/create#hls) 1. Akış, çalışır duruma geldikten sonra kullanabileceğiniz [Azure Media Player](https://ampdemo.azureedge.net/) (AMP tanıtım sayfası) ve "Düşük gecikme süresi buluşsal yöntemler profili" kullanmak için kayıttan yürütme seçeneklerini ayarlayın.

> [!NOTE]
> Şu anda, Azure Media Player LowLatency HeuristicProfile Akış Protokolü DASH veya HLS CMAF ile kayıttan yürütme için tasarlanmıştır. HLS TS ile MacOS ya da iOS aygıtına hedeflediğiniz durumlarda (örneğin, `format=m3u8-aapl` veya `format=m3u8-aapl-v3`), AMP, doğrudan bu durumda, işletim sistemi tarafından sağlanan yerel Yürütücü kullandığı için bu ayarı kullanmamanız gerekir.

Aşağıdaki .NET örnek nasıl ayarlanacağını gösterir **LowLatency** üzerinde **Livestream**:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Standard to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
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

## <a name="live-events-latency"></a>Canlı olayları gecikme süresi

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

