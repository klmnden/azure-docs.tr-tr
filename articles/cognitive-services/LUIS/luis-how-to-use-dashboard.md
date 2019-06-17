---
title: Pano - dil anlama
titleSuffix: Azure Cognitive Services
description: Intents analytics panosunu, görselleştirilmiş bir raporlama aracına düzeltin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/22/2019
ms.author: diberry
ms.openlocfilehash: 055d113a2bc77f8de1b4b881718007c869470532
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66236955"
---
# <a name="how-to-use-the-dashboard-to-improve-your-app"></a>Uygulamanızı geliştirmek için Panoyu kullanma

Bulup örnek konuşma kullanırken eğitilen uygulamanızın ıntents sorunları düzeltin. Pano, düzeltilmesi gereken hedefleri en önemli özellikleri ile genel uygulama bilgilerini görüntüler. 

Gözden geçirme Pano analizi değiştikçe ve modelinizin geliştirilmesine Yinelenen yinelemeli bir işlemdir.

Bu sayfada herhangi bir örnek konuşma olarak bilinen bir amacı olmayan uygulamalar için uygun analiz olmaz _yalnızca deseni_ uygulamalar. 

## <a name="what-issues-can-be-fixed-from-dashboard"></a>Hangi sorunları panodan düzeltilebilir?

Panoda ele üç sorunlar şunlardır:

|Sorun|Çizelge rengi|Açıklama|
|--|--|--|
|Veri dengesizliği|-|Bu durum, örnek konuşma miktarını önemli ölçüde değişir oluşur. Tüm hedefleri gerek _kabaca_ aynı sayıda örnek konuşma - hiçbiri hedefi dışında. Bu gibi durumlarda, % 10-%15 konuşma toplam miktarı yalnızca uygulamada olmalıdır.<br><br> Veri imbalanced ancak hedefi doğruluğu belirli bir eşiğin üstünde, bu dengesizliği bir sorun bildirilmedi.<br><br>**Başlatmak bu sorunu ile - diğer sorunların kök nedenini olabilir.**|
|Belirsiz Öngörüler|Orange|Üst amaç ve sonraki amaç 's puanları bunlar nedeniyle sonraki eğitimle Çevir yeterince yakın olduğunda gerçekleşir [negatif örnekleme](luis-how-to-train.md#train-with-all-data) ya da daha fazla örnek konuşma ıntent'e eklendi. |
|Yanlış tahmin|Kırmızı|Bu durum, bir örnek utterance etiketli amaç (durumda amacı) için öngörülen değil oluşur.|

Doğru tahminler mavi ile gösterilir.

Pano bu sorunları gösterir ve hangi ıntents etkilendiğini bildirir ve uygulamayı iyileştirmek için ne yapmanız gerektiğini önerir. 

## <a name="before-app-is-trained"></a>Uygulama eğitildi önce 

Uygulamayı eğitme önce Pano düzeltmeleri için herhangi bir öneri içermiyor. Bu önerileri görmek için uygulamanızı eğitin.  

## <a name="check-your-publishing-status"></a>Yayımlama durumunuzu denetleme

**Yayımlama durumu** kart etkin hakkında bilgi içeren sürüm yayınlama son. 

Etkin sürüm çözmek istediğiniz sürüm olduğundan emin olun. 

![Pano gösterir uygulamanın dış hizmetler, bölgeler yayımlanan ve uç noktası İsabeti bir araya getirilir.](./media/luis-how-to-use-dashboard/analytics-card-1-shows-app-summary-and-endpoint-hits.png)

Bu da herhangi bir dış hizmetler, yayımlanan bölgeleri gösterir ve uç noktası isabet sayısı toplanır. 

## <a name="review-training-evaluation"></a>Eğitim değerlendirme gözden geçirin

**Eğitim değerlendirme** kart uygulamanızın genel doğruluğu alana göre toplanmış özetini içerir. Puan hedefi kalite gösterir. 

![Eğitim değerlendirme kart, ilk alanı, uygulamanızın genel doğruluğu hakkında bilgi içerir.](./media/luis-how-to-use-dashboard/analytics-card-2-shows-app-overall-accuracy.png)

Grafik, doğru şekilde tahmin edilen hedefleri ve farklı renklerde sorunlu alanları gösterir. Öneriler, bu puanı artış ile uygulama geliştirme gibi. 

