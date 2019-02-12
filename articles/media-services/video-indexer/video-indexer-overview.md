---
title: Video Indexer nedir?
titlesuffix: Azure Media Services
description: Bu konu Video Indexer hizmetine genel bir bakış sağlar.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 78b362eb75ab4e0ca17045f8c32761a65897d449
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000191"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Azure Video Indexer; Azure Media Analytics, Azure Search, Bilişsel Hizmetler (Yüz Tanıma API'si, Microsoft Translator, Görüntü İşleme API'si ve Özel Konuşma Tanıma Hizmeti gibi) ve temel alınarak oluşturulmuş bir bulut uygulamasıdır. Aşağıda açıklanan Video Indexer modellerini kullanarak videolarınızdan içgörü ayıklamanıza olanak sağlar:
 
- **Otomatik dil algılama**: Baskın konuşulan dili otomatik olarak tanımlar. Desteklenen diller: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Portekizce (Brezilya). Dil algılanamadığında İngilizce seçeneği kullanılır.
- **Ses tanıma**: 12 dillerde metni konuşmaya dönüştürür ve uzantıları sağlar. Desteklenen diller şunlardır: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Arapça, Rusça, Portekizce (Brezilya), Hintçe ve Kore dili.
- **Kapalı Açıklamalı Altyazı**: Kapalı Açıklamalı Altyazı üç formatta oluşturur: VTT, TTML, SRT.
- **İki işleme kanal**: Otomatik algılar, döküm ve tek bir zaman çizelgesinde birleştirmeler ayırın.
- **Gürültü azaltma**:  Telefon sesini veya gürültülü kayıtları (Skype filtreleri temel alarak)'kurmak temizler.
- **Transkript özelleştirme (CRI)**: Eğitir ve sektöre özgü dökümleri oluşturmak için metin modelleri için genişletilmiş özel konuşma yürütür.
- **Konuşmacı numaralandırma**:  Eşler ve hangi konuşmacının uç hangi sözcükleri anlar ve ne zaman.
- **Konuşmacı istatistikleri**: Konuşmacılar konuşma oranları için istatistikler sağlar.
- **Görsel metin tanıma (OCR)**: Videoda görsel olarak görüntülenen metin ayıklar.
- **Ana kare ayıklama**: Bir videoda kararlı ana kareleri algılar.
- **Yaklaşım analizi**: Konuşma ve görsel metin pozitif, negatif ve nötr yaklaşımları tanımlar.
- **Görsel içerik denetleme**: Yetişkinlere yönelik ve/veya müstehcen görsel algılar.
- **Anahtar sözcükleri ayıklama**: Anahtar sözcükler, konuşma ve görsel metin ayıklar.
- **Etiket Kimliği**: Görsel nesneler ve görüntülenen eylemleri tanımlar.
- **Ayıklama markalar**: Markaları, konuşma ve görsel metin ayıklar.
- **Yüz algılama**: Algılar ve videoda görünen yüzleri gruplandırır.
- **Yüzleri ("en iyi yüz") için küçük resim ayıklama**: Otomatik olarak (kalite, boyutu ve tamamen çıplak konuma göre) dikdörtgenlerini her grupta en iyi yakalanan yüz tanımlar ve bir resim varlığı ayıklayın.
- **Ünlü belirleme**: Video Indexer, 1 milyondan ünlüleri – dünya lideri, aktörler ve Kadın Oyuncular, atletlerine, araştırmacılar, iş ve teknoloji liderleri çapında gibi otomatik olarak tanımlar. Bu ünlülerle ilgili bilgilere IMDB ve Wikipedia gibi birçok ünlü siteden de ulaşılabilir.
- **Hesap tabanlı yüz tanıma**: Video Indexer, özel bir hesap için bir modeli eğitir. Ardından bu hesaptaki videolar için özel olarak eğitilmiş olan modeli kullanarak videodaki yüzleri tanır.
- **Metinsel içerik denetleme**: Açık metin, ses transkripti algılar.
- **Algılama görüntüsü**: Videoda bir Sahne değiştiğinde belirler.
- **Kare algılama siyah**: Videoda sunulan siyah çerçeveler tanımlar.
- **Ses efektleri**: Sessizlik el Islıklar ve konuşma gibi ses efekti tanımlar.
- **Konu çıkarımı**: Dökümler ana konulardan biri çıkarımı yapar. 1. düzey [IPTC](https://iptc.org/standards/media-topics/) sınıflandırması dahildir.
- **Duygu algılama**: Konuşma tanıma ve ses ipuçları üzerinde bağlı olarak duyguları tanımlar. Şu duygular belirlenebilir: sevinç, üzüntü, kızgınlık ve korku.
- **Yapıtları**: "Sonraki ayrıntı düzeyini" zengin ayıklar yapıtlar modellerinin her biri için.
- **Çeviri**: Ses transkripti 54 farklı dillere çevirileri oluşturur.

Video Indexer işlemi ve analizi tamamladıktan sonra video içgörülerini gözden geçirebilir, düzenleyebilir, içerisinde arama yapabilir ve yayımlayabilirsiniz.

Video Indexer hizmeti, hem içerik yöneticilerinin hem de geliştiricilerin ihtiyaçlarını karşılayacak şekilde tasarlanmıştır. İçerik yöneticileri Video Indexer web portalını kullanarak tek bir kod satırı yazmadan hizmetten faydalanabilir, bkz. [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md). Geliştiriciler, büyük ölçekli verileri işlemek için API'leri kullanabilirler, bkz. [Video Indexer REST API'sini kullanma](video-indexer-use-apis.md). Hizmet ayrıca müşterilerin pencere öğelerini kullanarak video akışlarını ve ayıklanan içgörüleri kendi uygulamalarında yayımlamalarını sağlar, bkz. [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md).

