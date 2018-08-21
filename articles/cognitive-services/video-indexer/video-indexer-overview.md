---
title: Azure Video Indexer'a genel bakış | Microsoft Docs
description: Bu konu Video Indexer hizmetine genel bir bakış sağlar.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 07/25/2018
ms.author: nolachar
ms.openlocfilehash: f52c4af29d0c7de8b5edbe869640ffc5dddb5c5e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397900"
---
# <a name="what-is-video-indexer-preview"></a>Video Indexer nedir? (önizleme)

Video Indexer; Azure Media Analytics, Bilişsel Hizmetler (Yüz Tanıma API'si, Microsoft Translator, Görüntü İşleme API'si ve Özel Konuşma Tanıma Hizmeti) ve Azure Search kullanılarak oluşturulmuş olan bir bulut uygulamasıdır. Bu hizmet, yapay zeka teknolojilerini kullanarak videolarınızdan aşağıdaki içgörüleri elde etmenizi sağlar:

- **Otomatik dil algılama**: Video Indexer, videonun dilini otomatik olarak algılayabilir. Otomatik dil algılama özelliği şu anda İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca ve Rusça dillerini algılamaktadır. Dil algılanamadığında İngilizce seçeneği kullanılır.
- **Ses transkripsiyonu**: Video Indexer, müşterilerin konuşulan kelimelerin transkripsiyonunu elde etmelerini sağlayan konuşmayı metne dönüştürme özelliğine sahiptir. Desteklenen diller: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Portekizce (Brezilya), Japonca ve Rusça (ileride daha fazla dil eklenecektir). 
- **Yüz izleme ve tanıma**: Yüz teknolojileri, videodaki yüzlerin tanınmasını sağlar. Algılanan yüzler, ünlü kişi veritabanıyla eşleştirilerek videodaki ünlülerin tanınması sağlanır. Müşteriler, ünlü kişilerle eşleşmeyen yüzleri de etiketleyebilir. Video Indexer, bu etiketleri kullanarak bir yüz modeli oluşturur ve daha sonra gönderilen videolarda bu yüzleri tanıyabilir.
- **Konuşmacı dizini oluşturma**: Video Indexer, hangi konuşmacının ne zaman konuştuğunu ve ne söylediğini tespit edebilir ve anlayabilir.
- **Görsel metin tanıma**: Video Indexer, bu teknolojiyi kullanarak videolarda görüntülenen metni ayıklar.  
- **Ses etkinliği algılama**: Bu algılama özelliği sayesinde Video Indexer, arka plan gürültüsünü ve ses etkinliğini birbirinden ayırabilir. 
- **Sahne algılama**: Video Indexer, videoda görsel analiz gerçekleştirerek sahne değişimlerini algılayabilir.
- **Ana kare ayıklama**: Video Indexer, videodaki ana kareleri otomatik olarak algılar. 
- **Yaklaşım analizi**: Video Indexer, konuşmayı metne dönüştürme ve optik karakter tanıma özellikleriyle ayıklanan metinlerde yaklaşım analizi gerçekleştirerek sonuçları zaman kodlarıyla birlikte pozitif, negatif veya nötr olarak gösterir.
- **Çeviri**: Video Indexer, ses transkripsiyonunu bir dilden diğerine çevirebilir. Desteklenen diller: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Portekizce (Brezilya), Japonca ve Rusça. Çeviri sonrasında kullanıcı video oynatıcısında diğer dillerde açıklamalı alt yazı da gösterebilir.
- **Görsel içerik moderasyonu**: Bu teknoloji, videodaki yetişkinlere yönelik ve/veya müstehcen içeriğin algılanmasını sağlar ve içerik filtreleme amacıyla kullanılabilir. 
- **Anahtar sözcük ayıklama**: Video Indexer, konuşmaların transkripsiyonu ve görsel metin tanıma ile elde edilen metinlere göre anahtar sözcükleri ayıklar.
- **Etiketler**: Video Indexer kedi, köpek, masa, araba gibi görsel nesneler ve durma, koşma veya uçma gibi eylemler için etiketler sunar.
- **Markalar**: Video Indexer, konuşmaların transkripsiyonu ve görsel metin tanıma ile elde edilen metinlere göre markaları ayıklar.

Video Indexer işlemi ve analizi tamamladıktan sonra video içgörülerini gözden geçirebilir, düzenleyebilir, içerisinde arama yapabilir ve yayımlayabilirsiniz.

Video Indexer hizmeti, hem içerik yöneticilerinin hem de geliştiricilerin ihtiyaçlarını karşılayacak şekilde tasarlanmıştır. İçerik yöneticileri Video Indexer web portalını kullanarak tek bir kod satırı yazmadan hizmetten faydalanabilir, bkz. [Video Indexer portalını kullanmaya başlama](video-indexer-get-started.md). Geliştiriciler, büyük ölçekli verileri işlemek için API'leri kullanabilirler, bkz. [Video Indexer REST API'sini kullanma](video-indexer-use-apis.md). Hizmet ayrıca müşterilerin pencere öğelerini kullanarak video akışlarını ve ayıklanan içgörüleri kendi uygulamalarında yayımlamalarını sağlar, bkz. [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md).

Bu hizmete var olan AAD, LinkedIn, Facebook, Google veya MSA hesabınızla kaydolabilirsiniz. Daha fazla bilgi için [başlarken](video-indexer-get-started.md) bölümüne bakın.

## <a name="scenarios"></a>Senaryolar

Aşağıda Video Indexer hizmetinin yararlı olabileceği senaryolar verilmiştir

- Arama – Videodan ayıklanan içgörüler, bir video kitaplığındaki arama deneyimini geliştirmek için kullanılabilir. Örneğin konuşmaları ve yüzleri dizine eklemek, videoda belirli bir kişinin belirli bir sözcüğü söylediği veya iki kişinin bir arada göründüğü anları bulmayı sağlayacak arama deneyimini mümkün kılabilir. Videolardan elde edilen bu içgörülerle yapılan aramalar haber ajansları, eğitim kurumları, yayıncılar, eğlence içeriği sahipleri, kurumsal iş kolu uygulamaları ve kullanıcıların arama yapması gereken bir video kitaplığına sahip olan tüm sektörleri hedeflemektedir.

- Paraya dönüştürme – Video Indexer, videoların değerinin artırılmasına yardımcı olabilir. Örneğin reklam gelirine bağlı olan sektörler (haber, sosyal medya vb.), ayıklanan içgörüleri reklam sunucusuna ek sinyal olarak ekleyerek daha ilgili reklamların görüntülenmesini sağlayabilir (örneğin bir spor ayakkabısı reklamının, yüzme yarışması yerine futbol maçının ortasında gösterilmesi daha uygun olacaktır).

- Kullanıcı etkileşimi – Video içgörüleri, kullanıcılara ilgili video anlarının sunularak kullanıcı etkileşiminin geliştirilmesini sağlayabilir. Örneğin ilk 30 dakika boyunca kürelerin, ikinci 30 dakika boyunca da piramitlerin anlatıldığı bir video olduğunu düşünün. Videonun 30. dakikadan başlatılması, piramitlerle ilgili çalışma yapan bir öğrenci için daha yararlı olacaktır.

Daha fazla bilgi için bu [bloga](http://aka.ms/videoindexerblog) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Video Indexer hizmetini kullanmaya başlamaya hazırsınız. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Video Indexer portalını kullanmaya başlama](video-indexer-get-started.md)
- [Video Indexer REST API'si ile içerik işleme](video-indexer-use-apis.md)
- [Görsel pencere öğelerini uygulamanıza ekleme](video-indexer-embed-widgets.md)
