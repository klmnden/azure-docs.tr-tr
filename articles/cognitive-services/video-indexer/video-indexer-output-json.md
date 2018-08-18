---
title: Azure Video dizinleyici çıktısını İnceleme | Microsoft Docs
description: Bu konu Video dizinleyici çıktısını inceler.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cflower
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 7f61833d70cea3a6d176f5a08eb0db1c30e70e86
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355889"
---
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>V1 API'si tarafından üretilen Video dizinleyici çıktısını İnceleme

> [!Note]
> Video Indexer V1 API'leri artık kullanımdan kaldırıldı ve 1 Ağustos 2018 tarihinde kaldırılacak. Kesintilerinden kaçınmak için Video Indexer v2 API'lerini kullanarak başlamanız gerekir.
>
> Video Indexer v2 API'leri ile geliştirme için lütfen bulunan yönergeleri okuyun [burada](https://api-portal.videoindexer.ai/). 

Çağırdığınızda **dökümü Al** API ve yanıt durumunu Tamam, yanıt içeriği olarak ayrıntılı bir JSON çıktısını alın. JSON içeriği de dahil olmak üzere belirtilen video öngörüleri (transkripti, karakterlerini, kişiler) ayrıntılarını içerir. Ayrıntılar (konuları) anahtar sözcükleri, yüzleri, blokları içerir. Zaman aralıkları, döküm satırları, OCR satırlar, yaklaşımları, yüz, her bir blok içerir ve küçük resimleri engelleyin.

