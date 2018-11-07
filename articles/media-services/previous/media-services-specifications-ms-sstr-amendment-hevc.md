---
title: Azure medya Hizmetleri - kesintisiz akış HEVC için Protokolü (MS-SSTR) değişiklik Protokolü | Microsoft Docs
description: Bu belirtim, protokol ve parçalanmış MP4 tabanlı Canlı HEVC Azure Media Services akış biçimi açıklar. Bu, kesintisiz Akış Protokolü documentaiton HEVC alma için destek eklemek için (MS-SSTR) ve akış için bir değişiklik olur. Bu makalede, yalnızca HEVC sunmak için gereken değişiklikler belirtilen dışında olan "(değişiklik)" metin yalnızca açıklama kopyalanır belirtir.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: ''
ms.assetid: f27d85de-2cb8-4269-8eed-2efb566ca2c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: johndeu;
ms.openlocfilehash: 6330de2aa67fd83a5d4762c2c13d4916f642743d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250943"
---
# <a name="smooth-streaming-protocol-ms-sstr-amendment-for-hevc"></a>Kesintisiz Akış Protokolü (MS-SSTR) değişiklik için HEVC

## <a name="1-introduction"></a>1 giriş 

Bu makalede, kesintisiz akış protokolü belirtimi [kesintisiz akış, kodlanmış HEVC video etkinleştirmek için MS-SSTR] uygulanacak ayrıntılı tarihli amendments sağlar. Bu belirtiminde biz yalnızca HEVC video codec sunmak için gereken değişiklikleri ana hat. Makale aynı numaralandırma şeması belirtimi [MS-SSTR] olarak izler. Makale boyunca sunulan boş başlıkları konumlarına [MS-SSTR] belirtiminde okuyucu yönlendirmek için sağlanır.  "(Değişiklik)" metin yalnızca açıklama amaçlıdır kopyalanır belirtir.

Bu makalede bildiriminde kesintisiz akış HEVC video codec sinyal teknik uygulama gereksinimleri ve HEVC HEVC ortak şifreleme ve kutusunu geçerli MPEG standartları başvurmak için güncelleştirilmiş örnek oluşturan başvurular ISO temel medya dosya biçimi için adlar, en son özellikleri ile tutarlı olacak şekilde güncelleştirildi. 

