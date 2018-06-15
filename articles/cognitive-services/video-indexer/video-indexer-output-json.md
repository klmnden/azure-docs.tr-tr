---
title: Azure Video dizin oluşturucu çıktıyı inceleyin | Microsoft Docs
description: Bu konuda Video dizin oluşturucu çıkış inceler.
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
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>V1 API'si tarafından üretilen Video dizin oluşturucu çıktıyı inceleyin

> [!Note]
> Video dizin oluşturucu V1 API'leri artık kullanımdan kaldırıldı ve 1 Ağustos 2018 kaldırılacak. Kesintilerini önlemek için Video dizin oluşturucu v2 API'leri kullanarak başlamanız gerekir.
>
> Video dizin oluşturucu v2 API'leri ile geliştirmek için lütfen bulunan yönergeleri başvurun [burada](https://api-portal.videoindexer.ai/). 

Çağırdığınızda **dökümü alma** API ve yanıt durumu Tamam, ayrıntılı bir JSON çıkış yanıt içeriği olarak alın. JSON içeriği de dahil olmak üzere belirtilen video Öngörüler (dökümü, karakterlerini, kişiler) ayrıntılarını içerir. Ayrıntılar anahtar sözcükler (konuları), yüzler, blokları içerir. Zaman aralıkları, dökümü satırları, OCR satırları, düşüncelerin, yüzler, her bloğu içerir ve küçük resimleri engelleyin.

Kullanabileceğiniz **dökümü alma** video tam dökümünü JSON olarak içerik almak için API.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
Video özetlenen Öngörüler tuşlarına basarak da görsel olarak inceleyebilirsiniz **Yürüt** Video dizin oluşturucu portal videoda düğmesinde. Daha fazla bilgi için bkz: [görüntüleme ve düzenleme video Öngörüler](video-indexer-view-edit.md).

