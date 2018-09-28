---
title: Video Indexer nedir?
titlesuffix: Azure Cognitive Services
description: Bu konu Video Indexer hizmetine genel bir bakış sağlar.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 09/15/2018
ms.author: nolachar
ms.openlocfilehash: fd92e91989bd1a37626227b327d644c9d704ab6c
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983024"
---
# <a name="what-is-video-indexer"></a>Video Indexer nedir?

Azure Video Indexer; Azure Media Analytics, Azure Search, Bilişsel Hizmetler (Yüz Tanıma API'si, Microsoft Translator, Görüntü İşleme API'si ve Özel Konuşma Tanıma Hizmeti gibi) ve temel alınarak oluşturulmuş bir bulut uygulamasıdır. Aşağıda açıklanan Video Indexer modellerini kullanarak videolarınızdan içgörü ayıklamanıza olanak sağlar:
 
- **Otomatik dil algılama**: En baskın olarak konuşulan dili otomatik olarak belirler. Desteklenen diller: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Rusça ve Portekizce (Brezilya). Dil algılanamadığında İngilizce seçeneği kullanılır.
- **Ses transkripsiyonu**: 10 dildeki konuşmaları metne dönüştürür. Uzantılarla kullanılabilir. Desteklenen diller: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Arapça, Rusça ve Portekizce (Brezilya).
- **Açıklamalı altyazı**: Üç biçimde açıklamalı altyazı oluşturur: VTT, TTML, SRT.
- **İki kanallı işleme**: Otomatik olarak algılar, ayrı transkriptler hazırlar ve tek bir zaman çizelgesinde birleştirir.
- **Gürültü azaltma**: Telefon sesini veya gürültülü kayıtları temizler (Skype filtrelerini temel alarak).
- **Transkript özelleştirme (CRIS)**: Genişletilmiş özel konuşmayı metne dönüştürme modellerini sektöre özgü transkriptler oluşturacak şekilde eğitir ve yürütür.
- **Konuşmacı numaralandırma**: Hangi konuşmacının ne zaman ve hangi sözcükleri söylediğini anlar ve eşler.
- **Konuşmacı istatistikleri**: Konuşmacıların konuşma oranlarına ilişkin istatistikler sağlar.
- **Görsel metin tanıma (OCR)**: Videoda görsel olarak görüntülenen metinleri ayıklar.
- **Ana kare ayıklama**: Videodaki kararlı ana kareleri algılar.
- **Yaklaşım analizi**: Konuşmalardaki ve görsel metinlerdeki olumlu, olumsuz ve nötr yaklaşımları belirler.
- **Görsel içerik moderasyonu**: Yetişkinlere yönelik ve/veya müstehcen görselleri algılar.
- **Anahtar sözcük ayıklama**: Konuşmalardaki ve görsel metinlerdeki anahtar sözcükleri ayıklar.
- **Etiket belirleme**: Görüntülenen görsel nesneleri ve eylemleri belirler.
- **Marka ayıklama**: Konuşmalardaki ve görsel metinlerdeki markaları ayıklar.
- **Yüz algılama**: Videoda görünen yüzleri algılar ve gruplandırır.
- **Yüzler için küçük resim ayıklama ("en iyi yüz")**: Her yüz grubundaki en iyi yakalanmış yüzü otomatik bir şekilde belirler (kaliteye, boyuta ve ön konumda olmaya göre) ve resim varlığı olarak ayıklar.
- **Ünlü belirleme**: 1 milyon ünlü içeren veritabanını kullanarak videodaki ünlüleri tanır. Kaynak olarak IMDB, Wikipedia ve Linkedin’de en fazla etki sahibi olanlar kullanılır.
- **Özel yüz belirleme**: İlgili hesap için eğitilmiş özel bir modele dayalı olarak videodaki yüzleri tanır.
- **Metinsel içerik moderasyonu**: Ses transkriptlerindeki müstehcen metinleri algılar.
- **Çekim algılama**: Videoda sahne değiştiğinde bunu belirler.
- **Siyah kare algılama**: Videoda yer alan siyah kareleri belirler.
- **Ses efektleri**: El çırpma, konuşma ve sessizlik gibi ses efektlerini belirler.
- **Konu çıkarımı**: Transkriptlerdeki ana konuların çıkarımını yapar. 1. düzey [IPTC](https://iptc.org/standards/media-topics/) sınıflandırması dahildir.
- **Duygu algılama**: Konuşmaya ve sesli ipuçlarına dayalı olarak duyguları belirler. Şu duygular belirlenebilir: sevinç, üzüntü, kızgınlık ve korku.
- **Yapıtlar**: Modellerin her biri için "daha üst düzeyde ayrıntıya sahip" zengin bir yapıt kümesini ayıklar.
- **Çeviri**: Ses transkriptinin 54 farklı dile çevirisini oluşturur.

Video Indexer işlemi ve analizi tamamladıktan sonra video içgörülerini gözden geçirebilir, düzenleyebilir, içerisinde arama yapabilir ve yayımlayabilirsiniz.

Video Indexer hizmeti, hem içerik yöneticilerinin hem de geliştiricilerin ihtiyaçlarını karşılayacak şekilde tasarlanmıştır. İçerik yöneticileri Video Indexer web portalını kullanarak tek bir kod satırı yazmadan hizmetten faydalanabilir, bkz. [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md). Geliştiriciler, büyük ölçekli verileri işlemek için API'leri kullanabilirler, bkz. [Video Indexer REST API'sini kullanma](video-indexer-use-apis.md). Hizmet ayrıca müşterilerin pencere öğelerini kullanarak video akışlarını ve ayıklanan içgörüleri kendi uygulamalarında yayımlamalarını sağlar, bkz. [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md).

