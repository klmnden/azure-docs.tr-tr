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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 21fb2b84fd58fb7cca7551ee1cef0c79179cfa40
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65467141"
---
# <a name="dynamic-manifests"></a>Dinamik bildirimler

Medya Hizmetleri sunan **dinamik bildirimlerini** önceden tanımlanmış filtrelere göre. Filtreler tanımladıktan sonra (bkz [filtreleri tanımlar](filters-concept.md)), istemcilerinize bunları belirli bir işleme veya alt klip videonuzun akışını sağlamak için kullanabilirsiniz. Bunlar, akış URL'SİNDE filtreleri belirtmeniz gerekir. Filtreler hızı Uyarlamalı akış için uygulanabilir: Apple HTTP canlı akış (HLS), MPEG-DASH ve kesintisiz akış. 

Aşağıdaki tabloda, filtrelerle URL'leri bazı örnekler gösterilmektedir:

|Protokol|Örnek|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

> [!NOTE]
> Olan dinamik bildirimler, varlık ve bu varlık için varsayılan bildirimi değiştirmeyin. İstemci, bir akış ile veya olmadan filtreleri istemek seçebilirsiniz. 
> 

Bu konu ile ilgili kavramları açıklar **dinamik bildirimlerini** istediğiniz bu özelliği kullanmak senaryo örnekleri sağlar.

## <a name="manifests-overview"></a>Bildirimlere genel bakış

