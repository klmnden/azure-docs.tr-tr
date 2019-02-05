---
title: Filtreler ve Azure Media Services olan dinamik bildirimler | Microsoft Docs
description: Bu konuda, istemci akışı için bir stream'ın belirli bölümlerine kullanabilmesi için filtreler oluşturmayı açıklar. Media Services, bu seçmeli akış arşivlemek için olan dinamik bildirimler oluşturur.
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
ms.date: 01/24/2019
ms.author: juliako
ms.openlocfilehash: 139f6283c2b59aee53afa3f0dd52e06e2b0eff4c
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55695228"
---
# <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimlere

İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services hesap filtreleri ve içeriğiniz için varlık filtrelerini tanımlamanızı sağlar. 

Filtreleri gibi şeyler müşterilerinize izin veren sunucu tarafı kurallar şunlardır: 

- Yalnızca bir bölümünü (tüm video oynatma) yerine bir video kayıttan yürütme. Örneğin:

    - Bir alt klip ("alt klip filtreleme"), Canlı etkinlik göstermek için bildirim azaltmak veya
    - ("Kırpma video") bir video başlangıcını kesim.

- Yalnızca belirtilen yorumlama ve/veya içeriği ("işleme filtreleme") çalmak için kullanılan cihaz tarafından desteklenen belirtilen dil parçaları sunun. 
- ("Ayarlama sunu pencere") player DVR penceresinde sınırlı bir süre sağlamak amacıyla sunu penceresi DVR ayarlayın.

