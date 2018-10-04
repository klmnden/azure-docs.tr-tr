---
title: Konuşma metin Azure hizmeti hakkında sık sorulan sorular
description: Konuşma metin hizmeti hakkında en yaygın soruların yanıtlarını alın.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: d176c33a37b26b1e13d5b9beb7ac68d335cc7862
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249205"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Konuşmayı metne dönüştürme hakkında sık sorulan sorular

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, kullanıma [diğer destek seçenekleri](support.md).

## <a name="general"></a>Genel

**S: metin modeli için bir temel modeli ve özel konuşma arasındaki fark nedir?**

**A**: bir temel modeli Microsoft'a ait verileri kullanılarak geliştirilen ve bulut üzerinde zaten dağıtılmış.  Bir model belirli ortam gürültü veya dil olan belirli bir ortama daha iyi uyacak şekilde uyarlamak için özel bir model kullanabilirsiniz. Uyarlanmış bir akustik model yararlanarak, arabalarda veya gürültülü sokaklar gerektirir. Özel kısaltmalar uyarlanmış dil modeli gerektirecek ve konuları gibi biyolojisi, fizik, radyoloji, ürün adları.

**S: temel model kullanmak istiyorsanız nereden başlamalıyım?**

**A**: ilk olarak, alın bir [abonelik anahtarı](get-started.md). REST çağrılarını predeployed temel modellere yapmak istiyorsanız, bkz. [REST API'leri](rest-apis.md). WebSockets, kullanmak istiyorsanız [SDK'sını indirin](speech-sdk.md).

**S: her zaman bir özel konuşma modeli oluşturmak ihtiyacım?**

**A**: Hayır Uygulamanızı genel kullanıyorsa, gündelik dil modeli özelleştirme gerekmez. Uygulamanızı bir ortamda kullanılıyorsa, çok az kayıpla veya hiç arka plan gürültüsü olduğunda, bir model özelleştirme gerekmez. 

Taban çizgisi ve portaldaki özelleştirilmiş modelleri dağıtabilir ve bunlara karşı doğruluk testi çalıştırın. Bu özellik, bir temel modeli yerine özel bir model doğruluğunu ölçmek için kullanabilirsiniz.

**S: veri kümesi veya model için işlem tamamlandığında nasıl bilebilirim?**

**A**: şu anda, model veya veri kümesi tablodaki durumunu öğrenmek için tek yoludur. İşleme tamamlandığında durumudur **başarılı**.

**Birden fazla model oluşturabilirim miyim?**

**A**: sahip olabilir, koleksiyonunuzda modelleri sayısına bir sınır yoktur.

**S: ben bir hata yaptığınızı gerçekleşmiş. Veri içe aktarma işlemi iptal etmek veya nasıl devam eden oluşturma modeli?**

**A**: şu anda bir akustik veya dil uyarlama işlemi geri alamazsınız. Bir terminal durumda olduklarında, içeri aktarılan veriler ve modelleri silebilirsiniz.

**S: arama ve dikte modeli ve konuşma modeli arasındaki fark nedir?**

**A**: konuşma hizmeti birden fazla temel modele arasından seçim yapabilirsiniz. Konuşma modeli, konuşma stilde konuşulan konuşma tanıma için kullanışlıdır. Bu model, telefon aramaları fotoğrafını için idealdir. Arama ve dikte modeli ses tetiklenen uygulamalar için idealdir. Her iki senaryolara amaçlar için yeni bir modeli Evrensel modelidir.

**My var olan model (model yığın) güncelleştirebilirim miyim?**

**A**: mevcut bir model güncelleştirilemiyor. Bir çözüm olarak eski veri kümesinin yeni veri kümesi ile birleştirebilir ve readapt.

Eski veri kümesinin ve yeni veri kümesi, tek bir .zip dosyası (için akustik verilerini) veya bir .txt dosyasında (dil veri için) birleştirilmelidir. Yeni ve güncelleştirilmiş model uyarlama tamamlandığında, yeni bir uç noktası almak için dağıtılması gerekiyor

**Temel yeni bir sürümü kullanılabilir olduğunda s: dağıtımım otomatik olarak güncelleştirilir mi?**

**A**: dağıtımları otomatik olarak güncelleştirilmeyecek. 

Uyarlanmış ve temel V1.0 modeliyle dağıtılan, o dağıtım olduğu gibi kalır. Müşteriler decommision dağıtılan model taban daha yeni sürümünü kullanarak yeniden uyum ve yeniden dağıtın.

**S: ihtiyacım olursa ne yapabilirim daha yüksek Eş zamanlılık Portalı'nda sunulan daha my dağıtılan modeli için?** 

**A**: 20 eş zamanlı istek artışlarla modelinizde ölçekleme yapabilirsiniz. 

Daha yüksek ölçek ihtiyacınız varsa bizimle iletişim kurun.

**S: Ben my modeli indirebilir ve yerel olarak çalıştırma?**

**A**: modelleri indirilir ve yerel olarak yürütülür.

**S: oturum isteklerim misiniz?**

**A**: izlemeyi devre dışı geçiş yapmak için bir dağıtım oluşturduğunuzda vardır. Bu noktada, hiçbir ses ve dökümler günlüğe kaydedilir. Aksi takdirde, istekleri Azure'da genellikle güvenli depolama veritabanında kaydedilir. 

**S: kısıtlanan isteklerim misiniz?**

**A**: REST API istekleri 5 saniye başına 25 sınırlar. Ayrıntılar için sayfalarımızın bulunabilir [Konuşmayı metne dönüştürme](speech-to-text.md). 