Başvurulan kesintisiz akış protokolü belirtimi [MS-SSTR] içinde aşağıdaki yolla canlı ve isteğe bağlı dijital medya, ses ve video gibi sunmak için kullanılan gönderme biçimini açıklar: bir kodlayıcı bir web sunucusuna bir sunucudan başka bir sunucuya ve öğesinden gelen bir sunucu için bir HTTP istemcisi.
MPEG-4 kullanımını ([MPEG4-RA)](https://go.microsoft.com/fwlink/?LinkId=327787)-temel alan veri yapısı teslim HTTP üzerinden neredeyse gerçek zamanlı farklı kalite düzeylerine sıkıştırılmış medya içeriklerinin arasında geçiş sorunsuz sağlar. İstemci bilgisayarı veya cihazı için ağ ve video işleme koşulları değiştirseniz sonucu HTTP istemci son kullanıcı için sabit bir kayıttan yürütme deneyimidir.

## <a name="11-glossary"></a>1.1 sözlüğü 

Aşağıdaki terimler tanımlanan *[MS-GLOS]*:

>   **genel benzersiz tanıtıcısı (GUID) evrensel benzersiz tanımlayıcı (UUID)**

Aşağıdaki koşulları bu belgeye özgü:

>  **oluşturma zamanı:** zaman bir örnek istemcide tanımlandığı şekilde sunulan [[ISO/IEC-14496-12].](https://go.microsoft.com/fwlink/?LinkId=183695)

>   **CENC**: ortak [ISO/IEC 23001-7] İkinci Sürüm'de tanımlandığı gibi şifreleme.

>   **kod çözme zamanı:** zamanı örnek sınıfında tanımlandığı gibi istemci üzerinde kodu çözülecek gereklidir [[ISO/IEC http://go.microsoft.com/fwlink/?LinkId=18369514496-12].](https://go.microsoft.com/fwlink/?LinkId=183695)

**Parça:** bağımsız olarak indirilebilir bir ölçü **medya** bir veya daha fazla oluşur **örnekleri**.

>   **HEVC:** yüksek verimlilik Video kodlama, [ISO/IEC 23008 2]'içinde tanımlanan

>   **bildirim:** hakkındaki meta verileri **sunu** olanak sağlayan bir istemci isteği yapmak **medya**. **Medya:** yürütmek için istemcinin kullandığı ses, video ve metin veri sıkıştırılmış bir **sunu**. **medya biçimi:** ses veya video sıkıştırılmış temsil etmek için iyi tanımlanmış bir biçimde **örnek**.

>   **Sunu:** tüm kümesini **akışları** ve tek bir filmi yürütmek için gereken ilgili meta veriler. **İstek:** tanımlandığı gibi istemciden sunucuya gönderilen bir HTTP iletisi [[RFC2616].](https://go.microsoft.com/fwlink/?LinkId=90372) **Yanıt:** tanımlandığı gibi sunucudan istemciye gönderilen HTTP iletisi [[RFC2616].](https://go.microsoft.com/fwlink/?LinkId=90372)

>   **Örnek:** , en küçük temel birimi (örneğin, bir çerçeve) **medya** depolanabilir ve işlenebilir.

>   **Mayıs, SHOULD, gerekir, GEREKTİĞİ, gerekir:** işbu koşullarda (tümü büyük harf) bölümünde anlatıldığı gibi kullanılan [[RFC2119].](https://go.microsoft.com/fwlink/?LinkId=90317) İsteğe bağlı bir davranış kullanım tüm deyimler ya da olabilir, SHOULD veya olmamalıdır.

## <a name="12-references"></a>1.2 başvuruları 
-----------

>   En son sürüme sık sık güncelleştirilir belgelerin bağlantıları olduğundan Microsoft açık belirtimleri belgeleri başvuruları yayımlama yıl içermez. Kullanılabilir olduğunda bir yayımlama bir yıl diğer belgeler için başvurular içerir.

 ### <a name="121-normative-references"></a>1.2.1 örnek oluşturan başvurular 

>  [MS-SSTR] Kesintisiz Akış Protokolü *v20140502* [ http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/[MS-SSTR] .pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

>   [ISO/IEC 14496 12] International Organization for Standardization, "--işitsel nesnelerin kodlama--bilgi teknolojileri bölümü 12: ISO temel medya dosyası biçimi", ISO/IEC 14496-12:2014, sürüm 4 artı Corrigendum 1 tarihli Amendments 1 ve 2.
>   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c061988_ISO_IEC_14496-12_2012.zip>

>   [ISO/IEC 14496 15] International Organization for Standardization, "bilgi teknolojisi--işitsel nesnelerin kodlama--15. Bölüm: satır başı NAL yapısal birim video ISO temel medya dosyası biçiminde", ISO 14496-15:2015, sürüm 3.
>   <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=65216>

>   [ISO/IEC 23008 2] Bilgi teknolojisi--yüksek verimlilik medya kodlama ve teslim heterojen ortamlarda--bölüm 2: yüksek verimlilik, video kodlama: 2013 veya en yeni sürümü   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c035424_ISO_IEC_23008-2_2013.zip>

>   [ISO/IEC 23001-7] Bilgi teknolojisi — MPEG sistem teknolojilerini — Bölüm 7: ISO, ortak şifreleme temel CENC Edition 2:2015 medya dosyası biçimi dosyaları <http://www.iso.org/iso/catalogue_detail.htm?csnumber=65271>

>   [6381 RFC] IETF RFC-6381 "'Codec' ve 'Profilleri' parametrelerini"demetine"medya türleri" <http://tools.ietf.org/html/rfc6381>

>   [MPEG4-RA] "MP4REG" MP4 kayıt yetkilisi [http://www.mp4ra.org   ](https://go.microsoft.com/fwlink/?LinkId=327787)

>   [RFC2119] Bradner, S., "anahtar sözcükler gösterin gereksinimi düzeylerini için RFC kullanmak için" BCP 14, RFC 2119, Mart 1997'den   [http://www.rfc-editor.org/rfc/rfc2119.txt   ](https://go.microsoft.com/fwlink/?LinkId=90317)

### <a name="122-informative-references"></a>1.2.2 bilgilendirici referanslar 

>   [MS-GLOS] Microsoft Corporation "*Windows protokolleri ana sözlüğü*."

>   [RFC3548] Josefsson, S., Ed. "Base16 Base32 ve Base64 veri Kodlamalar", RFC 3548, Temmuz 2003 [http://www.ietf.org/rfc/rfc3548.txt   ](https://go.microsoft.com/fwlink/?LinkId=90432)

>   [RFC5234] Crocker, d, Ed. ve Overell, p, "BNF sözdizimi belirtimleri için genişletilmiş: ABNF", 68, RFC 5234'ü, Ocak 2008 STD   [http://www.rfc-editor.org/rfc/rfc5234.txt   ](https://go.microsoft.com/fwlink/?LinkId=123096)


## <a name="13-overview"></a>1.3 genel bakış 
---------

>   Kesintisiz akış HEVC teslimat için gerekli belirtimi yalnızca değişiklikler aşağıda belirtilmiştir. Aynı bölüm başlıkları, başvurulan bir kesintisiz akış belirtimi [MS-SSTR] bir konumda saklamak için listelenir.

## <a name="14-relationship-to-other-protocols"></a>1.4 diğer protokollerle ilişki 
--------------------------------

## <a name="15-prerequisitespreconditions"></a>1.5 önkoşulları/önkoşulları 
----------------------------

## <a name="16-applicability-statement"></a>1.6 Uygulanabilirlik deyimi 
------------------------

## <a name="17-versioning-and-capability-negotiation"></a>1.7 Sürüm ve özellik anlaşma 
--------------------------------------

## <a name="18-vendor-extensible-fields"></a>1.8 satıcı Genişletilebilir alan 
-------------------------

>   Aşağıdaki yöntem kullanılacak HEVC video biçimini kullanarak akışları tanımlayın:

>   * **Medya biçimleri için açıklayıcı özel kodları:** bu özellik tarafından sağlanan **FourCC** bölümünde belirtilen alanının *2.2.2.5*.
>   Uygulayıcılar, uzantıları ile belirtildiği gibi MPEG4-RA uzantı kodlarını kaydederek çakışmadığından sağlayabilirsiniz [[ISO/IEC-14496-12] ](https://go.microsoft.com/fwlink/?LinkId=183695)

## <a name="19-standards-assignments"></a>1.9 standartları atamaları 
----------------------

# <a name="2-messages"></a>2 mesaj 

## <a name="21-transport"></a>2.1 taşıma 
----------

## <a name="22-message-syntax"></a>2.2 ileti söz dizimi 
---------------

### <a name="221-manifest-request"></a>2.2.1 bildirim isteği 

### <a name="222-manifest-response"></a>2.2.2 yanıt bildirimi 

#### <a name="2221-smoothstreamingmedia"></a>2.2.2.1 SmoothStreamingMedia 

>   **MinorVersion (değişken):** bildirim yanıt iletisinin alt sürümü. 2'ye ayarlamanız gerekir. (Değişiklik)

>   **Zaman Çizelgesi (değişken):** artış sayısını bir saniye içinde belirtilen süresi özniteliği zaman ölçeğini. Varsayılan değer:
>   10000000. (Değişiklik)

>   Tam süresi yakalayın ve kesirli kare hızını video (örneğin, 30/1.001 Hz) içeren parçalarını temsil eden 90000 önerilen değerdir.

#### <a name="2222-protectionelement"></a>2.2.2.2 ProtectionElement 

ProtectionElement ortak şifreleme (CENC) için video veya ses akışları uygulandığında mevcut olacaktır. Şifrelenmiş HEVC akışları, ortak şifreleme için uygun 2 sürümü [ISO/IEC 23001-7]. Yalnızca dilim verileri VCL NAL birimleri GELECEKTİR şifrelenir.

#### <a name="2223-streamelement"></a>2.2.2.3 StreamElement 

>   **StreamTimeScale (değişken):** zaman ölçeği artış sayısını bir saniye içinde belirtilen süre ve saat değerleri bu akış için. 90000 değerini HEVC akışlar için önerilir. Oluşturulan dalga biçiminin Örnek sıklığı (örneğin, 48000 veya 44100) ile eşleşen bir değer ses akışları için önerilir.

##### <a name="22231-streamprotectionelement"></a>2.2.2.3.1 StreamProtectionElement

#### <a name="2224-urlpattern"></a>2.2.2.4 UrlPattern 

#### <a name="225-trackelement"></a>2.2.5 TrackElement 

>   **FourCC (değişken):** her örnek için kullanılan hangi medya biçimini tanımlayan bir dört karakter kodu. Aşağıdaki değerleri aralığı ile aşağıdaki anlamsal anlamları ayrılmıştır:

>  * "hev1": Bu izleme için Video örnekleri [ISO/IEC-14496-15'te] belirtilen 'hev1' örnek açıklama biçimi kullanarak HEVC video kullanın.

>   **CodecPrivateData (değişken):** izde medya biçimi özgüdür ve tüm örnekleri ortak parametreleri belirtir bir veri bayt onaltılık kodlanmış bir dize olarak temsil edilir. Biçim ve anlam bayt dizisinin değişir değeriyle **FourCC** gibi alan:

>   * Bir TrackElement HEVC video açıkladığında **FourCC** alan eşit **"hev1"** ve;

>   **CodecPrivateData** alan ABNF içinde belirtilen aşağıdaki bayt sırası, onaltılık kodlanmış dize gösterimini içeren [[RFC5234]:](https://go.microsoft.com/fwlink/?LinkId=123096) (MS-SSTR hiçbir değişiklik)

>   * %x 00 %x 00 00 %x %x 01 SPSField 00 %x %x 00 00 %x %x 01 PPSField

>   * SPSField dizisi parametresi ayarlayın (SPS) içerir.

>   * PPSField dilim parametre kümesi (PPS) içerir.

>   Not: Video parametre kümesi (VP'LERİDİR) CodecPrivateData içinde yer almaz, ancak 'hvcC' kutusunda depolanan dosyaların dosya üstbilgisinde yer almalıdır. Kesintisiz Akış Protokolü kullanan sistemlerde "codec." özel özniteliğini kullanarak ek kod çözme parametreler (örneğin, katman HEVC) sinyal gerekir

##### <a name="22251-customattributeselement"></a>2.2.2.5.1 CustomAttributesElement 

#### <a name="226-streamfragmentelement"></a>2.2.6 StreamFragmentElement 

>   **SmoothStreamingMedia'nın MajorVersion** alan 2'ye ayarlanması gerekir ve **MinorVersion** alan 2'ye ayarlanması gerekir. (Değişiklik)

##### <a name="22261-trackfragmentelement"></a>2.2.2.6.1 TrackFragmentElement 

### <a name="223-fragment-request"></a>2.2.3 parça isteği 

>   **Not**: varsayılan medya biçimi için istenen **MinorVersion** 2 'hev1' ise 'iso8' marka [ISO/IEC 14496 12] ISO temel medya dosyası biçimi dördüncü sürümüdür ve [ISO/IEC 23001-7] belirtilen ISO temel medya dosyası biçimi Ortak şifreleme ikinci sürüm.

### <a name="224-fragment-response"></a>2.2.4 parça yanıt 

#### <a name="2241-moofbox"></a>2.2.4.1 MoofBox 

#### <a name="2242-mfhdbox"></a>2.2.4.2 MfhdBox 

#### <a name="2243-trafbox"></a>2.2.4.3 TrafBox 

#### <a name="2244-tfxdbox"></a>2.2.4.4 TfxdBox 

>   **TfxdBox** kullanım dışıdır ve onun işlevini parça parça kod çözme zamanı 8.8.12 [ISO/IEC 14496 12] bölümünde belirtilen kutusunu ('tfdt') tarafından değiştirildi.

>   **Not**: bir istemci izleme çalıştırın ('trun') kutusundaki örnek süreleri toplayarak bir parça süresi hesaplayabilir veya örnekleri sayısı, varsayılan örnek süresi zaman. Sonraki parça URL zaman parametresi 'tfdt' artı parça süresi baseMediaDecodeTime eşittir.

>   Bir üretici başvuru saati kutusu ('prft') bir film parçasını kutusunu ('moof') önce gerektiğinde eklenmesi gereken, parça parça kod çözme süresi olarak film parça kutusu tarafından başvurulan ilk örneğinin karşılık gelen UTC saati göstermek için [ISO/IEC 14496 içinde belirtilen -12] bölümü 8.16.5.

#### <a name="2245-tfrfbox"></a>2.2.4.5 TfrfBox 

>   **TfrfBox** kullanım dışıdır ve onun işlevini parça parça kod çözme zamanı 8.8.12 [ISO/IEC 14496 12] bölümünde belirtilen kutusunu ('tfdt') tarafından değiştirildi.

>   **Not**: bir istemci izleme çalıştırın ('trun') kutusundaki örnek süreleri toplayarak bir parça süresi hesaplayabilir veya örnekleri sayısı, varsayılan örnek süresi zaman. Sonraki parça URL zaman parametresi 'tfdt' artı parça süresi baseMediaDecodeTime eşittir. Canlı akış gecikme nedeniyle görünüm tamamlanan adresleri kullanım dışı bırakılmıştır.

#### <a name="2246-tfhdbox"></a>2.2.4.6 TfhdBox 

>   **TfhdBox** ve ilgili alanları kapsülle başına örnek meta veri parçası için varsayılan değerleri. Söz dizimi **TfhdBox** alandır parça parça üst bilgisi içinde tanımlanan kutusu söz dizimi özelliklerinin katı bir alt [[ISO/IEC-14496-12]](https://go.microsoft.com/fwlink/?LinkId=183695) 8.8.7 bölümü.

>   **BaseDataOffset (8 bayt):** başından itibaren bayt uzaklığını **MdatBox** örnek alanı alanı **MdatBox** alan. Bu kısıtlama göstermek için varsayılan-base-olup-moof bayrağı (0x020000) ayarlamanız gerekir.

#### <a name="2247-trunbox"></a>2.2.4.7 TrunBox 

>   **TrunBox** ve ilgili alanları kapsülle başına istenen parça için örnek meta verileri. Söz dizimi **TrunBox** sürüm 1 parça parça Çalıştır tanımlanan kutusuna katı bir alt kümesi [[ISO/IEC-14496-](https://go.microsoft.com/fwlink/?LinkId=183695)*12]* 8.8.8 bölümü.

>   **SampleCompositionTimeOffset (4 bayt):** her örnek, örnek oluşturma saat farkı ayarlanmış ilk sunulan örnek parçası sunu zamanını ilk kodu çözülmüş örnek kod çözme süresini eşit olmasını sağlayın. Negatif bir video örneği oluşturma uzaklıkları kullanılır,

>   sınıfında tanımlandığı gibi [[ISO/IEC-14496-12].](https://go.microsoft.com/fwlink/?LinkId=183695)

>   Not: Bu video, ses eşit en büyük kodu çözülmüş resim arabellek temizleme gecikmesi geciken kaynaklanan bir video eşitleme hatası önler ve sunu zamanlama farklı kaldırma gecikmeler olabilir alternatif parçalar arasında tutar.

>   Bu bölümde, belirtilen ABNF içinde tanımlanan alanlara söz dizimi [[RFC5234]](https://go.microsoft.com/fwlink/?LinkId=123096) aynı dışında gibi kalacaktır:

>   SampleCompositionTimeOffset SIGNED_INT32 =

#### <a name="2248-mdatbox"></a>2.2.4.8 MdatBox 

#### <a name="2249-fragment-response-common-fields"></a>2.2.4.9 yanıt ortak alanlar parçası 

### <a name="225-sparse-stream-pointer"></a>2.2.5 seyrek Stream işaretçi 

### <a name="226-fragment-not-yet-available"></a>2.2.6 henüz parçası 

### <a name="227-live-ingest"></a>2.2.7 Canlı alma 

#### <a name="2271-filetype"></a>2.2.7.1 FileType 

>   **FileType (değişken):** MPEG-4 alt türü ile bir belirtir ([MPEG4-RA)](https://go.microsoft.com/fwlink/?LinkId=327787) dosya ve üst düzey öznitelikleri.

>   **MajorBrand (değişken):** medya dosyasının ana marka. "İsml" olarak ayarlanmalıdır

>   **MinorVersion (değişken):** medya dosyasını ikincil sürümü. 1 olarak ayarlanması gerekir.

>   **CompatibleBrands (değişken):** MPEG-4'ün desteklenen markalar belirtir.
>   "Ccff" ve "iso8."

>   Bu bölümde, belirtilen ABNF içinde tanımlanan alanlara söz dizimi [[RFC5234]](https://go.microsoft.com/fwlink/?LinkId=123096) aşağıdaki gibidir:

    FileType = MajorBrand MinorVersion CompatibleBrands
    MajorBrand = STRING_UINT32
    MinorVersion = STRING_UINT32
    CompatibleBrands = "ccff" "iso8" 0\*(STRING_UINT32)

**Not**: parçaları "Ortak kapsayıcı dosya biçimi" ve ortak şifreleme [ISO/IEC 23001-7] ve ISO temel medya dosyası biçim sürümü 4 [ISO/IEC 14496 12] uygun uyumluluk markaları 'ccff' ve 'iso8' belirtin.

#### <a name="2272-streammanifestbox"></a>2.2.7.2 StreamManifestBox 

##### <a name="22721-streamsmil"></a>2.2.7.2.1 StreamSMIL 

#### <a name="2273-liveservermanifestbox"></a>2.2.7.3 LiveServerManifestBox 

##### <a name="22731-livesmil"></a>2.2.7.3.1 LiveSMIL 

#### <a name="2274-moovbox"></a>2.2.7.4 MoovBox 

#### <a name="2275-fragment"></a>2.2.7.5 parçası 

##### <a name="22751-track-fragment-extended-header"></a>2.2.7.5.1 üst bilgi genişletilmiş parça parça 

### <a name="228-server-to-server-ingest"></a>2.2.8 sunucudan sunucuya alma 

# <a name="3-protocol-details"></a>3 Protokolü ayrıntıları 


## <a name="31-client-details"></a>3.1 istemci ayrıntıları 

### <a name="311-abstract-data-model"></a>3.1.1 soyut bir veri modeli 

#### <a name="3111-presentation-description"></a>3.1.1.1 sunu açıklaması 

>   Sunu açıklaması veri öğesinin tüm meta verilerini sunumu için kapsüller.

>   Sunu meta verileri: Tüm akışları sunuda ortak meta veri kümesi. Sunu meta veri bölümünde belirtilen aşağıdaki alanları kapsar *2.2.2.1*:

>   * **MajorVersion**
>   * **MinorVersion**
>   * **Zaman Çizelgesi**
>   * **Süresi**
>   * **IsLive**
>   * **LookaheadCount**
>   * **DVRWindowLength**

>   Akış HEVC GELECEKTİR içeren sunu ayarlayın:

    MajorVersion = 2
    MinorVersion = 2

>   LookaheadCount = 0 (Not: kutuları kullanım dışı)

>   Sunu de ayarlamanız gerekir:

    TimeScale = 90000

>   Stream koleksiyonu: Belirtilen bölüm olarak Stream açıklama veri öğelerinin koleksiyonu *3.1.1.1.2*.

>   Koruma açıklaması: Belirtilen bölüm olarak koruma sistem meta veri açıklamasını veri öğelerinin koleksiyonu *3.1.1.1.1*.

##### <a name="31111-protection-system-metadata-description"></a>3.1.1.1.1 koruma sistem meta veri tanımı 

>   Koruma sistem meta veri açıklamasını veri öğesi meta verileri tek bir içerik koruma sistemi belirli saklar. (Değişiklik)

>   Koruma üstbilgi açıklaması: tek bir içerik koruma sistemi ilgili içerik koruma meta verileri. Koruma üstbilgi açıklaması bölümünde belirtilen aşağıdaki alanları kapsar *2.2.2.2*:

>   * **Systemıd**
>   * **ProtectionHeaderContent**

##### <a name="31112-stream-description"></a>3.1.1.1.2 Stream açıklaması 

###### <a name="311121-track-description"></a>3.1.1.1.2.1 İzle açıklaması 

###### <a name="3111211-custom-attribute-description"></a>3.1.1.1.2.1.1 özel öznitelik açıklaması 

##### <a name="3113-fragment-reference-description"></a>3.1.1.3 parça başvuru açıklaması 

###### <a name="31131-track-specific-fragment-reference-description"></a>3.1.1.3.1 izleme özgü parça başvuru açıklaması 

#### <a name="3112-fragment-description"></a>3.1.1.2 parça açıklaması 

##### <a name="31121-sample-description"></a>3.1.1.2.1 örnek açıklaması 

### <a name="312-timers"></a>3.1.2 zamanlayıcılar 

### <a name="313-initialization"></a>3.1.3 başlatma 

### <a name="314-higher-layer-triggered-events"></a>3.1.4 daha yüksek katmanlı tetiklenen olayları 

#### <a name="3141-open-presentation"></a>3.1.4.1 sunu açın 

#### <a name="3142-get-fragment"></a>3.1.4.2 parça Al 

#### <a name="3143-close-presentation"></a>Sunu 3.1.4.3 kapatın 

### <a name="315-processing-events-and-sequencing-rules"></a>3.1.5 olayları işleme ve sıralama kuralları 

#### <a name="3151-manifest-request-and-manifest-response"></a>3.1.5.1 bildirim isteği ve yanıt bildirimi 

#### <a name="3152-fragment-request-and-fragment-response"></a>3.1.5.2 parçalara istek ve yanıt parçası

## <a name="32-server-details"></a>3.2 sunucu ayrıntıları

## <a name="33-live-encoder-details"></a>3.3 gerçek zamanlı Kodlayıcı ayrıntıları 

# <a name="4-protocol-examples"></a>4 Protokolü örnekleri 

# <a name="5-security"></a>5 güvenlik 

## <a name="51-security-considerations-for-implementers"></a>5.1 uygulayıcılar için güvenlik konuları 
-----------------------------------------

>   Bu protokolü kullanarak taşınan içerik yüksek ticari değer varsa, içeriği yetkisiz kullanımını önlemek için bir içerik koruma sistemi kullanılmalıdır. **ProtectionElement** bir içerik koruma sisteminin kullanımıyla ilgili meta veriler yürütmek için kullanılabilir. Korumalı ses ve video içeriği şifrelenir MPEG ortak şifreleme ikinci sürüm belirtildiği gibi: 2015 [ISO/IEC 23001-7].

>   **Not**: HEVC video için yalnızca VCL NALs dilim verileri şifrelenir. Dilim üst bilgiler ve diğer NALs sunumu uygulamaları önce şifre çözme erişilebilir. güvenli bir video yolda şifrelenmiş bilgi sunumu uygulamaları için kullanılabilir değil.

# <a name="52-index-of-security-parameters"></a>5.2 güvenlik parametreleri dizini 
-----------------------------


| **Güvenlik parametresi**  | **Section**         |
|-------------------------|---------------------|
| ProtectionElement       | *2.2.2.2*           |
| Ortak şifreleme kutuları | *[ISO/IEC 23001-7]* |

# <a name="53-common-encryption-boxes"></a>5.3 ortak şifreleme kutuları
-----------------------

Ortak şifreleme uygulanır ve [ISO/IEC 23001-7] belirtilen aşağıdaki kutuları parça yanıtlarını mevcut olabilir ya da [ISO/IEC 14496 12]:

1.  Koruma sistemi özel üst bilgi kutusunu ('pssh')

2.  Örnek şifreleme kutusu ('senc')

3.  Örnek yardımcı bilgi Kaydırma kutusu ('saio')

4.  Örnek yardımcı bilgi boyut kutusu ('saiz')

5.  Örnek Grup Açıklama kutusuna ('sgpd')

6.  Örnek grup kutusu ('sbgp')

-----------------------

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
