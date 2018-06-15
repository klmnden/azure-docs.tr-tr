---
title: Azure Media Services - HEVC Protokolü (MS-SSTR) eki akış kesintisiz | Microsoft Docs
description: Bu belirtimi protokolü ve parçalanmış MP4 tabanlı Canlı HEVC Azure Media Services ile akış için biçimi açıklar. Kesintisiz Akış Protokolü documentaiton HEVC alma için destek eklemek için (MS-SSTR) ve akış için bir düzeltme budur. Bu makalede, yalnızca HEVC sunmak için gereken değişiklikler belirtilen dışında olan "(değişiklik)" yalnızca açıklama kopyalanan metni belirtir.
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
ms.openlocfilehash: 78ec0e3ee4304e820bf64afa26440380887630a1
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790513"
---
# <a name="smooth-streaming-protocol-ms-sstr-amendment-for-hevc"></a>Kesintisiz Akış Protokolü (MS-SSTR) eki HEVC için

## <a name="1-introduction"></a>1 giriş 

Bu makale, kesintisiz akış protokolü belirtimi [kesintisiz akış, kodlanmış HEVC video etkinleştirmek için MS-SSTR] uygulanacak ayrıntılı tarihli amendments sağlar. Bu belirtiminde yalnızca HEVC video codec sunmak için gereken değişiklikler verilmiştir. Makale aynı numaralandırma şeması [MS-SSTR] belirtimi olarak izler. Makaleyi sunulan boş başlıkları konumlarına [MS-SSTR] belirtiminde okuyucu yönlendirmek için sağlanır.  "(Değişiklik)" yalnızca açıklama amaçlıdır kopyalanan metni gösterir.

Makale HEVC video codec kesintisiz akış bildiriminde sinyal teknik uygulama gereksinimleri sunar ve örnek oluşturan başvurular HEVC, ortak şifreleme HEVC ve kutusu dahil geçerli MPEG standartlarını başvurmak için güncelleştirilir ISO temel Media dosya biçiminde adları, en son belirtimlere tutarlı olacak şekilde güncelleştirildi. 

