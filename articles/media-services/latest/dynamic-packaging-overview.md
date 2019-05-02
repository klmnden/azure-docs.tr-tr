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
ms.date: 04/27/2019
ms.author: juliako
ms.openlocfilehash: a907e35e8e39b9dadd9106e7fd99063db28647a5
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869666"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve çeşitli istemci teknolojiler (örneğin, iOS ve XBOX gibi) için içerik koruma biçimlendirir. Bu istemciler farklı protokollere anlamak, kesintisiz akış, bir HTTP canlı akışı (HLS) biçimi ve Xbox gerektiren iOS örneğin gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) bir dizi varsa MP4 (ISO temel medya 14496-12) dosyaları ve HLS, MPEG DASH veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi, avantajlarından faydalanabilirsiniz  **Dinamik paketleme**. Paketleme video çözümü belirsiz olduğundan, SD/HD/UHD - 4K desteklenir.

Medya Hizmetleri'nde bir [akış uç noktası](streaming-endpoint-concept.md) yaygın akış birini kullanarak doğrudan bir istemci oynatıcı uygulaması için canlı ve isteğe bağlı içerik teslim eden bir dinamik (tam zamanında) paketleme ve kaynak hizmetini temsil eder Medya protokolleri (HLS veya DASH). Dinamik paketleme, tüm standart gelen bir özelliktir **akış uç noktalarını** (standart veya Premium). 

Yararlanmak için **dinamik paketleme**, ihtiyacınız bir **varlık** Uyarlamalı bit hızı MP4 dosyaları ve akış yapılandırma dosyalarını, Media Services dinamik paketleme tarafından gereken bir dizi. Dosyaları almak için kullanabileceğiniz yöntemlerden biri, ara (kaynak) dosyanızı Media Services ile kodlamaktır. Oluşturmak zorunda video kodlanmış varlıkta kayıttan yürütme için istemcilere kullanabilmek için bir **akış Bulucu** ve akış URL'lerini oluşturun. Ardından, akış istemci bildirimi (HLS, DASH veya kesintisiz) belirtilen biçime bağlı olarak, akışın seçtiğiniz protokolde alırsınız.

Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar. 

Media Services'de, dinamik paketleme, Canlı veya isteğe bağlı Akış olup olmadığını kullanılır. 

## <a name="common-on-demand-workflow"></a>İsteğe bağlı ortak iş akışı

Dinamik paketleme kullanıldığı akış iş akışı ortak bir Media Services verilmiştir.

