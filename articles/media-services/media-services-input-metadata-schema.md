---
title: "Azure Media Services giriş meta veri şema | Microsoft Docs"
description: "Konu Azure Media Services giriş meta veri şema genel bir bakış sağlar."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d72848e2-4b65-4c84-94bc-e2a90a6e7f47
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 85a3662fec08cfc081095ef252842a30fde7dd27
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="input-metadata"></a>Meta veri girişi
Kodlama işinin bir giriş varlık (veya varlıklar) ile ilişkili kodlama bazı görevleri gerçekleştirmek istediğiniz üzerinde.  Bir görev tamamlandığında, çıkış varlık oluşturulur.  Çıkış varlığına, video, ses, küçük resimleri, bildirim, vb. içerir. Çıkış varlığına da giriş varlık hakkındaki meta verileri ile bir dosya içeriyor. Meta veri XML dosyasının adı şu biçimdedir: &lt;asset_id&gt;_metadata.xml (örneğin, 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), burada &lt;asset_id&gt; giriş varlık AssetID değeri.  

Meta veri dosyası incelemek isterseniz, oluşturabileceğiniz bir **SAS** Bulucu ve dosyayı yerel bilgisayarınıza indirin. Bir SAS Bulucu oluşturun ve dosya indirme konusunda bir örnek bulabilirsiniz [Media Services .NET SDK uzantıları kullanarak](media-services-dotnet-get-started.md).  

Bu makalede öğeleri ve XML Şeması türleri üzerinde ele giriş metada (&lt;asset_id&gt;_metadata.xml) dayanır.  Çıkış varlığına hakkındaki meta verileri içeren dosyası hakkında daha fazla bilgi için bkz: [çıkış meta verileri](media-services-output-metadata-schema.md).  