Kullanabileceğiniz **dökümü Al** video tam dökümü JSON olarak içerik almak için API.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
Videonun özetlenen ınsights tuşlarına basarak da görsel olarak inceleyebilirsiniz **Play** Video Indexer portalı videoda düğmesine. Daha fazla bilgi için [görüntüleme ve düzenleme video öngörüleri](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Bu makalede tarafından döndürülen JSON içeriği inceler **dökümü Al** API. Gözden geçirmek faydalı olabilir [kavramları](video-indexer-concepts.md) makalesi.

> [!NOTE]
> Video Indexer tüm erişim belirteçlerinde süresi bir saattir.

## <a name="root-elements"></a>Kök öğe

Öznitelik | Açıklama
---|---
id|Bu videonun kimliği. Örneğin, 63c6d532ff.
bölüm|Bir mantıksal bölüm kullanıcı daha sonra arama için karşıya yükleme belirtebilirsiniz.
ad|Videonun adı. Örneğin, Azure İzleyici.
açıklama|Videonun açıklaması. Örneğin, "John Kemnetz Scott Hanselman, Azure İzleyici ile Azure izleme verilerinin gücünü kilidini açma gösterilecek birleştirir."
Kullanıcı adı|Görüntü Oluşturucusu. Örneğin, Channel9 videoları.
CreateTime |Zaman oluşturulur. Örneğin, 2017-03-31T16:36:41.4504249 + 00:00.
privacyMode|Videonuzu şu modlardan birine sahip olabilir: **özel**, **genel**. **Genel** -video herkes hesabınızı ve videoya bir bağlantı olan herkes tarafından görülebilir. **Özel** -video hesabınızdaki herkes tarafından da görülebilir.
isOwned|TRUE, geçerli kullanıcının videonun sahibi olur. Aksi takdirde false.  
isBase|Bir video kaynağını dökümü alıyorsa true. Başka bir dökümünü türetilmiş bir çalma listesi dökümü ise false.
Durationınseconds|Video süresi.
summarizedInsights|Bir tane var. [summarizedInsights](#summarizedInsights).
dökümü|Bir veya daha fazla içerebilir [dökümü](#breakdowns)
Sosyal|Bir tane var. **sosyal** sayısını tanımlayan bir öğe beğenilerin ve videonun görünümleri.

## <a name="summarizedinsights"></a>summarizedInsights

Bu bölümde, içgörüler özetini gösterir.

Öznitelik | Açıklama
---|---
ad|Videonun adı. Örneğin, Azure İzleyici.
shortId|Video kimliği. Örneğin, 63c6d532ff.
privacyMode|Döküm şu modlardan birine sahip olabilir: **özel**, **genel**. **Genel** -video herkes hesabınızı ve videoya bir bağlantı olan herkes tarafından görülebilir. **Özel** -video hesabınızdaki herkes tarafından da görülebilir.
süre|Bir öngörü gerçekleştiği zaman açıklayan bir süresini içerir. Saniyeler içinde süresidir.
thumbnailUrl|Video küçük resmi tam URL'si. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Videonun özel ise, bir saat erişim belirteci URL'sini içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').
yüzleri|Bir veya daha fazla içerebilir [yüzleri](#faces)
konuları|Bir veya daha fazla içerebilir [konuları](#topics)
yaklaşımlar|Bir veya daha fazla içerebilir [yaklaşımları](#sentiments)
audioEffects| Bir veya daha fazla içerebilir [audioEffects](#audioEffects)
markalar| Sıfır veya daha fazla içerebilir [markalar](#brands)
İstatistikler|Daha fazla bilgi için [istatistikleri](#Statistics)
.
### <a name="statistics"></a>İstatistikler

|Ad|Açıklama|
|---|---|
|CorrespondenceCount|Videoda yazışmalar sayısı.|
|WordCount|Konuşmacı sözcük sayısı.|
|SpeakerNumberOfFragments|Parçaları miktarı bulunan bir videoyu Konuşmacı vardır.|
|SpeakerLongestMonolog|Konuşmacı uzun monolog. Konuşmacı silences monolog içinde varsa dahil edilir. Başında ve sonunda monolog sessizlik kaldırılır.| 
|SpeakerTalkToListenRatio|Hesaplama videonun toplam zaman bölünmüş konuşmacının monolog (olmadan arasındaki sessizlik) üzerinde harcanan zamanı temel alır. Saat üçüncü ondalık noktasına yuvarlanır.|

## <a name="breakdowns"></a>dökümü

Bu bölüm öngörüleri ayrıntılarını gösterir.

Öznitelik | Açıklama
---|---
id|Çözümleme kimliği Örneğin, 63c6d532ff.
durum|Belirli çözümleme kimliği işlem durumu. Aşağıdakilerden biri olabilir: karşıya yüklenen, işleme, işlenen, başarısız.
processingProgress|İlerleme durumu. Örneğin, % 10.
externalId| Karşıya yükleme sırasında externalID ayarlayabilirsiniz. Örneğin, "4f9c3500-eca7-4ab3-987e-a745017af698." Bu dış kimliğe göre videolarınız için daha sonra arama yapabileceğiniz
externalUrl|Karşıya yükleme sırasında externalUrl ayarlayabilirsiniz. 
meta veriler|Meta verilerini karşıya yükleme sırasında ayarlayabilirsiniz. 
görüşler|Bir veya daha fazla içerebilir [öngörüleri](#insights)
thumbnailUrl|Video küçük resmi tam URL'si. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Videonun özel ise, bir saat erişim belirteci URL'sini içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').
publishedUrl|Yayımlanan URL. Örneğin, "https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest."
viewToken|Taşıyıcı belirteç
sourceLanguage|Kaynak dili. Aşağıdakiler desteklenir: Çince, İngilizce, Fransızca, Almanca, İtalyanca, Japonca, Portekizce, Rusça, İspanyolca.
Dil|Döküm dili.

## <a name="insights"></a>görüşler

Öznitelik | Açıklama 
---|---
transcriptBlocks|Bir veya daha fazla içerebilir [transcriptBlocks](#transcriptBlocks)
konuları|Bir veya daha fazla içerebilir [konuları](#topics)
yüzleri|Bir veya daha fazla içerebilir [yüzleri](#faces)
Katılımcılar|Bir veya daha fazla içerebilir [katılımcıları](#participants)
contentModeration|Bir içerebilir [contentModeration](#contentModeration)
audioEffectsCategories|Bir veya daha fazla içerebilir [audioEffectsCategories](#audioEffectsCategories)

## <a name="faces"></a>yüzleri

### <a name="summarizedinsights"></a>summarizedInsights

**yüzleri** altında görüntülenen **summarizedInsights**, videoda bulunan her bir yüz özetini gösterir.

Öznitelik | Açıklama 
---|---
id|Bir kişinin kimliği. Örneğin, 11775.
shortId|Kısa kimliği. Bir çalma listesi birkaç dökümü türetilmiş çünkü bu çözümlemeler hangisinin her yüz kaynağı olduğunu anlamak için bu kimliği gereklidir.  
ad|Yüz tanıma tanınırsa, kişinin adı eklenir. Örneğin, "Scott Hanselman." Yüz tanıma bilinmiyorsa, "Bilinmeyen #" eklenir. 
açıklama|Yüz tanıma tanınırsa, açıklama Bing API aramaya bağlı doldurulur. Aksi takdirde, açıklamasıdır **null**.
başlık|Yüz tanıma tanınırsa, açıklama Bing API aramaya bağlı doldurulur. Aksi takdirde, başlıktır **null**.
thumbnailFullUrl|Küçük yüzünün tam URL'si. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Videonun özel ise, bir saat erişim belirteci URL'sini içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [accessToken]').
görünümleri|Bir veya daha fazla içerebilir [görünümleri](#appearances)
seenDuration|İçin ne kadar yüzünü (saniye cinsinden) görüldü.
seenDurationRatio|Video süresi (0-1) göre varlığı.

### <a name="breakdown-insights"></a>Döküm istatistikleri

**yüzleri** altında görüntülenen **dökümü**, videoda bulunan her bir yüz hakkında ayrıntılar açıklanmaktadır.

Öznitelik | Açıklama 
---|---
id|Bir kişinin kimliği. Örneğin, 11775.
bingId|
ad|Yüz tanıma tanınırsa, kişinin adı eklenir. Örneğin, "Scott Hanselman". Yüz tanıma bilinmiyorsa, "Bilinmeyen #" eklenir. 
thumbnailId|Örneğin, 616468f0-1636-4efa-94e7-262f2e575059.
açıklama|Yüz tanıma tanınırsa, açıklama Bing API aramaya bağlı doldurulur. Aksi takdirde, açıklamasıdır **null**.
başlık|Yüz tanıma tanınırsa, açıklama Bing API aramaya bağlı doldurulur. Aksi takdirde, başlıktır **null**.
ImageUrl|Bu URL, kaynaktan video alınmış bir görüntüye işaret eder.  
güven|Güvenilirlik puanı (daha yüksek daha iyidir).
knownPersonId|Bilinen bir kişi (örneğin, ünlü) kimliği. Bir kişi bilinmiyorsa kimliğini sıfırları içerir. Örneğin, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>konuları

### <a name="summarizedinsights"></a>summarizedInsights

**konuları** altında görüntülenen **summarizedInsights**, videoda bulunan her konunun özetini gösterir.

Öznitelik | Açıklama 
---|---
ad|Konu adı (örneğin, "Azure"). 
görünümleri|Bir veya daha fazla içerebilir [görünümlerini](#appearances).
isTranscript|Gerekirse true, bir dökümde bulunamadı. False ise bir OCR bulunamadı.

### <a name="breakdown-insights"></a>Döküm istatistikleri

**konuları** altında görüntülenen **dökümü**, videoda bulunan her konuyla ilgili ayrıntılar açıklanmaktadır.

|Öznitelik | Açıklama |
|---|---|
|id|Konu benzersiz kimliği.|
|ad|Konu adı.|
|fetemm|Şu anda, bu öznitelik kullanılmaz.|
|sözcükler|Şu anda, bu öznitelik kullanılmaz.|
|boyut sayısı|İlgi puanı (daha yüksek daha iyidir).|

## <a name="sentiments"></a>yaklaşımlar

Öznitelik | Açıklama
---|---
sentimentKey| Şu anda aşağıdaki yaklaşımları desteklenmektedir: pozitif, nötr, negatif. 
görünümleri|Bir veya daha fazla içerebilir [görünümleri](#appearances)|.
seenDurationRatio|Video süresi (0-1) göre varlığı.

## <a name="audioeffects"></a>audioEffects

Öznitelik | Açıklama 
---|---
audioEffectKey| Geçerli değerler: sessizlik, konuşma HandClaps.
görünümleri|Bir veya daha fazla içerebilir [görünümleri](#appearances)
seenDurationRatio|Video süresi (0-1) göre varlığı.
seenDuration|(Saniye cinsinden) için ne kadar ses efekti yoktu.

## <a name="appearances"></a>görünümleri

Öznitelik | Açıklama 
---|---
startTime| Saat değeri.
endTime|Saat değeri.
startSeconds| Saat değeri.
endSeconds| Saat değeri.

## <a name="participants"></a>Katılımcılar

Öznitelik | Açıklama 
---|---
id|Katılımcı kimliği.
ad|Katılımcı adı. Örneğin, konuşmacı #1.
pictureUrl|**PictureUrl** özniteliği, gelecekte kullanılmak üzere ayrılmıştır.

## <a name="contentmoderation"></a>contentModeration

Öznitelik | Açıklama 
---|---
adultClassifierValue|Video yetişkinlere yönelik içeriğe sahip güven düzeyi.
racyClassifierValue|Video müstehcen içerik sahip güven düzeyi.
bannedWordsCount|Küfür sözcük sayısı. 
bannedWordsRatio|Sözcük toplam sayısını küfür sözcükleri oranı.
reviewRecommended|Video el ile geçirilmesi gereken varsa gösteren Boole değeri.
isAdult|Video yetişkin video el ile gözden geçirdikten sonra olarak kabul edileceğini gösteren Boole değerleri.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Öznitelik | Açıklama 
---|---
type|Kategori Kimliği.
anahtar|Aşağıdakilerden biri: sessizlik, konuşma HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Öznitelik | Açıklama
---|---
id|Blok kimliği.
satırları|Bir veya daha fazla içerebilir [satırları](#lines)
sentimentIds|**SentimentIds** özniteliği, gelecekte kullanılmak üzere ayrılmıştır.
thumbnailIds|**ThumbnailIds** özniteliği, gelecekte kullanılmak üzere ayrılmıştır.
yaklaşım|Yaklaşım bloğundaki (0-1, pozitif için negatif).
yüzleri|Bir veya daha fazla içerebilir [yüzleri](#faces).
karakterlerini|Bir veya daha fazla içerebilir [karakterlerini](#ocrs).
audioEffectInstances|Bir veya daha fazla içerebilir [audioEffectInstances](#audioEffectInstances).
sahneleri|Bir veya daha fazla içerebilir [sahneler](#scenes).
ek açıklamaları|Sıfır veya daha fazla içerebilir [ek açıklamalar](#annotations).

## <a name="ocrs"></a>karakterlerini

Metin içeriği bulunamadı videoda noktada, açıklar. 

Öznitelik | Açıklama 
---|---
timeRange|Özgün videoyu zaman aralığında.
adjustedTimeRange|AdjustedTimeRange geçerli çalma göreli zaman aralığı. Bir çalma listesi farklı videoları farklı satırlardan oluşturma olduğundan, bir saat video alabilir ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15, ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
satırları|Bir veya daha fazla içerebilir [satırları](#lines).

## <a name="lines"></a>satırları

### <a name="transcriptblocks"></a>transcriptBlocks

**satırları** altında görüntülenen **transcriptBlocks**, videoda bulunan dökümleri satırlarını açıklar.

Öznitelik | Açıklama 
---|---
id| Satır kimliği.
timeRange|Özgün videoyu zaman aralığında.
adjustedTimeRange|AdjustedTimeRange geçerli çalma göreli zaman aralığı. Bir çalma listesi farklı videoları farklı satırlardan oluşturma olduğundan, bir saat video alabilir ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15, ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
participantID| Bu satır konuşmacının kimliği.
metin| Dökümü.
IsIncluded| Temel dökümleri her zaman true. Türetilmiş çalma listesi içinde video, kaynakta bulunan satırlar için IsIncluded ayarlanır = true. Diğer tüm satırlar false.

### <a name="ocrs"></a>karakterlerini

**satırları** altında görüntülenen **karakterlerini**, videoda bulunan karakterlerini satırlarını açıklar.

Öznitelik | Açıklama 
---|---
id|OCR kimliği.
Genişlik|Şu anda, bu öznitelik kullanılmaz.
Yükseklik|Şu anda, bu öznitelik kullanılmaz.
Dil|OCR dili.
TextData|OCR metin.
güven|Güvenilirlik puanı (daha yüksek daha iyidir).

## <a name="scenes"></a>sahneleri

Öznitelik | Açıklama 
---|---
id|Sahne kimliği.
timeRange|Bir tane var. **timeRange**.
ana kare|Anahtar çerçeve zamanı.
anlık görüntüleri|Bir veya daha fazla içerebilir [görüntüleri](#shots).

## <a name="shots"></a>anlık görüntüleri

Öznitelik | Açıklama 
---|---
id||Görüntüsü kimliği.
timeRange|Bir tane var. **timeRange**.
ana kare|Anahtar çerçeve zamanı.

## <a name="audioeffectinstances"></a>audioEffectInstances

Öznitelik | Açıklama 
---|---
type|Ses olay dizini: kahkahaları = 1, HandClaps = 2, müzik = 3, konuşma = 4, sessizlik = 5
aralıkları|Bir veya daha fazla içerebilir [aralıkları](#ranges).

## <a name="ranges"></a>aralıkları

**aralıkları** altında görüntülenen **audioEffectInstances**, bu aralıklardaki ses efekti açıklar.

Öznitelik | Açıklama 
---|---
timeRange|Özgün videoyu zaman aralığında.
adjustedTimeRange|AdjustedTimeRange geçerli çalma göreli zaman aralığı. Bir çalma listesi farklı videoları farklı satırlardan oluşturma olduğundan, bir saat video alabilir ve bunu, örneğin, 10:00-10:15 tek satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15, ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.

## <a name="annotations"></a>ek açıklamaları

Tanınabilir nesne, canlı, manzara, Eylemler ve görsel desenleri alan etiketler döndürür.

|Öznitelik|Açıklama|
|---|---|
|id|Ek açıklama kimliği.|
|Ad|Ek Açıklama (örneğin, kişi, Athletic game, siyah çerçeveler) adı.|
|Görünümleri|Bir veya daha fazla görünümlerini içerebilir.|

## <a name="brands"></a>markalar

İş ve ürün marka konuşma metin dökümü ve/veya videoyu OCR algılandı adları. Bu, markaları veya logosu algılama visual tanıma içermez.

Öznitelik | Açıklama
---|---
id | Marka kimliği.
ad | Bing marka veya özelleştirilmiş bir marka adı.  
wikiId | Marka wikipedia URL'si son eki. Örneğin, "Target_Corporation" soneki eklenir [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | Marka, Wikipedia URL'si, kullanıcının bulunmaktadır. Örneğin, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
güven | Video Indexer marka algılayıcısı (0-1) güvenle değeri.
açıklama | Wikipedia ayıklanan marka açıklaması. 
görünümleri | Bir veya daha fazla görünümlerini içerebilir.
seenDuration | Video süresi (0-1) göre varlığı.

## <a name="next-steps"></a>Sonraki adımlar

Kendi döküm oluşturma hakkında daha fazla bilgi için bkz [görünümü ve düzenleme Video dizinleyici öngörülerini](video-indexer-view-edit.md).

Uygulamanıza pencere öğeleri hakkında daha fazla bilgi için bkz: [uygulamalarınıza ekleme Video Indexer pencere öğeleri](video-indexer-embed-widgets.md). 

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Video Indexer genel bakış](video-indexer-overview.md)