Başvurulan kesintisiz akış protokolü belirtimi [MS-SSTR] canlı ve isteğe bağlı bir dijital medyayı, ses ve video gibi aşağıdaki yollardan teslim etmek için kullanılan gönderme biçimini tanımlar: bir sunucudan başka bir sunucuya ve gelen bir web sunucusuna bir kodlayıcıdan bir sunucusu için bir HTTP istemcisi.
MPEG-4 kullanımını ([[MPEG4-RA])](http://go.microsoft.com/fwlink/?LinkId=327787)-tabanlı veri yapısı teslim HTTP üzerinden yakın gerçek zamanlı sıkıştırılmış medya içeriği farklı kalite düzeyleri arasında geçiş yapma sorunsuz sağlar. İstemci bilgisayar veya cihaz için ağ ve video işleme koşulları değiştirme olsa bile HTTP istemci son kullanıcı için bir sabit oynatma deneyimi sonucudur.

## <a name="11-glossary"></a>1.1 sözlüğü 

Aşağıdaki terimler tanımlanan *[MS-GLOS]*:

>   **genel benzersiz tanıtıcısı (GUID) evrensel benzersiz tanımlayıcı (UUID)**

Bu belgede aşağıdaki terimler özeldir:

>  **oluşturma saati:** zaman bir örnek istemcide tanımlandığı şekilde sunulan [[ISO/IEC-14496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

>   **CENC**: ortak [ISO/IEC 23001-7] ikinci Edition'da tanımlanan şifreleme.

>   **kod çözme süresi:** zamanı bir örnek tanımlandığı gibi istemci üzerinde çözülecek gereklidir [[ISO/IEChttp://go.microsoft.com/fwlink/?LinkId=18369514496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

**Parça:** bağımsız olarak indirilebilir bir birimi **medya** bir veya daha fazla oluşur **örnekleri**.

>   **HEVC:** yüksek verimlilik Video kodlama, [ISO/IEC 23008-2] tanımlanan

>   **bildirim:** hakkındaki meta verileri **sunu** isteklerini yapmak için bir istemci sağlayan **medya**. **Medya:** sıkıştırılmış yürütmek için istemci tarafından kullanılan ses, video ve metin verileri bir **sunu**. **medya biçimi:** ses veya video sıkıştırılmış olarak temsil etmek için iyi tanımlanmış bir biçim **örnek**.

>   **Sunu:** tüm kümesini **akışları** ve tek bir filmi yürütmek için gereken ilgili meta veriler. **İstek:** tanımlandığı gibi istemciden sunucuya gönderilen bir HTTP iletisi [[RFC2616].](http://go.microsoft.com/fwlink/?LinkId=90372) **Yanıt:** tanımlandığı gibi sunucudan istemciye gönderilen bir HTTP iletisi [[RFC2616].](http://go.microsoft.com/fwlink/?LinkId=90372)

>   **Örnek:** (örneğin, bir çerçeve) temel en küçük birim, **medya** depolanabilir ve işlenebilir.

>   **Mayıs, SHOULD, gerekir, VERMEMELİSİNİZ, gerekir:** açıklandığı gibi bu koşullarını (tümü büyük harf) kullanılan [[RFC2119].](http://go.microsoft.com/fwlink/?LinkId=90317) İsteğe bağlı davranışı kullanım tüm deyimleri ya da olabilir, SHOULD veya olmamalıdır.

## <a name="12-references"></a>1.2 başvuruları 
-----------

>   Bağlantılar sık sık güncelleştirilen belgelerin en son sürüm olduğundan Microsoft açık belirtimleri belgelerine başvuruları yayımlama yıl dahil etmeyin. Diğer belgelere başvurular biri kullanılabilir olduğunda yayımlama bir yıl içerir.

 ### <a name="121-normative-references"></a>1.2.1 örnek oluşturan başvurular 

>  [MS-SSTR] Kesintisiz Akış Protokolü *v20140502* [ http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/[MS-SSTR] .pdf](http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

>   [ISO/IEC 14496-12] International Organization for Standardization, "--ses ve görüntü nesnelerin kodlama--bilgi teknolojileri bölümü 12: ISO temel Media dosya biçiminde", ISO/IEC 14496-12:2014, sürüm 4 artı Corrigendum 1, tarihli Amendments 1 ve 2.
>   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c061988_ISO_IEC_14496-12_2012.zip>

>   [ISO/IEC 14496-15] International Organization for Standardization, "--ses ve görüntü nesnelerin kodlama--bilgi teknolojileri bölümü 15: satır ISO temel medya dosyası biçiminde NAL yapısal birim videonun", ISO 14496-15:2015, sürüm 3.
>   <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=65216>

>   [ISO/IEC 23008-2] Bilgi teknolojisi--yüksek verimlilik kodlama ve medya teslim heterojen ortamlarda--bölüm 2: yüksek verimlilik video kodlama: 2013 veya en yeni sürümü   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c035424_ISO_IEC_23008-2_2013.zip>

>   [ISO/IEC 23001-7] Bilgi teknolojisi — MPEG sistem teknolojileri — bölümü 7: ISO ortak şifreleme temel medya dosyası biçimi dosyaları, CENC Edition 2:2015 <http://www.iso.org/iso/catalogue_detail.htm?csnumber=65271>

>   [RFC-6381] IETF RFC-6381, "'Codec bileşenleri' ve 'Profilleri' parametreleri"aralığı"medya türleriyle" <http://tools.ietf.org/html/rfc6381>

>   [MPEG4-RA] "MP4REG" MP4 kayıt yetkilisi [http://www.mp4ra.org   ](http://go.microsoft.com/fwlink/?LinkId=327787)

>   [RFC2119] Bradner, S., "anahtar sözcükler gösterin gereksinim düzeylerine RFC'leri kullanmak için" BCP 14, RFC 2119, Mart 1997   [http://www.rfc-editor.org/rfc/rfc2119.txt   ](http://go.microsoft.com/fwlink/?LinkId=90317)

### <a name="122-informative-references"></a>1.2.2 bilgilendirici başvuruları 

>   [MS-GLOS] Microsoft Corporation'ın "*Windows protokolleri ana sözlük*."

>   [RFC3548] Josefsson, S., Ed. "Base16 Base32 ve Base64 veri Kodlamalar", RFC 3548, Temmuz 2003 [http://www.ietf.org/rfc/rfc3548.txt   ](http://go.microsoft.com/fwlink/?LinkId=90432)

>   [RFC5234] Crocker, D., Ed. ve Overell, P., "BNF sözdizimi belirtimleri Engagement'ta: ABNF", STD 68, RFC 5234'ü, Ocak 2008   [http://www.rfc-editor.org/rfc/rfc5234.txt   ](http://go.microsoft.com/fwlink/?LinkId=123096)


## <a name="13-overview"></a>1.3 genel bakış 
---------

>   Yalnızca değişiklikler HEVC teslim için gerekli kesintisiz akış belirtimine aşağıda belirtilmiştir. Başvurulan bir kesintisiz akış belirtimi [MS-SSTR] konumda korumak için değişmeden bölüm üstbilgileri listelenmektedir.

## <a name="14-relationship-to-other-protocols"></a>1.4 diğer protokollerle ilişki 
--------------------------------

## <a name="15-prerequisitespreconditions"></a>1.5 Önkoşullar/önkoşulları 
----------------------------

## <a name="16-applicability-statement"></a>1.6 Uygulanabilirlik deyimi 
------------------------

## <a name="17-versioning-and-capability-negotiation"></a>1.7 sürümü oluşturma ve yetenek anlaşması 
--------------------------------------

## <a name="18-vendor-extensible-fields"></a>1.8 satıcı genişletilebilir alanları 
-------------------------

>   Aşağıdaki yöntemi için kullanılacak HEVC video biçimi kullanılarak akışları tanımlayın:

>   * **Media biçimleri için özel açıklayıcı kodları:** bu özelliği tarafından sağlanan **FourCC** bölümünde belirtildiği gibi alan *2.2.2.5*.
>   Uygulayıcılar olun uzantıları belirtildiği gibi MPEG4-RA ile uzantısı kodları kaydederek çakışmadığından [[ISO/IEC-14496-12] ](http://go.microsoft.com/fwlink/?LinkId=183695)

## <a name="19-standards-assignments"></a>1.9 standartları atamaları 
----------------------

# <a name="2-messages"></a>2 mesaj 

## <a name="21-transport"></a>2.1 taşıma 
----------

## <a name="22-message-syntax"></a>2.2 ileti sözdizimi 
---------------

### <a name="221-manifest-request"></a>2.2.1 bildirim isteği 

### <a name="222-manifest-response"></a>2.2.2 yanıt bildirimi 

#### <a name="2221-smoothstreamingmedia"></a>2.2.2.1 SmoothStreamingMedia 

>   **MinorVersion (değişken):** bildirimi yanıt iletisinin alt sürümü. 2 olarak ayarlanmış olması gerekir. (Değişiklik yok)

>   **Ölçeği (değişken):** artışlarla numarası bir saniye içinde belirtilen süre özniteliği zaman ölçeğini. Varsayılan değer:
>   10000000. (Değişiklik yok)

>   Video çerçeveleri ve kesirli kare hızı video (örneğin, 30/1.001 Hz) içeren parçaları tam süresini temsil eden 90000 önerilen değerdir.

#### <a name="2222-protectionelement"></a>2.2.2.2 ProtectionElement 

ProtectionElement ortak şifreleme (CENC) video ve ses akışları uygulandığında mevcut olacaktır. Şifrelenmiş HEVC akışları ortak şifreleme için uygun 2 sürümü [ISO/IEC 23001-7]. Yalnızca dilim verilerde VCL NAL birimleri GELECEKTİR şifrelenir.

#### <a name="2223-streamelement"></a>2.2.2.3 StreamElement 

>   **StreamTimeScale (değişken):** zaman ölçeği artırır numarası bir saniye içinde belirtilen süre ve saat değerleri bu akış. 90000 değerini HEVC akışlar için önerilir. Dalga biçiminin Örnek sıklığı (örneğin, 48000 veya 44100) eşleşen bir değeri ses akışları için önerilir.

##### <a name="22231-streamprotectionelement"></a>2.2.2.3.1 StreamProtectionElement

#### <a name="2224-urlpattern"></a>2.2.2.4 UrlPattern 

#### <a name="225-trackelement"></a>2.2.5 TrackElement 

>   **FourCC (değişken):** hangi ortam biçimi her örnek için kullanılan tanımlayan bir dört karakter kodu. Aşağıdaki değerleri aralığı aşağıdaki anlam anlamları ile ayrılmıştır:

>  * "hev1": Video örnekleri bu izleme için belirtilen [ISO/IEC-14496-15 '] 'hev1' örnek açıklama biçimi kullanarak HEVC video kullanır.

>   **CodecPrivateData (değişken):** İzle'medya biçimi özgüdür ve tüm örnekleri ortak parametreleri belirten verileri temsil bayt onaltılık kodlanmış bir dize olarak. Biçim ve bayt dizisi anlamsal anlamını değişir değeriyle **FourCC** gibi alan:

>   * Bir TrackElement HEVC video açıklar olduğunda **FourCC** alan eşit **"hev1"** ve;

>   **CodecPrivateData** alan ABNF içinde belirtilen aşağıdaki bayt sırası onaltılık kodlanmış dize gösterimini içeren [[RFC5234]:](http://go.microsoft.com/fwlink/?LinkId=123096) (MS-SSTR hiçbir değişiklik)

>   * %x 00 %x 00 %x 00 %x 01 SPSField %x 00 %x 00 %x 00 %x 01 PPSField

>   * SPSField dizisi parametresi ayarlayın (SP) içerir.

>   * PPSField dilim parametresini ayarlayın (PPS) içerir.

>   Not: Video parametresini ayarlayın (VP'LERİDİR) CodecPrivateData içinde yer almıyor, ancak 'hvcC' kutusunda depolanan dosyaların dosya üstbilgisinde yer almalıdır. Kesintisiz Akış Protokolü kullanarak sistemleri "codec." özel özniteliği kullanarak ek kod çözme parametreler (örneğin, HEVC katman) sinyal gerekir

##### <a name="22251-customattributeselement"></a>2.2.2.5.1 CustomAttributesElement 

#### <a name="226-streamfragmentelement"></a>2.2.6 StreamFragmentElement 

>   **SmoothStreamingMedia'nın MajorVersion** alan 2'ye ayarlanması gerekir ve **MinorVersion** alan 2'ye ayarlanması gerekir. (Değişiklik yok)

##### <a name="22261-trackfragmentelement"></a>2.2.2.6.1 TrackFragmentElement 

### <a name="223-fragment-request"></a>2.2.3 parça isteği 

>   **Not**: için varsayılan medya biçimi istenen **MinorVersion** 2 'hev1' ise 'iso8' marka [ISO/IEC 14496-12] ISO temel Media dosya biçimi dördüncü Edition ve [ISO/IEC 23001-7] belirtilen ISO temel medya dosyası biçimi Ortak şifreleme ikinci sürümü.

### <a name="224-fragment-response"></a>2.2.4 parça yanıt 

#### <a name="2241-moofbox"></a>2.2.4.1 MoofBox 

#### <a name="2242-mfhdbox"></a>2.2.4.2 MfhdBox 

#### <a name="2243-trafbox"></a>2.2.4.3 TrafBox 

#### <a name="2244-tfxdbox"></a>2.2.4.4 TfxdBox 

>   **TfxdBox** kullanım dışıdır ve onun işlevini parça parça kod çözme süresi 8.8.12 [ISO/IEC 14496-12] bölümünde belirtilen kutusunu ('tfdt') olarak değiştirilir.

>   **Not**: bir istemci izleme Çalıştır kutusunda ('trun') listelenen örnek süreleri toplayarak bir parça süresi hesaplayabilir veya örnek sayısını çarparak zaman varsayılan örnek süre. 'Tfdt' artı parça süresi baseMediaDecodeTime sonraki parça URL zaman parametresi eşittir.

>   Üretici Başvurusu süresi kutusunu ('prft') bir filmi parça kutusu ('moof') önce gerektiğinde eklenecek, parça parça kod çözme süresi olarak film parça kutusu tarafından başvurulan ilk örneğinin karşılık gelen UTC saati belirtmek için [ISO/IEC 14496 içinde belirtilen -12] bölümü 8.16.5.

#### <a name="2245-tfrfbox"></a>2.2.4.5 TfrfBox 

>   **TfrfBox** kullanım dışıdır ve onun işlevini parça parça kod çözme süresi 8.8.12 [ISO/IEC 14496-12] bölümünde belirtilen kutusunu ('tfdt') olarak değiştirilir.

>   **Not**: bir istemci izleme Çalıştır kutusunda ('trun') listelenen örnek süreleri toplayarak bir parça süresi hesaplayabilir veya örnek sayısını çarparak zaman varsayılan örnek süre. 'Tfdt' artı parça süresi baseMediaDecodeTime sonraki parça URL zaman parametresi eşittir. Canlı akış gecikme nedeniyle görünüm tamamlanan adresleri kullanım dışı bırakılmıştır.

#### <a name="2246-tfhdbox"></a>2.2.4.6 TfhdBox 

>   **TfhdBox** ve ilişkili alanlar için varsayılan örnek meta verilerde parça başına yalıtma. Söz dizimi **TfhdBox** alandır parça parça üstbilgi tanımlanan kutusu sözdizimi katı bir kısmı [[ISO/IEC-14496-12]](http://go.microsoft.com/fwlink/?LinkId=183695) bölümünde 8.8.7.

>   **BaseDataOffset (8 bayt):** başından itibaren bayt cinsinden uzaklık **MdatBox** örnek alanı alanı **MdatBox** alan. Bu kısıtlama göstermek için (0x020000) varsayılan-base-olduğu-moof bayrağının ayarlanması gerekir.

#### <a name="2247-trunbox"></a>2.2.4.7 TrunBox 

>   **TrunBox** ve ilgili alanları kapsüllemek başına istenen parça için örnek meta verileri. Söz dizimi **TrunBox** sürüm 1 parça parça Çalıştır tanımlanan kutusunu katı bir alt kümesidir [[ISO/IEC-14496-](http://go.microsoft.com/fwlink/?LinkId=183695)*12]* bölümünde 8.8.8.

>   **SampleCompositionTimeOffset (4 bayt):** örnek oluşturma saat farkı her örneğinin ayarlanmış parça ilk sunulan örnek sunu zamanını ilk kodu çözülmüş örnek kod çözme süresini eşit olmasını sağlayın. Negatif video örneği birleşim uzaklıkları kullanılır,

>   ' da tanımlandığı gibi [[ISO/IEC-14496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

>   Not: Bu büyük kodu çözülmüş resim arabellek kaldırma gecikme ses eşit geciken video neden bir video eşitleme hatası önler ve sunu zamanlama farklı kaldırma gecikmeler olabilir alternatif parçaları arasında tutar.

>   Bu bölümde, ABNF içinde belirtilen tanımlanan alanları söz dizimi [[RFC5234]](http://go.microsoft.com/fwlink/?LinkId=123096) aynı dışında şu şekilde kalır:

>   SampleCompositionTimeOffset SIGNED_INT32 =

#### <a name="2248-mdatbox"></a>2.2.4.8 MdatBox 

#### <a name="2249-fragment-response-common-fields"></a>2.2.4.9 yanıt ortak alanları parçalara 

### <a name="225-sparse-stream-pointer"></a>2.2.5 seyrek akış işaretçi 

### <a name="226-fragment-not-yet-available"></a>2.2.6 henüz kullanılabilir parçalara 

### <a name="227-live-ingest"></a>2.2.7 Canlı alma 

#### <a name="2271-filetype"></a>2.2.7.1 dosya türü 

>   **Dosya türü (değişken):** MPEG-4 alt ve amaçlanan kullanımını belirtir ([[MPEG4-RA])](http://go.microsoft.com/fwlink/?LinkId=327787) dosya ve üst düzey öznitelikleri.

>   **MajorBrand (değişken):** medya dosyasının ana marka. "İsml" olarak ayarlanmalıdır

>   **MinorVersion (değişken):** medya dosyasının alt sürümü. 1 olarak ayarlanması gerekir.

>   **CompatibleBrands (değişken):** MPEG-4'ün desteklenen markalar belirtir.
>   "Ccff" ve "iso8." içermelidir

>   Bu bölümde, ABNF içinde belirtilen tanımlanan alanları söz dizimi [[RFC5234]](http://go.microsoft.com/fwlink/?LinkId=123096) aşağıdaki gibidir:

    FileType = MajorBrand MinorVersion CompatibleBrands
    MajorBrand = STRING_UINT32
    MinorVersion = STRING_UINT32
    CompatibleBrands = "ccff" "iso8" 0\*(STRING_UINT32)

**Not**: parçaları "Ortak kapsayıcı dosya biçimi" ve ortak şifreleme [ISO/IEC 23001-7] ve ISO temel medya dosyası biçimi sürüm 4 [ISO/IEC 14496-12] uygun uyumluluk markalar 'ccff' ve 'iso8' gösterir.

#### <a name="2272-streammanifestbox"></a>2.2.7.2 StreamManifestBox 

##### <a name="22721-streamsmil"></a>2.2.7.2.1 StreamSMIL 

#### <a name="2273-liveservermanifestbox"></a>2.2.7.3 LiveServerManifestBox 

##### <a name="22731-livesmil"></a>2.2.7.3.1 LiveSMIL 

#### <a name="2274-moovbox"></a>2.2.7.4 MoovBox 

#### <a name="2275-fragment"></a>2.2.7.5 parça 

##### <a name="22751-track-fragment-extended-header"></a>2.2.7.5.1 üstbilgi genişletilmiş parça parça 

### <a name="228-server-to-server-ingest"></a>2.2.8 sunucudan sunucuya alma 

# <a name="3-protocol-details"></a>3 Protokolü ayrıntıları 


## <a name="31-client-details"></a>3.1 istemci ayrıntıları 

### <a name="311-abstract-data-model"></a>3.1.1 soyut veri modeli 

#### <a name="3111-presentation-description"></a>3.1.1.1 sunu açıklaması 

>   Sunu açıklaması veri öğesi tüm meta veri sunum yalıtır.

>   Sunu meta veriler: Sunuya tüm akışlar için ortak olan meta veri kümesi. Sunu meta verileri içeren bölümünde belirtilen aşağıdaki alanları *2.2.2.1*:

>   * **MajorVersion**
>   * **MinorVersion**
>   * **Zaman Çizelgesi**
>   * **Süre**
>   * **IsLive**
>   * **LookaheadCount**
>   * **DVRWindowLength**

>   Akışlar HEVC GELECEKTİR içeren sunuları ayarlayın:

    MajorVersion = 2
    MinorVersion = 2

>   LookaheadCount = 0 (Not: kutuları kullanım dışı)

>   Sunular de ayarlamanız gerekir:

    TimeScale = 90000

>   Akış koleksiyonu: Belirtilen bölüm olarak akış açıklama veri öğeleri koleksiyonu *3.1.1.1.2*.

>   Koruma Açıklama: Belirtilen bölüm olarak koruma sistem meta veri açıklamasını veri öğeleri koleksiyonu *3.1.1.1.1*.

##### <a name="31111-protection-system-metadata-description"></a>3.1.1.1.1 koruma sistem meta veri açıklaması 

>   Koruma sistem meta veri açıklamasını veri öğesi, tek bir içerik koruma sistem için özel meta verileri saklar. (Değişiklik yok)

>   Koruma üstbilgi Açıklama: tek bir içerik koruma sistem ilgili içerik koruma meta veriler. Koruma üstbilgi açıklaması oluşur bölümünde belirtilen aşağıdaki alanları *2.2.2.2*:

>   * **SistemKimliği**
>   * **ProtectionHeaderContent**

##### <a name="31112-stream-description"></a>3.1.1.1.2 akış açıklaması 

###### <a name="311121-track-description"></a>3.1.1.1.2.1 izleme açıklaması 

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

### <a name="315-processing-events-and-sequencing-rules"></a>3.1.5 olayları işleme ve kuralları sıralama 

#### <a name="3151-manifest-request-and-manifest-response"></a>3.1.5.1 istek bildirim ve bildirim yanıt 

#### <a name="3152-fragment-request-and-fragment-response"></a>3.1.5.2 parçalara istek ve yanıt parçalara

## <a name="32-server-details"></a>3.2 sunucu ayrıntıları

## <a name="33-live-encoder-details"></a>3.3 gerçek zamanlı Kodlayıcı ayrıntıları 

# <a name="4-protocol-examples"></a>4 Protokolü örnekleri 

# <a name="5-security"></a>5 güvenlik 

## <a name="51-security-considerations-for-implementers"></a>5.1 Uygulayanlar için güvenlik konuları 
-----------------------------------------

>   Bu protokolü kullanarak taşınan içerik yüksek ticari değerine sahipse, içeriği yetkisiz kullanımını önlemek için bir içerik koruma sistemi kullanılmalıdır. **ProtectionElement** içerik koruması sistem kullanımı için ilgili meta verileri taşımak için kullanılır. Korumalı ses ve video içeriği şifrelenir MPEG ortak şifreleme ikinci sürüm tarafından belirtildiği gibi: 2015 [ISO/IEC 23001-7].

>   **Not**: HEVC video için yalnızca VCL NALs dilim verileri şifrelenir. Dilim üstbilgiler ve diğer NALs şifre çözme öncesinde sunu uygulamaları erişilebilir. Güvenli video yolunda şifreli bilgiler sunu uygulamaları için kullanılabilir değil.

# <a name="52-index-of-security-parameters"></a>5.2 güvenlik parametreleri dizini 
-----------------------------


| **Güvenlik parametresi**  | **Section**         |
|-------------------------|---------------------|
| ProtectionElement       | *2.2.2.2*           |
| Ortak şifreleme kutuları | *[ISO/IEC 23001-7]* |

# <a name="53-common-encryption-boxes"></a>5.3 ortak şifreleme kutuları
-----------------------

Ortak şifreleme uygulanır ve [ISO/IEC 23001-7] belirtilen aşağıdaki kutulara parça yanıtları mevcut olabilir ya da [ISO/IEC 14496-12]:

1.  Koruma sistem belirli üstbilgi kutusu ('pssh')

2.  Örnek şifreleme kutusu ('senc')

3.  Örnek yardımcı bilgiler Kaydırma kutusu ('saio')

4.  Örnek yardımcı bilgiler boyut kutusu ('saiz')

5.  Örnek Grup Açıklama kutusunda ('sgpd')

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
