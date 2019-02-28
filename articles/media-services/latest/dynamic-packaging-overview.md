---
title: Azure Media Services dinamik paketlemeye genel bakış | Microsoft Docs
description: Konu, Media Services dinamik paketleme genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2019
ms.author: juliako
ms.openlocfilehash: cd4eacc918acdf50bb256077030b86e121f663f0
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985810"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve çeşitli istemci teknolojiler (örneğin, iOS ve XBOX gibi) için içerik koruma biçimlendirir. Bu istemciler farklı protokollere anlamak, kesintisiz akış, bir HTTP canlı akışı (HLS) biçimi ve Xbox gerektiren iOS örneğin gerektirir. (Çoklu bit hızı) bit hızı Uyarlamalı bir kümeniz varsa MP4 (ISO temel medya 14496-12) dosyaları veya HLS, MPEG DASH veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesini dinamik avantajlarından faydalanabilirsiniz Paketleme. Paketleme video çözümü belirsiz olduğundan, SD/HD/UHD - 4K desteklenir.

[Akış uç noktaları](streaming-endpoint-concept.md) istemci oyuncular medya içeriği teslim etmek için kullanılan Media Services dinamik paketleme hizmetidir. Dinamik paketleme, tüm standart gelen bir özelliktir **akış uç noktalarını** (standart veya Premium). Hiçbir ek yok. Bu özellik, Media Services v3 ile ilişkili maliyeti. 

Yararlanmak için **dinamik paketleme**, ihtiyacınız bir **varlık** Uyarlamalı bit hızı MP4 dosyaları ve bildirim dosyalarında bir dizi. Dosyaları almak için bir ara (kaynak) dosyanızı Media Services ile kodlanacak yoludur. Oluşturmak zorunda video kayıttan yürütme için istemciler tarafından kullanılabilir varlık (ile kodlanmış MP4 ve sunucu ve istemci bildirimlerini) sağlamak için bir **akış Bulucu** ve daha sonra akış URL'lerini oluşturabilirsiniz. Ardından, istemci bildirimi belirtilen biçime bağlı olarak, akışın seçtiğiniz protokolde alırsınız.

Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar. 

Media Services'de, dinamik paketleme, Canlı veya isteğe bağlı Akış olup olmadığını kullanılır. Aşağıdaki diyagram, talep üzerine akış dinamik paketleme iş akışıyla gösterir.

![Dinamik kodlama](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

## <a name="common-video-on-demand-workflow"></a>Video isteğe bağlı ortak iş akışı

Dinamik paketleme kullanıldığı akış iş akışı ortak bir Media Services verilmiştir.

1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için bkz. [Media Encoder Standard tarafından desteklenen biçimleri](media-encoder-standard-formats.md).
2. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın.
3. Hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın. Yayımladığınız oluşturarak bir **akış Bulucu**.
4. Farklı biçimlerde (HLS, Dash ve kesintisiz akış) hedef URL'leri oluşturun. **Akış uç noktası** hizmet istekleri için bu farklı biçimler ve doğru bildirimi ilgileniriz.

## <a name="encode-to-adaptive-bitrate-mp4s"></a>Uyarlamalı bit hızı MP4 için kodlama

Hakkında bilgi için [Media Services ile bir video kodlama](encoding-concept.md), aşağıdaki örneklere bakın:

* [HTTPS kullanarak yerleşik hazır bir URL'den kodlayın](job-input-from-http-how-to.md)
* [Yerleşik hazır kullanarak yerel bir dosya kodlama](job-input-from-local-file-how-to.md)
* [Özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden derleme](customize-encoder-presets-how-to.md)

Media Encoder Standard biçimleri ve codec bileşenleri listesi için bkz. [biçimleri ve codec bileşenleri](media-encoder-standard-formats.md)

## <a name="delivery-protocols"></a>Teslim protokollerine

|Protokol|Örnek|
|---|---|
|HLS V4 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl)`|
|HLS V3 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl-v3)`|
|HLS CMAF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-cmaf)`|
|MPEG DASH CSF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-csf)` |
|MPEG DASH CMAF|`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-cmaf)` |
|Kesintisiz Akış| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest`|

## <a name="video-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen video codec bileşenleri

Dinamik paketleme ile kodlanmış bir video içeren MP4 dosyalarını destekler [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC veya AVC1) [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1 veya hvc1).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen ses codec bileşenleri

Dinamik paketleme ile kodlanmış bir ses içeren MP4 dosyalarını destekler [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Gelişmiş AC 3 veya E-AC3) veya [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Hızlı, DTS LBR, DTS HD kayıpsız DTS HD).

> [!NOTE]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="manifest-files-overview"></a>Bildirim dosyaları genel bakış

A **bildirim** (çalma listesi) dosyası (metin tabanlı veya XML tabanlı) içeren meta verileri gibi akış: izleme türü (ses, video veya metin), izleme adı, başlangıç ve bitiş zamanı, bit hızı (kalitelerini), izleme dil (kayan sunu penceresi pencerenin sabit süre), video codec (FourCC). Sonraki yürütülebilir video parçalar kullanılabilir ve bulundukları konumlar ilgili bilgi sağlayarak bir sonraki parça almak için player talimatı verir. Parçaları (veya parçaları) gerçek "" bir video içeriğinizi öbekleridir.

HLS bildirim dosyasının bir örnek aşağıda verilmiştir: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama, video akışı](stream-files-tutorial-with-api.md)