Bu hizmete var olan AAD, LinkedIn, Facebook, Google veya MSA hesabınızla kaydolabilirsiniz. Daha fazla bilgi için [başlarken](video-indexer-get-started.md) bölümüne bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıda Video Indexer hizmetinin yararlı olabileceği senaryolar verilmiştir

- Arama – Videodan ayıklanan içgörüler, bir video kitaplığındaki arama deneyimini geliştirmek için kullanılabilir. Örneğin konuşmaları ve yüzleri dizine eklemek, videoda belirli bir kişinin belirli bir sözcüğü söylediği veya iki kişinin bir arada göründüğü anları bulmayı sağlayacak arama deneyimini mümkün kılabilir. Videolardan elde edilen bu içgörülerle yapılan aramalar haber ajansları, eğitim kurumları, yayıncılar, eğlence içeriği sahipleri, kurumsal iş kolu uygulamaları ve kullanıcıların arama yapması gereken bir video kitaplığına sahip olan tüm sektörleri hedeflemektedir.

- Paraya dönüştürme – Video Indexer, videoların değerinin artırılmasına yardımcı olabilir. Örneğin reklam gelirine bağlı olan sektörler (haber, sosyal medya vb.), ayıklanan içgörüleri reklam sunucusuna ek sinyal olarak ekleyerek daha ilgili reklamların görüntülenmesini sağlayabilir (örneğin bir spor ayakkabısı reklamının, yüzme yarışması yerine futbol maçının ortasında gösterilmesi daha uygun olacaktır).

- Kullanıcı etkileşimi – Video içgörüleri, kullanıcılara ilgili video anlarının sunularak kullanıcı etkileşiminin geliştirilmesini sağlayabilir. Örneğin ilk 30 dakika boyunca kürelerin, ikinci 30 dakika boyunca da piramitlerin anlatıldığı bir video olduğunu düşünün. Videonun 30. dakikadan başlatılması, piramitlerle ilgili çalışma yapan bir öğrenci için daha yararlı olacaktır.

Daha fazla bilgi için bu [bloga](https://aka.ms/videoindexerblog) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md)
- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)