Daha fazla özel konuşma hizmeti kullanmaları yasaktır Gizlilik sorunları varsa, destek kanallarını başvurun.

## <a name="importing-data"></a>Verileri içeri aktarma

**S: bir veri kümesi ve sınır neden olduğu boyutu sınırı nedir?**

**A**: bir veri kümesi için geçerli sınırı 2 GB'dir. HTTP yüklemek için bir dosya boyutu kısıtlaması nedeniyle sınırlıdır. 

**Metin dosyalarımı, daha büyük bir metin dosyası karşıya yükleyebilir, böylece zip miyim?** 

**A**: Hayır Şu anda, yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**S: veri raporu başarısız konuşma vardı diyor. Sorun nedir?**

**A**: sesleri dosyasındaki yüzde 100 karşıya yüklerken sorun olmaz. Konuşma akustik veya dil bir veri kümesinde (için örnek, birden fazla yüzde 95 ') büyük çoğunluğu başarıyla içeri aktarıldı, veri kümesi kullanışlı olabilir. Ancak, konuşma başarısız olmasının anlamanıza ve sorunları gidermek aşağıdakileri öneririz. Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-an-acoustic-model"></a>Akustik model oluşturma

**S: ne kadar akustik verilerini ihtiyacım var?**

**A**: 30 dakika arasında bir saat akustik veri ile başlamanızı öneririz.

**S: hangi veri miyim toplamanız gerekir?**

**A**: uygulama senaryosuna gibi yakın olan veri toplama ve kullanım örneği mümkün olduğunca. Veri toplama, hedef uygulamanın ve kullanıcıların cihaz veya cihazları, ortamlar ve konuşmacıları tür bakımından eşleşmelidir. Genel olarak, mümkün olduğunca konuşmacıları bir dizi geniş kapsamlı olarak verileri toplamanız gerekir. 

**S: nasıl akustik verilerini toplamanız gerekir?**

**A**: bağımsız veri koleksiyonu uygulama oluşturma veya hazır ses kayıt yazılımı kullanın. Ses verilerini günlüğe kaydeder ve sonra verileri kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**Kendim uyarlama veri konuşmaların gerekiyor s:?**

**A**: Evet! Kendiniz özelliği ya da professional transkripsiyonu hizmet kullanın. Bazı kullanıcılar profesyonel transcribers tercih eder ve diğerleri kitle kaynak kullanın ya da döküm yapın.

## <a name="accuracy-testing"></a>Doğruluk testi

**Çevrimdışı my özel akustik modelini, özel dil modeli kullanarak test gerçekleştirmek miyim?**

**A**: Evet, çevrimdışı test ayarladığınızda özel dil modeli açılan menüde seçmeniz yeterlidir.

**Çevrimdışı my özel dil modelini, bir özel akustik model kullanarak test gerçekleştirmek miyim?**

**A**: Evet, çevrimdışı test ayarladığınızda özel akustik model açılan menüde seçmeniz yeterlidir.

**S: word hatası oranı (WER) nedir ve nasıl hesaplanır?**

**A**: WER olan değerlendirme ölçüm konuşma tanıma. WER'i eklemeler, silme ve değişimler, bölü başvuru transkripsiyonu sözcükleri toplam sayısı içeren hatalarının toplam sayısını, kabul edilir. Daha fazla bilgi için [word hata oranı](https://en.wikipedia.org/wiki/Word_error_rate).

**S: bir doğruluk testi sonuçlarını iyi olup olmadığını nasıl belirlerim?**

**A**: temel modeli ve özelleştirdiğiniz modeli arasında bir karşılaştırma sonuçlarını göster. Özelleştirme faydalı hale getirmek için temel model beat için indirmeyi amaçlamanız gerekir.

**S: bir geliştirme olup olmadığını görebilmeniz için temel bir modelin WER nasıl belirlerim?** 

**A**: Çevrimdışı test sonuçlarını temel özel model ve geliştirme temel doğruluğunu göster.

## <a name="creating-a-language-model"></a>Dil modeli oluşturma

**S: ne kadar metin verileri karşıya yüklemek ihtiyacım var?**

**A**: üzerinde nasıl farklı sözlük bağlıdır ve uygulamanızda kullanılan ifadeler tarafından sağlanan başlangıç dil modellerini. Tüm yeni sözcükler için bu bir kelimelerin kullanımı mümkün olduğunca çok örnekleri sağlamak kullanışlıdır. Ayrıca bu koşulları dinlemek için sisteme söyler uygulamanızda kullanılan genel ifadeleri için ifadeleri dili verileri de dahil olmak üzere de kullanışlıdır. En az 100 ve yüzlerce ya da daha fazla genellikle birkaç konuşma, dil kümesinde çok yaygındır. Ayrıca, bazı sorgu türleri diğerlerinden daha sık olması bekleniyorsa, veri kümesinde birden çok kopyasını ortak sorgular ekleyebilirsiniz.

**Yalnızca de sözcüklerin listesini yükleyebilir miyim?**

**A**: sözcüklerin listesini karşıya ekleyecek sözcükleri sözlüğü, ancak sistem nasıl sözcükler genellikle kullanılan öğretin olmaz. Dil modeli tam veya kısmi konuşma (cümleler ya da tümcelere kullanıcılar söyleyin olasılığı yüksek olan şeyleri) sağlayarak yeni sözcükler ve nasıl kullanıldıkları öğrenebilirsiniz. Özel dil modeli, yalnızca yeni sözcükleri sisteme ekleme ancak uygulamanız için bilinen bir kelimelerin olasılığını ayarlamak için uygundur. Tam konuşma sağlayarak daha iyi bilgi sistemi yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
