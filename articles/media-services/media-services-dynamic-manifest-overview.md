---
title: Filtreler ve dinamik bildirimleri | Microsoft Docs
description: "Bu konuda, istemci akış belirli bölümlerine bir akış için kullanabilmeniz için filtreleri oluşturmayı açıklar. Media Services bu seçmeli akış arşivlemek için dinamik bildirimleri oluşturur."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 4034fd0aa64627c107a43208dcca766f7f44d5d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimleri
2.11 sürümünden başlayarak, Media Services, varlıklarınızı filtrelerini tanımlamanızı sağlar. Bu filtreler müşterilerinizin gibi şeyler seçmesine izin veren sunucu tarafı kurallardır: kayıttan yürütme (yerine tüm video oynatma), bir video yalnızca bir bölümünü veya yalnızca bir alt kümesini, müşterinizin aygıt (yerine varlıkla ilişkilendirilen tüm yorumlama) işleyebilir ses ve video yorumlama belirtin. Bu varlıklarınızı filtreleme aracılığıyla arşivlenmiş **dinamik bildirim**video akışını sağlamak için Müşteri'nin istek üzerine oluşturulan s tabanlı üzerinde belirtilen filtreler.

Bu konular açıklanır ortak senaryolarda filtreleri kullanarak müşteriler ve filtreleri program aracılığıyla oluşturma göstermek konulara bağlantılar için çok faydalı olacaktır (şu anda, filtreleri REST API'leri ile oluşturabilmeniz için).

## <a name="overview"></a>Genel Bakış
İçeriğinizi (Canlı olayları veya isteğe bağlı video akış) müşterilere teslim ederken hedefiniz farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli bir video teslim etmek için ' dir. Bu hedef aşağıdakileri yapın elde etmek için:

* Çoklu bit hızı akışına kodlayın ([bit hızı Uyarlamalı](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) (Bu ilgilenebilmek kalitesini ve ağ koşullarını) video akışına ve 
* Media Services'i kullanma [dinamik paketleme](media-services-dynamic-packaging-overview.md) dinamik olarak akışınızı içine farklı protokollere (Bu ilgilenebilmek farklı cihazlarda akış) yeniden paketler. Media Services teknolojileri aşağıdaki bit hızı Uyarlamalı akış teslimini destekler: HTTP canlı akışı (HLS), kesintisiz akış ve MPEG DASH. 

### <a name="manifest-files"></a>Bildirim dosyası
Bir varlığı, bit hızı Uyarlamalı akış, kodlarken bir **bildirim** (çalma listesi) dosyası oluşturulur (dosya metin veya XML tabanlı). **Bildirim** dosyası, meta verileri gibi akış içerir: izleme türü (ses, video veya metin), izleme adı, başlangıç ve bitiş zamanı, bit hızı (nitelikleri), izleme dilleri sunu penceresinde (kayan pencere sabit süresi), video codec (FourCC). Sonraki oynanabilir video parçalar kullanılabilir ve konumlarını hakkında bilgi sağlayarak sonraki parça almak için player talimatı verir. Parçaları (veya kesimler) gerçek "" bir video içeriği öbekleridir.

Burada, bildirim dosyası örneği verilmiştir: 

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

### <a name="dynamic-manifests"></a>Dinamik bildirimleri
Vardır [senaryoları](media-services-dynamic-manifest-overview.md#scenarios) varsayılan varlığın bildirim dosyasında açıklanan'den daha fazla esneklik istemciniz gerektiği zaman. Örneğin:

* Belirli cihaz: yalnızca belirtilen yorumlama ve/veya cihaz tarafından desteklenen dil parçaları belirtilen teslim kayıttan yürütme için içerik ("yorumlama filtre") kullanılır. 
* ("Alt küçük filtreleme") canlı bir olay alt küçük göstermek için bildirim azaltın.
* ("Kırpma bir video") bir video başlangıcı kesim.
* ("Ayarlama sunu pencere") player DVR penceresinde sınırlı bir süre sağlamak amacıyla sunu penceresi (DVR) ayarlayın.

Bu esneklik elde etmek için Media Services sunar **dinamik bildirimleri** dayalı üzerinde önceden tanımlanmış [filtreleri](media-services-dynamic-manifest-overview.md#filters).  Filtreleri tanımladıktan sonra istemcilerinizin belirli yorumlama veya alt klipleri videonuzu akışla aktarmak için bunları kullanabilirsiniz. Bunlar, akış URL'SİNDE filtreleri belirtmeniz gerekir. Filtreler uygulanması bit hızı Uyarlamalı akış tarafından desteklenen protokolleri [dinamik paketleme](media-services-dynamic-packaging-overview.md): HLS, MPEG DASH ve kesintisiz akış. Örneğin:

MPEG DASH URL filtreli

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Kesintisiz akış URL'si Filtresi ile

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


İçeriği ve akış URL'lerini yapı teslim etmek hakkında daha fazla bilgi için bkz: [içerik genel bakış sunan](media-services-deliver-content-overview.md).

> [!NOTE]
> Dinamik bildirimleri varlık ve bu varlık için varsayılan bildirimi değiştirmemenizi unutmayın. İstemci bir akış ile veya olmadan filtreleri istemek seçebilirsiniz. 
> 
> 

### <a id="filters"></a>Filtreleri
Varlık filtreleri iki tür vardır: 

* Genel filtrelerin (Azure Media Services hesabı herhangi bir varlığını uygulanabilir, hesap ömürlü) ve 
* Yerel filtreleri (yalnızca ile filtresi oluşturma sırasında ilişkilendirilmiş bir varlık uygulanabilir, varlık ömrü vardır). 

Genel ve yerel filtre türleri tam olarak aynı özelliklere sahip. İkisi arasındaki temel fark, bir dosya türünü hangi senaryoları için daha uygun olduğunu ' dir. Genel filtrelerin belirli bir varlık kırpma için yerel filtreleri kullanıldığı (yorumlama filtreleme) cihaz profillerinin genellikle uygundur.

## <a id="scenarios"></a>Yaygın senaryolar
İçeriğinizi (Canlı olayları veya isteğe bağlı video akış) müşterilere teslim ederken hedefiniz farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli bir video teslim etmek için önce belirtildiği gibi. Ayrıca, sizin kaybolabileceğini varlıklarınızı filtreleme ve birini kullanma ile ilgili diğer gereksinimlerin **dinamik bildirim**s. Aşağıdaki bölümlerde farklı filtreleme senaryolar kısa bir genel bakış sağlar.

* Yalnızca bir alt kümesini belirli cihazlar (yerine varlıkla ilişkilendirilen tüm yorumlama) işleyebilir ses ve video yorumlama belirtin. 
* Yalnızca bir bölümünü (yerine tüm video oynatma) bir video oynatma.
* DVR Sunu penceresini ayarlayın.

## <a name="rendition-filtering"></a>Yorumlama filtreleme
Birden çok kodlama profilleri (H.264 temel H.264 yüksek, AACL, AACH, Dolby dijital Plus) ve çoklu kalite bit, varlık kodlama tercih edebilirsiniz. Ancak, tüm istemci cihazları tüm varlığınızın profilleri ve bit destekleyecektir. Örneğin, eski Android aygıtları yalnızca destekler H.264 temel + AACL. Yüksek bit avantajları elde edilemiyor bir aygıt için gönderme, bant genişliği ve cihaz hesaplama boşa harcar. Böyle bir aygıt için görüntü yalnızca ölçeklendirmek için tüm verilen bilgileri çözmelisiniz.

Dinamik bildirim aygıtı profilleri oluşturabilirsiniz gibi mobile, konsol, HD/SD, vb. ve izler ve her profilin parçası olmasını istediğiniz nitelikleri içerir.

![Yorumlama filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir kodlayıcı mezzanine varlık yedi ISO MP4s video yorumlama (Başlangıç 180 p 1080 p) kodlayın için kullanıldı. Kodlanmış varlık dinamik olarak aşağıdaki akış protokolleri hiçbirine paketlenmesi: HLS, kesintisiz ve MPEG DASH.  Diyagram üst kısmında, hiçbir filtrelerle varlık HLS bildirimi gösterilir (tüm yedi yorumlama içerir).  Alt sol "ott" adlı bir filtre uygulandığı HLS bildirim gösterilir. Aşağıda, yanıtta atılmış alt iki kalite düzeyi ile sonuçlandı 1 MB/sn, tüm bit kaldırmak için "ott" filtresini belirtir.  Sağ alt "mobil" adlı bir filtre uygulandığı HLS bildirim gösterilir. Çözümleme olduğu iki sonuçlandı 720 p büyük yorumlama kaldırmak için "mobil" filtresini belirtir 1080 p yorumlama atılmış devre dışı.

![Yorumlama filtreleme][renditions1]

## <a name="removing-language-tracks"></a>Kaldırma dil parçaları
Varlıklarınızı İngilizce, İspanyolca, Fransızca, vb. gibi birden çok ses dilleri içerebilir. Genellikle, ses izleme seçimi Player SDK yöneticileri varsayılan ve kullanıcı seçimine göre kullanılabilir ses izler. Bu tür Player SDK'ları geliştirmek için zor, cihaza özgü player-çerçeveler arasında farklı uygulamaları gerektirir. Ayrıca, bazı platformlarda Player API'leri sınırlıdır ve burada kullanıcıları seçin veya varsayılan ses izleme değiştirmek Ses Seçimi özelliğini içermez. Varlık filtrelerle yalnızca gerekli ses dilleri dahil filtreleri oluşturarak davranışını kontrol edebilirsiniz.

![Dil parçaları filtreleme][language_filter]

## <a name="trimming-start-of-an-asset"></a>Bir varlık Kırpma Başlangıcı
Çoğu canlı akış olayda işleçleri gerçek olayın önce bazı testleri çalıştırın. Örneğin, şöyle bir Kurşun olay başlamadan önce içerebilir: "Programın kısa bir süre içinde başlayacak". Program arşivleme, test ve slate veri da arşivlenmiş ve sunuya dahil edilir. Ancak, bu bilgileri istemcilere gösterilmeyecek. Dinamik bildirim başlangıç zaman filtresi oluşturmak ve istenmeyen verileri bildirimden kaldırın.

![Kırpma Başlat][trim_filter]

## <a name="creating-sub-clips-views-from-a-live-archive"></a>Canlı arşivinden alt klipleri (görünümler) oluşturma
Birçok Canlı olayları uzun süredir çalışıp ve canlı arşiv birden çok olay içerebilir. Canlı olayından sonra sona erer yayıncıları mantıksal program başlangıç Canlı arşivine bölmeniz ve dizileri durdurmak isteyebilirsiniz. Sonra sanal bu programların, Canlı arşiv işleme ve (Alacak avantajı, mevcut önbelleğe alınmış parçacıkları CDN'ler değil) ayrı varlıklar oluşturmadığından post olmadan ayrı olarak yayınlayın. Bir futbol veya Basketbol oyun, innings Beyzbol içinde üç aylık dönem ya da bir öğleden sonra Olimpiyatlar programının olayları tek tek sanal programlara (alt klipleri) örnekleridir.

Dinamik bildirim başlangıç/bitiş zamanına kullanarak filtreler oluşturun ve canlı Arşiviniz üst sanal görünümleri oluşturun. 

![Filtre subclip][subclip_filter]

Filtrelenmiş varlık:

![Kayak][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Sunu penceresini (DVR) ayarlama
Şu anda Azure Media Services süresi yapılandırılabileceği 5 dakika arasında - 25 saat döngüsel arşiv sunar. Bildirim filtreleme medya silmeden arşiv üst üzerinde çalışırken DVR penceresini oluşturmak için kullanılabilir. Yayıncıları hangi taşır Canlı edge ile ve aynı anda daha büyük bir arşivleme penceresi tutmak sınırlı bir DVR penceresi sağlamak istediğiniz yere birçok senaryo vardır. Yayıncı klipleri vurgulamak için DVR penceresi dışında veri kullanmak isteyebilir veya he\she farklı aygıtlar için farklı DVR windows sağlamak isteyebilirsiniz. Örneğin, mobil aygıtların birçoğu (Masaüstü istemcileri için 2 dakika DVR penceresi mobil cihazlar için ve 1 saat olabilir) büyük DVR windows tanıtıcı yok.

![DVR penceresi][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (dinamik konum) ayarlama
Bildirim filtreleme birkaç saniye Canlı program Canlı kenarından kaldırmak için kullanılabilir. Bu önizleme yayın noktasında sunuyu izleyin ve görüntüleyiciler (genellikle yedeklenen-30 saniye kapalı) akış almadan önce tanıtım ekleme noktaları oluşturmak yayıncıları sağlar. Yayıncıları bu reklam kendi istemci çerçeveleri bunların zamanında alınan anında ve reklam fırsat önce bilgi işlem.

Tanıtım destek yanı sıra LiveBackoff istemcileri kayma ve canlı kenar isabet bunlar hala parçaları 404 veya 412 HTTP hataları alma yerine sunucudan böylece alabilir istemci Canlı indirme konumu ayarlamak için kullanılabilir.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Tek bir filtre içinde birden çok kural birleştirme
Tek bir filtre birden çok filtre kurallarında birleştirebilirsiniz. Örneğin, Canlı arşivinden Kurşun kaldırın ve ayrıca kullanılabilir bit filtrelemek için bir aralığı kural tanımlayabilirsiniz. Birden çok filtreleme kurallarını sonuç (yalnızca kesişim) bu kuralların bileşimdir.

![birden çok kural][multiple-rules]

## <a name="create-filters-programmatically"></a>Filtreler program aracılığıyla oluşturma
Aşağıdaki konu filtrelerle ilgili Media Services varlıklar açıklanır. Konu aynı zamanda program aracılığıyla filtreleri oluşturma gösterilmektedir.  

[REST API'leri ile filtreleri oluşturma](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Birden çok filtre (filtresi oluşturma) birleştirme
Ayrıca, tek bir URL içinde birden çok filtre birleştirebilirsiniz. 

Aşağıdaki senaryoyu neden filtrelerini birleştirmeye isteyebilirsiniz gösterir:

1. (Video nitelikleri sınırlamak için), video nitelikleri Android veya iPAD gibi mobil cihazlar için filtre gerekir. İstenmeyen nitelikleri kaldırmak için aygıt profilleri için uygun olan genel bir filtre oluşturursunuz. Yukarıda belirtildiği gibi genel filtreler tüm varlıklarınızı başka ilişkilendirme olmadan aynı media services hesabı altında için kullanılabilir. 
2. Bir varlık başlangıç ve bitiş saati kırpma istiyor. Bunun için yerel bir filtre oluşturun ve başlangıç/bitiş saati ayarlamak. 
3. Bu filtreler (olmadan birlikte kalite filtreleme filtre kullanım zor hale getirir ve kırpma filtre eklemek için gerekir) her ikisi de birleştirmek istediğiniz.

Filtreler birleştirmek için filtre adları bildirimi/çalma ayarlamanız gerekir noktalı virgülle ayrılmış URL. Adlı bir filtre sahip varsayalım *MyMobileDevice* nitelikleri filtreler ve başka adlı sahip *MyStartTime* belirli bir başlangıç saati ayarlamak için. Bunları şöyle birleştirebilirsiniz:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

En fazla 3 filtreleri birleştirebilirsiniz. 

Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a name="know-issues-and-limitations"></a>Sorunlar ve sınırlamalar
* Dinamik bildirimi çalışır GOP bu nedenle kırpma sınırları (anahtar çerçeveler) sahip GOP doğruluğu. 
* Yerel ve genel filtrelerin aynı filtre adı kullanabilirsiniz. Bu yerel filtre Not daha yüksek önceliğe sahiptir ve genel filtreleri geçersiz kılar.
* Bir filtre güncelleştirirseniz, kuralları yenilemek akış uç 2 dakika kadar sürebilir. İçerik bazı filtreleri kullanarak sunulduğu (ve proxy'ler ve CDN önbellekleri önbelleğe alınmış), bu filtreleri güncelleştiriliyor player hatalarına neden olabilir. Bu filtre güncelleştirdikten sonra önbelleğini temizlemek için önerilir. Bu seçenek yoksa, başka bir filtre kullanmayı düşünün.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[İçerik teslim müşteriler genel bakış](media-services-deliver-content-overview.md)

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