1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, MP4, MOV veya MXF (desteklenen biçimler listesi için bkz. [Media Encoder Standard tarafından desteklenen biçimleri](media-encoder-standard-formats.md).
2. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın.
3. Hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın. Yayımladığınız oluşturarak bir **akış Bulucu**.
4. Farklı biçimlerde (HLS, Dash ve kesintisiz akış) hedef URL'leri oluşturun. **Akış uç noktası** hizmet istekleri için bu farklı biçimler ve doğru bildirimi ilgileniriz.

Aşağıdaki diyagram, talep üzerine akış dinamik paketleme iş akışıyla gösterir.

![Dinamik paketleme](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

### <a name="encode-to-adaptive-bitrate-mp4s"></a>Uyarlamalı bit hızı MP4 için kodlama

Hakkında bilgi için [Media Services ile bir video kodlama](encoding-concept.md), aşağıdaki örneklere bakın:

* [HTTPS kullanarak yerleşik hazır bir URL'den kodlayın](job-input-from-http-how-to.md)
* [Yerleşik hazır kullanarak yerel bir dosya kodlama](job-input-from-local-file-how-to.md)
* [Özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden derleme](customize-encoder-presets-how-to.md)

Media Encoder Standard biçimleri ve codec bileşenleri listesi için bkz. [biçimleri ve codec bileşenleri](media-encoder-standard-formats.md)

## <a name="common-live-streaming-workflow"></a>Ortak canlı akış iş akışı

Canlı akış iş akışı için adımlar şunlardır:

1. Oluşturma bir [canlı olay](live-events-outputs-concept.md).
1. Alma URL'leri alma ve akış katkı göndermek için URL'yi kullanmak için şirket içi Kodlayıcı yapılandırın.
1. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
1. Yeni bir **varlık**.
1. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.<br/>**Canlı çıkış** akışa arşiv **varlık**.
1. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.<br/>İçeriğinizi şifrelemek istiyorsanız, gözden [içerik korumaya genel bakış](content-protection-overview.md).
1. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri dönebilirsiniz.
1. Konak adı için alma **akış uç noktası** alanından akışını yapmak istiyor.
1. Farklı biçimlerde (HLS, Dash ve kesintisiz akış) hedef URL'leri oluşturun. **Akış uç noktası** hizmet istekleri için bu farklı biçimler ve doğru bildirimi ilgileniriz.

Canlı bir olay iki türden biri olabilir: doğrudan ve canlı kodlama. Media Services v3 sürümünde canlı akış hakkında daha fazla ayrıntı için bkz [Canlı akışa genel bakış](live-streaming-overview.md).

Canlı akış ile dinamik paketleme iş akışı aşağıdaki diyagramda gösterilmiştir.

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

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

Dinamik paketleme ile kodlanmış bir ses içeren MP4 dosyalarını destekler [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus)(Gelişmiş AC 3 veya E-AC3) Dolby Atmos veya [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD kayıpsız DTS HD). Akış Dolby Atmos içeriği yaygın akış biçimi (CSF) ya da ortak medya uygulama biçim (CMAF) ile MPEG-DASH Protokolü parçalanmış MP4 gibi standartları ve aracılığıyla HTTP canlı akışı (HLS) CMAF ile desteklenir.

> [!NOTE]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="dynamic-encryption"></a>Dinamik şifreleme

**Dinamik şifreleme** dinamik olarak Canlı veya isteğe bağlı içeriğinizi AES-128 veya üç ana dijital hak yönetimi (DRM) sistemlerinden şifrelemenizi sağlar: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. Daha fazla bilgi için [dinamik şifreleme](content-protection-overview.md).

## <a name="manifests"></a>Bildirimler 
 
Media Services, HLS, MPEG DASH, kesintisiz akış protokolleri destekler. Bir parçası olarak **dinamik paketleme**, akış istemci bildirimlerinin (HLS ana çalma listesi, DASH medya sunu açıklaması (MPD) ve kesintisiz akış), URL biçimi seçicide göre dinamik olarak oluşturulur. İçinde teslim protokollerine bkz [Bu bölümde](#delivery-protocols). 

Bir bildirim dosyası meta verileri gibi akış içerir: izleme türü (ses, video veya metin), izleme adı, başlangıç ve bitiş zamanı, bit hızı (kalitelerini), izleme dilleri Sunu penceresini (sabit süre kayan pencere), video codec bileşeni (FourCC). Sonraki yürütülebilir video parçalar kullanılabilir ve bulundukları konumlar ilgili bilgi sağlayarak bir sonraki parça almak için player talimatı verir. Parçaları (veya parçaları) gerçek "" bir video içeriğinizi öbekleridir.

### <a name="hls-master-playlist"></a>HLS ana çalma listesi

HLS bildirim dosyasının bir örnek aşağıda verilmiştir: 

```
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(381048)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(381048)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721495)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721495)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1154816)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1154816)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2217354)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2217354)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579827)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579827)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="dash-media-presentation-description-mpd"></a>DASH medya sunu açıklaması (MPD)

Bir tire bildiriminin bir örnek aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="static" mediaPresentationDuration="PT1M10.315S" minBufferTime="PT7S">
   <Period>
      <AdaptationSet id="1" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2" lang="en">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60160000" r="10" />
               <S d="41386666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="5_A_aac_eng_2_128041_2_1_1" bandwidth="128041" audioSamplingRate="48000" />
      </AdaptationSet>
      <AdaptationSet id="2" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60060000" r="10" />
               <S d="42375666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="1_V_video_1" bandwidth="3579827" width="1280" height="720" />
         <Representation id="1_V_video_2" bandwidth="2217354" codecs="avc1.64001F" width="960" height="540" />
         <Representation id="1_V_video_3" bandwidth="1154816" codecs="avc1.64001E" width="640" height="360" />
         <Representation id="1_V_video_4" bandwidth="721495" codecs="avc1.640015" width="480" height="270" />
         <Representation id="1_V_video_5" bandwidth="381048" codecs="avc1.64000D" width="320" height="180" />
      </AdaptationSet>
   </Period>
</MPD>
```
### <a name="smooth-streaming"></a>Kesintisiz Akış

Kesintisiz akış bildirimin bir örnek aşağıda verilmiştir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="703146666" TimeScale="10000000">
   <StreamIndex Chunks="12" Type="audio" Url="QualityLevels({bitrate})/Fragments(aac_eng_2_128041_2_1={start time})" QualityLevels="1" Language="eng" Name="aac_eng_2_128041_2_1">
      <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="128041" FourCC="AACL" CodecPrivateData="1190" Channels="2" PacketSize="4" SamplingRate="48000" />
      <c t="0" d="60160000" r="11" />
      <c d="41386666" />
   </StreamIndex>
   <StreamIndex Chunks="12" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="5">
      <QualityLevel Index="0" Bitrate="3579827" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="0000000167640020ACD9405005BB011000003E90000EA600F18319600000000168EBECB22C" />
      <QualityLevel Index="1" Bitrate="2217354" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FACD940F0117EF01100000303E90000EA600F1831960000000168EBECB22C" />
      <QualityLevel Index="2" Bitrate="1154816" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EACD940A02FF9701100000303E90000EA600F162D960000000168EBECB22C" />
      <QualityLevel Index="3" Bitrate="721495" FourCC="H264" MaxWidth="480" MaxHeight="270" CodecPrivateData="0000000167640015ACD941E08FEB011000003E90000EA600F162D9600000000168EBECB22C" />
      <QualityLevel Index="4" Bitrate="381048" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DACD941419F9F011000003E90000EA600F14299600000000168EBECB22C" />
      <c t="0" d="60060000" r="11" />
      <c d="42375666" />
   </StreamIndex>
</SmoothStreamingMedia>
```

## <a name="dynamic-manifest"></a>Dinamik bildirimi

Dinamik filtreleme izler, biçimleri, bit hızlarına dönüştürme ve oyunculara gönderilen sunu zaman pencereleri sayısını denetlemek için kullanılır. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](developers-guide.md) birini kullanın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama, video akışı](stream-files-tutorial-with-api.md)