Bu konu başlığı altında açıklanır [kavramları](#concepts) ve [gösterir filtreler tanımları](#definitions). Ardından, yaygın senaryolar hakkında ayrıntılar sağlar. Makalenin sonunda, filtreleri program aracılığıyla nasıl oluşturulacağını gösteren bağlantıları bulabilirsiniz.  

## <a name="concepts"></a>Kavramlar

### <a name="dynamic-manifests"></a>Dinamik bildirimlerin

Medya Hizmetleri sunan **dinamik bildirimlerini** temel üzerinde önceden tanımlanmış [filtreleri](#filters). Filtreler tanımladıktan sonra istemcilerinize bunları belirli bir işleme veya alt klip videonuzun akışını sağlamak için kullanabilirsiniz. Bunlar, akış URL'SİNDE filtreleri belirtmeniz gerekir. Filtreler hızı Uyarlamalı akış için uygulanabilir: Apple HTTP canlı akış (HLS), MPEG-DASH ve kesintisiz akış. 

Aşağıdaki tabloda, filtrelerle URL'leri bazı örnekler gösterilmektedir:

|Protokol|Örnek|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Kesintisiz Akış|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

> [!NOTE]
> Olan dinamik bildirimler, varlık ve bu varlık için varsayılan bildirimi değiştirmeyin. İstemci, bir akış ile veya olmadan filtreleri istemek seçebilirsiniz. 
> 
> 

### <a name="manifest-files"></a>Bildirim dosyaları

Bir varlığı, bit hızı Uyarlamalı akış için kodlarken bir **bildirim** (çalma listesi) dosyası oluşturulur (metin tabanlı veya XML tabanlı dosyasıdır). **Bildirim** dosya ekleme gibi meta veri akış: izleme türü (ses, video veya metin), izleme adı, başlangıç ve bitiş zamanı, bit hızı (kalitelerini), izleme diller Sunu penceresini (sabit süre kayan pencere), video codec bileşeni ( FourCC). Sonraki yürütülebilir video parçalar kullanılabilir ve bulundukları konumlar ilgili bilgi sağlayarak bir sonraki parça almak için player talimatı verir. Parçaları (veya parçaları) gerçek "" bir video içeriğinizi öbekleridir.

HLS bildirim dosyasının bir örnek aşağıda verilmiştir: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="get-and-examine-manifest-files"></a>Almak ve bildirim dosyalarını inceleyin

Akışınız (Canlı veya isteğe bağlı Video) parçaları dinamik olarak oluşturulan bildirime eklenmelidir filtre izleme özelliği koşulları listesine göre belirttiğiniz. Almak ve parçaları özelliklerini incelemek için kesintisiz akış bildirimi ilk yüklemek zorunda.

[Karşıya yükleme, kodlama ve akışını dosyaları .NET ile](stream-files-tutorial-with-api.md) Öğreticisi, .NET ile akış URL'leri oluşturmak nasıl gösterir (bkz: [URL'ler oluşturma](stream-files-tutorial-with-api.md#get-streaming-urls) bölümü). Uygulamayı çalıştırırsanız, URL'lerden birini kesintisiz akış bildirimine işaret eden: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Kopyalayın ve bir tarayıcının adres çubuğuna URL'yi yapıştırın. Dosya indirilir. Tercih ettiğiniz metin düzenleyicisinde açabilirsiniz.

REST örnek için bkz: [karşıya yükleme, kodlama ve akışını REST dosyalarıyla](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>İzleyici bir video Akış hızı

Kullanabileceğiniz [Azure Media Player tanıtımını sayfası](http://aka.ms/amp) video akışının hızı izlemek için. Tanılama bilgilerini tanıtım sayfasını görüntüler **tanılama** sekmesinde:

![Azure Media Player tanılama][amp_diagnostics]

## <a name="defining-filters"></a>Filtrelerin tanımlanması

Varlık filtreleri iki tür vardır: 

* [Hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) (Genel) - Azure Media Services hesabı olan herhangi bir varlığı uygulanabilir, hesabın bir ömre sahiptir.
* [Varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters) (yerel) - yalnızca bir varlık ile filtre oluşturma sırasında ilişkilendirilmiş uygulanabilir, bir varlığın ömrünü sahip. 

[Hesap filtre](https://docs.microsoft.com/rest/api/media/accountfilters) ve [varlık filtre](https://docs.microsoft.com/rest/api/media/assetfilters) türleri tanımlama/tanımlayan filtre için tam olarak aynı özelliklere sahiptir. Oluştururken dışında **varlık filtre**, istediğiniz filtreyi ilişkilendirmek varlık adı belirtmeniz gerekir.

Senaryonuza bağlı olarak, hangi türde bir filtre daha uygun (varlık filtre Hesap Filtresi) mı karar verin. Hesap filtreleri trim belirli bir varlık için varlık filtreleri nerede kullanılabilir (işleme filtreleme) cihaz profilleri için uygundur.

Filtreler tanımlamak için aşağıdaki özellikleri kullanın. 

|Ad|Açıklama|
|---|---|
|firstQuality|Filtrenin ilk kalite hızı.|
|presentationTimeRange|Sunu zaman aralığı. Bu özellik, bildirim başlangıç/bitiş noktaları, sunu penceresi uzunluğu ve canlı bir başlangıç konumu filtreleme için kullanılır. <br/>Daha fazla bilgi için [PresentationTimeRange](#PresentationTimeRange).|
|Parçaları|Parçaları seçimi koşulları. Daha fazla bilgi için [izler](#tracks)|

### <a name="presentationtimerange"></a>PresentationTimeRange

Bu özelliği kullanmak **varlık filtreleri**. Özellik ayarlamak için önerilmez **hesap filtreleri**.

|Ad|Açıklama|
|---|---|
|**endTimestamp**|Mutlak bitiş zamanı sınırını. Video isteğe bağlı (VoD) için geçerlidir. Canlı bir sunumu için daha sessiz bir şekilde göz ardı uygulanan ve sunu sona erer ve akış olduğunda VoD.<br/><br/>Değerin bir mutlak akış uç noktasını temsil eder. Yuvarlatılmış en yakın sonraki GOP başlatma.<br/><br/>StartTimestamp ve EndTimestamp içerecek şekilde kırpmanıza çalma listesi (manifest) kullanın. Örneğin, StartTimestamp 40000000 ve EndTimestamp = = 100000000 StartTimestamp ve EndTimestamp arasında medya içeren bir çalma listesi üretir. Sınır bir parçasını ayrımı idare etmeye, tüm parça bildirime dahil edilir.<br/><br/>Ayrıca bkz **forceEndTimestamp** izleyen tanımı.|
|**forceEndTimestamp**|Canlı Filtreleri için geçerlidir.<br/><br/>**forceEndTimestamp** gösteren bir Boole değeri olup olmadığını **endTimestamp** geçerli bir değere ayarlandı. <br/><br/>Değer ise **true**, **endTimestamp** değeri belirtilmelidir. Ardından belirtilmezse, hatalı istek döndürdü.<br/><br/>Örneğin, giriş video 5 dakikada başlatılır bir filtre tanımlamak istediğiniz ve akış sonuna kadar bağlanabilmelerini, ayarlarsanız **forceEndTimestamp** false ve ayarın atlanarak **endTimestamp**.|
|**liveBackoffDuration**|Canlı yalnızca geçerlidir. Özelliği, Canlı kayıttan yürütme konumunu tanımlamak için kullanılır. Bu kural kullanarak, Canlı kayıttan yürütme konumu gecikme ve oyuncu için sunucu tarafı arabelleği oluşturun. LiveBackoffDuration göre Canlı konumdur. En fazla Canlı geri alma süresi 300 saniyedir.|
|**presentationWindowDuration**|Canlı için geçerlidir. Kullanım **presentationWindowDuration** çalma listesine bir kayan pencereye uygulamak için. Örneğin, presentationWindowDuration ayarlamak iki dakikalık kayan pencere uygulanacak 1200000000 =. Canlı Edge 2 dakika içinde Media Çalma listesinde dahil edilir. Sınır bir parçasını ayrımı idare etmeye, tüm parça çalma listesi dahil edilir. En düşük sunu pencere süresi 60 saniyedir.|
|**startTimestamp**|VoD veya canlı akış için geçerlidir. Değeri akışa ilişkin bir mutlak başlangıç noktasını temsil eder. Değerin yuvarlanmış en yakın sonraki GOP başlatma.<br/><br/>Kullanım **startTimestamp** ve **endTimestamp** içerecek şekilde kırpmanıza çalma listesi (bildirim). Örneğin, startTimestamp 40000000 ve endTimestamp = = 100000000 StartTimestamp ve EndTimestamp arasında medya içeren bir çalma listesi üretir. Sınır bir parçasını ayrımı idare etmeye, tüm parça bildirime dahil edilir.|
|**Zaman Çizelgesi**|VoD veya canlı akış için geçerlidir. Zaman damgaları tarafından kullanılan ölçeği ve yukarıda belirtilen süre. Varsayılan zaman ölçeğini 10000000 ' dir. Alternatif bir ölçeği kullanılabilir. 10000000 HNS (yüz içerir) varsayılandır.|

### <a name="tracks"></a>Parçaları

Akışınız (Canlı veya isteğe bağlı Video) parçaları dinamik olarak oluşturulan bildirime eklenmelidir filtre izleme özelliği koşulları (FilterTrackPropertyConditions) listesine göre belirttiğiniz. Filtreler mantıksal kullanılarak birleştirilir **ve** ve **veya** işlemi.

Filtre izleme özelliği koşulları parça türleri, değerleri (aşağıdaki tabloda açıklanmıştır) ve işlemler (eşittir, eşit değildir) açıklanmaktadır. 

|Ad|Açıklama|
|---|---|
|**Bit hızı**|Bit hızını parça filtreleme için kullanın.<br/><br/>Saniyedeki bit bit hızlarında çeşitli önerilen değerdir. Örneğin, "0-2427000".<br/><br/>Not: 250000 (bit / saniye) gibi belirli hızı yer alan bir değer kullanabilirsiniz, ancak tam bit hızlarına dönüştürme başka bir varlığından dalgalanma gibi bu yaklaşım önerilmez.|
|**FourCC**|Filtreleme için izleme FourCC değerini kullanın.<br/><br/>Belirtilen codec biçim öğesinin ilk öğesinin değeridir [RFC 6381](https://tools.ietf.org/html/rfc6381). Şu anda aşağıdakileri destekler: <br/>Video: "Avc1", "hev1", "hvc1"<br/>Ses: "Mp4a", "AB-3."<br/><br/>Bir varlık parçalar FourCC değerleri belirlemek için [almak ve bildirim dosyası inceleyin](#get-and-examine-manifest-files).|
|**Dil**|Filtreleme için izleme dili kullanın.<br/><br/>Belirtilen RFC 5646 eklemek istediğiniz bir dil etiketi değerdir. Örneğin, "en".|
|**Ad**|Filtreleme için izleme adını kullanın.|
|**Tür**|İzleme türü, filtreleme için kullanın.<br/><br/>Aşağıdaki değerlerine izin verilir: "Görüntü", "ses" veya "metin".|

### <a name="example"></a>Örnek

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="rendition-filtering"></a>İşleme filtreleme

Varlığınız için birden çok kodlama profilleri (H.264 temel H.264 yüksek, AACL, AACH, Dolby dijital Plus) ve çoklu kalite bit hızlarında kodlamanız tercih edebilirsiniz. Ancak, tüm istemci cihazları tüm varlık profilleri ve bit hızlarına dönüştürme destekleyecektir. Örneğin, eski Android cihazları, yalnızca H.264 temel + AACL destekler. Yüksek bit hızlarına dönüştürme avantajları elde edilemiyor, bir cihaz için gönderme, bant genişliği ve cihaz hesaplama boşa harcar. Böyle bir cihaz, yalnızca ölçeğini ona görüntülenmek üzere tüm belirli bilgileri çözmelisiniz.

Dinamik bildirim ile cihaz profillerini oluşturabilirsiniz gibi mobile, konsol, HD/SD vb. ve izler ve her profilinin bir parçası olmasını istediğiniz nitelikleri de içerir.

![İşleme filtreleme örneği][renditions2]

Aşağıdaki örnekte, bir kodlayıcı, yedi ISO MP4 video yorumlama (Başlangıç 180 p 1080 p) mezzanine varlık kodlayın için kullanıldı. Kodlanmış varlık dinamik olarak şu protokolden herhangi birini akış paketlenmiş: HLS, MPEG DASH ve kesintisiz.  Diyagramın üstünde filtre varlıkla HLS bildirimi gösterilir (tüm yedi önayarda içerir).  Alt sol, "ott" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Yanıtta çıkartılır alt iki kalite düzeyi sonuçlandı 1 MB/sn altındaki tüm bit hızlarına dönüştürme kaldırmak için "ott" filtresini belirtir. Sağ alt "mobil" adlı bir filtre uygulandığı HLS bildirimde gösterilir. Çözüm olduğu iki sonuçlandı 720 p büyük yorumlama kaldırmak için "mobil" filtresi belirtir 1080 p yorumlama çıkartılır devre dışı.

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

## <a name="considerations-and-limitations"></a>Önemli noktalar ve sınırlamalar

- Değerleri **forceEndTimestamp**, **presentationWindowDuration**, ve **liveBackoffDuration** için VoD filtre ayarlanmamalıdır. Bunlar, yalnızca dinamik filtre senaryoları için kullanılır. 
- Dinamik bildirim GOP içinde çalışır sınırları (anahtar çerçeveler) bu nedenle kırpma GOP doğruluğu sahiptir. 
- Hesap ve varlık filtrelerinin aynı filtre adı kullanabilirsiniz. Varlık filtreleri, daha yüksek önceliğe sahiptir ve hesap filtreleri geçersiz kılar.
- Bir filtre güncelleştirirseniz, kuralları yenilemek akış uç noktası 2 dakika kadar sürebilir. İçerik bazı filtreler kullanılarak sunulduğu (ve önbellekler proxy'leri ve CDN önbelleğe), bu filtreleri güncelleştiriliyor player hatalarına neden olabilir. Filtre güncelleştirdikten sonra önbelleği temizlemek için önerilir. Bu seçenek, mümkün değildir. farklı bir filtre kullanmayı düşünün.
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
