---
title: Filtreler ve Azure Media Services olan dinamik bildirimler | Microsoft Docs
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış elde etmek için olan dinamik bildirimler oluşturur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/11/2019
ms.author: juliako
ms.openlocfilehash: ee0200f7c007b437a27b8e9d0f36becc13b8f611
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835811"
---
# <a name="pre-filtering-manifests-by-using-dynamic-packager"></a>Dinamik Packager'ı kullanarak bildirimleri önceden filtreleme

Bit hızı Uyarlamalı akış cihazlara içerik teslim etme, genellikle hedef belirli cihaz işlevleri veya kullanılabilir ağ bant genişliği için bir bildirim birden fazla sürümünü yayımlama gerekir. [Dinamik Paketleyici](dynamic-packaging-overview.md) belirli codec filtreleyebilirsiniz filtreleri belirtmenize olanak tanır, çözünürlük ve bit hızlarına dönüştürme ses birleşimleri üzerinde birden fazla kopya oluşturmak için gereken kaldırma halindeyken izleme. Belirli bir hedef cihazlarınızı (iOS, Android, rekabeti veya tarayıcılar) ve ağ özellikleri (yüksek bant genişlikli, mobil veya düşük bant genişliğine sahip senaryolar) için yapılandırılmış filtre kümesi ile yeni bir URL Yayımlama yeterlidir. Bu durumda, istemcilerin içeriğinizi sorgu dizesi aracılığıyla akış işleyebilir (kullanılabilir belirterek [varlık filtreler veya hesap filtreleri](filters-concept.md)) ve akış belirli bölümlerini bir akış için filtreleri kullanın.

Bazı dağıtım senaryoları, bir müşteri belirli parçaları erişemiyor olduğundan emin olun gerektirir. Örneğin, belirli abone katmanına HD parçalar içeren bir bildirim yayımlama istemeyebilirsiniz. Veya ek yolundan yararlı değildir belirli cihazlara teslim maliyetini azaltmak için belirli bit hızı Uyarlamalı (ABR) parçaları kaldırmak isteyebilirsiniz. Bu durumda, önceden oluşturulmuş filtrelerle listesini ilişkilendirebiliyordunuz, [akış Bulucu](streaming-locators-concept.md) oluşturma. Bu durumda, istemcilerin içeriği nasıl sağlanacağına işlemek olamaz, tarafından tanımlanan **akış Bulucu**.