![Insights](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Bu makalede tarafından döndürülen JSON içeriği inceler **dökümü alma** API. Gözden geçirmek faydalı olabilir [kavramları](video-indexer-concepts.md) makalesi.

> [!NOTE]
> Video Oluşturucudaki tüm erişim belirteçleri sona erme tarihi bir saattir.

## <a name="root-elements"></a>Kök öğe

Öznitelik | Açıklama
---|---
id|Bu video kimliği. Örneğin, 63c6d532ff.
bölüm|Kullanıcı daha sonra aramak için karşıya yükleme belirtebilir bir mantıksal bölüm.
ad|Görüntü adı. Örneğin, Azure İzleyicisi.
açıklama|Videonun açıklaması. Örneğin, "John Kemnetz Azure İzleyicisi ile Azure izleme verilerini gücünü kilidini açma göstermek için Scott Hanselman birleştirir."
Kullanıcı adı|Video Oluşturucusu. Örneğin, Channel9 videolar.
CreateTime |Oluşturulan saat. Örneğin, 2017-03-31T16:36:41.4504249 + 00:00.
privacyMode|Videonuzu aşağıdaki modlarından birini içerebilir: **özel**, **ortak**. **Ortak** -video herkesin hesabınızı ve videoya bir bağlantı olan herkes tarafından da görülebilir. **Özel** -video hesabınızda herkes tarafından da görülebilir.
isOwned|Geçerli kullanıcının video sahipse true. Aksi takdirde false.  
isBase|Çözümleme bir video kaynağında alıyorsa true. Başka bir çözümleme türetilmiş bir çalma listesi dökümü ise false.
durationInSeconds|Video süresi.
summarizedInsights|İçeriyor [summarizedInsights](#summarizedInsights).
dökümü|Bir veya daha fazla içerebilir [dökümü](#breakdowns)
Sosyal|İçeriyor **sosyal** sayısını açıklayan öğesi yöntemlerine ve video görünümlerini.

## <a name="summarizedinsights"></a>summarizedInsights

Bu bölümde Öngörüler özetini gösterir.

Öznitelik | Açıklama
---|---
ad|Görüntü adı. Örneğin, Azure İzleyicisi.
shortId|Video kimliği. Örneğin, 63c6d532ff.
privacyMode|Çözümleme aşağıdaki modlarından birini içerebilir: **özel**, **ortak**. **Ortak** -video herkesin hesabınızı ve videoya bir bağlantı olan herkes tarafından da görülebilir. **Özel** -video hesabınızda herkes tarafından da görülebilir.
süre|Bir öngörü zamanını açıklayan bir süresini içerir. Süresi geçen saniye cinsinden süredir.
thumbnailUrl|Video küçük resim tam URL. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Video özel ise, URL bir saat erişim belirteci içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
yazıtipleri|Bir veya daha fazla içerebilir [yüzeyleri](#faces)
Konuları|Bir veya daha fazla içerebilir [konuları](#topics)
yaklaşımlar|Bir veya daha fazla içerebilir [düşüncelerin](#sentiments)
audioEffects| Bir veya daha fazla içerebilir [audioEffects](#audioEffects)
markalar| Sıfır veya daha fazla içerebilir [markalar](#brands)
İstatistikler|Daha fazla bilgi için bkz: [istatistikleri](#Statistics)
.
### <a name="statistics"></a>İstatistikler

|Ad|Açıklama|
|---|---|
|CorrespondenceCount|Video yazışmalar sayısı.|
|WordCount|Konuşmacı başına sözcükler sayısı.|
|SpeakerNumberOfFragments|Bir video Konuşmacı sahip parçaları miktarı.|
|SpeakerLongestMonolog|Konuşmacı uzun monolog. Konuşmacı monolog içinde silences varsa dahil edilir. Sessizlik başına ve monolog sonundaki kaldırılır.| 
|SpeakerTalkToListenRatio|Hesaplama video toplam zamanına göre bölünmüş Konuşmacı monolog (olmadan arasındaki sessizlik) üzerinde harcanan zamanı temel alır. Zaman üçüncü ondalık noktasına yuvarlanır.|

## <a name="breakdowns"></a>dökümü

Bu bölümde Öngörüler ayrıntılarını gösterir.

Öznitelik | Açıklama
---|---
id|Çözümleme kimliği. Örneğin, 63c6d532ff.
durum|Belirli çözümleme kimliği işlem durumu. Şunlardan biri olabilir: karşıya yüklenen, işleme, işlenen, başarısız.
processingProgress|Devam ediyor. Örneğin, % 10.
externalId| Karşıya yükleme sırasında externalID ayarlayabilirsiniz. Örneğin, "4f9c3500-eca7-4ab3-987e-a745017af698." Daha sonra videolarınızı bu dış kimliğine göre arama yapabilirsiniz
Dış URL'sinin gösterilmesini|Karşıya yükleme sırasında dış URL'sinin gösterilmesini ayarlayabilirsiniz. 
meta veriler|Meta veriler karşıya yükleme sırasında ayarlayabilirsiniz. 
görüşler|Bir veya daha fazla içerebilir [Öngörüler](#insights)
thumbnailUrl|Video küçük resim tam URL. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Video özel ise, URL bir saat erişim belirteci içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
publishedUrl|Yayımlanan URL. Örneğin, "https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest."
viewToken|Taşıyıcı belirteci
sourceLanguage|Kaynak dili. Aşağıdaki desteklenir: Çince, İngilizce, Fransızca, Almanca, İtalyanca, Japonca, Portekizce, Rusça, İspanyolca.
Dil|Dökümü dili.

## <a name="insights"></a>görüşler

Öznitelik | Açıklama 
---|---
transcriptBlocks|Bir veya daha fazla içerebilir [transcriptBlocks](#transcriptBlocks)
Konuları|Bir veya daha fazla içerebilir [konuları](#topics)
yazıtipleri|Bir veya daha fazla içerebilir [yüzeyleri](#faces)
Katılımcıların|Bir veya daha fazla içerebilir [katılımcıları](#participants)
contentModeration|Bir içerebilir [contentModeration](#contentModeration)
audioEffectsCategories|Bir veya daha fazla içerebilir [audioEffectsCategories](#audioEffectsCategories)

## <a name="faces"></a>yazıtipleri

### <a name="summarizedinsights"></a>summarizedInsights

**yazıtipleri** altında görüntülenen **summarizedInsights**, videoda bulunan her yüz özetini gösterir.

Öznitelik | Açıklama 
---|---
id|Bir kişinin kimliği. Örneğin, 11775.
shortId|Kısa kimliği. Bir çalma listesi birkaç dökümleri türetilmiş çünkü bu kimliği, bu dökümleri her yüz kökeni olduğunu bulmak için gereklidir.  
ad|Yüz tanımlıysa kişinin adını eklenir. Örneğin, "Scott Hanselman." Yüz bilinmiyorsa, "Bilinmeyen #" eklenir. 
açıklama|Yüz tanınırsa, açıklama Bing API'si aramaya bağlı doldurulur. Aksi takdirde açıklamasıdır **null**.
başlık|Yüz tanınırsa, açıklama Bing API'si aramaya bağlı doldurulur. Aksi takdirde, başlık değer **null**.
thumbnailFullUrl|Yüz kişinin küçük resim tam URL. Örneğin, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Video özel ise, URL bir saat erişim belirteci içerdiğine dikkat edin. Bir saat sonra URL artık geçerli olmayacak ve yeni bir url ile yeniden dökümünü alın ya da yeni bir erişim belirteci almak ve tam url el ile oluşturmak için GetAccessToken çağrısı gerekir ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
görünümleri|Bir veya daha fazla içerebilir [görünümleri](#appearances)
seenDuration|İçin ne kadar süreyle yüz (saniye cinsinden) görüldü.
seenDurationRatio|Varlığı video süresi (0-1) göre.

### <a name="breakdown-insights"></a>Çözümleme Öngörüler

**yazıtipleri** altında görüntülenen **dökümleri**, videoda bulunan her yüz hakkında ayrıntılar açıklanmaktadır.

Öznitelik | Açıklama 
---|---
id|Bir kişinin kimliği. Örneğin, 11775.
bingId|
ad|Yüz tanımlıysa kişinin adını eklenir. Örneğin, "Scott Hanselman". Yüz bilinmiyorsa, "Bilinmeyen #" eklenir. 
thumbnailId|Örneğin, 616468f0-1636-4efa-94e7-262f2e575059.
açıklama|Yüz tanınırsa, açıklama Bing API'si aramaya bağlı doldurulur. Aksi takdirde açıklamasıdır **null**.
başlık|Yüz tanınırsa, açıklama Bing API'si aramaya bağlı doldurulur. Aksi takdirde, başlık değer **null**.
ImageUrl|Bu URL kaynaktan video alınmış bir görüntüye işaret eder.  
Güven|GÜVENİRLİK puanı (daha iyidir).
knownPersonId|Bilinen bir kişinin (örneğin, ünlülerle) kimliği. Bir kişinin bilinmiyorsa kimliği sıfır içeriyor. Örneğin, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>Konuları

### <a name="summarizedinsights"></a>summarizedInsights

**Konular** altında görüntülenen **summarizedInsights**, videoda bulunan her konunun özetini gösterir.

Öznitelik | Açıklama 
---|---
ad|Konu adı (örneğin, "Azure"). 
görünümleri|Bir veya daha fazla içerebilir [görünümlerini](#appearances).
isTranscript|Gerekirse true, bir dökümü bulunamadı. False ise bir OCR bulundu.

### <a name="breakdown-insights"></a>Çözümleme Öngörüler

**Konular** altında görüntülenen **dökümleri**, videoda bulunan her konunun hakkında ayrıntılar açıklanmaktadır.

|Öznitelik | Açıklama |
|---|---|
|id|Benzersiz konu kimliği.|
|ad|Konu adı.|
|gövdesi|Şu anda, bu öznitelik kullanılmaz.|
|sözcükler|Şu anda, bu öznitelik kullanılmaz.|
|RANK|İlgi puanı (daha iyidir).|

## <a name="sentiments"></a>yaklaşımlar

Öznitelik | Açıklama
---|---
sentimentKey| Şu anda aşağıdaki düşüncelerin desteklenir: pozitif, nötr, negatif. 
görünümleri|Bir veya daha fazla içerebilir [görünümleri](#appearances)|.
seenDurationRatio|Varlığı video süresi (0-1) göre.

## <a name="audioeffects"></a>audioEffects

Öznitelik | Açıklama 
---|---
audioEffectKey| Geçerli değerler şunlardır: Okuma, sessizlik, HandClaps.
görünümleri|Bir veya daha fazla içerebilir [görünümleri](#appearances)
seenDurationRatio|Varlığı video süresi (0-1) göre.
seenDuration|İçin ne kadar süreyle ses efekti (saniye cinsinden) yoktu.

## <a name="appearances"></a>görünümleri

Öznitelik | Açıklama 
---|---
startTime| Saat değeri.
endTime|Saat değeri.
startSeconds| Saat değeri.
endSeconds| Saat değeri.

## <a name="participants"></a>Katılımcıların

Öznitelik | Açıklama 
---|---
id|Katılımcı kimliği.
ad|Katılımcı adı. Örneğin, konuşmacı #1.
pictureUrl|**PictureUrl** özniteliği, gelecekte kullanılmak üzere ayrılmıştır.

## <a name="contentmoderation"></a>contentModeration

Öznitelik | Açıklama 
---|---
adultClassifierValue|Video yetişkinlere yönelik içeriğe sahip anlamlılık düzeyi.
racyClassifierValue|Video saldırganlardan içeriğe sahip anlamlılık düzeyi.
bannedWordsCount|Uygunsuz metin sözcük sayısı. 
bannedWordsRatio|Toplam sözcük sayısını uygunsuz metin sözcük oranı.
reviewRecommended|Video el ile gözden geçirilmesi gereken varsa belirten Boolean değer.
isAdult|Video yetişkin video el ile gözden geçirdikten sonra kabul edilir, gösteren Boole değerleri.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Öznitelik | Açıklama 
---|---
type|Kategori Kimliği.
anahtar|Şunlardan biri: Okuma, sessizlik, HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Öznitelik | Açıklama
---|---
id|Blok kimliği.
satırları|Bir veya daha fazla içerebilir [satırları](#lines)
sentimentIds|**SentimentIds** özniteliği, gelecekte kullanılmak üzere ayrılmıştır.
thumbnailIds|**ThumbnailIds** özniteliği, gelecekte kullanılmak üzere ayrılmıştır.
Düşünceleri|(0-1, pozitif için negatif) bloktaki düşünceleri.
yazıtipleri|Bir veya daha fazla içerebilir [yüzeyleri](#faces).
karakterlerini|Bir veya daha fazla içerebilir [karakterlerini](#ocrs).
audioEffectInstances|Bir veya daha fazla içerebilir [audioEffectInstances](#audioEffectInstances).
planda|Bir veya daha fazla içerebilir [planda](#scenes).
Ek Açıklamalar|Sıfır veya daha fazla içerebilir [ek açıklamaları](#annotations).

## <a name="ocrs"></a>karakterlerini

Metin içeriği bulundu videoda noktada, açıklar. 

Öznitelik | Açıklama 
---|---
timeRange|Özgün videoda zaman aralığı.
adjustedTimeRange|AdjustedTimeRange geçerli çalma göreli zaman aralığıdır. Farklı videolar farklı satırlarından bir çalma listesi oluşturabilirsiniz olduğundan, bir saat video alın ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15 ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
satırları|Bir veya daha fazla içerebilir [satırları](#lines).

## <a name="lines"></a>satırları

### <a name="transcriptblocks"></a>transcriptBlocks

**satırları** altında görüntülenen **transcriptBlocks**, dökümleri videoda bulunan satırları açıklanmaktadır.

Öznitelik | Açıklama 
---|---
id| Satır kimliği.
timeRange|Özgün videoda zaman aralığı.
adjustedTimeRange|AdjustedTimeRange geçerli çalma göreli zaman aralığıdır. Farklı videolar farklı satırlarından bir çalma listesi oluşturabilirsiniz olduğundan, bir saat video alın ve bunu, örneğin, 10:00-10:15 yalnızca 1 satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15 ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.
participantID| Bu satırı Konuşmacı kimliği.
metin| Dökümü.
IsIncluded| Temel dökümleri her zaman true. Türetilen çalma listelerinize video kaynağında bulunan satırlar için IsIncluded ayarlanır = true. Diğer tüm satırlar false.

### <a name="ocrs"></a>karakterlerini

**satırları** altında görüntülenen **karakterlerini**, karakterlerini videoda bulunan satırları açıklanmaktadır.

Öznitelik | Açıklama 
---|---
id|OCR kimliği.
Genişlik|Şu anda, bu öznitelik kullanılmaz.
Yükseklik|Şu anda, bu öznitelik kullanılmaz.
Dil|OCR dili.
TextData|OCR metin.
Güven|GÜVENİRLİK puanı (daha iyidir).

## <a name="scenes"></a>planda

Öznitelik | Açıklama 
---|---
id|Sahne kimliği.
timeRange|İçeriyor **timeRange**.
ana kare|Anahtar çerçeve zamanı.
görüntüleri|Bir veya daha fazla içerebilir [görüntüleri](#shots).

## <a name="shots"></a>görüntüleri

Öznitelik | Açıklama 
---|---
id||Görüntüsü kimliği.
timeRange|İçeriyor **timeRange**.
ana kare|Anahtar çerçeve zamanı.

## <a name="audioeffectinstances"></a>audioEffectInstances

Öznitelik | Açıklama 
---|---
type|Ses olay dizinini: kahkahaları = 1, HandClaps = 2, müzik = 3, konuşma = 4, sessizlik = 5
aralıkları|Bir veya daha fazla içerebilir [aralıkları](#ranges).

## <a name="ranges"></a>aralıkları

**aralıkları** altında görüntülenen **audioEffectInstances**, bu aralıklardaki ses etkileri açıklanmaktadır.

Öznitelik | Açıklama 
---|---
timeRange|Özgün videoda zaman aralığı.
adjustedTimeRange|AdjustedTimeRange geçerli çalma göreli zaman aralığıdır. Farklı videolar farklı satırlarından bir çalma listesi oluşturabilirsiniz olduğundan, bir saat video alın ve bunu, örneğin, 10:00-10:15 tek satırından kullanın. Bu durumda olduğu zaman aralığı 10:00-10:15 ancak adjustedTimeRange 00:00-00 1 satır içeren bir çalma listesi olacaktır: 15.

## <a name="annotations"></a>Ek Açıklamalar

Etiketler tanınabilir nesneleri, yaşam önemlidir, manzara, Eylemler ve görsel desenleri dayalı döndürür.

|Öznitelik|Açıklama|
|---|---|
|id|Ek açıklama kimliği.|
|Ad|Ek açıklamanın (örneğin, kişi, Athletic oyun, siyah çerçeveler) adı.|
|Görünümleri|Bir veya daha fazla görünümler içerebilir.|

## <a name="brands"></a>markalar

İş ve ürün markasını konuşma metin dökümü ve/veya Video OCR algılandı adları. Bu görsel tanıma markalar veya logosu algılama içermez.

Öznitelik | Açıklama
---|---
id | Bir marka kimliği.
ad | Bing'den marka veya özelleştirilmiş bir marka adı.  
wikiId | Marka wikipedia url soneki. Örneğin, "Target_Corporation" soneki eklenir [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | Marka, Wikipedia url, kullanıcının bulunmaktadır. Örneğin, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
Güven | Video dizin oluşturucu marka algılayıcısı (0-1) güvenirlik değeri.
açıklama | Wikipedia ayıklanan marka açıklaması. 
görünümleri | Bir veya daha fazla görünümler içerebilir.
seenDuration | Varlığı video süresi (0-1) göre.

## <a name="next-steps"></a>Sonraki adımlar

Kendi döküm oluşturma hakkında daha fazla bilgi için bkz: [Görünüm ve Video dizin oluşturucu Öngörüler düzenleme](video-indexer-view-edit.md).

Uygulamanızda pencere öğeleri ekleme hakkında daha fazla bilgi için bkz: [katıştırmak Video dizin oluşturucu widgets, uygulamalara](video-indexer-embed-widgets.md). 

## <a name="see-also"></a>Ayrıca bkz.

[Video dizin oluşturucu API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Video dizin oluşturucu genel bakış](video-indexer-overview.md)