Bu hizmete var olan AAD, LinkedIn, Facebook, Google veya MSA hesabınızla kaydolabilirsiniz. Daha fazla bilgi için [başlarken](video-indexer-get-started.md) bölümüne bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıda Video Indexer hizmetinin yararlı olabileceği senaryolar verilmiştir

- Arama – Videodan ayıklanan içgörüler, bir video kitaplığındaki arama deneyimini geliştirmek için kullanılabilir. Örneğin konuşmaları ve yüzleri dizine eklemek, videoda belirli bir kişinin belirli bir sözcüğü söylediği veya iki kişinin bir arada göründüğü anları bulmayı sağlayacak arama deneyimini mümkün kılabilir. Videolardan elde edilen bu içgörülerle yapılan aramalar haber ajansları, eğitim kurumları, yayıncılar, eğlence içeriği sahipleri, kurumsal iş kolu uygulamaları ve kullanıcıların arama yapması gereken bir video kitaplığına sahip olan tüm sektörleri hedeflemektedir.

- Paraya dönüştürme – Video Indexer, videoların değerinin artırılmasına yardımcı olabilir. Örneğin reklam gelirine bağlı olan sektörler (haber, sosyal medya vb.), ayıklanan içgörüleri reklam sunucusuna ek sinyal olarak ekleyerek daha ilgili reklamların görüntülenmesini sağlayabilir (örneğin bir spor ayakkabısı reklamının, yüzme yarışması yerine futbol maçının ortasında gösterilmesi daha uygun olacaktır).

- Kullanıcı etkileşimi – Video içgörüleri, kullanıcılara ilgili video anlarının sunularak kullanıcı etkileşiminin geliştirilmesini sağlayabilir. Örneğin ilk 30 dakika boyunca kürelerin, ikinci 30 dakika boyunca da piramitlerin anlatıldığı bir video olduğunu düşünün. Videonun 30. dakikadan başlatılması, piramitlerle ilgili çalışma yapan bir öğrenci için daha yararlı olacaktır.

Daha fazla bilgi için bu [bloga](http://aka.ms/videoindexerblog) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer web sitesini kullanmaya başlama](video-indexer-get-started.md)
- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)
