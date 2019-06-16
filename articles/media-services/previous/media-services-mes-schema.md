---
title: Media Encoder Standard şeması | Microsoft Docs
description: Makalede, Media Encoder Standard şeması genel bir bakış sunulmaktadır.
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
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 837235e04ce190a4481e1f19789d8e9ff9cb7578
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61131594"
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard şeması
Bu makalede bazı öğeleri ve XML Şeması türleri üzerinde açıklanmıştır [Media Encoder Standard hazır ayarları](media-services-mes-presets-overview.md) temel alır. Makaleyi açıklama öğeleri ve geçerli değerlerini sağlar.  

## <a name="Preset"></a> Hazır (kök öğesi)
Bir kodlama Önayarı tanımlar.  

### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Kodlama** |[Kodlama](media-services-mes-schema.md#Encoding) |Kök öğe giriş kaynağı kodlanacak olduğunu gösterir. |
| **Çıkışlar** |[Çıkışlar](media-services-mes-schema.md#Output) |İstenen çıkış dosyaları koleksiyonu. |
| **StretchMode**<br/>minOccurs="0"<br/>Varsayılan = "Otomatik Boyutlandır|xs:string|Denetim çıkış video çerçeve boyutu, doldurma piksel veya en boy oranını görüntüleyebilirsiniz. **StretchMode** aşağıdaki değerlerden biri olabilir: **Hiçbiri**, **AutoSize** (varsayılan) veya **AutoFit**.<br/><br/>**Hiçbiri**: Kesin olarak çıktı çözünürlüğü izleyin (örneğin, **genişliği** ve **yükseklik** hazır) piksel en boy oranını veya ekran en boy oranını giriş videosunun dikkate almadan. Senaryolarda gibi önerilen [kırpma](media-services-crop-video.md), çıkış video girişi karşılaştırıldığında farklı bir en boy oranını sahip olduğu. <br/><br/>**AutoSize**: Çıktı çözünürlüğü penceresine sığacak (genişlik * yükseklik) tarafından önceden belirtilmiş. Ancak, kodlayıcı (1:1) piksel kare en boy oranı olan bir çıkış video üretir. Bu nedenle, çıkış genişlik veya yükseklik çıkış doldurma olmadan giriş ekran en boy oranını eşleşmesi için geçersiz kılınabilir. Örneğin, giriş 1920 x 1080 ve kodlama Önayarı ister 1280 x 1280, ardından hazır yükseklik değeri geçersiz kılınmış ve çıktısı 1280 x 16:9 giriş en boy oranını barındıran 720, olacaktır. <br/><br/>**Otomatik Sığdırma**: Gerekirse, çıkış videoyla (sinemaskop veya posta kutusu) çıktı video etkin bölgeyi aynı giriş en boy oranına sahip olduğunu sağlarken istenen çıkış çözümü uymanız doldurur. Örneğin, 1920 x 1080 giriştir ve kodlama Önayarı 1280 x 1280 ister varsayalım. Video çıktısı 1280 x 1280 ardından olacaktır, ancak bir iç 1280 x 720 dikdörtgen videonun' active' en boy oranı 16:9 ve sinemaskop bölgeleri 280 piksel üst ve alt yüksek içerecektir. Başka bir örnek için giriş 1440 x 1080 ise ve kodlama Önayarı 1280 x 720, ister sonra çıkış şu 960 x 720 pillar kutusu bölgeleri 160 piksel sağ ve sol yanı sıra 4:3 en boy oranını en içteki bir dikdörtgen içeren 1280 x 720 olacaktır. 

### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Sürüm**<br/><br/> Gerekli |**xs: ondalık** |Önceden oluşturulmuş sürümü. Aşağıdaki kısıtlamalar uygulanır: xs:fractionDigits değer = "1" ve xs:minInclusive value = "1" Örneğin, **sürüm "1.0" =** . |

## <a name="Encoding"></a> Kodlama
Bir dizi aşağıdaki öğeleri içerir:  

### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |H.264 video kodlama için ayarlar. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |AAC ses kodlama ayarları. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Bmp görüntüsünü ayarları. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Png görüntüsü için ayarları. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Jpg resmi için ayarlar. |

## <a name="H264Video"></a> H264Video
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs="0" |**xs:Boolean** |Şu anda yalnızca bir geçişli kodlama desteklenir. |
| **KeyFrameInterval**<br/><br/> minOccurs="0"<br/><br/> **default="00:00:02"** |**xs:time** |Sabit bir aralık saniye birimi IDR kareler arasındaki belirler. Bir GOP süresi olarak da bilinir. Bkz: **SceneChangeDetection** Kodlayıcı bu değerden sapma olup olmadığını denetlemek için. |
| **SceneChangeDetection**<br/><br/> minOccurs="0"<br/><br/> default=”false” |**xs: Boole** |True, kodlayıcı kümesine Sahne değişikliği videoda saptamaya çalışır ve bir IDR çerçeve ekler. |
| **Karmaşıklık**<br/><br/> minOccurs="0"<br/><br/> Varsayılan = "Dengeli" |**xs:string** |Dengelemeyi denetimleri arasında hızlı ve video kalitesi kodlayın. Aşağıdaki değerlerden biri olabilir: **Hızı**, **dengeli**, veya **kalite**<br/><br/> Varsayılan: **Dengeli** |
| **EşitlemeModu**<br/><br/> minOccurs="0" | |Özelliği, gelecekteki bir sürümde sunulur. |
| **H264Layers**<br/><br/> minOccurs="0" |[H264Layers](media-services-mes-schema.md#H264Layers) |Çıkış video katmanları koleksiyonu. |

### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | Giriş video varsa, tek renkli bir video kaydı ekleme için Kodlayıcı zorlamak isteyebilirsiniz. Bunu yapmak için koşulu kullanmak = "InsertBlackIfNoVideoBottomLayerOnly" (yalnızca en düşük hızı video eklemek için) veya koşul = "(video eklemek için çıktı bit hızlarına dönüştürme) InsertBlackIfNoVideo". Daha fazla bilgi için [bu makaleye](media-services-advanced-encoding-with-mes.md#no_video) bakın.|

## <a name="H264Layers"></a> H264Layers

Yalnızca ses ve video, içeren Kodlayıcı girdi gönderirseniz, varsayılan olarak, çıktı varlığına yalnızca ses veri dosyalarını içerir. Bazı oynatıcıları gibi çıkış akışları işlemek mümkün olmayabilir. H264Video'nın kullanabileceğiniz **InsertBlackIfNoVideo** özniteliği encoder çıkış Bu senaryodaki bir video izleme eklemek için zorlama ayarı. Daha fazla bilgi için [bu makaleye](media-services-advanced-encoding-with-mes.md#no_video) bakın.
              
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[H264Layer](media-services-mes-schema.md#H264Layer) |H264 Katmanlar koleksiyonu. |

## <a name="H264Layer"></a> H264Layer
> [!NOTE]
> Video sınırları içinde açıklanan değerlere dayanır [H264 düzeyleri](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tablo.  
> 
> 

### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0"<br/><br/> default=”Auto” |**xs: dize** |Aşağıdakilerden biri olabilir **xs: dize** değerleri: **Otomatik**, **temel**, **ana**, **yüksek**. |
| **Düzey**<br/><br/> minOccurs="0"<br/><br/> default=”Auto” |**xs: dize** | |
| **Bit hızı**<br/><br/> minOccurs="0" |**xs:int** |KB/sn belirtilen bu video katmanı için kullanılan bit hızı. |
| **MaxBitrate**<br/><br/> minOccurs="0" |**xs: int** |KB/sn belirtilen bu video katmanı için kullanılan en yüksek hızı. |
| **BufferWindow**<br/><br/> minOccurs="0"<br/><br/> default="00:00:05" |**xs: saat** |Video arabelleği uzunluğu. |
| **Genişlik**<br/><br/> minOccurs="0" |**xs: int** |Piksel cinsinden çıkış video çerçevenin genişliği.<br/><br/> Şu anda, genişlik ve yükseklik belirtmeniz gerekir. Genişlik ve yükseklik çift sayı olması gerekir. |
| **Yükseklik**<br/><br/> minOccurs="0" |**xs:int** |Çerçevenin çıkış video, piksel cinsinden yüksekliği.<br/><br/> Şu anda, genişlik ve yükseklik belirtmeniz gerekir. Genişlik ve yükseklik çift sayı olması gerekir.|
| **BFrames**<br/><br/> minOccurs="0" |**xs: int** |Başvuru kareler arasındaki B çerçeve sayısı. |
| **ReferenceFrames**<br/><br/> minOccurs="0"<br/><br/> default=”3” |**xs:int** |Bir GOP çerçevelerde başvuru sayısı. |
| **EntropyMode**<br/><br/> minOccurs="0"<br/><br/> Varsayılan = "Cabac" |**xs: dize** |Aşağıdaki değerlerden biri olabilir: **Cabac** ve **Cavlc**. |
| **Kare hızı**<br/><br/> minOccurs="0" |rasyonel sayı |Çıkış videonun kare hızını belirler. Video giriş olarak aynı kare hızı kullanmak Kodlayıcı izin vermek için varsayılan "0/1" kullanın. İzin verilen değerler, ortak video kare hızları olmaları beklenir. Ancak, herhangi bir geçerli rasyonel izin verilir. Örneğin, 1/1 1 fps olacaktır ve geçerlidir.<br/><br/> -12/1 (12 fps)<br/><br/> - 15/1 (15 fps)<br/><br/> -24/1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> - 25/1 (25 fps)<br/><br/>  - 30/1 (30 fps)<br/><br/> 30000/1001 (29.97 fps) <br/> <br/>**Not** Çoklu bit hızlı kodlama, özel önayarın hazır tüm katmanlar ardından oluşturuyorsanız, **gerekir** kare hızını aynı değeri kullanın.|
| **AdaptiveBFrame**<br/><br/> minOccurs="0" |**xs: Boole** |Azure Medya Kodlayıcısı ' kopyalama |
| **Dilimler**<br/><br/> minOccurs="0"<br/><br/> default="0" |**xs:int** |Bir çerçeve bölünmüştür kaç dilimleri belirler. Varsayılan kullanılması önerilir. |

## <a name="AACAudio"></a> AACAudio
 Aşağıdaki öğeleri ve grupları dizisi içerir.  

 AAC hakkında daha fazla bilgi için bkz: [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0 "<br/><br/> default="AACLC" |**xs: dize** |Aşağıdaki değerlerden biri olabilir: **AACLC**, **HEAACV1**, veya **HEAACV2**. |

### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: dize** |Hiç ses giriş varsa, sessiz bir ses kaydı içeren bir varlık oluşturmak için Kodlayıcı zorlamak için "InsertSilenceIfNoAudio" değerini belirtin.<br/><br/> Yalnızca video ve ses yok içeren Kodlayıcı girdi gönderdiğiniz varsayılan olarak, çıktı varlık yalnızca video veriler içeren dosyalar içerir. Bazı oynatıcıları gibi çıkış akışları işlemek mümkün olmayabilir. Bu senaryodaki çıkış sessiz bir ses kaydı eklemek için Kodlayıcı zorlamak için bu ayarı kullanabilirsiniz. |

### <a name="groups"></a>Gruplar

| Başvuru | Açıklama |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs="0" |Açıklamasını görmek [AudioGroup](media-services-mes-schema.md#AudioGroup) Kanallar, örnekleme hızını ve her bir profil için ayarlanabilir bit hızı uygun sayıda bilmek. |

## <a name="AudioGroup"></a> AudioGroup
Hangi her profil için geçerli değerler hakkında daha fazla bilgi için aşağıdaki "Ses codec Ayrıntıları" Tablo bakın.  

### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Kanallar**<br/><br/> minOccurs="0" |**xs: int** |Kodlanmış ses kanal sayısı. Geçerli seçenekler şunlardır: 1, 2, 5, 6, 8.<br/><br/> Varsayılan: 2. |
| **SamplingRate**<br/><br/> minOccurs="0" |**xs: int** |Hz içinde belirtilen ses örnekleme oranı. |
| **Bit hızı**<br/><br/> minOccurs="0" |**xs: int** |Ses kodlama belirtildiğinde KB/sn kullanılan bit hızı. |

### <a name="audio-codec-details"></a>Ses kodek bileşeni ayrıntıları

Ses kodek bileşeni|Ayrıntılar  
-----------------|---  
**AACLC** |1:<br/><br/> - 11025: 8 &lt;hızı = &lt; 16<br/><br/> - 12000: 8 &lt;hızı = &lt; 16<br/><br/> - 16000: 8 &lt;hızı = &lt;32<br/><br/>- 22050: 24 &lt;hızı = &lt; 32<br/><br/> - 24000: 24 &lt;hızı = &lt; 32<br/><br/> - 32000: 32 &lt;hızı = &lt;192 =<br/><br/> - 44100: 56 &lt;hızı = &lt;288 =<br/><br/> - 48000: 56 &lt;hızı = &lt;288 =<br/><br/> - 88200 : 128 &lt;hızı = &lt;288 =<br/><br/> - 96000 : 128 &lt;hızı = &lt;288 =<br/><br/> 2:<br/><br/> - 11025: 16 &lt;hızı = &lt; 24<br/><br/> - 12000: 16 &lt;hızı = &lt; 24<br/><br/> - 16000: 16 &lt;hızı = &lt; 40<br/><br/> - 22050: 32 &lt;hızı = &lt; 40<br/><br/> - 24000 : 32 &lt;hızı = &lt; 40<br/><br/> - 32000:  40 &lt;hızı = &lt;384 =<br/><br/> - 44100: 96 &lt;hızı = &lt;576 =<br/><br/> - 48000 : 96 &lt;hızı = &lt;576 =<br/><br/> - 88200: 256 &lt;hızı = &lt;576 =<br/><br/> - 96000: 256 &lt;hızı = &lt;576 =<br/><br/> 5/6:<br/><br/> - 32000: 160 &lt;hızı = &lt;896 =<br/><br/> - 44100: 240 &lt;hızı = &lt;1024 =<br/><br/> - 48000: 240 &lt;hızı = &lt;1024 =<br/><br/> - 88200: 640 &lt;hızı = &lt;1024 =<br/><br/> - 96000: 640 &lt;hızı = &lt;1024 =<br/><br/> 8:<br/><br/> - 32000 : 224 &lt;hızı = &lt;1024 =<br/><br/> - 44100 : 384 &lt;hızı = &lt;1024 =<br/><br/> - 48000: 384 &lt;hızı = &lt;1024 =<br/><br/> - 88200: 896 &lt;hızı = &lt;1024 =<br/><br/> - 96000: 896 &lt;hızı = &lt;1024 =  
**HEAACV1** |1:<br/><br/> -22050: bit hızı = 8<br/><br/> - 24000: 8 &lt;hızı = &lt;10 =<br/><br/> - 32000: 12 &lt;hızı = &lt;64 =<br/><br/> - 44100: 20 &lt;hızı = &lt;64 =<br/><br/> - 48000: 20 &lt;hızı = &lt;64 =<br/><br/> -88200: bit hızı 64 =<br/><br/> 2:<br/><br/> - 32000: 16 &lt;hızı = &lt;128 =<br/><br/> - 44100: 16 &lt;hızı = &lt;128 =<br/><br/> - 48000: 16 &lt;hızı = &lt;128 =<br/><br/> - 88200 : 96 &lt;hızı = &lt;128 =<br/><br/> - 96000: 96 &lt;hızı = &lt;128 =<br/><br/> 5/6:<br/><br/> - 32000 : 64 &lt;hızı = &lt;320 =<br/><br/> - 44100: 64 &lt;hızı = &lt;320 =<br/><br/> - 48000: 64 &lt;hızı = &lt;320 =<br/><br/> - 88200 : 256 &lt;hızı = &lt;320 =<br/><br/> - 96000: 256 &lt;hızı = &lt;320 =<br/><br/> 8:<br/><br/> - 32000: 96 &lt;hızı = &lt;448 =<br/><br/> - 44100: 96 &lt;hızı = &lt;448 =<br/><br/> - 48000: 96 &lt;hızı = &lt;448 =<br/><br/> - 88200: 384 &lt;hızı = &lt;448 =<br/><br/> - 96000: 384 &lt;hızı = &lt;448 =  
**HEAACV2** |2:<br/><br/> - 22050: 8 &lt;hızı = &lt;10 =<br/><br/> - 24000: 8 &lt;hızı = &lt;10 =<br/><br/> - 32000: 12 &lt;hızı = &lt;64 =<br/><br/> - 44100: 20 &lt;hızı = &lt;64 =<br/><br/> - 48000: 20 &lt;hızı = &lt;64 =<br/><br/> - 88200: 64 &lt;hızı = &lt;64 =  
  
## <a name="Clip"></a> Küçük resim
### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **startTime** |**xs:duration** |Sunu başlangıç saatini belirtir. StartTime değerinin giriş videosunun mutlak zaman damgalarının eşleşmesi gerekir. Giriş videosunun ilk karesine 12:00:10.000 bir zaman damgası varsa, örneğin, ardından StartTime en az olmalıdır 12:00:10.000 veya büyük. |
| **Süresi** |**xs:duration** |(Örneğin, bir video kaplama görünümünü) sunu süresini belirtir. |

## <a name="Output"></a> Çıkış
### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Dosya adı** |**xs:string** |Çıkış dosyasının adı.<br/><br/> Çıkış dosyası adı oluşturmak için aşağıdaki tabloda açıklanan makroları kullanabilirsiniz. Örneğin:<br/><br/> **"Çıktı": [{"Dosya adı": "{Basename} *{çözümleme}* {hızı} .mp4", "Format": {"Type": "MP4Format"}}]** |

### <a name="macros"></a>Makroları

| Makrosu | Açıklama |
| --- | --- |
| **{Basename}** |VoD kodlama yapıyorsanız {Basename} giriş varlığı birincil dosyanın AssetFile.Name özelliğin ilk 32 karakterdir.<br/><br/> Canlı arşiv giriş varlığı ise {Basename} trackName öznitelik sunucusu bildiriminde türetilir. TopBitrate olarak kullanarak bir alt klip iş gönderme,: "< VideoStream\>TopBitrate < / VideoStream\>" ve video çıkış dosyasını içeren ve ardından {Basename} video katmanın trackName ilk 32 karakterdir en yüksek hızı ile.<br/><br/> Bunun yerine tüm giriş bit hızlarında gibi kullanarak bir alt klip iş gönderiyorsanız "< VideoStream\>* < / VideoStream\>" ve video çıkış dosyasını içeren ve ardından {Basename} olduğundan trackName ilk 32 karakterleri ilgili video katmanı. |
| **{Codec}** |"H264" eşlenir video ve ses "AAC". |
| **{Hızı}** |Yalnızca ses çıkış dosyası içeriyorsa, video ve ses veya hedef ses hızına sahip çıkış dosyası içeriyorsa, hedef video hızı. Kullanılan bit hızını Kbps değerdir. |
| **{} Kanalı** |Ses dosyası içeriyorsa, ses kanal sayısı. |
| **{Width}** |Videonun video dosyası içeriyorsa, çıkış dosyasında piksel cinsinden genişliği. |
| **{Height}** |Video dosyası içeriyorsa, çıkış dosyasında piksel cinsinden görüntü yüksekliği. |
| **{Extension}** |Çıkış dosyası için "Type" özelliği devralır. Çıkış dosyası adı olan bir uzantıya sahip biri: "mp4", "TH", "jpg", "png" veya "bmp". |
| **{Index}** |Küçük resim için zorunlu. Yalnızca bir kez mevcut olmalıdır. |

## <a name="Video"></a> Video (karmaşık tür codec bileşeni devralır)
### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Start** |**xs:string** | |
| **Adım** |**xs:string** | |
| **Aralığı** |**xs:string** | |
| **PreserveResolutionAfterRotation** |**xs:Boolean** |Ayrıntılı açıklaması için aşağıdaki bölüme bakın: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a> PreserveResolutionAfterRotation
Kullanmak için önerilen **PreserveResolutionAfterRotation** yüzde cinsinden çözümleme değerlerle birlikte bayrağı (genişlik "100" Height = % = "%100").  

Varsayılan olarak 0 derece döndürme videolarda, kodla çözümleme ayarlarında (genişlik, yükseklik) medya Kodlayıcı standart (MES) hazır hedeflenir. Girdi videonuzun 1280 x 720 sıfır derece döndürme ile ise, örneğin, ardından varsayılan hazır çıkış aynı çözüm olduğundan emin olun.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Sıfır olmayan döndürme (örneğin, bir akıllı telefonundaki veya tabletindeki dikey olarak tutulan) ile giriş videosunun yakalandıktan, ardından MES varsayılan olarak giriş videosu kodla çözümleme ayarları (genişlik, yükseklik) uygular ve sonra döndürme için telafi. Örneğin, aşağıdaki resimde bakın. Hazır genişliğini kullanır "100" Height = % 100 hangi MES çıktısı 1280 piksel genişliğinde ve 720 piksel yüksekliğinde olmasını gerektiren olarak yorumlar = "%",. Görüntü döndürme sonra ardından sol ve sağda pillar-box alanlarına baştaki bu pencereye sığdırmak için resim küçültür.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Alternatif olarak, yapabileceğiniz kullanım **PreserveResolutionAfterRotation** bayrak ve değerini "true" (varsayılan değer "false"). Genişlik, önceden ayarlanmış varsa, bu nedenle = "%100", Height = "%100" ve ", bir giriş videosunun true" olarak PreserveResolutionAfterRotation 1280 piksel genişliğinde ve 720 piksel yüksekliğinde ile 90 derece döndürme olduğu sıfır derece döndürme, ancak 720 piksel genişliğinde ve 1280 içeren bir çıktı üretir piksel uzunluğa ayarlayın. Aşağıdaki resme bakın:  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a> FormatGroup (grubu)
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a> BmpLayer
### <a name="element"></a>Öğe

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Yükseklik**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | |

## <a name="PngLayer"></a> PngLayer
### <a name="element"></a>Öğe

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Yükseklik**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | |

## <a name="JpgLayer"></a> JpgLayer
### <a name="element"></a>Öğe

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Yükseklik**<br/><br/> minOccurs="0" |**xs:int** | |
| **Kalite**<br/><br/> minOccurs="0" |**xs:int** |Geçerli değerler: 1(worst)-100(Best) |

### <a name="attributes"></a>Öznitelikler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs:string** | |

## <a name="PngLayers"></a> PngLayers
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a> BmpLayers
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a> JpgLayers
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a> BmpImage (karmaşık tür videodan devralır)
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG katmanları |

## <a name="JpgImage"></a> JpgImage (karmaşık tür videodan devralır)
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG katmanları |

## <a name="PngImage"></a> PngImage (karmaşık tür videodan devralır)
### <a name="elements"></a>Öğeler

| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG katmanları |

## <a name="examples"></a>Örnekler
Bu şemaya göre oluşturulan XML hazır örneklere bakın bkz [(Media Encoder Standard) MES görev ön](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