> [!NOTE]
> Bulabileceğiniz [şeması kodu](media-services-input-metadata-schema.md#code) bir [XML örneği](media-services-input-metadata-schema.md#xml) bu makalenin sonunda.  
> 
> 

## <a name="AssetFiles"></a>AssetFiles öğesi (kök öğesi)
Bir koleksiyonu içerir [AssetFile öğesi](media-services-input-metadata-schema.md#AssetFile)kodlama işi için s.  

Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

| Ad | Açıklama |
| --- | --- |
| **AssetFile**<br /><br /> minOccurs "1" maxOccurs = "unbounded" = |Tek bir alt öğe. Daha fazla bilgi için bkz: [AssetFile öğesi](media-services-input-metadata-schema.md#AssetFile). |

## <a name="AssetFile"></a>AssetFile öğesi
 Öznitelikler ve bir varlık dosyası açıklayan öğeler içerir.  

 Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Ad**<br /><br /> Gerekli |**xs: String** |Varlık dosya adı. |
| **Boyut**<br /><br /> Gerekli |**xs:Long** |Varlık dosyasının bayt cinsinden boyutu. |
| **Süre**<br /><br /> Gerekli |**xs: Duration** |İçerik play geri süresi. Örnek: Süre = "PT25M37.757S". |
| **NumberOfStreams**<br /><br /> Gerekli |**xs:int** |Varlık dosyası akış sayısı. |
| **FormatNames**<br /><br /> Gerekli |**xs: dize** |Biçim adları. |
| **FormatVerboseNames**<br /><br /> Gerekli |**xs: dize** |Biçim ayrıntılı adları. |
| **StartTime** |**xs: Duration** |İçerik başlangıç saati. Örnek: StartTime = "PT2.669S". |
| **OverallBitRate** |**xs: int** |Ortalama bit hızı Kbps varlık dosyasının. |

> [!NOTE]
> Aşağıdaki dört alt öğeleri bir sırada yer almalıdır.  
> 
> 

### <a name="child-elements"></a>Alt öğeler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Programlar**<br /><br /> minOccurs = "0" | |Tüm koleksiyon [programları öğesi](media-services-input-metadata-schema.md#Programs) varlık dosyası MPEG-TS biçiminde olduğunda. |
| **VideoTracks**<br /><br /> minOccurs = "0" | |Daha fazla videolar parça, uygun bir kapsayıcı biçimine araya eklemeli veya her fiziksel varlık dosyası sıfır içerebilir. Bu öğe tüm koleksiyonunu içerir [VideoTracks](media-services-input-metadata-schema.md#VideoTracks) varlık dosyası bir parçası. |
| **AudioTracks**<br /><br /> minOccurs = "0" | |Her fiziksel varlık dosyası, uygun bir kapsayıcı biçimi, araya eklemeli sıfır veya daha fazla ses izleri içerebilir. Bu öğe tüm koleksiyonunu içerir [AudioTracks](media-services-input-metadata-schema.md#AudioTracks) varlık dosyası bir parçası. |
| **Meta veriler**<br /><br /> minOccurs "0" maxOccurs = "unbounded" = |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Varlık dosyanın meta verilerini key\value dize olarak temsil. Örneğin:<br /><br /> **&lt;Meta verilerinin anahtarı "language" value = "eng" = /&gt;** |

## <a name="TrackType"></a>TrackType
Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Kimliği**<br /><br /> Gerekli |**xs:int** |Bu ses veya video izlemeyi sıfır tabanlı dizini.<br /><br /> Bu mutlaka olarak TrackID bir MP4 dosyasında kullanılan değildir. |
| **Codec** |**xs: String** |Video İzle codec dizesi. |
| **CodecLongName** |**xs: dize** |Ses veya video izleme codec uzun adı. |
| **Zaman temeli**<br /><br /> Gerekli |**xs: String** |Süresi tabanı. Örnek: Zaman temeli "1/48000" = |
| **NumberOfFrames** |**xs:int** |Çerçeve (video parçalar mevcut) sayısı. |
| **StartTime** |**xs: süresi** |İzleme başlangıç saati. Örnek: StartTime "PT2.669S" = |
| **Süre** |**xs: Duration** |Süre izler. Örnek: Süre = "PTSampleFormat M37.757S". |

> [!NOTE]
> Aşağıdaki iki alt öğeleri bir sırada yer almalıdır.  
> 
> 

### <a name="child-elements"></a>Alt öğeler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Değerlendirme**<br /><br /> minOccurs = "0" maxOccurs = "1" |[StreamDispositionType](media-services-input-metadata-schema.md#StreamDispositionType) |Sunu bilgileri (örneğin, belirli bir ses izleme görme engelli izleyicilere olup olmadığı) içerir. |
| **Meta veriler**<br /><br /> minOccurs "0" maxOccurs = "unbounded" = |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Çeşitli bilgiler tutmak için kullanılan genel anahtar/değer dizeleri. Örneğin, anahtar = "dil" değeri "eng" =. |

## <a name="AudioTrackType"></a>AudioTrackType (TrackType devralan)
 **AudioTrackType** devraldığı genel bir karmaşık tür [TrackType](media-services-input-metadata-schema.md#TrackType).  

 Türü belirli bir ses izleme varlık dosyasında temsil eder.  

 Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **SampleFormat** |**xs: String** |Örnek biçimi. |
| **ChannelLayout** |**xs: dize** |Kanal düzeni. |
| **Kanalları**<br /><br /> Gerekli |**xs:int** |(0 veya daha fazla) ses kanal sayısı. |
| **SamplingRate**<br /><br /> Gerekli |**xs:int** |Saniye başına örnekleri veya Hz ses örnekleme hızı. |
| **Bit hızı** |**xs:int** |Varlık dosyasından hesaplanan olarak saniyedeki ortalama ses bit hızı. Yalnızca başlangıç akışı yükü kabul edilir ve paketleme yükünü bu sayıma dahil değildir. |
| **BitsPerSample** |**xs:int** |WFormatTag biçimi için örnek başına bit yazın. |

## <a name="VideoTrackType"></a>VideoTrackType (TrackType devralan)
**VideoTrackType** devraldığı genel bir karmaşık tür [TrackType](media-services-input-metadata-schema.md#TrackType).  

Türü belirli bir video izlemek varlık dosyasında temsil eder.  

Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **FourCC**<br /><br /> Gerekli |**xs: String** |Görüntü codec FourCC kodu. |
| **Profil** |**xs: dize** |Video parçasının profili. |
| **Düzey** |**xs: dize** |Video parçasının düzeyi. |
| **PixelFormat** |**xs: dize** |Video parçasının piksel biçimi. |
| **Genişlik**<br /><br /> Gerekli |**xs:int** |Kodlanmış video genişliğini piksel cinsinden. |
| **Yükseklik**<br /><br /> Gerekli |**xs:int** |Piksel cinsinden görüntü yüksekliği kodlanmış. |
| **DisplayAspectRatioNumerator**<br /><br /> Gerekli |**xs: çift** |Video görüntü en boy oranını pay. |
| **DisplayAspectRatioDenominator**<br /><br /> Gerekli |**xs:double** |Video görüntü en boy oranını payda. |
| **DisplayAspectRatioDenominator**<br /><br /> Gerekli |**xs: çift** |Video örneği en boy oranını pay. |
| **SampleAspectRatioNumerator** |**xs: çift** |Video örneği en boy oranını pay. |
| **SampleAspectRatioNumerator** |**xs:double** |Video örneği en boy oranını payda. |
| **Kare hızı**<br /><br /> Gerekli |**xs:decimal** |Video kare hızı .3f biçiminde ölçülür. |
| **Bit hızı** |**xs:int** |Varlık dosyasından hesaplanan olarak kilobit / saniye ortalama video bit hızı. Yalnızca başlangıç akışı yükü kabul edilir ve paketleme ek yük dahil değildir. |
| **MaxGOPBitrate** |**xs: int** |Max GOP ortalama bit hızı kilobit bu video izlemek için. |
| **HasBFrames** |**xs:int** |Video parça B çerçeve sayısı. |

## <a name="MetadataType"></a>MetadataType
**MetadataType** anahtar/değer dize olarak bir varlık dosya meta verileri tanımlayan genel bir karmaşık türü. Örneğin, anahtar = "dil" değeri "eng" =.  

Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **anahtarı**<br /><br /> Gerekli |**xs: String** |Anahtar/değer çifti anahtar. |
| **değer**<br /><br /> Gerekli |**xs: String** |Anahtar/değer çifti değer. |

## <a name="ProgramType"></a>ProgramType
**ProgramType** bir programı anlatır genel bir karmaşık tür.  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **ProgramId**<br /><br /> Gerekli |**xs:int** |Program Kimliği |
| **NumberOfPrograms**<br /><br /> Gerekli |**xs:int** |Programları sayısı. |
| **PmtPid**<br /><br /> Gerekli |**xs:int** |Program eşleme tabloları (PMTs) programlar hakkında bilgi içerir.  Daha fazla bilgi için bkz: [DEVRESEL_ÖDEME](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT). |
| **PcrPid**<br /><br /> Gerekli |**xs: int** |Kod Çözücü tarafından kullanılır. Daha fazla bilgi için bkz: [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR) |
| **StartPTS** |**xs: uzun** |Sunu zaman damgası başlatılıyor. |
| **EndPTS** |**xs: uzun** |Bitiş sunu zaman damgası. |

## <a name="StreamDispositionType"></a>StreamDispositionType
**StreamDispositionType** akış açıklar genel bir karmaşık tür.  

Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Varsayılan**<br /><br /> Gerekli |**xs: int** |Bu öznitelik bu varsayılan sunu olduğunu göstermek için 1 olarak ayarlayın. |
| **Dub**<br /><br /> Gerekli |**xs:int** |Bu öznitelik bu Dublaj sunu olduğunu göstermek için 1 olarak ayarlayın. |
| **Özgün**<br /><br /> Gerekli |**xs: int** |Bu öznitelik bu özgün sunu olduğunu göstermek için 1 olarak ayarlayın. |
| **Açıklama**<br /><br /> Gerekli |**xs:int** |Bu öznitelik bu parça yorumlar içerdiğini belirtmek için 1 olarak ayarlayın. |
| **Sözleri**<br /><br /> Gerekli |**xs:int** |Bu öznitelik bu parça sözleri içerdiğini belirtmek için 1 olarak ayarlayın. |
| **Karaoke**<br /><br /> Gerekli |**xs:int** |Bu öznitelik bu karaoke İzle (arka plan müzik, hiçbir vokallerle) temsil eden belirtmek için 1 olarak ayarlayın. |
| **Zorla**<br /><br /> Gerekli |**xs:int** |Bu öznitelik bu zorlanmış sunu olduğunu göstermek için 1 olarak ayarlayın. |
| **HearingImpaired**<br /><br /> Gerekli |**xs:int** |Bu öznitelik işitme engelli için bu izleme belirtmek için 1 olarak ayarlayın. |
| **VisualImpaired**<br /><br /> Gerekli |**xs:int** |Bu öznitelik bu izleme için görme engelli belirtmek için 1 olarak ayarlayın. |
| **CleanEffects**<br /><br /> Gerekli |**xs: int** |Bu öznitelik bu parça temiz etkileri olduğunu belirtmek için 1 olarak ayarlayın. |
| **AttachedPic**<br /><br /> Gerekli |**xs: int** |Bu öznitelik bu parça resimleri sahip belirtmek için 1 olarak ayarlayın. |

## <a name="Programs"></a>Programları öğesi
Birden çok tutan kapsayıcı öğe **Program** öğeleri.  

### <a name="child-elements"></a>Alt öğeler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Program**<br /><br /> minOccurs "0" maxOccurs = "unbounded" = |[ProgramType](media-services-input-metadata-schema.md#ProgramType) |MPEG-TS biçimindeki varlık dosyaları için varlık dosyasındaki programlar hakkında bilgi içerir. |

## <a name="VideoTracks"></a>VideoTracks öğesi
 Birden çok tutan kapsayıcı öğe **VideoTrack** öğeleri.  

 Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="child-elements"></a>Alt öğeler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **VideoTrack**<br /><br /> minOccurs "0" maxOccurs = "unbounded" = |[VideoTrackType (TrackType devralan)](media-services-input-metadata-schema.md#VideoTrackType) |Varlık dosyasındaki video parçaları hakkında bilgiler içerir. |

## <a name="AudioTracks"></a>AudioTracks öğesi
 Birden çok tutan kapsayıcı öğe **AudioTrack** öğeleri.  

 Bu makalenin sonunda XML örneği bkz: [XML örneği](media-services-input-metadata-schema.md#xml).  

### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **AudioTrack**<br /><br /> minOccurs "0" maxOccurs = "unbounded" = |[AudioTrackType (TrackType devralan)](media-services-input-metadata-schema.md#AudioTrackType) |Varlık dosyasındaki ses izleri hakkında bilgiler içerir. |

## <a name="code"></a>Şema kodu
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
        <xs:attribute name="Id" use="required">  
          <xs:annotation>  
            <xs:documentation>zero-based index of this video track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Width" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video width in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Height" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video height in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
              <xs:annotation>  
                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:decimal">  
                  <xs:minInclusive value="0"/>  
                  <xs:fractionDigits value="3"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="MaxGOPBitrate">  
              <xs:annotation>  
                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Channels" use="required">  
              <xs:annotation>  
                <xs:documentation>number of audio channels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SamplingRate" use="required">  
              <xs:annotation>  
                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for the encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is the collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>the media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of the asset file in kbps</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:int">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  


## <a name="xml"></a>XML örneği
Giriş meta veri dosyası örneği verilmiştir.  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