Media Services, HLS, MPEG DASH, kesintisiz akış protokolleri destekler. Bir parçası olarak [dinamik paketleme](dynamic-packaging-overview.md), akış istemci bildirimlerinin (HLS ana çalma listesi, DASH medya sunu açıklaması (MPD) ve kesintisiz akış), URL biçimi seçicide göre dinamik olarak oluşturulur. İçinde teslim protokollerine bkz [Bu bölümde](dynamic-packaging-overview.md#delivery-protocols). 

### <a name="get-and-examine-manifest-files"></a>Almak ve bildirim dosyalarını inceleyin

Akışınız (Canlı veya isteğe bağlı Video) parçaları dinamik olarak oluşturulan bildirime eklenmelidir filtre izleme özelliği koşulları listesine göre belirttiğiniz. Almak ve parçaları özelliklerini incelemek için kesintisiz akış bildirimi ilk yüklemek zorunda.

[Karşıya yükleme, kodlama ve akışını dosyaları .NET ile](stream-files-tutorial-with-api.md) Öğreticisi, .NET ile akış URL'leri oluşturmak nasıl gösterir (bkz: [URL'ler oluşturma](stream-files-tutorial-with-api.md#get-streaming-urls) bölümü). Uygulamayı çalıştırırsanız, URL'lerden birini kesintisiz akış bildirimine işaret eden: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Kopyalayın ve bir tarayıcının adres çubuğuna URL'yi yapıştırın. Dosya indirilir. Tercih ettiğiniz metin düzenleyicisinde açabilirsiniz.

REST örnek için bkz: [karşıya yükleme, kodlama ve akışını REST dosyalarıyla](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>İzleyici bir video Akış hızı

Kullanabileceğiniz [Azure Media Player tanıtımını sayfası](https://aka.ms/amp) video akışının hızı izlemek için. Tanılama bilgilerini tanıtım sayfasını görüntüler **tanılama** sekmesinde:

![Azure Media Player tanılama][amp_diagnostics]

## <a name="rendition-filtering"></a>İşleme filtreleme

Varlığınız için birden çok kodlama profilleri (H.264 temel H.264 yüksek, AACL, AACH, Dolby dijital Plus) ve çoklu kalite bit hızlarında kodlamanız tercih edebilirsiniz. Ancak, tüm istemci cihazları tüm varlık profilleri ve bit hızlarına dönüştürme destekleyecektir. Örneğin, eski Android cihazları, yalnızca H.264 temel + AACL destekler. Yüksek bit hızlarına dönüştürme avantajları elde edilemiyor, bir cihaz için gönderme, bant genişliği ve cihaz hesaplama boşa harcar. Böyle bir cihaz, yalnızca ölçeğini ona görüntülenmek üzere tüm belirli bilgileri çözmelisiniz.

Dinamik bildirim ile cihaz profillerini oluşturabilirsiniz gibi mobile, konsol, HD/SD vb. ve izler ve her profilinin bir parçası olmasını istediğiniz nitelikleri de içerir.

![İşleme filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir kodlayıcı, yedi ISO MP4 video yorumlama (Başlangıç 180 p 1080 p) mezzanine varlık kodlayın için kullanıldı. Kodlanmış varlık olabilir [dinamik olarak paketlenmiş](dynamic-packaging-overview.md) aşağıdaki akış protokollerine hiçbirine: HLS, MPEG DASH ve kesintisiz.  Diyagramın üstünde filtre varlıkla HLS bildirimi gösterilir (tüm yedi önayarda içerir).  Alt sol, "ott" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Yanıtta çıkartılır alt iki kalite düzeyi sonuçlandı 1 MB/sn altındaki tüm bit hızlarına dönüştürme kaldırmak için "ott" filtresini belirtir. Sağ alt "mobil" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Çözüm olduğu iki sonuçlandı 720 p büyük yorumlama kaldırmak için "mobil" filtresi belirtir 1080 p yorumlama çıkartılır devre dışı.

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

## <a name="combining-multiple-filters-filter-composition"></a>Birden çok filtre (filtre oluşturma) birleştirme

Ayrıca, tek bir URL içinde birden çok filtre birleştirebilirsiniz. 

Filtrelerini birleştirmeye neden isteyebileceğiniz aşağıdaki senaryoyu gösterir:

1. (Video kalitelerini sınırlamak için), video kalitelerini Android ya da iPAD gibi mobil cihazlar için filtre gerekir. İstenmeyen kalitelerini kaldırmak için cihaz profilleri için uygun bir hesabı filtresi oluşturursunuz. Hesap filtreleri başka ilişkilendirmesi olmadan aynı media services hesabı altındaki tüm varlıklarınız için kullanılabilir. 
2. Ayrıca, bir varlığın başlangıç ve bitiş zamanı trim istiyorsunuz. Bunu başarmak için bir varlık filtresi oluşturun ve başlangıç/bitiş saatini ayarlayın. 
3. Bu filtreler her ikisi de birleştirmek istediğiniz (birleşim olmadan, kalite filtresi kullanımını zorlaştırır kırpmayı filtre filtreleme eklemeniz gerekir).

Filtreleri birleştirerek için bildirimi/çalma listesine filtre adına ayarlamanız gerekir noktalı virgülle ayrılmış URL. Adlı bir filtre sahip *MyMobileDevice* kalitelerini filtrelere ve adlı başka bir sahip *MyStartTime* belirli bir başlangıç saati ayarlamak için. Bunları şu şekilde birleştirebilirsiniz:

En fazla üç filtreleri de birleştirebilirsiniz. 

Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a name="associate-filters-with-streaming-locator"></a>Filtreler akış Bulucu ile ilişkilendirme

Bir akış Bulucu için uygulamak varlık veya hesap filtrelerin listesini belirtebilirsiniz. [Dinamik Paketleyici](dynamic-packaging-overview.md) bu olanlar istemcinizin URL'SİNDE belirtir birlikte filtrelerinin listesi için geçerlidir. Bu birleşim oluşturur bir [dinamik bildirim](filters-dynamic-manifest-overview.md), URL'deki filtreleri + akış Bulucu üzerinde belirttiğiniz filtreleri temel. Filtre uygulamak istediğiniz, ancak URL filtresi adlarında kullanıma sunmak istiyorsanız değil, bu özelliği kullanmanızı öneririz.

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

- Değerleri **forceEndTimestamp**, **presentationWindowDuration**, ve **liveBackoffDuration** için VoD filtre ayarlanmamalıdır. Bunlar, yalnızca dinamik filtre senaryoları için kullanılır. 
- Dinamik bildirim GOP içinde çalışır sınırları (anahtar çerçeveler) bu nedenle kırpma GOP doğruluğu sahiptir. 
- Hesap ve varlık filtrelerinin aynı filtre adı kullanabilirsiniz. Varlık filtreleri, daha yüksek önceliğe sahiptir ve hesap filtreleri geçersiz kılar.
- Bir filtre güncelleştirirseniz, akış kuralları yenilemek uç nokta 2 dakika kadar sürebilir. İçerik bazı filtreler kullanılarak sunulduğu (ve önbellekler proxy'leri ve CDN önbelleğe), bu filtreleri güncelleştiriliyor player hatalarına neden olabilir. Filtre güncelleştirdikten sonra önbelleği temizlemek için önerilir. Bu seçenek, mümkün değildir. farklı bir filtre kullanmayı düşünün.
- Müşteriler bildirim el ile indirip zaman ölçeği ve tam startTimestamp ayrıştırma gerekir.
    
    - Bir varlık parçalar özelliklerini belirlemek için [almak ve bildirim dosyası inceleyin](#get-and-examine-manifest-files).
    - Varlık filtre zaman damgası özelliklerini ayarlamak için formül: <br/>startTimestamp = &lt;bildiriminde başlangıç saati&gt; +  &lt;beklenen filtre başlangıç süresini saniye cinsinden&gt;* ölçeği


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler filtre program aracılığıyla oluşturma işlemini göstermektedir.  

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
