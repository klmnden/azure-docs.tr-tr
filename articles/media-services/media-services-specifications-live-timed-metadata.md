---
title: Azure Media Services - sinyal, canlı akış meta veri zaman aşımına | Microsoft Docs
description: Bu belirtimi canlı akış içinde sinyal zaman aşımına meta veriler için Media Services tarafından desteklenen iki moddan özetlenmektedir. Bu genel zamanlanmış meta veri sinyalleri yanı sıra SCTE-35 ad splice ekleme için sinyal için destek içerir.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2018
ms.author: johndeu;
ms.openlocfilehash: ae726b141f5f44b1eb0887cbd988881e41e163c0
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Meta veri canlı akış zaman aşımına sinyali


## <a name="1-introduction"></a>1 giriş 
Reklamları veya özel bir istemci oynatıcı yayıncıları genellikle olaylarına ekleme yapma kolaylaştırmak için gömülü içinde video zamanlanmış meta veri kullanımı. Media Services bu senaryoları etkinleştirmek için istemci uygulaması canlı akış kanala alma noktasından medya birlikte zamanlanmış meta veri taşıma için destek sağlar.
Zamanlanmış meta veriler için Media Services tarafından desteklenen bu belirtimi anahatları iki moddan akış sinyalleri Canlı:

1. [SCTE-35], [SCTE-67 tarafından] özetlenen önerilen uygulamaları heeds sinyali

2. Genel olmayan iletiler için modu sinyal meta veriler [SCTE-35] zaman aşımına uğradı

### <a name="12-conformance-notation"></a>1.2 uygunluk gösterimi
Anahtar sözcükler "gerekir", "Değil gerekir", "Gerekli", "SHALL", "Değil GELECEKTİR", "SHOULD", "Olmamalıdır", "Önerilen", "Olabilir" ve bu belgedeki "isteğe bağlı" RFC 2119 anlatıldığı gibi yorumlanacağını üzeresiniz

### <a name="13-terms-used"></a>1.3 kullanılan terimler

| Sözleşme Dönemi              | Tanım                                                                                                                                                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sunu saati | Bir Olay Görüntüleyicisi'sunulan süre. Saati şu Olay Görüntüleyici görür medya zaman çizelgesinde temsil eder. Örneğin, sunu SCTE-35 splice_info() komutu iletinin splice_time() saattir. |
| Geliş saati      | Bir olay iletisi ulaşan süre. Olay iletileri olay sunu zamanını öncesinde göndermesinden itibaren geçen zamanı genellikle olay sunu zamandan farklıdır.                                     |
| Seyrek İzle      | ortam sürekli olmayan izlemek ve zaman üst ya da Denetim parça ile eşitlenir.                                                                                                                                    |
| Kaynak            | Azure medya akış hizmeti                                                                                                                                                                                                |
| Kanal havuzu      | Azure Media canlı akış hizmeti                                                                                                                                                                                           |
| HLS               | Apple HTTP canlı Akış Protokolü                                                                                                                                                                                               |
| TİRE              | HTTP üzerinden Uyarlamalı dinamik                                                                                                                                                                                             |
| Kesintisiz            | Kesintisiz Akış Protokolü                                                                                                                                                                                                        |
| MPEG2-TS          | MPEG 2 aktarım akışları                                                                                                                                                                                                         |
| RTMP              | Gerçek zamanlı multimedya Protokolü                                                                                                                                                                                                    |
| uimsbf            | İlk bit işaretsiz tamsayıyı, en önemli.                                                                                                                                                                                    |

-----------------------

## <a name="2-timed-metadata-ingest"></a>2 zamanlanmış meta verileri alma
## <a name="21-rtmp-ingest"></a>2.1 RTMP alma
RTMP RTMP akış içinde katıştırılmış AMF işaret iletileri olarak gönderilen zamanlanmış meta verileri sinyalleri destekler. İşaret iletileri gerçek olayın önce biraz zaman gönderilebilir veya ad splice ekleme gerçekleşmesi gerekir. Bu senaryoyu desteklemek için splice veya segment gerçek süre içinde kurut iletisi gönderilir. Daha fazla bilgi için [AMF0] bakın.

