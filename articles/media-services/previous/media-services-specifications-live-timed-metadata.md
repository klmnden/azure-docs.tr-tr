---
title: Sinyal azure medya Hizmetleri - canlı akış meta verileri zaman aşımına | Microsoft Docs
description: Bu belirtim canlı akış içinde sinyal zamanlanmış meta veriler için Media Services tarafından desteklenen iki modları açıklanmaktadır. Bu sinyalleri genel zamanlanmış meta verileri yanı sıra SCTE-35 ad splice eklemesi için sinyal için destek içerir.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: johndeu;
ms.openlocfilehash: 10dbf7e8cf67ab721cf525d4a1e7594473592bd4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459122"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Meta verileri canlı akış zaman aşımına sinyali 


## <a name="1-introduction"></a>1 giriş 
Reklamlar veya özel bir istemci oynatıcı yayıncıları genellikle olaylarına ekleme yapma kolaylaştırmak için eklenmiş videoyu içinde zamanlanmış meta verileri kullanın. Bu senaryoları sağlamak için Media Services canlı akış kanal istemci uygulamasına alma noktasından medya birlikte zamanlanmış meta veri taşıma için destek sağlar.
İçinde zamanlanmış meta veriler için Media Services tarafından desteklenen bu belirtimi anahatları iki mod akış sinyalleri Canlı:

1. [SCTE-35], [SCTE-67] tarafından açıklanan önerilen uygulamaları heeds sinyali

2. Genel olmayan iletiler için bir mod sinyal meta verileri SCTE-35 zaman aşımına uğradı.

### <a name="12-conformance-notation"></a>1.2 uygunluk gösterimi
Anahtar sözcükler "ZORUNDASINIZ", "NOT gerekir", "Gerekli", "SHALL", "NOT GELECEKTİR", "SHOULD", "NOT gerekir", "Önerilen", "Olabilir" ve bu belgedeki "isteğe bağlı" RFC 2119 içinde açıklanan şekilde yorumlanır üzeresiniz

### <a name="13-terms-used"></a>1.3 kullanılan terimler

| Sözleşme Dönemi              | Tanım                                                                                                                                                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sunu zaman | Bir Olay Görüntüleyicisi'ne verilen süre. Zaman bir Görüntüleyici olay gördüğünüzü medya zaman çizelgesi üzerinde şu temsil eder. Örneğin, sunu SCTE-35 splice_info() komut iletisinin splice_time() zamandır. |
| Geliş saati      | Olay iletisi ulaşan süre. Etkinliğin sunu saati önüne gönderilen olay iletileri bu yana olay sunu zamanından genellikle ayrı bir zamandır.                                     |
| Seyrek İzle      | ortam sürekli olmayan izlemek ve zaman bir üst ya da Denetim izleme ile eşitlenir.                                                                                                                                    |
| Kaynak            | Azure medya akış hizmeti                                                                                                                                                                                                |
| Kanal havuzu      | Azure Medya canlı akış hizmeti                                                                                                                                                                                           |
| HLS               | Apple HTTP canlı Akış Protokolü                                                                                                                                                                                               |
| TİRE              | HTTP üzerinden Uyarlamalı dinamik                                                                                                                                                                                             |
| Kesintisiz            | Kesintisiz Akış Protokolü                                                                                                                                                                                                        |
| MPEG2-TS          | MPEG-2 aktarım akışları                                                                                                                                                                                                         |
| RTMP              | Gerçek zamanlı multimedya Protokolü                                                                                                                                                                                                    |
| uimsbf            | Önce bit işaretsiz tamsayı, en önemli.                                                                                                                                                                                    |

-----------------------

## <a name="2-timed-metadata-ingest"></a>2 zamanlanmış meta verileri alma
## <a name="21-rtmp-ingest"></a>2.1 RTMP içe alma
RTMP RTMP akış içinde gömülü AMF işaret iletileri olarak gönderilen zamanlanmış meta verileri sinyalleri destekler. Bir süre önce gerçek olayın işaret iletileri gönderilebilir veya reklam yerleştirme splice gerçekleşmesi gerekir. Bu senaryoyu desteklemek için splice veya segment bu süre içinde hastalıktan ileti gönderilir. [AMF0] daha fazla bilgi için bkz.

Aşağıdaki tabloda, Media Services alma AMF ileti yükü biçimini açıklar.

