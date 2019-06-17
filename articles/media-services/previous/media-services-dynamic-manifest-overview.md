---
title: Filtreler ve dinamik bildirimlere | Microsoft Docs
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış arşivlemek için olan dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: cenkd;juliako
ms.openlocfilehash: 68eeb40e905d089601208d9fc181042c7b434843
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956805"
---
# <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimlere

> [!div class="op_single_selector" title1="Media Services, kullanmakta olduğunuz sürümünü seçin:"]
> * [Sürüm 2](media-services-dynamic-manifest-overview.md)
> * [Sürüm 3](../latest/filters-dynamic-manifest-overview.md)

2\.17 sürümünden başlayarak, Media Services, varlıklarınız için filtrelerini tanımlamanızı sağlar. Bu filtreler müşterileriniz gibi şeyler seçmesine izin veren sunucu tarafı kurallar şunlardır: yalnızca bir bölümünü (tüm video oynatma) yerine bir video kayıttan yürütme ya da yalnızca bir alt kümesini müşterinizin cihaz işleyebileceğini (ses ve video yorumlama belirtin yerine tüm önayarda varlıkla ilişkili olan). Bu varlıklarınızı filtreleme yoluyla elde edilir **dinamik bildirim**müşterinizin istek üzerine video akışı oluşturan s üzerinde belirtilen filtreleri temel.

Bu konuda, yaygın senaryoları tartışır filtreleri kullanarak filtreler program aracılığıyla nasıl oluşturulacağını gösteren konulara bağlantılar ve müşteriler için faydalı olacaktır.

## <a name="overview"></a>Genel Bakış
İçeriğinizi (Canlı etkinlik veya isteğe bağlı video akışı) müşterilere teslim ederken hedefiniz, farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli video teslim sağlamaktır. Bu hedef şunları yapmanız elde etmek için:

* kullanarak akışınızı Çoklu bit hızlı kodlama ([bit hızı Uyarlamalı](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) (Bu halleder kaliteyi ve ağ koşullarını) video akışına ve 
* Media Services'i kullanma [dinamik paketleme](media-services-dynamic-packaging-overview.md) dinamik olarak akışınız içine farklı protokollere (Bu halleder farklı cihazlarda akış) yeniden paketler. Media Services teslim aşağıdaki hızı Uyarlamalı akış teknolojilerini destekler: HTTP canlı akış (HLS), kesintisiz akış ve MPEG DASH. 

### <a name="manifest-files"></a>Bildirim dosyaları
Bir varlığı, bit hızı Uyarlamalı akış için kodlarken bir **bildirim** (çalma listesi) dosyası oluşturulur (metin tabanlı veya XML tabanlı dosyasıdır). **Bildirim** dosya ekleme gibi meta veri akış: izleme türü (ses, video veya metin), izleme adı, başlangıç ve bitiş zamanı, bit hızı (kalitelerini), izleme diller Sunu penceresini (sabit süre kayan pencere), video codec bileşeni ( FourCC). Sonraki yürütülebilir video parçalar kullanılabilir ve bulundukları konumlar ilgili bilgi sağlayarak bir sonraki parça almak için player talimatı verir. Parçaları (veya parçaları) gerçek "" bir video içeriğinizi öbekleridir.

Bir bildirim dosyası örneği aşağıdadır: 

    <?xml version="1.0" encoding="UTF-8"?>    
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">

    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />

    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>

    </SmoothStreamingMedia>

### <a name="dynamic-manifests"></a>Dinamik bildirimler
Vardır [senaryoları](media-services-dynamic-manifest-overview.md#scenarios) istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerektiğinde. Örneğin:

* Cihaz özel: yalnızca belirtilen yorumlama ve/veya içeriği ("işleme filtreleme") çalmak için kullanılan cihaz tarafından desteklenen belirtilen dil parçaları sunun. 
* Canlı etkinlik ("alt klip filtreleme") bir alt klip göstermek için bildirim azaltın.
* ("Kırpma video") bir video başlangıcını kesim.
* ("Ayarlama sunu pencere") player DVR penceresinde sınırlı bir süre sağlamak amacıyla sunu penceresi DVR ayarlayın.

Bu esneklik elde etmek için Media Services sunar **dinamik bildirimlerini** temel üzerinde önceden tanımlanmış [filtreleri](media-services-dynamic-manifest-overview.md#filters).  Filtreler tanımladıktan sonra istemcilerinize bunları belirli bir işleme veya alt klip videonuzun akışını sağlamak için kullanabilirsiniz. Bunlar, akış URL'SİNDE filtreleri belirtmeniz gerekir. Filtreler hızı Uyarlamalı akış protokollerine tarafından desteklenen uygulanabilir [dinamik paketleme](media-services-dynamic-packaging-overview.md): HLS, MPEG-DASH ve kesintisiz akış. Örneğin:

MPEG DASH URL filtresi

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Filtre ile kesintisiz akış URL'si

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


İçeriği ve akış URL'lerini derleme sunma konusunda daha fazla bilgi için bkz. [içerik teslimine genel bakış](media-services-deliver-content-overview.md).

> [!NOTE]
> Dinamik bildirimlerin varlık ve bu varlık için varsayılan bildirimi değiştirmemenizi unutmayın. İstemci, bir akış ile veya olmadan filtreleri istemek seçebilirsiniz. 
> 
> 

### <a id="filters"></a>Filtreleri
Varlık filtreleri iki tür vardır: 

* Genel filtrelerin (Azure Media Services hesabı olan herhangi bir varlığı uygulanabilir, hesabın bir ömre sahiptir) ve 
* Yerel filtreleri (yalnızca bir varlık ile filtre oluşturma sırasında ilişkilendirilmiş uygulanabilir, bir varlığın ömrünü vardır). 

Genel ve yerel filtre türleri tam olarak aynı özelliklere sahiptir. İkisi arasındaki temel fark, hangi senaryolar için ne tür bir olandan daha uygun olduğunu ' dir. Genel filtrelerin trim belirli bir varlık için yerel filtreleri nerede kullanılabilir cihaz profillerinin (işleme filtreleme) genellikle uygundur.

## <a id="scenarios"></a>Yaygın senaryolar
İçeriğinizi (Canlı etkinlik veya isteğe bağlı video akışı) müşterilere teslim ederken hedefiniz farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli video teslim etmek için önce olarak bahsedilen. Ayrıca, varlıklarınızı filtreleme ve birini kullanarak gerektiren diğer gereksinimleri olabilir **dinamik bildirim**s. Aşağıdaki bölümlerde, farklı filtreleme senaryolara kısa bir genel bakış sağlar.

* Yalnızca belirli cihazlar (yerine, varlıkla ilişkili olan tüm önayarda) işleyebilir ses ve video yorumlama kümesini belirtin. 
* Yalnızca bir bölümünü (tüm video oynatma) yerine bir video kayıttan yürütme.
* DVR sunu penceresi ayarlayın.

## <a name="rendition-filtering"></a>İşleme filtreleme
Varlığınız için birden çok kodlama profilleri (H.264 temel H.264 yüksek, AACL, AACH, Dolby dijital Plus) ve çoklu kalite bit hızlarında kodlamanız tercih edebilirsiniz. Ancak, tüm istemci cihazları tüm varlık profilleri ve bit hızlarına dönüştürme destekleyecektir. Örneğin, eski Android cihazları, yalnızca H.264 temel + AACL destekler. Yüksek bit hızlarına dönüştürme avantajları alınamıyor bir cihaza göndermek, bant genişliği ve cihaz hesaplama boşa harcanmasına neden olur. Böyle bir cihaz, yalnızca ölçeğini ona görüntülenmek üzere tüm belirli bilgileri çözmelisiniz.

Dinamik bildirim ile cihaz profillerini oluşturabilirsiniz gibi mobile, konsol, HD/SD vb. ve izler ve her profilinin bir parçası olmasını istediğiniz kalitelerini içerir.

![İşleme filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir kodlayıcı, yedi ISO MP4 video yorumlama (Başlangıç 180 p 1080 p) mezzanine varlık kodlayın için kullanıldı. Kodlanmış varlık dinamik olarak şu protokolden herhangi birini akış paketlenmiş: HLS, kesintisiz ve MPEG DASH.  Diyagramın üstünde filtre varlıkla HLS bildirimi gösterilir (tüm yedi önayarda içerir).  Alt sol, "ott" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Aşağıda, yanıtta çıkartılır alt iki kalite düzeyi sonuçlandı 1 MB/sn, tüm bit hızlarına dönüştürme kaldırmak için "ott" filtresini belirtir. Sağ alt "mobil" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Çözüm olduğu iki sonuçlandı 720 p büyük yorumlama kaldırmak için "mobil" filtresi belirtir 1080 p yorumlama çıkartılır devre dışı.

![İşleme filtreleme][renditions1]

## <a name="removing-language-tracks"></a>Kaldırma dil parçaları
Varlıklarınızı birden fazla ses dili İngilizce, İspanyolca, Fransızca, vb. içerebilir. Genellikle, oyuncu SDK yöneticileri varsayılan Ses Seçimi ve kullanıcı seçimine göre kullanılabilir ses izler. Bu tür Sdk'leri geliştirmek için zordur, cihaza özgü oynatıcı-çerçeveleri arasında farklı uygulamaları gerektirir. Ayrıca, bazı platformlarda oyuncu API'leri sınırlıdır ve bu kullanıcılar nerede seçin veya varsayılan ses izleme değişikliği Ses Seçimi özelliğini içermez. Varlık filtrelerle yalnızca gerekli ses dilleri dahil filtreleri oluşturarak davranışını kontrol edebilirsiniz.

![Filtreleme dili izler][language_filter]

## <a name="trimming-start-of-an-asset"></a>Bir varlığın başlangıç kırpma
Çoğu canlı akış olayları, işleçler, gerçek olayın önce bazı testler çalıştırın. Örneğin, şunun gibi bir maskeleme görüntüsü olay başlamadan önce şunlar olabilir: "Program kısa bir süre içinde başlayacak". Program arşivleme, test ve maskeleme görüntüsü veri arşivlenmiş da sunuda. Ancak, bu bilgileri istemcilere gösterilmeyecek. Dinamik bildirim başlangıç süresi filtre oluşturabilir ve bildirim istenmeyen verileri kaldırın.

![Kırpma Başlat][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Canlı bir arşivden subclips (görünümler) oluşturma
Uzun birçok Canlı etkinlikler ve canlı arşiv birden çok olayı içerebilir. Canlı olay bittikten sonra yayımcılar mantıksal program başlangıç Canlı arşive bölmeniz ve dizileri durdurmak isteyebilirsiniz. Ardından, bu sanal programlar, (Bu, var olan bir önbelleğe alınan parçaları avantajı CDN'ler almaz) Canlı arşiv işleme ve ayrı varlıklar oluşturmama post olmadan ayrı olarak yayımlayın. Bir futbol veya Basketbol oyun, innings Beyzbol, Çeyrek yıl veya spor programlarından olayları tek tek sanal programlara örnekleridir.

Dinamik bildirim filtreleri başlangıç/bitiş zamanlarına kullanarak oluşturup sanal görünümleri, Canlı arşiv üstüne oluşturabilirsiniz. 

![Alt klip filtresi][subclip_filter]

Filtrelenmiş varlık:

![Kayak][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Sunu penceresini (DVR) ayarlama
Şu anda, Azure Media Services, döngüsel arşiv süresi yapılandırılabileceği 5 dakika arasında - 25 saat sunar. Bildirim filtreleme medya silmeden arşiv üstüne üzerinde çalışırken DVR pencere oluşturmak için kullanılabilir. Yayımcılar Canlı edge ile taşıyın ve daha büyük bir arşivleme penceresi aynı anda korumak için sınırlı bir DVR penceresi sağlamak istediğiniz birçok senaryo vardır. Yayıncı klipleri vurgulamak için DVR penceresi dışında olan verileri kullanmak isteyebilirsiniz veya farklı cihazlar için farklı DVR windows sağlamak isteyebilirsiniz. Örneğin, mobil aygıtların birçoğu (mobil cihazlar için ve bir saat 2 dakikalık DVR penceresi için Masaüstü istemcilerindeki olabilir) büyük DVR windows işlemez.

![DVR penceresi][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (Canlı konum) ayarlama
Bildirim filtresi canlı bir canlı program kenarından birkaç saniye kaldırmak için kullanılabilir. Önizleme yayını noktası sunuyu izleyin ve görüntüleyiciler (desteklenen 30 saniye kapalı) akış almadan önce reklam ekleme noktaları oluşturmak yayımcılar filtrelemeye izin verir. Yayımcılar bu tanıtım için kendi istemci çerçevelerini kendisine zamanında alınan anında iletme ve reklam fırsatı önce bilgi işlem.

Tanıtım desteğine ek olarak, istemciler farklı ve canlı edge isabet bunlar yine de parçaları yerine bir HTTP 404 veya 412 hata sunucusundan alabilmeniz görüntüleyiciler konumu ayarlama konusunda LiveBackoff ayarı kullanılabilir.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Tek bir filtrede birden çok kural birleştirme
Tek bir filtrede birden çok filtreleme kurallarını birleştirebilirsiniz. Bir "aralık kuralına" tanımlayabilirsiniz örnek olarak canlı bir arşivden maskeleme görüntülerini kaldırmak ve ayrıca kullanıma kullanılabilir bit hızlarına dönüştürme filtrelemek için. Birden çok filtreleme kurallarını uygularken, sonuç tüm kuralları kesişimi ' dir.

![birden çok kural][multiple-rules]

## <a name="create-filters-programmatically"></a>Filtre program aracılığıyla oluşturma
Filtreler için ilgili Media Services varlıkları anlatılmaktadır. Makale ayrıca programlı filtreler oluşturmayı gösterir.  

[REST API'leri ile filtre oluşturma](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Birden çok filtre (filtre oluşturma) birleştirme
Ayrıca, tek bir URL içinde birden çok filtre birleştirebilirsiniz. 

Filtrelerini birleştirmeye neden isteyebileceğiniz aşağıdaki senaryoyu gösterir:

1. (Video kalitelerini sınırlamak için), video kalitelerini Android ya da iPAD gibi mobil cihazlar için filtre gerekir. İstenmeyen kalitelerini kaldırmak için uygun cihaz profillerine genel filtre oluşturursunuz. Bu makalede daha önce bahsedildiği gibi genel filtrelerin aynı media services hesabı başka ilişkisi olmayan altında tüm varlıklarınız için kullanılabilir. 
2. Ayrıca, bir varlığın başlangıç ve bitiş zamanı trim istiyorsunuz. Bunu başarmak için yerel bir filtre oluşturun ve başlangıç/bitiş saatini ayarlayın. 
3. Bu filtreler her ikisi de birleştirmek istediğiniz (birleşim olmadan, kalite filtresi kullanımını zorlaştırır kırpmayı filtre filtreleme eklemeniz gerekir).

Filtreleri birleştirerek için bildirimi/çalma listesine filtre adına ayarlamanız gerekir noktalı virgülle ayrılmış URL. Adlı bir filtre sahip *MyMobileDevice* kalitelerini filtrelere ve adlı başka bir sahip *MyStartTime* belirli bir başlangıç saati ayarlamak için. Bunları şu şekilde birleştirebilirsiniz:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

En fazla üç filtreleri de birleştirebilirsiniz. 

Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a name="know-issues-and-limitations"></a>Sorunlar ve sınırlamalar
* Dinamik bildirim GOP içinde çalışır sınırları (anahtar çerçeveler) bu nedenle kırpma GOP doğruluğu sahiptir. 
* Yerel ve genel filtrelerin aynı filtre adı kullanabilirsiniz. Yerel filtreleri, daha yüksek önceliğe sahiptir ve genel filtreleri geçersiz kılar.
* Bir filtre güncelleştirirseniz, kuralları yenilemek akış uç noktası 2 dakika kadar sürebilir. İçerik bazı filtreler kullanılarak sunulduğu (ve önbellekler proxy'leri ve CDN önbelleğe), bu filtreleri güncelleştiriliyor player hatalarına neden olabilir. Filtre güncelleştirdikten sonra önbelleği temizlemek için önerilir. Bu seçenek, mümkün değildir. farklı bir filtre kullanmayı düşünün.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[İçerik sağlama konusuna genel bakış müşteriler](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