Aşağıdaki tabloda, Media Services alma AMF ileti yükü biçimi açıklanmaktadır.

AMF ileti adını, aynı türde birden çok olay akışları ayırt etmek için kullanılabilir.  [SCTE-35] iletilerde AMF ileti adı "onAdCue" [SCTE-67] önerilen olması gerekir.  Böylece Bu tasarımın yenilik gelecekte Yasak değil, aşağıda listelenen olmayan herhangi bir alan dikkate gerekir.

### <a name="signal-syntax"></a>Sinyal sözdizimi
Media Services RTMP basit mod için aşağıdaki biçimde "onAdCue" adlı tek bir AMF işaret ileti destekler:

### <a name="simple-mode"></a>Basit mod

| Alan adı | Alan türü | Gerekli mi? | Açıklamalar                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| işaret        | Dize     | Gerekli | Olay iletisi.  Basit mod belirlemek için "SpliceOut" splice olması.                                              |
| id         | Dize     | Gerekli | Splice veya kesimi açıklayan benzersiz bir tanımlayıcı. Bu ileti örneğini tanımlar                            |
| Süre   | Sayı     | Gerekli | Splice süresini. Kesirli saniye birimleridir.                                                                |
| Geçen    | Sayı     | İsteğe bağlı | Sinyal desteklemek için tekrarlanırsa ince ayar, bu alan splice başlamasından bu yana geçen sunu süre miktarını olacaktır. Kesirli saniye birimleridir. Basit mod kullanırken, bu değer splice özgün süresini aşamaz.                                                  |
| time       | Sayı     | Gerekli | Splice süresini sunu zaman içinde tutulamaz. Kesirli saniye birimleridir.                                     |

---------------------------

### <a name="scte-35-mode"></a>SCTE-35 modu

| Alan adı | Alan türü | Gerekli mi? | Açıklamalar                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| işaret        | Dize     | Gerekli | Olay iletisi.  [SCTE-35] iletileri için bu base64 olmalıdır (IETF RFC 4648) ikili kodlanmış [SCTE-67] uyumlu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırayla splice_info_section().                                              |
| type       | Dize     | Gerekli | Bir URN veya ileti düzeni tanımlayan URL; Örneğin, "urn: Örnek: sinyal: 1.0".  [SCTE-35] iletileri için bu "urn: scte:scte35:2013a:bin" [SCTE-67] uyumlu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırada olmalıdır.  |
| id         | Dize     | Gerekli | Splice veya kesimi açıklayan benzersiz bir tanımlayıcı. Bu ileti örneğini tanımlar.  İletileri eşdeğer semantiği ile aynı değere sahip.|
| Süre   | Sayı     | Gerekli | Olay ya da ad splice-biliniyorsa segment süresi. Bilinmiyorsa, değeri 0 olmalıdır.                                                                 |
| Geçen    | Sayı     | İsteğe bağlı | [SCTE-35] ad sinyal ayarlamak için tekrarlanırsa, bu alan splice başlamasından bu yana geçen sunu süre miktarını olacaktır. Kesirli saniye birimleridir. [SCTE-35] modunda, bu değer özgün belirtilen süresi splice veya segment aşabilir.                                                  |
| time       | Sayı     | Gerekli | Olay ya da ad sunu zamanını splice.  Sunu saatini ve süresini akış erişim noktaları (SAP ile) 1 veya 2 ' türündeki [ISO-14496-12] eki ı içinde tanımlanan uyumlu olmalıdır. HLS çıkışı için zamanını ve süresini segment sınırları ile uyumlu olmalıdır. Sunu saatini ve süresini aynı içinde farklı olay iletilerinin olay akışının çakışmaması gerekir. Kesirli saniye birimleridir.

---------------------------

#### <a name="211-cancelation-and-updates"></a>2.1.1 iptali ve güncelleştirmeler