Belirtme aracılığıyla filtreleme birleştirebilirsiniz [akış Bulucu filtreleriyle](filters-concept.md#associating-filters-with-streaming-locator) + istemcinizin URL'SİNDE belirten ek cihaz belirli filtreler. Bu meta veriler ya da olay akışları, ses diller veya açıklayıcı ses parçaları gibi ek parçaları kısıtlamak yararlı olabilir. 

Akışınızı üzerinde farklı bir filtre belirtmek için bu özelliği sağlayan güçlü **dinamik bildirim** birden fazla kullanım örneği senaryoları için hedef cihazları hedeflemek için işleme çözümü. Bu konu ile ilgili kavramları açıklar **dinamik bildirimlerini** istediğiniz bu özelliği kullanmak senaryo örnekleri sağlar.

> [!NOTE]
> Olan dinamik bildirimler, varlık ve bu varlık için varsayılan bildirimi değiştirmeyin. 
> 

##  <a name="overview-of-manifests"></a>Bildirimleri genel bakış

Azure Media Services, HLS, MPEG DASH ve kesintisiz akış protokollerini destekler. Bir parçası olarak [dinamik paketleme](dynamic-packaging-overview.md), akış istemci bildirimlerinin (HLS ana çalma listesi, DASH medya sunu açıklaması [MPD] ve kesintisiz akış), URL biçimi seçicide göre dinamik olarak oluşturulur. İçinde teslim protokollerine bkz [ortak isteğe bağlı iş akışı](dynamic-packaging-overview.md#delivery-protocols). 

### <a name="get-and-examine-manifest-files"></a>Almak ve bildirim dosyalarını inceleyin

Filtre izleme özelliği koşulları tabanlı (dinamik veya [VOD] isteğe bağlı video), akışın hangi parçaları dinamik olarak oluşturulmuş bir bildirimde eklenmelidir üzerinde listesini belirtin. Almak ve parçaları özelliklerini incelemek için kesintisiz akış bildirimi ilk yüklemek zorunda.

[Karşıya yükleme, kodlama ve akışını dosyaları .NET ile](stream-files-tutorial-with-api.md#get-streaming-urls) Öğreticisi, .NET ile akış URL'leri oluşturmak nasıl gösterir. Uygulamayı çalıştırırsanız, URL'lerden birini kesintisiz akış bildirimine işaret eden: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/> Kopyalayın ve bir tarayıcının adres çubuğuna URL'yi yapıştırın. Dosya indirilir. Tercih ettiğiniz metin düzenleyicisinde açabilirsiniz.

REST örnek için bkz: [karşıya yükleme, kodlama ve akışını REST dosyalarıyla](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>İzleyici bir video Akış hızı

Kullanabileceğiniz [Azure Media Player tanıtımını sayfası](http://aka.ms/azuremediaplayer) video akışının hızı izlemek için. Tanılama bilgileri tanıtım sayfasını görüntüler **tanılama** sekmesinde:

![Azure Media Player tanılama][amp_diagnostics]
 
### <a name="examples-urls-with-filters-in-query-string"></a>Örnekler: Sorgu dizesi'nde filtrelerle URL'leri

Akış protokollerine bir ABR için filtreler uygulayabilirsiniz: HLS, MPEG-DASH ve kesintisiz akış. Aşağıdaki tabloda, filtrelerle URL'leri bazı örnekler gösterilmektedir:

|Protocol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="rendition-filtering"></a>İşleme filtreleme

Varlığınız için birden çok kodlama profilleri (H.264 temel H.264 yüksek, AACL, AACH, Dolby dijital Plus) ve çoklu kalite bit hızlarında kodlamanız seçebilirsiniz. Ancak, tüm istemci cihazları tüm varlık profilleri ve bit hızlarına dönüştürme destekleyecektir. Örneğin, yalnızca H.264 temel + AACL eski Android cihazları destekler. Yüksek bit hızlarına dönüştürme avantajları alamayan bir cihaza göndermek, bant genişliği ve cihaz hesaplama boşa harcanmasına neden olur. Böyle bir cihaz, yalnızca ölçeğini ona görüntülenmek üzere tüm belirli bilgileri çözmelisiniz.

Dinamik bildirim ile cihaz profillerini oluşturabilirsiniz (mobile gibi konsol veya HD/SD) ve izleyen ve her profilinin bir parçası olmasını istediğiniz kalitelerini içerir. Bu işleme filtreleme çağrılır. Aşağıdaki diyagramda bir örneği gösterilmektedir.

![İşleme filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir kodlayıcı, yedi ISO MP4 video yorumlama (Başlangıç 180 p 1080 p) mezzanine varlık kodlayın için kullanıldı. Kodlanmış varlık olabilir [dinamik olarak paketlenmiş](dynamic-packaging-overview.md) aşağıdaki akış protokollerine hiçbirine: HLS, MPEG DASH ve kesintisiz. 

Aşağıdaki diyagramda üstüne HLS için bir varlık ile filtre bildirimi gösterir. (Tüm yedi yorumlama içeriyor.)  Alt sol, HLS bildiriminde "ott" adlı bir filtre uygulandığı diyagramda gösterilmektedir. Alt iki kalite düzeylerine yanıtta çıkarılır "ott" filtre 1 MB/sn, aşağıda tüm bit hızlarına dönüştürme kaldırılmasını belirtir. Sağ alt köşesinde "mobil" adlı bir filtre uygulandığı HLS bildirimi diyagramda gösterilmektedir. "Mobil" filtre iki 1080 p yorumlama çıkarılır şekilde çözünürlük 720 p'den daha büyük olduğu önayarda kaldırılmasını belirtir.

![İşleme filtreleme][renditions1]

## <a name="removing-language-tracks"></a>Kaldırma dil parçaları
Varlıklarınızı birden fazla ses dili İngilizce, İspanyolca, Fransızca, vb. içerebilir. Genellikle, oyuncu SDK yöneticileri varsayılan Ses Seçimi ve kullanıcı seçimine göre kullanılabilir ses izler.

Farklı uygulamalar arasında cihaza özgü oynatıcı çerçeveleri gerektirdiği için bu tür Sdk'leri geliştirme, güç olabilir. Ayrıca, bazı platformlarda oyuncu API'leri sınırlıdır ve bu kullanıcılar nerede seçin veya varsayılan ses izleme değişikliği Ses Seçimi özelliğini içermez. Varlık filtrelerle yalnızca gerekli ses dilleri dahil filtreleri oluşturarak davranışını kontrol edebilirsiniz.

![Dil aşamalardan filtreleme][language_filter]

## <a name="trimming-the-start-of-an-asset"></a>Bir varlık başlangıcını kırpma

Çoğu canlı akış olayları, işleçler, gerçek olayın önce bazı testler çalıştırın. Örneğin, şunun gibi bir maskeleme görüntüsü olay başlamadan önce şunlar olabilir: "Program kısa bir süre içinde başlayacak." 

Program arşivleme, test ve maskeleme görüntüsü veri arşivlenmiş da sunuda. Ancak, bu bilgileri istemcilere gösterilmeyecek. Dinamik bildirim başlangıç süresi filtre oluşturabilir ve bildirim istenmeyen verileri kaldırın.

![Kırpma Başlat][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Canlı bir arşivden subclips (görünümler) oluşturma

Uzun birçok Canlı etkinlikler ve canlı arşiv birden çok olayı içerebilir. Canlı olay bittikten sonra yayımcılar mantıksal program başlangıç Canlı arşive bölmeniz ve dizileri durdurmak isteyebilirsiniz. 

(Bu, var olan bir önbelleğe alınan parçaları avantajı CDN'ler almaz) Canlı arşiv işleme ve ayrı varlıklar oluşturmama post olmadan sanal bu programlar ayrı olarak yayımlayabilirsiniz. Bir futbol veya Basketbol oyun, innings Beyzbol, Çeyrek yıl veya spor programlarından olayları tek tek sanal programlara örnekleridir.

Dinamik bildirim filtreleri başlangıç/bitiş zamanlarına kullanarak oluşturabilir ve, Canlı arşiv üstüne sanal görünümler oluşturun. 

![Alt klip filtresi][subclip_filter]

Filtrelenmiş varlık şu şekildedir:

![Kayak][skiing]

## <a name="adjusting-the-presentation-window-dvr"></a>Sunu penceresinin (DVR) ayarlama

Şu anda, Azure Media Services, 5 dakika ile 25 Saat süre yapılandırılabileceği döngüsel arşiv sunar. Bildirim filtreleme medya silmeden arşiv üstüne üzerinde çalışırken DVR pencere oluşturmak için kullanılabilir. Yayımcılar Canlı edge ile taşıyın ve daha büyük bir arşivleme penceresi aynı anda korumak için sınırlı bir DVR penceresi sağlamak istediğiniz birçok senaryo vardır. Yayıncı klipleri vurgulamak için DVR penceresi dışında olan verileri kullanmak isteyebilirsiniz veya farklı cihazlar için farklı DVR windows sağlamak isteyebilirsiniz. Örneğin, mobil aygıtların birçoğu (mobil cihazlar için ve 1 saat 2 dakikalık DVR penceresi için Masaüstü istemcilerindeki olabilir) büyük DVR windows işlemez.

![DVR penceresi][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (Canlı konum) ayarlama

Bildirim filtresi canlı bir canlı program kenarından birkaç saniye kaldırmak için kullanılabilir. Önizleme yayını noktası sunuyu izleyin ve görüntüleyiciler (30 saniye tarafından desteklenen) akış almadan önce reklam ekleme noktaları oluşturmak yayımcılar filtrelemeye izin verir. Yayımcılar sonra Bu tanıtımları kendi istemci çerçeveler için bunları almak ve reklam fırsatı önce bilgilerini işlemek zaman gönderebilirsiniz.

Tanıtım desteğine ek olarak Canlı geri alma ayarı, istemcilerin kayma ve canlı edge ENTER tuşuna basın, bunlar yine de parçaları sunucudan alabilmeniz izleyicilerin pozisyonunu ayarlamak için kullanılabilir. Bu şekilde, istemciler, HTTP 404 veya 412 hata elde etmezsiniz.

![Filtre için Canlı geri alma][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Tek bir filtrede birden çok kural birleştirme

Tek bir filtrede birden çok filtreleme kurallarını birleştirebilirsiniz. Bir "aralık kuralına" tanımlayabilirsiniz örnek olarak canlı bir arşivden maskeleme görüntülerini kaldırmak ve ayrıca kullanıma kullanılabilir bit hızlarına dönüştürme filtrelemek için. Birden çok filtreleme kurallarını uyguladığınız sonuç tüm kuralları kesişimi olur.

![Birden çok filtre kuralları][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Birden çok filtre (filtre oluşturma) birleştirme

Ayrıca, tek bir URL içinde birden çok filtre birleştirebilirsiniz. Filtrelerini birleştirmeye neden isteyebileceğiniz aşağıdaki senaryoyu gösterir:

1. (Video kalitelerini sınırlamak için), video kalitelerini Android ya da iPad gibi mobil cihazlar için filtre gerekir. İstenmeyen kalitelerini kaldırmak için cihaz profilleri için uygun bir hesabı filtresi oluşturacaksınız. Daha fazla ilişkilendirme olmadan aynı Media Services hesabı altında tüm varlıklarınız için hesap filtreleri kullanabilirsiniz.
1. Ayrıca, bir varlığın başlangıç ve bitiş zamanı trim istiyorsunuz. Bunu başarmak için bir varlık filtresi oluşturacak ve başlangıç/bitiş saatini ayarlayın. 
1. Bu filtreler her ikisi de birleştirmek istediğiniz. Birleşim olmadan, kalite filtresi kullanım daha zor hale getirir kırpmayı filtre filtreleme eklemek gerekir.


Filtreleri birleştirerek için bildirimi/çalma listesine filtre adına ayarlamanız gerekir noktalı virgül ile ayrılmış biçimde URL'si. Adlı bir filtre sahip *MyMobileDevice* kalitelerini filtrelere ve adlı başka bir sahip *MyStartTime* belirli bir başlangıç saati ayarlamak için. En fazla üç filtreleri de birleştirebilirsiniz. 

Daha fazla bilgi için [bu blog gönderisini](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

- Değerleri **forceEndTimestamp**, **presentationWindowDuration**, ve **liveBackoffDuration** için VOD filtre ayarlanmamalıdır. Bunlar yalnızca dinamik filtre senaryolar için kullanılırlar. 
-  Dinamik bildirim GOP sınırları (anahtar çerçeveler) çalışır, böylece kırpma GOP doğruluğu sahiptir.
-  Hesap ve varlık filtrelerinin aynı filtre adı kullanabilirsiniz. Varlık filtreleri, daha yüksek önceliğe sahiptir ve hesap filtreleri geçersiz kılar.
- Bir filtre güncelleştirirseniz, kuralları yenilemek akış uç noktası 2 dakika kadar sürebilir. İçeriğe hizmet edecek şekilde filtreleri kullanılan (ve proxy'ler içeriği önbelleğe ve CDN önbelleğe), bu filtreleri güncelleştiriliyor player hatalarına neden olabilir. Filtre güncelleştirdikten sonra önbelleği temizlemek öneririz. Bu seçenek mümkün değilse, başka bir filtre kullanarak göz önünde bulundurun.
- Müşteriler el ile bildirim indirin ve kesin başlama zaman damgası ayrıştırma ve ölçek zaman gerekir.
    
    - Bir varlık parçalar özelliklerini belirlemek için [almak ve bildirim dosyası inceleyin](#get-and-examine-manifest-files).
    - Varlık filtre zaman damgası özelliklerini ayarlamak için formül şöyledir: <br/>startTimestamp = &lt;bildiriminde başlangıç saati&gt; +  &lt;beklenen filtre başlangıç süresini saniye cinsinden&gt; * ölçeği

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler filtre program aracılığıyla oluşturma işlemini gösterir:  

- [REST API'leri ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [.NET ile filtre oluşturma](filters-dynamic-manifest-dotnet-howto.md)
- [CLI ile filtre oluşturma](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
