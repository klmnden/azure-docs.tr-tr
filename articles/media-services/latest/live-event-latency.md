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
ms.openlocfilehash: a74f2e53127b506f42ff49018c3df2985396646d
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619834"
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
                // To use low latency optimally, you should tune your encoder settings down to 1 second GOP size instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Tam örneğe bakın: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="pass-through-liveevents-latency"></a>Doğrudan LiveEvents gecikme süresi

Aşağıdaki tabloda, Media Services, katkı akışın ne zaman bir oynatıcı kayıttan yürütme isteyebilir hizmetinin ulaştığında zamandan itibaren ölçülür (LowLatency bayrağı etkinleştirildiğinde) gecikme süresi için tipik sonuçları gösterilmektedir.

||2S GOP düşük gecikme süresi etkin|1s GOP düşük gecikme süresi etkin|
|---|---|---|
|AMP tire|10'luk bloklar|8s|
|HLS üzerinde yerel iOS player|14s|10'luk bloklar|

> [!NOTE]
> Uçtan uca gecikme süresi, yerel ağ koşullara bağlı olarak ya da bir CDN önbelleğe alma katmanı ile tanışın farklılık gösterebilir. Tam yapılandırmalarınızı test etmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akış genel bakış](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)