İletileri iptal ya da aynı sunu zamanını ve kimliğini içeren birden fazla ileti göndererek güncelleştirildi Olay Kimliği ve sunu saat benzersiz şekilde tanımlamak ve son yayın öncesi kısıtlamaları karşılayan bir belirli bir sunu süre alınan ileti üzerinde işlem iletisidir. Güncelleştirilmiş olay daha önce alınan tüm iletiler yerini alır. Yayın öncesi kısıtlaması dört saniyedir. En az dört saniye sunu saatten önce alınan ileti üzerinde işlem yapılması.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 parçalanmış MP4 (kesintisiz akış) alma
Canlı akış gereksinimlerine alma [Canlı-FMP4] bakın. Aşağıdaki bölümler, zamanlanmış bir sunu meta verileri alma ilişkin ayrıntılar sağlar.  Hem dinamik sunucu bildirim kutusuna tanımlı bir seyrek parçası olarak zamanlanmış bir sunu meta veri alınan (MS-SSTR bakın) ve film kutusu ('moov').  Her seyrek parça 'mdat' kutusu ikili ileti olduğu bir film parça kutusu ('moof') ve ortam veri kutusu ('mdat') oluşur.

#### <a name="221-live-server-manifest-box"></a>2.2.1 Canlı sunucusu liste kutusu
Seyrek izleme Canlı sunucu bildirim kutusuyla bildirilmelidir bir \<textstream\> ve girişi ayarlamak aşağıdaki özniteliklerine sahip olmalıdır:

| **Öznitelik adı** | **Alan türü** | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Sayı         | Gerekli      | "Bilinmeyen, değişken hızına sahip bir izleme gösteren 0" olmalıdır.                                                                                                                                                                                                 |
| parentTrackName    | Dize         | Gerekli      | Seyrek izleme saat kodları hizalı ölçeği olan ana izleme adı olması gerekir. Üst izleme seyrek bir parçası olamaz.                                                                                                                    |
| manifestOutput     | Boole        | Gerekli      | "Seyrek izleme sorunsuz istemci bildiriminde ekli belirtmek için true", olması gerekir.                                                                                                                                                               |
| Alt Tür            | Dize         | Gerekli      | GEREKEN olması dört karakter kodu "DATA".                                                                                                                                                                                                                         |
| Düzeni             | Dize         | Gerekli      | İleti düzeni tanımlayan bir URN veya URL olmalıdır; Örneğin, "urn: Örnek: sinyal: 1.0". [SCTE-35] iletileri için bu "urn: scte:scte35:2013a:bin" [SCTE-67] uyumlu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırada olmalıdır. |
| TrackName          | Dize         | Gerekli      | Seyrek izleme adı olması gerekir. TrackName aynı düzeni ile birden çok olay akışları ayırt etmek için kullanılabilir. Her benzersiz olay akışında bir benzersiz izleme adı olması gerekir.                                                                           |
| Zaman Çizelgesi          | Sayı         | İsteğe bağlı      | Üst İzleme'nin zaman ölçeğini olması gerekir.                                                                                                                                                                                                                      |

-------------------------------------

### <a name="222-movie-box"></a>2.2.2 film kutusu

Film kutusu ('moov') Canlı sunucusu bildirim kutusu seyrek izlemek için akış üstbilgisinin bir parçası olarak izler.

