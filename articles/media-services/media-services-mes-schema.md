---
title: "Medya Kodlayıcısı standart şema | Microsoft Docs"
description: "Konu Medya Kodlayıcısı standart şema genel bir bakış sağlar."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4c060062-8ef2-41d9-834e-e81e8eafcf2e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 0d034e2c3827b297173262d294a2e566a6b45fac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="media-encoder-standard-schema"></a>Medya Kodlayıcısı standart şeması
Bu konuda bazı öğeler ve XML Şeması türleri üzerinde açıklanmaktadır [Medya Kodlayıcısı standart hazır ayarları](media-services-mes-presets-overview.md) temel alır. Konu öğeleri ve geçerli değerleri açıklamasını verir. Tam şemanın daha sonraki bir tarihte yayımlanır.  

## <a name="Preset"></a>Hazır (kök öğe)
Bir kodlama hazır tanımlar.  

### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Kodlama** |[Kodlama](media-services-mes-schema.md#Encoding) |Kök öğesi giriş kaynaklarıyla kodlanması olduğunu gösterir. |
| **Çıkışları** |[Çıkışları](media-services-mes-schema.md#Output) |İstenen çıkış dosyaları koleksiyonu. |

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Sürüm**<br/><br/> Gerekli |**xs:decimal** |Hazır sürümü. Aşağıdaki kısıtlamalar geçerlidir: xs:fractionDigits değer = "1" ve xs:minInclusive value = "1" Örneğin, **sürümü = "1.0"**. |

## <a name="Encoding"></a>Kodlama
Aşağıdaki öğeleri dizisi içerir.  

### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Video H.264 kodlamasını yönelik ayarlar. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Ses AAC kodlama ayarları. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Bmp resmini yönelik ayarlar. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Png resim yönelik ayarlar. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Jpg resim yönelik ayarlar. |

## <a name="H264Video"></a>H264Video
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs = "0" |**xs:Boolean** |Şu anda yalnızca bir geçişi kodlama desteklenir. |
| **KeyFrameInterval**<br/><br/> minOccurs = "0"<br/><br/> **Varsayılan = "00: 00:02"** |**xs: Time** |Saniye birimleri IDR çerçeveleri arasındaki sabit aralık belirler. GOP süresi olarak da bilinir. Bkz: **SceneChangeDetection** (aşağıda) Kodlayıcı bu değerden sapma olup olmadığını denetleme. |
| **SceneChangeDetection**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "false" |**xs:Boolean** |True, kodlayıcı kümesine Sahne değişikliği videoda algılamaya çalışır ve bir IDR çerçeve ekler. |
| **Karmaşıklık**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "Dengeli" |**xs: String** |Dengelemeyi denetimleri arasındaki hızı ve video kalitesini kodlayın. Aşağıdaki değerlerden biri olabilir: **hızı**, **dengeli**, veya **kalitesi**<br/><br/> Varsayılan: **dengeli** |
| **EşitlemeModu**<br/><br/> minOccurs = "0" | |Özellik gelecekteki sürümlerinde sunulur. |
| **H264Layers**<br/><br/> minOccurs = "0" |[H264Layers](media-services-mes-schema.md#H264Layers) |Çıkış video katmanları koleksiyonu. |

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: String** | Giriş video olduğunda, tek renkli bir video izlemek eklemek için Kodlayıcı zorlamak isteyebilirsiniz. Bunu yapmak için koşul kullanma "InsertBlackIfNoVideoBottomLayerOnly" = (yalnızca en düşük bit hızı bir video eklemek için) veya koşul = "InsertBlackIfNoVideo (video hiç eklemek için çıktı bit)". Daha fazla bilgi için [bu](media-services-advanced-encoding-with-mes.md#no_video) konu başlığına bakın.|

## <a name="H264Layers"></a>H264Layers

Yalnızca ses ve video, içeren Kodlayıcı girdi gönderirseniz, varsayılan olarak, çıkış varlık ses verilerle dosyaları içerir. Bazı oynatıcıları gibi çıkış akışları işlemek mümkün olmayabilir. H264Video's kullanabilirsiniz **InsertBlackIfNoVideo** özniteliği bu senaryonun çıktıda bir video izlemek eklemek için Kodlayıcı zorlamak için ayarı. Daha fazla bilgi için [bu](media-services-advanced-encoding-with-mes.md#no_video) konu başlığına bakın.
              
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs "0" maxOccurs = "unbounded" = |[H264Layer](media-services-mes-schema.md#H264Layer) |H264 katmanları koleksiyonu. |

## <a name="H264Layer"></a>H264Layer
> [!NOTE]
> Video sınırları açıklanan değerlere dayalı [H264 düzeyleri](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tablo.  
> 
> 

### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Profili**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "Auto" |**xs: String** |Aşağıdakilerden biri olabilir **xs: String** değerleri: **otomatik**, **temel**, **ana**, **yüksek**. |
| **Düzeyi**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "Auto" |**xs: String** | |
| **Bit hızı**<br/><br/> minOccurs = "0" |**xs:int** |KB/sn belirtilen bu video katmanı için kullanılan bit hızı. |
| **MaxBitrate**<br/><br/> minOccurs = "0" |**xs:int** |KB/sn belirtilen bu video katmanı için kullanılan en büyük bit hızı. |
| **BufferWindow**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "00: 00:05" |**xs: Time** |Video arabellek uzunluğu. |
| **Genişlik**<br/><br/> minOccurs = "0" |**xs:int** |Çıkış video çerçeve, piksel cinsinden genişliği.<br/><br/> Şu anda, genişlik ve yükseklik belirtmeniz gerektiğini unutmayın. Genişlik ve yükseklik çift sayı olması gerekir. |
| **Yükseklik**<br/><br/> minOccurs = "0" |**xs:int** |Çıkış video çerçeve, piksel cinsinden yüksekliği.<br/><br/> Şu anda, genişlik ve yükseklik belirtmeniz gerektiğini unutmayın. Genişlik ve yükseklik çift sayı olması gerekir.|
| **BFrames**<br/><br/> minOccurs = "0" |**xs:int** |Başvuru çerçeveler arasında B çerçeve sayısı. |
| **ReferenceFrames**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "3" |**xs:int** |Bir GOP başvuru çerçevelere sayısı. |
| **EntropyMode**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "Cabac" |**xs: String** |Aşağıdaki değerlerden biri olabilir: **Cabac** ve **Cavlc**. |
| **Kare hızı**<br/><br/> minOccurs = "0" |rasyonel sayı |Çıkış video çerçeve oranını belirler. Video giriş olarak aynı kare hızı kullanmak Kodlayıcı izin vermek için varsayılan "0/1" kullanın. Aşağıda gösterildiği gibi izin verilen değerler ortak video kare hızları olması beklenmektedir. Bununla birlikte, her geçerli rasyonel izin verilir. Örneğin 1/1 1 fps olur ve geçerli değil.<br/><br/> -12/1 (12 fps)<br/><br/> -15/1 (15 fps)<br/><br/> -24/1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> -25/1 (25 fps)<br/><br/>  -30/1 (30 fps)<br/><br/> 30000/1001 (29.97 fps) <br/> <br/>**Not** Çoklu bit hızlı kodlama, özel bir Önayar tüm katmanlar hazır sonra oluşturuyorsanız, **gerekir** kare hızı aynı değeri kullanın.|
| **AdaptiveBFrame**<br/><br/> minOccurs = "0" |**xs:Boolean** |Azure media kodlayıcıdan kopyalayın |
| **Dilimleri**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "0" |**xs:int** |Bir çerçeve bölünmüştür kaç dilimler belirler. Varsayılan kullanmanızı öneririz. |

## <a name="AACAudio"></a>AACAudio
 Aşağıdaki öğeleri ve grupları dizisini içerir.  

 AAC hakkında daha fazla bilgi için bkz: [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Profili**<br/><br/> minOccurs = "0"<br/><br/> Varsayılan = "AACLC" |**xs: String** |Aşağıdaki değerlerden biri olabilir: **AACLC**, **HEAACV1**, veya **HEAACV2**. |

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: String** |Hiçbir ses giriş sahip olduğunda, sessiz bir ses parçası içeren bir varlık oluşturmak için Kodlayıcı zorlamak için "InsertSilenceIfNoAudio" değerini belirtin.<br/><br/> Yalnızca video ve ses yok içeren Kodlayıcı girdi gönderirseniz, varsayılan olarak, ardından çıkış varlığına yalnızca video verileri içeren dosyaları içerir. Bazı oynatıcıları gibi çıkış akışları işlemek mümkün olmayabilir. Bu senaryoda çıkış sessiz ses parça eklemek için Kodlayıcı zorlamak için bu ayarı kullanın. |

### <a name="groups"></a>Gruplar
| Başvuru | Açıklama |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs = "0" |Açıklamasını görmek [AudioGroup](media-services-mes-schema.md#AudioGroup) kanalları, örnekleme hızını ve her bir profil için ayarlanabilir bit hızı uygun sayıda bilmek. |

## <a name="AudioGroup"></a>AudioGroup
Hangi her bir profil için geçerli değerler hakkında daha fazla bilgi için aşağıdaki "Ses codec Ayrıntıları" tablosuna bakın.  

### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Kanalları**<br/><br/> minOccurs = "0" |**xs:int** |Kodlanmış ses kanal sayısı. Geçerli seçenekler şunlardır: 1, 2, 5, 6, 8.<br/><br/> Varsayılan: 2. |
| **SamplingRate**<br/><br/> minOccurs = "0" |**xs:int** |Hz içinde belirtilen ses örnekleme oranını. |
| **Bit hızı**<br/><br/> minOccurs = "0" |**xs:int** |Ses kodlama belirtildiğinde kbps içinde kullanılan bit hızı. |

### <a name="audio-codec-details"></a>Ses codec ayrıntıları
Ses Codec|Ayrıntılar  
-----------------|---  
**AACLC**|1:<br/><br/> -11025: 8 &lt;bit hızı = &lt; 16<br/><br/> -12000: 8 &lt;bit hızı = &lt; 16<br/><br/> -16000: 8 &lt;bit hızı = &lt;32<br/><br/>-22050: 24 &lt;bit hızı = &lt; 32<br/><br/> -24000: 24 &lt;bit hızı = &lt; 32<br/><br/> -32000: 32 &lt;bit hızı = &lt;192 =<br/><br/> -44100: 56 &lt;bit hızı = &lt;288 =<br/><br/> -48000: 56 &lt;bit hızı = &lt;288 =<br/><br/> -88200: 128 &lt;bit hızı = &lt;288 =<br/><br/> -96000: 128 &lt;bit hızı = &lt;288 =<br/><br/> 2:<br/><br/> -11025: 16 &lt;bit hızı = &lt; 24<br/><br/> -12000: 16 &lt;bit hızı = &lt; 24<br/><br/> -16000: 16 &lt;bit hızı = &lt; 40<br/><br/> -22050: 32 &lt;bit hızı = &lt; 40<br/><br/> -24000: 32 &lt;bit hızı = &lt; 40<br/><br/> -32000: 40 &lt;bit hızı = &lt;384 =<br/><br/> -44100: 96 &lt;bit hızı = &lt;576 =<br/><br/> -48000: 96 &lt;bit hızı = &lt;576 =<br/><br/> -88200: 256 &lt;bit hızı = &lt;576 =<br/><br/> -96000: 256 &lt;bit hızı = &lt;576 =<br/><br/> 5/6:<br/><br/> -32000: 160 &lt;bit hızı = &lt;896 =<br/><br/> -44100: 240 &lt;bit hızı = &lt;1024 =<br/><br/> -48000: 240 &lt;bit hızı = &lt;1024 =<br/><br/> -88200: 640 &lt;bit hızı = &lt;1024 =<br/><br/> -96000: 640 &lt;bit hızı = &lt;1024 =<br/><br/> 8:<br/><br/> -32000: 224 &lt;bit hızı = &lt;1024 =<br/><br/> -44100: 384 &lt;bit hızı = &lt;1024 =<br/><br/> -48000: 384 &lt;bit hızı = &lt;1024 =<br/><br/> -88200: 896 &lt;bit hızı = &lt;1024 =<br/><br/> -96000: 896 &lt;bit hızı = &lt;1024 =  
**HEAACV1**|1:<br/><br/> -22050: bit hızı = 8<br/><br/> -24000: 8 &lt;bit hızı = &lt;= 10<br/><br/> -32000: 12 &lt;bit hızı = &lt;64 =<br/><br/> -44100: 20 &lt;bit hızı = &lt;64 =<br/><br/> -48000: 20 &lt;bit hızı = &lt;64 =<br/><br/> -88200: bit hızı 64 =<br/><br/> 2:<br/><br/> -32000: 16 &lt;bit hızı = &lt;= 128<br/><br/> -44100: 16 &lt;bit hızı = &lt;= 128<br/><br/> -48000: 16 &lt;bit hızı = &lt;= 128<br/><br/> -88200: 96 &lt;bit hızı = &lt;= 128<br/><br/> -96000: 96 &lt;bit hızı = &lt;= 128<br/><br/> 5/6:<br/><br/> -32000: 64 &lt;bit hızı = &lt;320 =<br/><br/> -44100: 64 &lt;bit hızı = &lt;320 =<br/><br/> -48000: 64 &lt;bit hızı = &lt;320 =<br/><br/> -88200: 256 &lt;bit hızı = &lt;320 =<br/><br/> -96000: 256 &lt;bit hızı = &lt;320 =<br/><br/> 8:<br/><br/> -32000: 96 &lt;bit hızı = &lt;= 448 oluşturun<br/><br/> -44100: 96 &lt;bit hızı = &lt;= 448 oluşturun<br/><br/> -48000: 96 &lt;bit hızı = &lt;= 448 oluşturun<br/><br/> -88200: 384 &lt;bit hızı = &lt;= 448 oluşturun<br/><br/> -96000: 384 &lt;bit hızı = &lt;= 448 oluşturun  
**HEAACV2**|2:<br/><br/> -22050: 8 &lt;bit hızı = &lt;= 10<br/><br/> -24000: 8 &lt;bit hızı = &lt;= 10<br/><br/> -32000: 12 &lt;bit hızı = &lt;64 =<br/><br/> -44100: 20 &lt;bit hızı = &lt;64 =<br/><br/> -48000: 20 &lt;bit hızı = &lt;64 =<br/><br/> -88200: 64 &lt;bit hızı = &lt;64 =  
  

## <a name="Clip"></a>Küçük
### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **StartTime** |**xs: Duration** |Sunu başlangıç saatini belirtir. StartTime değerinin mutlak zaman damgaları giriş video ile eşleşmesi gerekir. Giriş videonun ilk çerçevesi 12:00:10.000 damgasıdır varsa, örneğin, ardından StartTime en az olmalıdır 12:00:10.000 veya daha büyük. |
| **Süre** |**xs: Duration** |Sunu (örneğin, bir katmana videoda görünümünü) süresini belirtir. |

## <a name="Output"></a>Çıktı
### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Dosya adı** |**xs: String** |Çıkış dosyasının adı.<br/><br/> Aşağıdaki tabloda açıklanan makroları, çıktı dosyası adları oluşturmak için kullanabilirsiniz. Örneğin:<br/><br/> **"Çıktı": [{"Dosya adı": "{Basename}*{çözümleme}*{hızı} .mp4", "Format": {"Tür": "MP4Format"}}] ** |

### <a name="macros"></a>Makroları
| Makrosu | Açıklama |
| --- | --- |
| **{Basename}** |VoD kodlama yapıyorsanız, {Basename} giriş varlık birincil dosyasında AssetFile.Name özelliğinin ilk 32 karakterdir.<br/><br/> Giriş varlık canlı bir arşiv ise, {Basename} sunucusu bildiriminde trackName öznitelik türetilir. Olarak TopBitrate kullanarak subclip işi gönderiliyor varsa: "< VideoStream\>TopBitrate < / VideoStream\>", çıktı dosyası videoyu içeren tıklatıp {Basename} yüksek bit hızı video katmanla trackName ilk 32 karakterdir.<br/><br/> Bunun yerine tüm giriş bit gibi kullanarak subclip işi gönderiyorsanız "< VideoStream\>* < / VideoStream\>" ve çıktı dosyası video içerir ve ardından {Basename} olan karşılık gelen video katmanı trackName ilk 32 karakteri. |
| **{Codec}** |"H264" eşlenir video ve ses için "AAC". |
| **{Hızı}** |Yalnızca ses çıkış dosyası içeriyorsa, video ve ses ya da hedef ses bit hızı çıktı dosyası içeriyorsa, hedef video bit hızı. Kullanılan bit hızı Kbps değerdir. |
| **{} Kanalı** |Ses dosyası içeriyorsa, ses kanal sayısı. |
| **{Genişliği}** |Video dosyası video içeriyorsa çıktı dosyasında piksel cinsinden genişliği. |
| **{Yükseklik}** |Video dosya içeriyorsa, çıktı dosyasında piksel cinsinden görüntü yüksekliği. |
| **{} Uzantısı** |Çıktı dosyası için "Type" özelliği devralır. Çıktı dosyası adı olan bir uzantısına sahip olacaktır,: "mp4", "TH", "jpg", "png" veya "bmp". |
| **{Index}** |Küçük resim için zorunlu. Yalnızca bir kez mevcut olması. |

## <a name="Video"></a>Video (karmaşık tür devralır codec bileşeni)
### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Start** |**xs: String** | |
| **Adım** |**xs: String** | |
| **Aralık** |**xs: String** | |
| **PreserveResolutionAfterRotation** |**xs:Boolean** |Aşağıdaki bölümde ayrıntılı açıklaması için bkz: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a>PreserveResolutionAfterRotation
PreserveResolutionAfterRotation bayrağı yüzdesi terimleriyle ifade çözünürlüğü değerlerle birlikte kullanmak için önerilen (Width = "%100", yükseklik = "% 100").  

Varsayılan olarak, kodla çözümleme ayarlarında (genişlik, yükseklik) Medya Kodlayıcısı standart (MES) hazır 0 derece döndürme ile video adresindeki hedefler. Giriş videonuzun 1280 x 720 sıfır derece döndürme ile ise, örneğin, ardından varsayılan hazır çıktı aynı çözünürlüğü olduğundan emin olun. Aşağıdaki resme bakın.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Giriş video ile sıfır döndürme (ör. yakalandıktan varsa ancak, bunun anlamı Akıllı telefon veya tablet tutulan dikey olarak), varsayılan MES kodla çözümleme ayarlarını (genişlik, yükseklik) video giriş uygulayın ve dönüş dengelemek sonra. Örneğin, aşağıdaki resimde bakın. Önceden ayarlanmış genişliğini kullanır "%100", yükseklik = 100, 1280 piksel genişliğinde ve 720 piksel uzunluğunda olacak şekilde çıkış gerektiren olarak MES yorumlar = "%",. Video döndürme sonra ardından sol ve sağ pillar-box alanlarına baştaki bu pencereye sığacak şekilde resmi küçültür.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Yukarıdaki istenen davranışı değil sonra PreserveResolutionAfterRotation bayrağı kullanın ve ayarlamak yapabileceğiniz "true" (varsayılan değer "false"). Bu nedenle varsa, önceden belirlenmiş genişliği olan = "%100", yükseklik = "% 100" PreserveResolutionAfterRotation kümesine "true", 1280 piksel genişliğinde olan bir giriş video ve 720 piksel uzunluğunda ile 90 derece döndürme sıfır derece döndürme, ancak 720 piksel genişliğinde ve 1280 piksel uzunluğunda çıktısı oluşturacak. Aşağıdaki resme bakın.  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a>FormatGroup (Grup)
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a>BmpLayer
### <a name="element"></a>Öğesi
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs = "0" |**xs:int** | |
| **Yükseklik**<br/><br/> minOccurs = "0" |**xs:int** | |

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: String** | |

## <a name="PngLayer"></a>PngLayer
### <a name="element"></a>Öğesi
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs = "0" |**xs:int** | |
| **Yükseklik**<br/><br/> minOccurs = "0" |**xs:int** | |

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: String** | |

## <a name="JpgLayer"></a>JpgLayer
### <a name="element"></a>Öğesi
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Genişlik**<br/><br/> minOccurs = "0" |**xs:int** | |
| **Yükseklik**<br/><br/> minOccurs = "0" |**xs:int** | |
| **Kalite**<br/><br/> minOccurs = "0" |**xs:int** |Geçerli değerler: 1(worst)-100(best) |

### <a name="attributes"></a>Öznitelikler
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **Koşul** |**xs: String** | |

## <a name="PngLayers"></a>PngLayers
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs "0" maxOccurs = "unbounded" = |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a>BmpLayers
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs "0" maxOccurs = "unbounded" = |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a>JpgLayers
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs "0" maxOccurs = "unbounded" = |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a>BmpImage (karmaşık tür Video devralır)
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG katmanları |

## <a name="JpgImage"></a>JpgImage (karmaşık tür Video devralır)
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG katmanları |

## <a name="PngImage"></a>PngImage (karmaşık tür Video devralır)
### <a name="elements"></a>Öğeleri
| Ad | Tür | Açıklama |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = "0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG katmanları |

## <a name="examples"></a>Örnekler
Bu şemasını temel alan yerleşik olarak bulunan XML hazır örneklere bakın bkz [MES (Medya Kodlayıcısı standart) için görev hazır](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