AMF ileti adı, aynı türde birden fazla olay akışları ayırt etmek için kullanılabilir.  SCTE-35 iletileri için [SCTE-67] önerildiği gibi "onAdCue" AMF ileti adı olmalıdır.  Böylece Bu tasarımın yenilik gelecekte etkinleştirileceğini değil, aşağıda listelenen değil herhangi bir alan yok SAYILMALIDIR.

### <a name="signal-syntax"></a>Sinyal söz dizimi
RTMP basit mod için Media Services ile şu biçimde "onAdCue" adlı tek bir AMF işaret ileti destekler:

### <a name="simple-mode"></a>Basit mod

| Alan Adı | Alan türü | Gerekli mi? | Açıklamalar                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| işaret        | String     | Gerekli | Olay iletisi.  Basit mod belirtmek için "SpliceOut" splice olması.                                              |
| id         | String     | Gerekli | Splice veya segment açıklayan benzersiz bir tanımlayıcı. Bu iletinin örneğini tanımlar                            |
| süre   | Sayı     | Gerekli | Splice süresini. Kesirli saniye birimleridir.                                                                |
| elapsed    | Sayı     | İsteğe bağlı | Sinyal desteklemek için yinelenen etkinliğindeki, bu alan süreyi splice başlamasından bu yana geçen sunu olacaktır. Kesirli saniye birimleridir. Basit mod kullanırken, bu değer özgün splice süresi aşmamalıdır.                                                  |
| time       | Sayı     | Gerekli | Splice süresini sunu zamanda tutulamaz. Kesirli saniye birimleridir.                                     |

---------------------------

### <a name="scte-35-mode"></a>SCTE-35 modu

| Alan Adı | Alan türü | Gerekli mi? | Açıklamalar                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| işaret        | String     | Gerekli | Olay iletisi.  SCTE-35 iletileri için bu base64 olmalıdır (IETF RFC 4648) ikili splice_info_section() [67 SCTE] uyduğunuzu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırada kodlanmış.                                              |
| type       | String     | Gerekli | Bir URN veya ileti şeması tanımlayan URL. SCTE-35 iletileri için bu "urn: scte:scte35:2013a:bin" [67 SCTE] uyduğunuzu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırada olması gerekir.  |
| id         | String     | Gerekli | Splice veya segment açıklayan benzersiz bir tanımlayıcı. Bu iletinin örneğini tanımlar.  Eşdeğer semantiğine sahip iletileri aynı değere sahip olamaz.|
| süre   | Sayı     | Gerekli | Olay ya da ad splice-biliniyorsa segment, süresi. Bilinmiyorsa, değeri 0 olmalıdır.                                                                 |
| elapsed    | Sayı     | İsteğe bağlı | Bu alan, SCTE-35 ad sinyal etkinliğindeki için tekrarlanırsa, süreyi splice başlamasından bu yana geçen sunu olacaktır. Kesirli saniye birimleridir. SCTE-35 modunda, bu değer özgün belirtilen süre splice veya segment aşabilir.                                                  |
| time       | Sayı     | Gerekli | Olay ya da ad sunu zamanını splice.  Sunu saatini ve süresini Stream erişim noktaları (SAP ile) 1 veya 2 türünde tanımlanan [ISO-14496-12] Annex bilgisinde hizalamanız gerekir. HLS çıkışı için zamanını ve süresini segment sınırları ile hizalamanız gerekir. Sunu saatini ve süresini farklı olay iletileri aynı olay akışının çakışmaması gerekir. Kesirli saniye birimleridir.

---------------------------

#### <a name="211-cancellation-and-updates"></a>2.1.1 iptal ve güncelleştirmeler

İletileri iptal edildi veya birden çok ileti aynı sunu zamanını ve kimliği ile göndererek güncelleştirildi Olay Kimliği ve sunu saat benzersiz şekilde tanımlamak ve öncesi kısıtlamalar karşılayan belirli bir sunu zaman için alınan son ileti üzerinde işlem iletisi. Güncelleştirilen etkinlik, daha önce alınan iletileri değiştirir. Öncesi kısıtlaması dört saniyedir. Sunu saatten önce en az dört saniye alınan iletilerin üzerinde yapılmasının.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 parçalanmış MP4 (kesintisiz akış) alma
Gereksinimlerinize göre canlı akış içe alma için [Canlı-FMP4] bakın. Aşağıdaki bölümler, zamanlanmış bir sunu meta verileri alma ile ilgili ayrıntıları sağlar.  Zamanlanmış bir sunu meta veri hem de canlı sunucu bildirim kutusunda tanımlanan seyrek bir parçası olarak alınan (MS-SSTR bakın) ve film kutusunu ('moov').  'Mdat' kutusu ikili ileti olduğu bir film parçasını kutusunu ('moof') ve medya Data Box ('mdat'), seyrek her parça oluşur.