'Moov' kutusu içermesi gereken bir **TrackHeaderBox ('tkhd')** kutusuna [ISO-14496-12'de] aşağıdaki kısıtlamalarla tanımlanan:

| **Alan adı** | **Alan türü**          | **Gerekli?** | **Açıklama**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| Süre       | 64 bit işaretsiz tamsayıyı | Gerekli      | 0 ise, izleme kutusu sıfır örnekleri sahiptir ve izleme kutusunu örneklerinde toplam süresi 0 olduğundan olmalıdır. |
-------------------------------------

'Moov' kutusu içermesi gereken bir **HandlerBox ('hdlr')** [ISO-14496-12'de] aşağıdaki kısıtlamalarla tanımlanan:

| **Alan adı** | **Alan türü**          | **Gerekli?** | **Açıklama**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | 32 bit işaretsiz tamsayıyı | Gerekli      | 'Meta' olmalıdır. |
-------------------------------------

'Stsd' kutusu kodlama adı MetaDataSampleEntry kutusuyla [ISO-14496-12'de] tanımlanan içermelidir.  Örneğin, SCTE-35 iletilerde 'scte' kodlama adı olmalıdır.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 film parça ve medya veri kutularının

Seyrek parça parça film parça kutusu ('moof') ve bir ortam veri kutusu ('mdat') oluşur.

MovieFragmentBox ('moof') kutusunu içermesi gereken bir **TrackFragmentExtendedHeaderBox ('UUID')** kutusuna [FMP4] içinde aşağıdaki alanlarla tanımlanan:

| **Alan adı**         | **Alan türü**          | **Gerekli?** | **Açıklama**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | 64 bit işaretsiz tamsayıyı | Gerekli      | Olayın geliş saati olması gerekir. Bu değer, iletinin üst izleme ile hizalar.   |
| fragment_duration      | 64 bit işaretsiz tamsayıyı | Gerekli      | Olayın süresi olması gerekir. Süre süre bilinmeyen olduğunu belirtmek için sıfır olabilir. |

------------------------------------


MediaDataBox ('mdat') kutusunu şu biçimde olmalıdır:

| **Alan adı**          | **Alan türü**                   | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sürüm                 | 32 bit işaretsiz tamsayıyı (uimsbf) | Gerekli      | 'Mdat' kutusunun içeriğini biçimini belirler. Tanınmayan sürümleri yoksayılacak. Şu anda yalnızca desteklenen sürüm 1'dir.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32 bit işaretsiz tamsayıyı (uimsbf) | Gerekli      | Bu ileti örneğini tanımlar. İletileri eşdeğer semantiği ile aynı değere sahip; diğer bir deyişle, aynı kimliğe sahip herhangi bir olay ileti kutusu işleme yeterli olur.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32 bit işaretsiz tamsayıyı (uimsbf) | Gerekli      | Belirtilen TrackFragmentExtendedHeaderBox ve presentation_time_delta fragment_absolute_time toplamını olay sunu sırada olmalıdır. Sunu saatini ve süresini akış erişim noktaları (SAP ile) 1 veya 2 ' türündeki [ISO-14496-12] eki ı içinde tanımlanan uyumlu olmalıdır. HLS çıkışı için zamanını ve süresini segment sınırları ile uyumlu olmalıdır. Sunu saatini ve süresini aynı içinde farklı olay iletilerinin olay akışının çakışmaması gerekir. |
| message                 | bayt dizisi                       | Gerekli      | Olay iletisi. [SCTE-67] başka bir şey önerir rağmen [SCTE-35] iletileri için ikili splice_info_section() iletisidir. [SCTE-35] iletileri için bu splice_info_section() [SCTE-67] uyumlu HLS, kesintisiz ve tire istemcilere gönderilecek iletilerin sırada olmalıdır. [SCTE-35] iletileri için ikili splice_info_section() 'mdat' kutusu yükü ve base64 ile kodlanmış değil.                                                            |

------------------------------


### <a name="224-cancelation-and-updates"></a>2.2.4 iptali ve güncelleştirmeler
İletileri iptal ya da aynı sunu zamanını ve kimliğini içeren birden fazla ileti göndererek güncelleştirildi  Kimliği ve sunu saat olay benzersiz şekilde tanımlar. Son yayın öncesi kısıtlamaları karşılayan bir belirli bir sunu süredir alınan ileti üzerinde işlem iletisidir. Güncelleştirilmiş ileti tüm daha önce alınan iletiler değiştirir.  Yayın öncesi kısıtlaması dört saniyedir. En az dört saniye sunu saatten önce alınan ileti üzerinde işlem yapılması. 


## <a name="3-timed-metadata-delivery"></a>Meta veri teslim 3 zaman aşımına uğradı

Olay akışı Media Services'e donuk verilerdir. Media Services üç parça bilgi alma uç noktası ve istemci uç arasında yalnızca geçirir. Aşağıdaki özellikleri [SCTE-67] ile uyumlu bir istemciye teslim edilir ve/veya istemci iletişim kuralı akış:

1.  Düzeni – bir URN veya iletinin düzeni tanımlayan URL.

2.  Sunu saati – sunu medya zaman çizelgesi olayının.

3.  Süresi – olay süresi.

4.  Kimliği – olay için isteğe bağlı bir benzersiz tanımlayıcı.

5.  İleti – olay verileri.


## <a name="31-smooth-streaming-delivery"></a>3.1 kesintisiz akış teslimi

[FMP4] belirtimleri ayrıntılarındaki işleme seyrek izleme bakın ve [MS-SSTR].

#### <a name="smooth-client-manifest-example"></a>Kesintisiz istemci bildirim örneği
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
Zamanlanmış meta verileri için Apple HTTP canlı akışı (HLS), özel bir M3U etiket içinde segment çalma katıştırılmış.  Uygulama katmanı M3U çalma listesi ayrıştırma ve M3U etiketleri işlem. Azure Media Services zamanlanmış meta veriler [HLS] içinde tanımlı EXT X işaret etiketinde katıştırır.  EXT X işaret etiketi ADI3 türden iletileri için şu anda DynaMux tarafından kullanılır.  SCTE-35 ve olmayan SCTE-35 iletileri desteklemek için aşağıda tanımlanan EXT X işaret etiket özniteliklerini ayarlayın:

| **Öznitelik adı** | **Tür**                      | **Gerekli?**                             | **Açıklama**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CUE                | Tırnak işaretli dize                 | Gerekli                                  | Bölümünde açıklandığı gibi bir base64 dizesi olarak kodlanmış bir ileti [IETF RFC 4648](http://tools.ietf.org/html/rfc4648). [SCTE-35] iletilerde base64 ile kodlanmış splice_info_section() budur.                                                                                                |
| TÜR               | Tırnak işaretli dize                 | Gerekli                                  | Bir URN veya ileti düzeni tanımlayan URL; Örneğin, "urn: Örnek: sinyal: 1.0". [SCTE-35] iletileri için türü "scte35" özel değerini alır.                                                                                                                                |
| Kimlik                 | Tırnak işaretli dize                 | Gerekli                                  | Olayı için benzersiz bir tanımlayıcı. İleti alınan zaman kimliği belirtilmezse, Azure Media Services benzersiz bir kimliği oluşturur.                                                                                                                                          |
| SÜRE           | ondalık kayan nokta sayısı | Gerekli                                  | Olayın süresi. Bilinmiyorsa, değeri 0 olmalıdır. Factional saniye birimleridir.                                                                                                                                                                                           |
| GEÇEN            | ondalık kayan nokta sayısı | İsteğe bağlı, ancak kayan pencere için gereklidir | Bu alan, sinyal bir kayan bir sunu pencere desteklemek için tekrarlanırsa, olay başlamasından bu yana geçen sunu süre miktarını olması gerekir. Kesirli saniye birimleridir. Bu değer, özgün belirtilen süresi splice veya segment aşabilir. |
| ZAMAN               | ondalık kayan nokta sayısı | Gerekli                                  | Olay sunu zamanı. Kesirli saniye birimleridir.                                                                                                                                                                                                                    |


HLS player uygulama katmanı türü, ileti biçimi tanımlamak, ileti kod çözme, gerekli saat dönüşümleri uygulamak ve olay işleme için kullanır.  Üst izleme segment çalma listesi eşitlemesinde göre olay zaman damgası olaylardır.  Bunların en yakın segment (#EXTINF etiketi) önce eklenir.

#### <a name="hls-segment-playlist-example"></a>HLS Segment çalma listesi örneği
~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:6
#EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
#EXTINF:6.000000,no-desc
Fragments(video=0,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=60000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
#EXT-X-CUE: ID=”metadata-12.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”12.000000”, DURATION=”18.000000”,CUE=”HrwOi8vYmWVkaWEvhhaWFRlRDa=”
Fragments(video=120000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=180000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=240000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=300000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=360000000,format=m3u8-aapl)
#EXT-X-CUE: ID=”metadata-42.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”42.000000”, DURATION=”60.000000”,CUE=”PD94bWwgdm0iMS4wIiBlbmNvpD4=”
#EXTINF:6.000000,no-desc
Fragments(video=420000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=480000000,format=m3u8-aapl)
…
~~~

#### <a name="hls-message-handling"></a>HLS ileti işleme

Olayları her video ve ses izleme segment çalma listesi sinyal. EXT X işaret etiketi konumunu her zaman olması ya da ilk HLS (splice çıkışı veya segment Başlat) segmentlere hemen önce veya sonra hemen son HLS (splice içinde veya segment son) hangi ZAMANINI ve süresini özniteliklerini başvurduğu segment gerekir, [gerektirdiği HLS].

Bir kayan bir sunu pencere etkinleştirildiğinde, EXT X işaret etiketi sıklıkta splice veya segment her zaman segment çalma listesi tam olarak açıklanan ve geçen özniteliği splice süreyi belirtmek için kullanılması gereken veya kesimi vardır yinelenmesi gerekir [HLS] gerektirdiği şekilde etkin bırakıldı.

Bir kayan bir sunu pencere etkinleştirildiğinde, başvurdukları medya zaman kayan sunu pencere dışında gezinirken EXT X işaret etiketleri segment çalma listesinden kaldırılır.

## <a name="33--dash-delivery"></a>3.3 DASH teslim
[Tire] sinyal olayları için üç yol sunar:

1.  MPD işaret olayları
2.  Bant dışı (olay ileti kutusu ('emsg') kullanarak gösteriminde olayları işareti
3.  1 ve 2 birleşimi

Olayların MPD işaret istemcileri hemen MPD yüklendiğinde tüm olayları, erişiminiz olduğundan VOD akış için kullanışlıdır. Bant dışı çözüm, istemciler MPD yeniden indirmek gerekmediği için canlı akış için yararlıdır. Zaman tabanlı kesimleme süresini ve zaman damgası geçerli kesimin ekleyerek istemci sonraki segment URL'sini belirler. Bu isteği başarısız olursa, istemci süreksizlik varsayar ve MPD indirir, ancak Aksi takdirde MPD indirmeden akış sürdürür.

Azure Media Services hem sinyal MPD yapın ve olay iletisi kutusunu kullanarak sinyal bant içi.

#### <a name="mpd-signaling"></a>MPD sinyali

Olayları dönem öğe içinde görünür EventStream öğesi kullanılarak MPD içinde işaret.

EventStream öğesi aşağıdaki özniteliklere sahiptir:

| **Öznitelik adı** | **Tür**                | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | string                  | Gerekli      | İletinin düzeni tanımlar. Düzeni düzeni öznitelik değeri için Canlı sunucu bildirim kutusunda ayarlanır. Değer bir URN veya ileti düzeni tanımlayan URL'si olmalıdır; Örneğin, "urn: Örnek: sinyal: 1.0".                                                                |
| değer              | string                  | İsteğe bağlı      | İleti semantiği özelleştirmek için düzeni sahipleri tarafından kullanılan bir ek bir dize değeri. Aynı düzeni ile birden çok olay akışları birbirinden ayırmak için değeri (kesintisiz için alma trackName veya AMF ileti adı RTMP alma) olay akışında adına ayarlanması gerekir. |
| Zaman Çizelgesi          | 32 bit işaretsiz tamsayıyı | Gerekli      | İkinci kez ve 'emsg' kutusu içinde süresi alanlarının her ölçeği.                                                                                                                                                                                                       |


Sıfır veya daha fazla olay öğeler EventStream öğe içinde yer alır ve aşağıdaki özniteliklere sahiptir:

| **Öznitelik adı**  | **Tür**                | **Gerekli?** | **Açıklama**                                                                                                                                                                                                             |
|---------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| presentation_time   | 64 bit işaretsiz tamsayıyı | İsteğe bağlı      | Medyanın dönem başlangıcı göre olayın sunu saatini olması gerekir. Sunu saatini ve süresini akış erişim noktaları (SAP ile) 1 veya 2 ' türündeki [ISO-14496-12] eki ı içinde tanımlanan uyumlu olmalıdır. |
| Süre            | 32 bit işaretsiz tamsayıyı | İsteğe bağlı      | Olayın süresi. Süre bilinmiyorsa, bu gözardı gerekir.                                                                                                                                                 |
| id                  | 32 bit işaretsiz tamsayıyı | İsteğe bağlı      | Bu ileti örneğini tanımlar. İletileri eşdeğer semantiği ile aynı değere sahip. İleti alınan zaman kimliği belirtilmezse, Azure Media Services benzersiz bir kimliği oluşturur.             |
| Olay öğesi değeri | string                  | Gerekli      | Olay iletisi açıklandığı gibi bir base64 dizesi olarak [IETF RFC 4648](http://tools.ietf.org/html/rfc4648).                                                                                                                   |

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

<EventStream schemeIdUri=”urn:example:signaling:1.0” timescale=”1000” value=”player-statistics”>
  <Event presentationTime=”0” duration=”10000” id=”0”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
  <Event presentationTime=”20000” duration=”10000” id=”1”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
</EventStream>
~~~

>[!NOTE]
>Bu presentationTime sunu zamanını olay değil ileti geliş saati olduğuna dikkat edin.
>

### <a name="431-in-band-event-message-box-signaling"></a>4.3.1 bant dışı olay ileti kutusu sinyali
Bant dışı olay akışının MPD InbandEventStream öğenin uyarlama kümesi düzeyinde olmasını gerektirir.  Bu öğe ayrıca ileti kutusu ('emsg') olay görünür bir zorunlu schemeIdUri özniteliği ve isteğe bağlı ölçeği özniteliği vardır.  Olay ileti kutuları MPD tanımlı değil düzeni tanımlayıcılarına sahip mevcut olmamalıdır. Bir tire istemci MPD tanımlanmamış bir düzeni içeren bir olay ileti kutusu algılarsa, istemci yok sayın beklenir.
Olay iletisi kutusu ('emsg') medya sunu saati ile ilgili genel olaylar için sinyal sağlar. Varsa, herhangi bir 'emsg' kutusunu önce herhangi bir 'moof' kutusunu yerleştirilmesi.

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

DASHEventMessageBox alanları aşağıda tanımlanmıştır:

| **Alan adı**          | **Alan türü**          | **Gerekli?** | **Açıklama**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | string                  | Gerekli      | İletinin düzeni tanımlar. Düzeni düzeni öznitelik değeri için Canlı sunucu bildirim kutusunda ayarlanır. Değer bir URN veya ileti düzeni tanımlayan URL'si olmalıdır; Örneğin, "urn: Örnek: sinyal: 1.0". [SCTE-67] başka bir şey önerir rağmen [SCTE-35] iletileri için bu özel değerini "urn: scte:scte35:2013a:bin" alır. |
| Değer                   | string                  | Gerekli      | İleti semantiği özelleştirmek için düzeni sahipleri tarafından kullanılan bir ek bir dize değeri. Aynı düzeni ile birden çok olay akışları birbirinden ayırmak için değer (kesintisiz için alma trackName veya AMF ileti adı RTMP alma) olay akışında adına ayarlanır.                                                                  |
| Zaman Çizelgesi               | 32 bit işaretsiz tamsayıyı | Gerekli      | İkinci kez ve 'emsg' kutusu içinde süresi alanlarının her ölçeği.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | 32 bit işaretsiz tamsayıyı | Gerekli      | Medya sunu zaman aralığı olay sunu zamanı ve bu kesimdeki erken sunu saati. Sunu saatini ve süresini akış erişim noktaları (SAP ile) 1 veya 2 ' türündeki [ISO-14496-12] eki ı içinde tanımlanan uyumlu olmalıdır.                                                                                            |
| event_duration          | 32 bit işaretsiz tamsayıyı | Gerekli      | Olay ya da bir bilinmeyen süresi belirtmek için 0xFFFFFFFF süresi.                                                                                                                                                                                                                                                                                          |
| Kimlik                      | 32 bit işaretsiz tamsayıyı | Gerekli      | Bu ileti örneğini tanımlar. İletileri eşdeğer semantiği ile aynı değere sahip. İleti alınan zaman kimliği belirtilmezse, Azure Media Services benzersiz bir kimliği oluşturur.                                                                                                                                                    |
| Message_data            | bayt dizisi              | Gerekli      | Olay iletisi. [SCTE-67] başka bir şey önerir rağmen [SCTE-35] iletileri için ileti ikili splice_info_section() veridir.                                                                                                                                                                                                                                 |

### <a name="332-dash-message-handling"></a>3.3.2 ileti işleme tire

Olayları işaret bant, video ve ses parçaları için 'emsg' kutunun içinde.  Sinyal tüm istekleri presentation_time_delta 15 saniye olduğu segmentlere ayırmak için veya daha az oluşur. Bir kayan bir sunu pencere etkinleştirildiğinde, toplam süre ve olay iletisi süresi bildiriminde medya verilerini saatinden daha küçük olduğunda olay iletileri MPD kaldırılır.  Diğer bir deyişle, bunlar başvurduğu medya zaman kayan sunu pencere dışında gezinirken olay iletileri bildirimden kaldırılır.

## <a name="4-scte-35-ingest"></a>4 SCTE-35 alma

[SCTE-35] iletileri şeması kullanarak ikili dosya biçiminde alınan **"urn: scte:scte35:2013a:bin"** için kesintisiz alma ve türü **"scte35"** için RTMP alma. MPEG-2 aktarım akışı sunu zaman damgaları (NK) NK arasında bir eşleme dayalı [SCTE-35] zamanlama dönüştürülmesi kolaylaştırmak için (pts_time + pts_adjustment splice_time()) ve medya zaman çizelgesi sağlanır tarafından olay sunu saati ( "yumuşak fragment_absolute_time alan alma ve zaman alan RTMP için alma). Eşleme gerekli çünkü 33-bit NK değeri 26,5 saatte yaklaşık yapar.

Kesintisiz akış alma medya verileri kutusu ('mdat') içermelidir gerektirir **splice_info_section()** Tablo 8-1'de [SCTE-35] tanımlı. İçin RTMP alma, AMF iletisinin işaret özniteliği base64encoded ayarlanmış **splice_info_section()**. Yukarıda açıklanan biçimde iletileri sahip olduğunuzda, [SCTE-67] uyumlu HLS, kesintisiz ve tire istemcilere gönderilir.


## <a name="5-references"></a>5 başvuruları

**[SCTE-35]**  ANSI/SCTE 35 2013a – kablo, 2013a için dijital Program ekleme Cueing iletisi

**[SCTE-67]**  ANSI/SCTE 67 2014 – SCTE 35 yöntem önerilir: kablo için dijital Program ekleme Cueing iletisi

**[TİRE]**  ISO/IEC 23009-1 2014 – bilgi teknolojisi – dinamik HTTP (DASH üzerinde) – Kısım 1 Uyarlamalı: Media sunu açıklaması ve segment biçimleri, 2 sürümü

**[HLS]**  ["HTTP canlı akış", draft-pantos-http-live-streaming-14, 14 Ekim 2014](http://tools.ietf.org/html/draft-pantos-http-live-streaming-14)

**[MS-SSTR]**  ["Microsoft kesintisiz Akış Protokolü", 15 Mayıs 2014](http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

**[AMF0]**  ["Eylemi ileti biçimi AMF0"](http://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)

**[FMP4]**  [IIS kesintisiz akış dosyası/kablo biçim belirtimi](https://microsoft.sharepoint.com/teams/mediaservices/_layouts/15/WopiFrame.aspx?sourcedoc=%7bAC5A31A4-E455-4000-96E1-AB17BD083144%7d&file=IIS%20Smooth%20Streaming%20File%20Format%20Specification%20-%20v%202%203%2001%20latest%20draft.docx&action=default)

**[LIVE-FMP4]** [Azure Media Services Fragmented MP4 Live Ingest Specification](https://microsoft.sharepoint.com/teams/mediaservices/_layouts/15/WopiFrame.aspx?sourcedoc=%7b5CEE1122-AA28-4368-BC8E-9C0048BF1529%7d&file=AMS%20F-MP4%20Live%20Ingest%20Specification.docx&action=default)

**[ISO-14496-12]** ISO/IEC 14496-12: Part 12 ISO base media file format, Fourth Edition 2012-07-15.

**[RTMP]**  ["Adobe gerçek zamanlı ileti Protokolü" 21 Kasım 2012](http://wwwimages.adobe.com/www.adobe.com/content/dam/Adobe/en/devnet/rtmp/pdf/rtmp_specification_1.0.pdf) 

------------------------------------------

## <a name="next-steps"></a>Sonraki adımlar
Görünüm Media Services'i öğrenme yolları.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
