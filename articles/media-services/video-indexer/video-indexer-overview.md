---
title: Video Indexer nedir?
titlesuffix: Azure Media Services
description: Bu konu Video Indexer hizmetine genel bir bakış sağlar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 03/12/2019
ms.author: juliako
ms.openlocfilehash: a182b9ec0fb945b4c2ffddd7a977df8ad9a8d250
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58895977"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Azure Video Indexer; Azure Media Analytics, Azure Search, Bilişsel Hizmetler (Yüz Tanıma API'si, Microsoft Translator, Görüntü İşleme API'si ve Özel Konuşma Tanıma Hizmeti gibi) ve temel alınarak oluşturulmuş bir bulut uygulamasıdır. Aşağıda açıklanan Video Indexer video ve ses modelleri kullanarak videolarınızdan içgörüleri ayıklanacak sağlar:
  
## <a name="video-insights"></a>Video içgörüleri

- **Yüz algılama**: Algılar ve videoda görünen yüzleri gruplandırır.
- **Ünlü belirleme**: Video Indexer, 1 milyondan ünlüleri – dünya lideri, aktör, Kadın Oyuncular, atletlerine, araştırmacılar, iş ve teknoloji liderleri gibi dünya çapında otomatik olarak tanımlar. Bu ünlülerle ilgili bilgilere IMDB ve Wikipedia gibi birçok ünlü siteden de ulaşılabilir.
- **Hesap tabanlı yüz tanıma**: Video Indexer, özel bir hesap için bir modeli eğitir. Ardından, eğitilen modele dayalı video yüzlerini algılar. Daha fazla bilgi için [Video Indexer Web sitesinden bir kişi model özelleştirme](customize-person-model-with-website.md) ve [Video Indexer API ile bir kişi modeli özelleştirme](customize-person-model-with-api.md).
- **Küçük resim ayıklama yüzler için** ("en iyi yüz tanıma"): Otomatik olarak (kalite, boyutu ve tamamen çıplak konuma göre) dikdörtgenlerini her grupta en iyi yakalanan yüz tanımlar ve bir resim varlığı ayıklayın.
- **Görsel metin tanıma** (OCR): Videoda görsel olarak görüntülenen metin ayıklar.
- **Görsel içerik denetleme**: Yetişkinlere yönelik ve/veya müstehcen görsel algılar.
- **Etiket Kimliği**: Görsel nesneler ve görüntülenen eylemleri tanımlar.
- **Sahne Segment**: Sahne videoda üzerinde görsel ipuçları değiştiğinde belirler. Tek bir olayı Sahne gösterir ve anlamsal olarak ilişkili ardışık görüntüleri bir dizi tarafından oluşur. 
- **Algılama görüntüsü**: bir görüntüsü videoda üzerinde görsel ipuçları değiştiğinde belirler. Bir görüntüsü, aynı hareket resim kameradan gerçekleştirilen kare dizisidir. Daha fazla bilgi için [sahneler, görüntüleri ve ana kareleri](scenes-shots-keyframes.md).
- **Kare algılama siyah**: Videoda sunulan siyah çerçeveler tanımlar.
- **Ana kare ayıklama**: Bir videoda kararlı ana kareleri algılar.
- **Krediler çalışırken**: başlangıcını ve bitişini TV programları ve filmler sonuna çalışırken krediler tanımlayın.

## <a name="audio-insights"></a>Ses öngörüleri

