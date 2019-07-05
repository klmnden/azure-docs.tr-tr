---
title: Azure Media Services dinamik paketlemeye genel bakış | Microsoft Docs
description: Makale, Azure Media Services dinamik paketleme genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/03/2019
ms.author: juliako
ms.openlocfilehash: 4836ec4bb66bbf8ced921dd1095665d004f8a28b
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542586"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve çeşitli istemci teknolojiler (örneğin, iOS ve XBOX gibi) için içerik koruma biçimlendirir. Bu istemciler farklı protokollere anlamak, kesintisiz akış, bir HTTP canlı akışı (HLS) biçimi ve Xbox gerektiren iOS örneğin gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) bir dizi varsa MP4 (ISO temel medya 14496-12) dosyaları ve HLS, MPEG DASH veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi, avantajlarından faydalanabilirsiniz  *dinamik paketleme*. Paketleme video çözümü belirsiz olduğundan, SD/HD/UHD - 4K desteklenir.

Medya Hizmetleri'nde bir [akış uç noktası](streaming-endpoint-concept.md) yaygın akış birini kullanarak doğrudan bir istemci oynatıcı uygulaması için canlı ve isteğe bağlı içerik teslim eden bir dinamik (tam zamanında) paketleme ve kaynak hizmetini temsil eder Medya protokolleri (HLS veya DASH). Dinamik paketleme, tüm standart gelen bir özelliktir **akış uç noktalarını** (standart veya Premium). 

Dinamik paketlemeden yararlanmak için ihtiyacınız bir **varlık** Uyarlamalı bit hızı MP4 dosyaları ve akış yapılandırma dosyalarını, Media Services dinamik paketleme tarafından gereken bir dizi. Dosyaları almak için kullanabileceğiniz yöntemlerden biri, ara (kaynak) dosyanızı Media Services ile kodlamaktır. Oluşturmak zorunda video kodlanmış varlıkta kayıttan yürütme için istemcilere kullanabilmek için bir **akış Bulucu** ve akış URL'lerini oluşturun. Ardından, akış istemci bildirimi (HLS, DASH veya kesintisiz) belirtilen biçime bağlı olarak, akışın seçtiğiniz protokolde alırsınız.

Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar. 

Media Services'de, dinamik paketleme, Canlı veya isteğe bağlı Akış olup olmadığını kullanılır. 

> [!NOTE]
> Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks) birini kullanın.

## <a name="on-demand-streaming-workflow"></a>İsteğe bağlı Akış iş akışı

Media Services dinamik paketleme ile talep üzerine akış için ortak bir iş akışı şu şekildedir:

1. Bir giriş veya kaynak dosyasını karşıya yükle (adlı bir *mezzanine* dosyası). Bir MP4 veya MOV MXF dosyası örneklerindendir. 
1. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın. 
1. Hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın. Bir akış Bulucu oluşturarak yayımladığınız.
1. Farklı biçimlerde (HLS, MPEG-DASH ve kesintisiz akış) hedef URL'leri oluşturun. Akış uç noktasının doğru bildirimi ve istekleri farklı biçimleri için hizmet üstlenir.

Bu diyagramda, dinamik paketleme ile isteğe bağlı Akış iş akışı gösterilmektedir:

![Dinamik paketleme ile isteğe bağlı akış için bir iş akışı diyagramı](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Canlı etkinlik iki türden biri olabilir: doğrudan ya da Canlı kodlama. 

Dinamik paketleme ile canlı akış için ortak bir iş akışı şu şekildedir:

1. Oluşturma bir [Canlı etkinlik](live-events-outputs-concept.md).
1. Alma URL'si almak ve akış katkı göndermek için URL'yi kullanmak için şirket içi Kodlayıcı yapılandırın.
1. Önizleme URL'sini alın ve giriş kodlayıcıdan alındığını doğrulamak için kullanın.
1. Yeni bir varlık oluşturun.
1. Canlı bir çıktı oluşturmak ve oluşturduğunuz varlık adı kullanın.<br />Canlı çıkış akışı varlığa arşivler.
1. Akış Bulucusu, akış yerleşik ilke türleriyle oluşturun.<br />İçeriğinizi şifrelemek istiyorsanız, gözden [içerik korumaya genel bakış](content-protection-overview.md).
1. Kullanılacak URL'leri almak için akış Bulucusu yollarında listeleyin.
1. Gelen akışı gerçekleştirmek istediğiniz akış uç noktası ana bilgisayar adını alın.
1. Farklı biçimlerde (HLS, MPEG-DASH ve kesintisiz akış) hedef URL'leri oluşturun. Akış uç noktasının doğru bildirimi ve istekleri farklı biçimleri için hizmet üstlenir.

Bu diyagramda, dinamik paketleme ile canlı akış iş akışı gösterilmektedir:

![Dinamik paketleme ile doğrudan kodlaması için bir iş akışı diyagramı](./media/live-streaming/pass-through.svg)

Media Services v3 sürümünde canlı akış hakkında daha fazla bilgi için bkz: [Canlı akışa genel bakış](live-streaming-overview.md).

## <a name="delivery-protocols"></a>Teslim protokollerine

İçin içeriğinizi Media Services dinamik paketleme, bu teslim protokollerine kullanabilirsiniz:

|Protocol|Örnek|
|---|---|
|HLS V4 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl)`|
|HLS V3 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl-v3)`|
|HLS CMAF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-cmaf)`|
|MPEG-DASH CSF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-csf)` |
|MPEG-DASH CMAF|`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-cmaf)` |
|Kesintisiz Akış| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest`|

## <a name="encode-to-adaptive-bitrate-mp4s"></a>Uyarlamalı bit hızı MP4 için kodlama

Aşağıdaki makaleler örnekler [Media Services ile bir video kodlama](encoding-concept.md):

* [Bir HTTPS URL'si yerleşik hazır'ı kullanarak kodlama](job-input-from-http-how-to.md)
* [Yerleşik hazır kullanarak yerel bir dosya kodlama](job-input-from-local-file-how-to.md)
* [Özel bir senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden derleme](customize-encoder-presets-how-to.md)

Media Encoder Standard listesini görmek [biçimleri ve codec bileşenleri](media-encoder-standard-formats.md).

## <a name="video-codecs"></a>Görüntü codec bileşenleri

Dinamik paketleme, aşağıdaki video çözümleyicilerini destekler:
* İle kodlanmış bir video içeren MP4 dosyaları [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC veya AVC1) veya [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1, veya hvc1).

## <a name="audio-codecs"></a>Ses codec bileşenleri

Dinamik paketleme, aşağıdaki ses protokollerini destekler:
* Mp4 dosyaları
* Birden çok ses parçaları

Dinamik paketleme, içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

### <a name="mp4-files"></a>Mp4 dosyaları

Dinamik paketleme ile şu protokolden kodlanmış ses içeren MP4 dosyaları destekler: 

* [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE AAC v1 veya v2 HE AAC)
* [Dolby yanı sıra dijital](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (AC 3 ya da E-AC3 Gelişmiş)
* Dolby Atmos<br />
   Dolby Atmos içerik akışı yaygın akış biçimi (CSF) ya da ortak medya uygulama biçim (CMAF) MPEG-DASH protokolüyle parçalanmış MP4 gibi standartlar ve aracılığıyla HTTP canlı akışı (HLS) CMAF ile desteklenir.

* [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29)<br />
   DASH CSF, CMAF DASH, HLS M2TS ve HLS CMAF paketleme biçimlerini tarafından desteklenen DTS codec bileşenleri şunlardır:  

    * DTS dijital Surround (dtsc)
    * DTS HD yüksek çözünürlüklü ve DTS HD ana ses (dtsh)
    * DTS Express (dtse)
    * DTS HD Kayıpsız (çekirdek yok) (dtsl)

### <a name="multiple-audio-tracks"></a>Birden çok ses parçaları

Dinamik paketleme, birden çok codec bileşenleri ve diller birden çok ses parçaları sahip varlıklar akış HLS çıkış (sürüm 4 veya sonrası) için çoklu ses parçaları destekler.

## <a name="dynamic-encryption"></a>Dinamik şifreleme

Kullanabileceğiniz *dinamik şifreleme* dinamik olarak Canlı veya isteğe bağlı içeriğinizi AES-128 veya üç ana dijital hak yönetimi (DRM) sistemlerinden herhangi birini şifrelemek için: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services, yetkili istemcilere AES anahtarları ve DRM lisanslarını teslim etmek üzere bir hizmet de sağlar. Daha fazla bilgi için [dinamik şifreleme](content-protection-overview.md).

## <a name="manifest-examples"></a>Liste Örnekleri 
 
Media Services dinamik paketleme, HLS, MPEG-DASH ve kesintisiz akış için akış istemci bildirimlerini URL biçimi seçicide göre dinamik olarak oluşturulur. Daha fazla bilgi için [teslim protokollerine](#delivery-protocols). 

Adı, başlangıç ve bitiş zamanı, bit hızı (kalitelerini), izleme diller, (sabit süresi kayan pencere) Sunu penceresini ve video codec bileşeni (FourCC) izlemek, parça türü (ses, video veya metin) gibi meta veri akış bir bildirim dosyası içerir. Ayrıca, mevcut olan sonraki yürütülebilir video parçasının ve bulundukları konumlar ilgili bilgi sağlayarak bir sonraki parça almak için player bildirir. Parçaları (veya parçaları) gerçek "" video içeriğinizi öbekleridir.

### <a name="hls"></a>HLS

HLS ana çalma listesi olarak da adlandırılan bir HLS bildirim dosyası örneği aşağıdadır: 

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

### <a name="mpeg-dash"></a>MPEG-DASH

Bir MPEG-DASH medya sunu açıklaması (MPD) olarak da adlandırılan bir MPEG-DASH bildirim dosyası örneği aşağıdadır:

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

Kesintisiz akış bir bildirim dosyasının bir örnek aşağıda verilmiştir:

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

Parçalar, biçimleri, bit hızlarına dönüştürme ve oyuncular gönderilen sunu zaman pencereleri sayısını denetlemek için dinamik filtreleme ile Media Services dinamik Paketleyici kullanabilirsiniz. Daha fazla bilgi için [önceden filtreleme bildirimleri ile dinamik Paketleyici](filters-dynamic-manifest-overview.md).

## <a name="more-information"></a>Daha fazla bilgi

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını öğrenmek için.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [karşıya yükleme, kodlama ve akışını videoları](stream-files-tutorial-with-api.md).