Önerilen düzeltmeler, sorun türü tarafından ayrılmış ve uygulamanız için en önemli olan. Amaç, kullanım başına sorunlarını gözden geçirin ve tercih ediyorsanız **[hatalarla hedefleri](#intents-with-errors)** sayfanın alt kısmındaki kartını. 

Her sorun alanı düzeltilmesi gereken hedefleri sahiptir. Hedefi adı seçtiğinizde **hedefi** filtre uygulanmış konuşma sayfası açılır. Bu filtre soruna neden olan konuşma üzerinde odaklanmanıza olanak verir.

### <a name="compare-changes-across-versions"></a>Sürümleri arasında değişiklikleri Karşılaştır

Uygulamada bir değişiklik yapmadan önce yeni bir sürümünü oluşturun. Yeni sürümünde, önerilen değişiklikleri amaç'ın örnek konuşma olun, ardından yeniden eğitin. Pano sayfasındaki 's **eğitim değerlendirme** kartında, kullanın **Show değişiklik eğitilen sürümünden** değişiklikleri karşılaştırmak için. 

![Sürümleri arasında değişiklikleri Karşılaştır](./media/luis-how-to-use-dashboard/compare-improvement-across-versions.png)

### <a name="fix-version-by-adding-or-editing-example-utterances-and-retraining"></a>Sürüm ekleme veya örnek konuşma düzenleme ve yeniden eğitme Düzelt

Uygulamanızı düzeltme birincil yöntemi ekleyin veya örnek konuşma düzenleyin ve yeniden eğitme olacaktır. Yeni veya değiştirilmiş konuşma için yönergeleri takip etmeniz [değiştirilen konuşma](luis-concept-utterance.md).

Örnek Konuşma ekleme yapılmalıdır kişi tarafından kimin:

* Konuşma içinde farklı hedefleri nelerdir anlamak yüksek derecede sahip
* nasıl konuşma tek amacı, başka bir hedefle aklınızı karıştırabilir bilir.
* sık birbiriyle karıştırılabilecek, iki amacı, tek bir hedefi daraltılmış olabilir ve farklı veri varlıklarıyla çekilen karar kuramıyor

### <a name="patterns-and-phrase-lists"></a>Desenler ve ifade listeleri

Ne zaman kullanılacağı analiz sayfasını göstermez [desenleri](luis-concept-patterns.md) veya [tümcecik listeleri](luis-concept-feature.md). Ekleme, hatalı veya belirsiz Öngörüler ile yardımcı olabilir ancak veri dengesizliği ile size yardımcı olmayacaktır. 

### <a name="review-data-imbalance"></a>Gözden geçirme veri dengesizliği

Başlatmak bu sorunu ile - diğer sorunların kök nedenini olabilir.

**Veri dengesizliği** hedefi liste veri dengesizliği düzeltmek için daha fazla konuşma gereken hedefleri gösterir. 

**Bu sorunu gidermek için**:

* Daha fazla konuşma ıntent'e ekleme daha sonra yeniden eğitin. 

Panoda önerilen sürece konuşma hiçbiri hedefi eklemeyin.

> [!Tip]
> Sayfasında, üçüncü bölüm kullanmak **konuşma amacı başına** ile **konuşma (sayı)** , hedefleri daha fazla konuşma gereken hızlı görsel bir kılavuz olarak ayarlama.  
    ![Veri dengesizliği amaçlarıyla bulmak için 'Konuşma (sayı)' kullanın.](./media/luis-how-to-use-dashboard/predictions-per-intent-number-of-utterances.png)

### <a name="review-incorrect-predictions"></a>Yanlış tahminleri gözden geçirin

**Yanlış tahmin** amaç listesi sahip örnek olarak belirli bir amaç için kullanılır, ancak farklı amaçlar için tahmin, konuşma amacı gösterir. 

**Bu sorunu gidermek için**:

* Konuşma yeniden eğitme ve hedefi için daha belirgin olacak şekilde düzenleyin.
* Konuşma çok yakın bir şekilde hizalı olup ve yeniden eğit ıntents birleştirin.

### <a name="review-unclear-predictions"></a>Belirsiz Öngörüler gözden geçirin

**Belirsiz tahmin** amaç listesi gösterir utterance için üst amacı nedeniyle sonraki eğitimle değişebilir, en yakın müsabık kadar yeterli şekilde olmayan tahmin puanları ile Konuşma amaçlarıyla [ Negatif örnekleme](luis-how-to-train.md#train-with-all-data).

**Bu sorunu gidermek için**;

* Konuşma yeniden eğitme ve hedefi için daha belirgin olacak şekilde düzenleyin.
* Konuşma çok yakın bir şekilde hizalı olup ve yeniden eğit ıntents birleştirin.

## <a name="utterances-per-intent"></a>Konuşma amacı başına

Bu kart arasında amacı genel durumunu gösterir. Hedefleri ve retrain düzeltme gibi sorunlar için bu kart, genel bakış devam eder.

Aşağıdaki grafikte, düzeltmek için neredeyse hiçbir sorun ile iyi dengelenmiş bir uygulaması gösterilmektedir.

![Aşağıdaki grafikte, düzeltmek için neredeyse hiçbir sorun ile iyi dengelenmiş bir uygulaması gösterilmektedir.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-balance.png)

Aşağıdaki grafikte, düzeltmek için çok sayıda soruna sahip kötü dengeli bir uygulama gösterilmektedir.

![Aşağıdaki grafikte, düzeltmek için neredeyse hiçbir sorun ile iyi dengelenmiş bir uygulaması gösterilmektedir.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-imbalance.png)

Amacı hakkında bilgi almak için her amaç'ın çubuğu üzerine gelin. 

![Aşağıdaki grafikte, düzeltmek için neredeyse hiçbir sorun ile iyi dengelenmiş bir uygulaması gösterilmektedir.](./media/luis-how-to-use-dashboard/utterances-per-intent-with-details-of-errors.png)

Kullanım **sıralama ölçütü** en sorunlu hedefleri ile bu sorun üzerinde odaklanabilirsiniz ıntents sorun türü olarak düzenlemek için özellik. 

## <a name="intents-with-errors"></a>Hatalarla hedefleri

Bu kart, belirli bir amaç için sorunları gözden geçirmek sağlar. Çabalarınıza odaklanmak nereye göndereceğimizi en sorunlu hedefleri bu kartın varsayılan görünümü şu şekildedir.

![Hataları kartı hedefleri belirli bir amaç için sorunları gözden geçirmek sağlar. Çabalarınıza odaklanmak nereye göndereceğimizi kartın varsayılan olarak, en sorunlu hedefleri için filtrelenir.](./media/luis-how-to-use-dashboard/most-problematic-intents-with-errors.png)

İlk halka grafik amaç sorunlar arasında üç sorun türleri gösterir. Üç sorunu türlerinde bir sorun varsa, her türü kendi grafiğin altına, tüm müsabık hedefleri birlikte sahiptir. 

### <a name="filter-intents-by-issue-and-percentage"></a>Filtre ıntents sorunu ve yüzdesi

Kartın bu bölümü, hata eşiği dışında kalan örnek konuşma bulmanızı sağlar. İdeal olarak, önemli olarak doğru tahminler elde etmek istersiniz. Bu, iş ve müşteri odaklı yüzdesidir. 

İşletmeniz için alışık olduğunuz eşiğini yüzde belirleyin. 

Filtre belirli hedefleri bulmanıza olanak tanır:

|Filtre|Önerilen yüzdesi|Amaç|
|--|--|--|
|En sorunlu hedefleri|-|**Buradan başlayın** -konuşma bu amaca düzeltme artıracak diğer düzeltmeleri birden çok uygulama.|
|Aşağıya doğru tahminler|60%|Seçili amacını doğru ancak bir güven puanı eşiğin altına konuşma yüzdesidir. |
|Yukarıdaki belirsiz Öngörüler|%15|Seçili amacı, en yakın müsabık hedefle birbiriyle karıştırılabilecek konuşma yüzdesidir.|
|Yukarıdaki yanlış tahmin|%15|Seçili amacı, yanlış tahmin edilen konuşma yüzdesidir. |

### <a name="correct-prediction-threshold"></a>Doğru tahmin eşiği

Size bir başarılara tahmin güvenilirlik puanı nedir? Uygulama geliştirme başlangıcında, hedef % 60 olabilir. Kullanım **düzeltmek Öngörüler aşağıdaki** yüzde 60 oranında bir konuşma düzeltilmesi gereken seçili amaca bulmak için.

### <a name="unclear-or-incorrect-prediction-threshold"></a>Belirsiz veya yanlış tahmin eşiği

Bu iki filtreleri konuşma seçili amaç, eşiğini aşan bulmak olanak sağlar. Bu iki yüzdelerini hata yüzde olarak düşünebilirsiniz. Tahminler elde etmek için 10-%15 hata oranıyla rahatça kullanabiliyorsanız, %15, bu değer yukarıdaki tüm konuşma bulmak için filtre eşik ayarlayın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarınızı yönetme](luis-how-to-azure-subscription.md)