#### <a name="221-live-server-manifest-box"></a>2.2.1 Canlı sunucusu liste kutusu
Canlı bildirim sunucusu kutusunda seyrek izleme bildirilmelidir bir \<textstream\> girişi ve aşağıdaki öznitelik kümesini olması gerekir:

| **Öznitelik adı** | **Alan türü** | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Sayı         | Gerekli      | "0" olarak belirten bir parça ile bilinmeyen, değişken hızına sahip olması gerekir.                                                                                                                                                                                                 |
| parentTrackName    | String         | Gerekli      | Seyrek izleme zaman kodlarını ölçeği hizalı olan ana izleme adı olmalıdır. Üst izleme seyrek bir parçası olamaz.                                                                                                                    |
| manifestOutput     | Boolean        | Gerekli      | "Seyrek izleme kesintisiz istemci bildiriminde gömülü belirtmek için true", olmalıdır.                                                                                                                                                               |
| Alt tür            | String         | Gerekli      | GEREKEN olması dört karakter kodu "Veri".                                                                                                                                                                                                                         |
| Düzeni             | String         | Gerekli      | İleti şeması tanımlayan bir URN veya URL olmalıdır. SCTE-35 iletileri için bu "urn: scte:scte35:2013a:bin" [67 SCTE] uyduğunuzu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırada olması gerekir. |
| TrackName          | String         | Gerekli      | Seyrek izleme adı olmalıdır. TrackName aynı düzeni ile birden çok olay akışları ayırt etmek için kullanılabilir. Her benzersiz olay akışının benzersiz parça adı olmalıdır.                                                                           |
| Zaman Çizelgesi          | Sayı         | İsteğe bağlı      | Üst izleme ölçeğini olması gerekir.                                                                                                                                                                                                                      |

-------------------------------------

### <a name="222-movie-box"></a>2.2.2 film kutusu

Film kutusu ('moov'), Canlı sunucusu bildirim kutusu akış üst bilgisi seyrek izlemek için bir parçası olarak izler.