- **Otomatik dil algılama**: Baskın konuşulan dili otomatik olarak tanımlar. Desteklenen diller: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Portekizce (Brezilya). Dil algılanamadığında İngilizce seçeneği kullanılır.
- **Ses tanıma**: 12 dillerde metni konuşmaya dönüştürür ve uzantıları sağlar. Desteklenen diller şunlardır: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Arapça, Rusça, Portekizce (Brezilya), Hintçe ve Kore dili.
- **Kapalı Açıklamalı Altyazı**: Kapalı Açıklamalı Altyazı üç formatta oluşturur: VTT, TTML, SRT.
- **İki işleme kanal**: Otomatik algılar, döküm ve tek bir zaman çizelgesinde birleştirmeler ayırın.
- **Gürültü azaltma**: Telefon sesini veya gürültülü kayıtları (Skype filtreleri temel alarak)'kurmak temizler.
- **Transkript özelleştirme** (CRI): Özel konuşma sektöre özgü dökümleri oluşturmak için metin modellerine eğitir. Daha fazla bilgi için [Video Indexer Web sitesinden bir dil modelini özelleştirin](customize-language-model-with-website.md) ve [Video Indexer API'ları ile bir dil modelini özelleştirin](customize-language-model-with-api.md).
- **Konuşmacı numaralandırma**: Eşler ve hangi konuşmacının uç hangi sözcükleri anlar ve ne zaman.
- **Konuşmacı istatistikleri**: Konuşmacılar konuşma oranları için istatistikler sağlar.
- **Metinsel içerik denetleme**: Açık metin, ses transkripti algılar.
- **Ses efektleri**: Sessizlik el Islıklar ve konuşma gibi ses efekti tanımlar.
- **Duygu algılama**: Konuşma tanıma ve ses ipuçları üzerinde bağlı olarak duyguları tanımlar. Şu duygular belirlenebilir: sevinç, üzüntü, kızgınlık ve korku.
- **Çeviri**: Ses transkripti 54 farklı dillere çevirileri oluşturur.

## <a name="audio-and-video-insights-multi-channels"></a>Ses ve video öngörüleri (çoklu kanalı)

Kısmi bir kanal tarafından dizin oluşturulurken bu modelleri için sonuç kullanıma sunulacaktır

- **Anahtar sözcükleri ayıklama**: Anahtar sözcükler, konuşma ve görsel metin ayıklar.
- **Ayıklama markalar**: Markaları, konuşma ve görsel metin ayıklar.
- **Konu çıkarımı**: Dökümler ana konulardan biri çıkarımı yapar. 1. düzey IPTC sınıflandırma dahil edilir.
- **Yapıtları**: "Sonraki ayrıntı düzeyini" zengin ayıklar yapıtlar modellerinin her biri için.
- **Yaklaşım analizi**: Konuşma ve görsel metin pozitif, negatif ve nötr yaklaşımları tanımlar.
 
Video Indexer işlemi ve analizi tamamladıktan sonra video içgörülerini gözden geçirebilir, düzenleyebilir, içerisinde arama yapabilir ve yayımlayabilirsiniz.

Video Indexer hizmeti, hem içerik yöneticilerinin hem de geliştiricilerin ihtiyaçlarını karşılayacak şekilde tasarlanmıştır. İçerik yöneticileri Video Indexer web portalını kullanarak tek bir kod satırı yazmadan hizmetten faydalanabilir, bkz. [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md). Geliştiriciler, büyük ölçekli verileri işlemek için API'leri kullanabilirler, bkz. [Video Indexer REST API'sini kullanma](video-indexer-use-apis.md). Hizmet ayrıca müşterilerin pencere öğelerini kullanarak video akışlarını ve ayıklanan içgörüleri kendi uygulamalarında yayımlamalarını sağlar, bkz. [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md).

Bu hizmete var olan AAD, LinkedIn, Facebook, Google veya MSA hesabınızla kaydolabilirsiniz. Daha fazla bilgi için [başlarken](video-indexer-get-started.md) bölümüne bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıda Video Indexer hizmetinin yararlı olabileceği senaryolar verilmiştir

- Arama – Videodan ayıklanan içgörüler, bir video kitaplığındaki arama deneyimini geliştirmek için kullanılabilir. Örneğin konuşmaları ve yüzleri dizine eklemek, videoda belirli bir kişinin belirli bir sözcüğü söylediği veya iki kişinin bir arada göründüğü anları bulmayı sağlayacak arama deneyimini mümkün kılabilir. Videolardan elde edilen bu içgörülerle yapılan aramalar haber ajansları, eğitim kurumları, yayıncılar, eğlence içeriği sahipleri, kurumsal iş kolu uygulamaları ve kullanıcıların arama yapması gereken bir video kitaplığına sahip olan tüm sektörleri hedeflemektedir.
- İçerik oluşturma – ınsights videolardan ayıklanan ve etkili bir şekilde klipleri vb. kuruluş arşiv mevcut içerikten tanıtımları, sosyal medya içeriği, haber gibi içerik oluşturmasına yardımcı olmak 
- Paraya dönüştürme – Video Indexer, videoların değerinin artırılmasına yardımcı olabilir. Örneğin reklam gelirine bağlı olan sektörler (haber, sosyal medya vb.), ayıklanan içgörüleri reklam sunucusuna ek sinyal olarak ekleyerek daha ilgili reklamların görüntülenmesini sağlayabilir (örneğin bir spor ayakkabısı reklamının, yüzme yarışması yerine futbol maçının ortasında gösterilmesi daha uygun olacaktır).
- Kullanıcı etkileşimi – Video içgörüleri, kullanıcılara ilgili video anlarının sunularak kullanıcı etkileşiminin geliştirilmesini sağlayabilir. Örneğin ilk 30 dakika boyunca kürelerin, ikinci 30 dakika boyunca da piramitlerin anlatıldığı bir video olduğunu düşünün. Videonun 30. dakikadan başlatılması, piramitlerle ilgili çalışma yapan bir öğrenci için daha yararlı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md)
- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)