'Moov' kutusu içermesi gereken bir **TrackHeaderBox ('tkhd')** kutusuna aşağıdaki kısıtlamalarla [ISO-14496-12'de] tanımlandığı şekilde:

| **Alan adı** | **Alan türü**          | **Gerekli?** | **Açıklama**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| süre       | 64-bit işaretsiz tamsayı | Gerekli      | İzleme kutusunu sıfır örnekleri vardır ve İzle kutusunu örneklerinde toplam süresi 0 olduğundan 0 olmalıdır. |

-------------------------------------

'Moov' kutusu içermesi gereken bir **HandlerBox ('hdlr')** [ISO-14496-12'de] aşağıdaki kısıtlamalarla tanımlanan:

| **Alan adı** | **Alan türü**          | **Gerekli?** | **Açıklama**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | 32-bit işaretsiz tamsayı | Gerekli      | 'Meta' olmalıdır. |

-------------------------------------

'Stsd' kutusu [ISO-14496-12'de] tanımlandığı gibi bir kodlama adı MetaDataSampleEntry kutusuyla içermelidir.  Örneğin, SCTE-35 iletileri için kodlama adı 'scte' olmalıdır.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 film parça ve ortam verilerini kutularının

Seyrek parça parça film parçasını kutusunu ('moof') ve bir ortam Data Box ('mdat') oluşur.

MovieFragmentBox ('moof') kutusuna içermelidir bir **TrackFragmentExtendedHeaderBox ('UUID')** kutusuna şu alanlar [MS-SSTR] tanımlandığı şekilde:

| **Alan adı**         | **Alan türü**          | **Gerekli?** | **Açıklama**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | 64-bit işaretsiz tamsayı | Gerekli      | Olayın geliş saati olması gerekir. Bu değer, iletinin üst izleme ile hizalar.   |
| fragment_duration      | 64-bit işaretsiz tamsayı | Gerekli      | Olay süresi olması gerekir. Süresi, süresi bilinmeyen olduğunu belirtmek için sıfır olabilir. |

------------------------------------


MediaDataBox ('mdat') kutusunda, aşağıdaki biçimde olmalıdır:

| **Alan adı**          | **Alan türü**                   | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version                 | 32-bit işaretsiz tamsayı (uimsbf) | Gerekli      | 'Mdat' kutusunun içeriğini biçimini belirler. Tanınmayan sürüm göz ardı edilir. Şu anda desteklenen tek sürüm 1'dir.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32-bit işaretsiz tamsayı (uimsbf) | Gerekli      | Bu iletinin örneğini tanımlar. Eşdeğer semantiğine sahip iletileri aynı değere sahip olamaz; diğer bir deyişle, herhangi bir olay ileti kutusu aynı kimliğe sahip işleme yeterli olur.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32-bit işaretsiz tamsayı (uimsbf) | Gerekli      | Etkinliğin sunu saati TrackFragmentExtendedHeaderBox ve presentation_time_delta belirtilen fragment_absolute_time toplamı olması gerekir. Sunu saatini ve süresini Stream erişim noktaları (SAP ile) 1 veya 2 türünde tanımlanan [ISO-14496-12] Annex bilgisinde hizalamanız gerekir. HLS çıkışı için zamanını ve süresini segment sınırları ile hizalamanız gerekir. Sunu saatini ve süresini farklı olay iletileri aynı olay akışının çakışmaması gerekir. |
| message                 | bayt dizisi                       | Gerekli      | Olay iletisi. [67 SCTE] başka bir şey önerir rağmen SCTE-35 iletileri için ikili splice_info_section() iletisidir. SCTE-35 iletileri için bu sırayla [67 SCTE] uyduğunuzu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin splice_info_section() olması gerekir. SCTE-35 iletileri için ikili splice_info_section() 'mdat' kutusu yüktür ve kodlanmış bir base64 değeri değil.                                                            |

------------------------------


### <a name="224-cancellation-and-updates"></a>2.2.4 iptal ve güncelleştirmeler
İletileri iptal edildi veya birden çok ileti aynı sunu zamanını ve kimliği ile göndererek güncelleştirildi  Kimliği ve sunu saat olay benzersiz şekilde tanımlar. Öncesi kısıtlamalar karşılayan bir belirli bir sunu süre için alınan son ileti izlemede bir iletidir. İleti, daha önce alınan iletileri değiştirir.  Öncesi kısıtlaması dört saniyedir. Sunu saatten önce en az dört saniye alınan iletilerin üzerinde yapılmasının. 


## <a name="3-timed-metadata-delivery"></a>Meta veri teslim 3 zaman aşımına uğradı

Olay veri akışı, Media Services için donuktur. Media Services, üç parça bilgi yalnızca alma uç noktası ve istemci uç noktası arasında geçirir. Aşağıdaki özellikleri [67 SCTE] uyumlu istemci teslim edilir ve/veya istemci Akış Protokolü:

1.  Şema – bir URN veya iletinin şeması tanımlayan URL.

2.  Sunu saati – sunu medya zaman çizelgesi olayının.

3.  Süresi – olay süresi.

4.  KODU – olay için isteğe bağlı bir benzersiz tanımlayıcı.

5.  İleti – olay verileri.


## <a name="31-smooth-streaming-delivery"></a>3.1 kesintisiz akış halinde teslim

[MS-SSTR] ayrıntılarında işleme seyrek izleme bakın.

#### <a name="smooth-client-manifest-example"></a>Kesintisiz istemci bildirimi örneği
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-event-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013a:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- <f> contains base-64 encoded splice_info_section() message 
    <c t=”600000000” d=”300000000”>
<f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia> 
~~~ 

## <a name="32--apple-hls-delivery"></a>3.2 Apple HLS teslim
Zamanlanmış meta verileri için Apple HTTP canlı akış (HLS), özel bir M3U etiket içinde segment çalma katıştırılmış.  Uygulama katmanı M3U çalma listesi ayrıştırabilir ve M3U etiketleri işlem. Azure Media Services, zamanlanmış meta veri [HLS] içinde tanımlı EXT X işaret etiketinde katıştırır.  EXT X işaret etiket, şu anda ADI3 türündeki iletilerde DynaMux tarafından kullanılır.  SCTE-35 ve olmayan SCTE-35 iletileri desteklemek için aşağıda tanımlanan EXT X işaret etiketi özniteliklerini ayarlayın:

| **Öznitelik adı** | **Tür**                      | **Gerekli?**                             | **Açıklama**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İŞARET                | Tırnak işaretli dize                 | Gerekli                                  | Bir base64 dizesi olarak açıklandığı gibi kodlanmış ileti [IETF RFC 4648](https://tools.ietf.org/html/rfc4648). SCTE-35 iletileri için base64 olarak kodlanmış splice_info_section() budur.                                                                                                |
| TÜR               | Tırnak işaretli dize                 | Gerekli                                  | Bir URN veya ileti şeması tanımlayan URL. SCTE-35 iletileri için türü özel "scte35" değerini alır.                                                                                                                                |
| Kimlik                 | Tırnak işaretli dize                 | Gerekli                                  | Olay için benzersiz bir tanımlayıcı. Alınan ileti kimliği belirtilmedi, Azure Media Services benzersiz bir kimliği oluşturur.                                                                                                                                          |
| SÜRESİ           | ondalık kayan nokta sayısı | Gerekli                                  | Olay süresi. Bilinmiyorsa, değeri 0 olmalıdır. Factional saniye birimleridir.                                                                                                                                                                                           |
| GEÇEN SÜRE            | ondalık kayan nokta sayısı | İsteğe bağlı, ancak kayan pencere için gerekli | Bu alan, sinyal kayan bir sunu pencere desteklemek için tekrarlanırsa, olay başlamasından bu yana, geçen süreyi sunu olması gerekir. Kesirli saniye birimleridir. Bu değer, özgün belirtilen süre splice veya segment aşabilir. |
| TIME               | ondalık kayan nokta sayısı | Gerekli                                  | Etkinliğin sunu saati. Kesirli saniye birimleridir.                                                                                                                                                                                                                    |


HLS player uygulama katmanı, ileti biçimi belirlemek, iletisinin kodunu çözün, gerekli saat dönüşümleri uygulama ve olayı işlemek için türü kullanır.  Üst izleme segment çalma listesi eşitlenmesi olay zaman damgası göre olaylardır.  Bunlar, en yakın segment (#EXTINF etiketi) önce eklenir.

#### <a name="hls-segment-playlist-example"></a>HLS Segment çalma listesi örneği
~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:346
#EXT-X-TARGETDURATION:6
#EXT-X-I-FRAMES-ONLY
#EXT-X-PROGRAM-DATE-TIME:2018-12-13T15:54:19.462Z
#EXTINF:4.000000,no-desc
KeyFrames(video_track=15447164594627600,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447164634627600,format=m3u8-aapl)
#EXT-X-CUE:ID="1026",TYPE="scte35",DURATION=30.000000,TIME=1544716520.022760,CUE="/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w=="
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447165474627600,format=m3u8-aapl)
~~~

#### <a name="hls-message-handling"></a>HLS ileti işleme

Her video ve ses izleme segment çalma listesi sinyal olayları. EXT X işaret etiket konumu her zaman olması ya da hemen (splice out veya segment Başlat) ilk HLS segmentlere önce veya sonra hemen son HLS (splice içinde veya segment sonu) ZAMANINI ve süresini özniteliklerini bakın, segmentlere gerekir, [gerektirdiği HLS].

Kayan bir sunu pencere etkin olduğunda, EXT X işaret etiketi sıklıkta splice veya segment her zaman segment çalma listesi tam olarak açıklanan ve geçen öznitelik splice süreyi belirtmek için kullanılması gereken veya Segmentte yinelenmesi gerekir [HLS] gerektirdiği etkin bırakıldı.

Kayan bir sunu pencere etkin olduğunda, başvurdukları medya zamanı dışında kayan sunu pencere alındı EXT X işaret etiketleri segment çalma listesinden kaldırılır.

## <a name="33--dash-delivery"></a>3.3 DASH teslim
[DASH] sinyal olayları için üç yol sunar:

1.  İçinde MPD sinyal olayları
2.  Bant dışı (olay ileti kutusunu ('emsg') kullanarak gösteriminde olayları sinyal
3.  Hem 1 ve 2

İçinde MPD sinyal olayları, istemciler MPD hemen karşıdan yüklendiğinde tüm olaylar, erişimi olduğu VOD akışı için kullanışlıdır. Bant dışı çözüm, istemciler MPD yeniden indirmeniz gerekmez çünkü, canlı akış için kullanışlıdır. Zamana bağlı kesimlemesi için istemci, sonraki kesim URL'sini süre ve zaman damgası geçerli kesiminin ekleyerek belirler. İstek başarısız olursa istemci süreksizlik varsayar ve MPD indirir ancak Aksi halde MPD indirmeden akış sürdürür.

Azure Media Services hem sinyal MPD içinde yapın ve bant içinde olay ileti kutusunu kullanarak sinyal.

#### <a name="mpd-signaling"></a>MPD sinyali

Olaylar, dönem öğesinde görünür EventStream öğesini kullanarak MPD içinde sinyal.

EventStream öğenin öznitelikleri şunlardır:

| **Öznitelik adı** | **Tür**                | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | string                  | Gerekli      | İletinin düzenini tanımlar. Düzeni dinamik bildirim sunucusu kutusuna düzeni öznitelik değerine ayarlanır. Değer bir URN veya ileti şeması tanımlayan URL olmalıdır; Örneğin, "urn: scte:scte35:2013a:bin".                                                                |
| value              | string                  | İsteğe bağlı      | İleti semantiği özelleştirmek için Düzen sahipleri tarafından kullanılan bir ek dize değeri. Aynı düzeni ile birden çok olay akışı farklılaştırmak için değeri olay akışının (alma trackName kesintisiz için veya AMF ileti adı RTMP alma) adına ayarlanmalıdır. |
| Timescale          | 32-bit işaretsiz tamsayı | Gerekli      | Ticks 'emsg' kutusunda saatleri ve süresi saniyede ölçeği.                                                                                                                                                                                                       |


Sıfır veya daha fazla olay öğeler EventStream öğe içinde yer alır ve bunlar aşağıdaki özniteliklere sahiptir:

| **Öznitelik adı**  | **Tür**                | **Gerekli?** | **Açıklama**                                                                                                                                                                                                             |
|---------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| presentation_time   | 64-bit işaretsiz tamsayı | İsteğe bağlı      | Medya sunu zamanını olayın döneminizin göreli olmalıdır. Sunu saatini ve süresini Stream erişim noktaları (SAP ile) 1 veya 2 türünde tanımlanan [ISO-14496-12] Annex bilgisinde hizalamanız gerekir. |
| süre            | 32-bit işaretsiz tamsayı | İsteğe bağlı      | Olay süresi. Süre bilinmiyorsa, bu gözardı gerekir.                                                                                                                                                 |
| id                  | 32-bit işaretsiz tamsayı | İsteğe bağlı      | Bu iletinin örneğini tanımlar. Eşdeğer semantiğine sahip iletileri aynı değere sahip olamaz. Alınan ileti kimliği belirtilmedi, Azure Media Services benzersiz bir kimliği oluşturur.             |
| Olay öğe değeri | string                  | Gerekli      | Olay iletisi açıklandığı bir base64 dizesi olarak [IETF RFC 4648](https://tools.ietf.org/html/rfc4648).                                                                                                                   |

#### <a name="xml-syntax-and-example-for-dash-manifest-mpd-signaling"></a>XML sözdizimi ve tire örneğin (MPD) sinyal bildirimi

~~~ xml
<!-- XML Syntax -->
<xs:complexType name=”EventStreamType”>
  <xs:sequence>
    <xs:element name=”Event” type=”EventType” minOccurs=”0” maxOccurs=”unbounded”/>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute ref=”xlink:href”/>
  <xs:attribute ref=”xlink:actuate” default=”onRequest”/>
  <xs:attribute name=”schemeIdUri” type=”xs:anyURI” use=”required”/>
  <xs:attribute name=”value” type=”xs:string”/>
  <xs:attribute name=”timescale” type=”xs:unsignedInt”/>
</xs:complexType>
<!-- Event -->
<xs:complexType name=”EventType”>
  <xs:sequence>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute name=”presentationTime” type=”xs:unsignedLong” default=”0”/>
  <xs:attribute name=”duration” type=”xs:unsignedLong”/>
  <xs:attribute name=”id” type=”xs:unsignedInt”/>
  <xs:anyAttribute namespace=”##other” processContents=”lax”/>
</xs:complexType><?xml version=”1.0” encoding=”utf-8”?>


<!-- Example Section in MPD -->
  <EventStream schemeIdUri="urn:scte:scte35:2013a:bin" value="scte35_track_001_000" timescale="10000000">
        <Event presentationTime="15447165200227600" duration="300000000" id="1026">/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w==</Event>
        <Event presentationTime="15447166250227600" duration="300000000" id="1027">/DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==</Event>
        <Event presentationTime="15447167300227600" duration="600000000" id="1028">/DAlAAAAAAAAAP/wFAUAAAQEf+//KjkknP4AUmXAAAAAAAAAWcEldA==</Event>
        <Event presentationTime="15447168350227600" duration="600000000" id="1029">/DAlAAAAAAAAAP/wFAUAAAQFf+//KslyqP4AUmXAAAAAAAAAvKNt0w==</Event>
        <Event presentationTime="15447169400227600" duration="300000000" id="1030">/DAlAAAAAAAAAP/wFAUAAAQGf+//K1mIvP4AKTLgAAAAAAAAt2zEbw==</Event>
        <Event presentationTime="15447170450227600" duration="600000000" id="1031">/DAlAAAAAAAAAP/wFAUAAAQHf+//K+hc/v4AUmXAAAAAAAAANNRzVw==</Event>
    </EventStream>
~~~

>[!NOTE]
>Bu presentationTime sunu zamanını değil geliş saati iletinin olayın olduğuna dikkat edin.
>

### <a name="431-in-band-event-message-box-signaling"></a>4.3.1 bant dışı olay ileti kutusunda sinyalleri
Bir bant dışı olay akışı MPD InbandEventStream öğenin uyarlama kümesi düzeyinde olmasını gerektirir.  Bu öğe bir zorunlu schemeIdUri özniteliği ve isteğe bağlı ölçeği özniteliği de olay görüntülenen ileti kutusunda ('emsg') var.  Olay ileti kutuları MPD içinde tanımlı değil düzeni tanımlayıcıları ile mevcut olmamalıdır. Bir olay ileti kutusu MPD içinde tanımlanmamış bir düzeni ile DASH istemci algılarsa, bunu yoksaymak için istemci bekleniyor.
Olay iletisi kutusu ('emsg'), medya sunu zaman ilgili genel olaylar için sinyal sağlar. Varsa, herhangi bir 'emsg' kutusu önce herhangi bir 'moof' kutusu yerleştirilmesi.

### <a name="dash-event-message-box-emsg"></a>Olay iletisi kutusu 'emsg' tire
~~~
Box Type: `emsg’
Container: Segment
Mandatory: No
Quantity: Zero or more
~~~

~~~ c
aligned(8) class DASHEventMessageBox extends FullBox(‘emsg’, version = 0, flags =0) 
{
    string scheme_id_uri;
    string value;
    unsigned int(32) timescale;
    unsigned int(32) presentation_time_delta;
    unsigned int(32) event_duration;
    unsigned int(32) id;
    unsigned int(8) message_data[];
}
~~~

DASHEventMessageBox alanlarını aşağıda tanımlanmıştır:

| **Alan adı**          | **Alan türü**          | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | string                  | Gerekli      | İletinin düzenini tanımlar. Düzeni dinamik bildirim sunucusu kutusuna düzeni öznitelik değerine ayarlanır. Değer bir URN veya ileti şeması tanımlayan URL olmalıdır. [67 SCTE] başka bir şey önerir rağmen SCTE-35 iletileri için bu özel değer "urn: scte:scte35:2013a:bin" götürür. |
| Değer                   | string                  | Gerekli      | İleti semantiği özelleştirmek için Düzen sahipleri tarafından kullanılan bir ek dize değeri. Aynı düzeni ile birden çok olay akışı farklılaştırmak için değeri olay akışının (alma trackName kesintisiz için veya AMF ileti adı RTMP alma) adına ayarlanır.                                                                  |
| Timescale               | 32-bit işaretsiz tamsayı | Gerekli      | Ticks 'emsg' kutusunda saatleri ve süresi saniyede ölçeği.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | 32-bit işaretsiz tamsayı | Gerekli      | Etkinliğin sunu saati ve bu segmentteki en erken sunu zamanını medya sunu zaman aralığı. Sunu saatini ve süresini Stream erişim noktaları (SAP ile) 1 veya 2 türünde tanımlanan [ISO-14496-12] Annex bilgisinde hizalamanız gerekir.                                                                                            |
| event_duration          | 32-bit işaretsiz tamsayı | Gerekli      | Olayın ya da bir bilinmeyen süresi belirtmek için 0xFFFFFFFF süresi.                                                                                                                                                                                                                                                                                          |
| Kimlik                      | 32-bit işaretsiz tamsayı | Gerekli      | Bu iletinin örneğini tanımlar. Eşdeğer semantiğine sahip iletileri aynı değere sahip olamaz. Alınan ileti kimliği belirtilmedi, Azure Media Services benzersiz bir kimliği oluşturur.                                                                                                                                                    |
| Message_data            | bayt dizisi              | Gerekli      | Olay iletisi. [67 SCTE] başka bir şey önerir rağmen SCTE-35 ileti için ileti ikili splice_info_section() verilerdir.                                                                                                                                                                                                                                 |

### <a name="332-dash-message-handling"></a>3.3.2 ileti işleme tire

Olayları sinyal bant içi, video ve ses parçaları için 'emsg' kutunun içinde.  Sinyal tüm istekleri presentation_time_delta 15 saniye olduğu segmentlere ayırmak için ya da daha az gerçekleşir. Toplam süre ve olay iletisi süresi medya veri bildiriminde bir saatten daha az olduğunda olay iletileri, kayan bir sunu pencere etkin olduğunda, MPD kaldırılır.  Diğer bir deyişle, bunlar başvurduğu medya zamanı dışında kayan sunu pencere gezinirken olay iletileri bildirimden kaldırıldı.

## <a name="4-scte-35-ingest"></a>4 SCTE-35 alma

SCTE-35 iletileri düzenini kullanarak ikili biçimde alınan **"urn: scte:scte35:2013a:bin"** kesintisiz için alma ve türü **"scte35"** için RTMP içe alma. MPEG-2 aktarım akışı sunu zaman damgaları (NK) NK arasındaki eşlemeyi temel aldığı SCTE-35 zamanlama dönüştürülmesi kolaylaştırmak için (pts_time + splice_time()) ve medya zaman çizelgesi pts_adjustment sağlanan olay sunu zaman (tarafından Kesintisiz fragment_absolute_time alanını içe alma ve zaman alan için RTMP içe alma). 33 bit NK değeri yaklaşık 26,5 saat yapar çünkü eşlemesi gereklidir.

Kesintisiz akış alma medya Data Box ('mdat') içermelidir gerektirir **splice_info_section()** Tablo 8-1'de [SCTE-35] tanımlı. İçin RTMP içe alma, işaret özniteliği AMF iletisinin base64encoded için ayarlanmış **splice_info_section()**. Yukarıda açıklanan biçimde iletileri sahip olduğunuzda, HLS, kesintisiz ve tire istemcilere [67 SCTE] uyduğunuzu gönderilir.


## <a name="5-references"></a>5 başvuruları

**[SCTE-35]**  ANSI/SCTE 35 2013a – dijital Program ekleme Cueing ileti kablo, 2013a için

**[67 SCTE]**  ANSI/SCTE 67 2014 – uygulama SCTE 35 için önerilir: Dijital Program ekleme Cueing ileti kablosu

**[DASH]**  ISO/IEC 23009 1 – bilgi teknolojisi – 2014 dinamik Uyarlamalı akış HTTP (DASH üzerinde) – bölüm 1: Ortam sunumunu açıklaması ve segment biçimleri, 2 sürümü

**[HLS]**  ["HTTP canlı akış", draft-pantos-http-live-streaming-14, 14 Ekim 2014](http://tools.ietf.org/html/draft-pantos-http-live-streaming-14)

**[MS-SSTR]**  ["Microsoft kesintisiz Akış Protokolü", 15 Mayıs 2014](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

**[AMF0]**  ["Eylem ileti biçimi AMF0"](https://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)

**[CANLI FMP4]**  [Azure Media Services bölünmüş MP4 Canlı alma belirtimi](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview)

**[ISO-14496-12]** ISO/IEC 14496-12: Dosya biçimi, dördüncü sürüm 2012-07-15 bölümü 12 ISO temel medya.

**[RTMP]**  ["Adobe gerçek zamanlı Mesajlaşma Protokolü", 21 Kasım 2012](https://www.adobe.com/devnet/rtmp.html) 

------------------------------------------

## <a name="next-steps"></a>Sonraki adımlar
Görünüm Media Services'i öğrenme yolları.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
