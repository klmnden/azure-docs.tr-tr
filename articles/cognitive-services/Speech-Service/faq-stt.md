---
title: Konuşma metin Azure hizmeti hakkında sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Konuşma metin hizmeti hakkında en yaygın soruların yanıtlarını alın.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 6cc530d2680c0410081ad3ad3e573cd59d5583d6
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341953"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Konuşmayı metne dönüştürme hakkında sık sorulan sorular

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, kullanıma [diğer destek seçenekleri](support.md).

## <a name="general"></a>Genel

**S: Metin modeli için bir temel modeli ve özel konuşma arasındaki fark nedir?**

**A**: Bir temel modeli Microsoft'a ait verileri kullanılarak geliştirilen ve bulut üzerinde zaten dağıtılmış.  Bir model belirli ortam gürültü veya dil olan belirli bir ortama daha iyi uyacak şekilde uyarlamak için özel bir model kullanabilirsiniz. Uyarlanmış bir akustik model yararlanarak, arabalarda veya gürültülü sokaklar gerektirir. Özel kısaltmalar uyarlanmış dil modeli gerektirecek ve konuları gibi biyolojisi, fizik, radyoloji, ürün adları.

**S: Temel model kullanmak istiyorsanız nereden başlamalıyım?**

**A**: İlk olarak, alın bir [abonelik anahtarı](get-started.md). REST çağrılarını predeployed temel modellere yapmak istiyorsanız, bkz. [REST API'leri](rest-apis.md). WebSockets, kullanmak istiyorsanız [SDK'sını indirin](speech-sdk.md).

**S: Her zaman bir özel konuşma modeli oluşturmak ihtiyacım var?**

**A**: Hayır. Uygulamanızı genel kullanıyorsa, gündelik dil modeli özelleştirme gerekmez. Uygulamanızı bir ortamda kullanılıyorsa, çok az kayıpla veya hiç arka plan gürültüsü olduğunda, bir model özelleştirme gerekmez.

Taban çizgisi ve portaldaki özelleştirilmiş modelleri dağıtabilir ve bunlara karşı doğruluk testi çalıştırın. Bu özellik, bir temel modeli yerine özel bir model doğruluğunu ölçmek için kullanabilirsiniz.

**S: Veri kümesi veya model için işleme tamamlandıktan sonra nasıl bilebilirim?**

**A**: Şu anda, model veya veri kümesi tablodaki durumunu öğrenmek için tek yoludur. İşleme tamamlandığında durumudur **başarılı**.

**S: Birden fazla model oluşturabilir miyim?**

**A**: Modelleri, koleksiyonunuzda olabilir, sayısına bir sınır yoktur.

**S: Ben bir hata yaptığım gerçekleşmiş. Veri içe aktarma işlemi iptal etmek veya nasıl devam eden oluşturma modeli?**

**A**: Şu anda bir akustik veya dil uyarlama işlemi geri alamazsınız. Bir terminal durumda olduklarında, içeri aktarılan veriler ve modelleri silebilirsiniz.

**S: Arama ve dikte modeli ve konuşma modeli arasındaki fark nedir?**

**A**: Konuşma hizmeti birden fazla temel modele arasından seçim yapabilirsiniz. Konuşma modeli, konuşma stilde konuşulan konuşma tanıma için kullanışlıdır. Bu model, telefon aramaları fotoğrafını için idealdir. Arama ve dikte modeli ses tetiklenen uygulamalar için idealdir. Her iki senaryolara amaçlar için yeni bir modeli Evrensel modelidir. Evrensel şu anda çoğu yerel damıtarak konuşma bağlamında kullanılabilen modelin kalite düzeyi veya üzerindeki modelidir.

**S: My var olan model (model yığın) güncelleştirebilirim?**

**A**: Mevcut bir model güncelleştirilemiyor. Bir çözüm olarak eski veri kümesinin yeni veri kümesi ile birleştirebilir ve readapt.

Eski veri kümesinin ve yeni veri kümesi, tek bir .zip dosyası (için akustik verilerini) veya bir .txt dosyasında (dil veri için) birleştirilmelidir. Yeni ve güncelleştirilmiş model uyarlama tamamlandığında, yeni bir uç noktası almak için dağıtılması gerekiyor

**S: Temel yeni bir sürümü kullanılabilir olduğunda otomatik olarak güncelleştirilen dağıtımım var mı?**

**A**: Dağıtımları otomatik olarak güncelleştirilmez.

Uyarlanmış ve temel V1.0 modeliyle dağıtılan, o dağıtım olduğu gibi kalır. Müşteriler dağıtılan model yetkisini, temel daha yeni sürümünü kullanarak yeniden uyum ve yeniden dağıtın.

**S: İhtiyacım olursa ne yapabilirim daha yüksek Eş zamanlılık Portalı'nda sunulan daha my dağıtılan modeli için?**

**A**: Modelinizi 20 eş zamanlı istek artışlarla ölçeklendirme yapabilir.

İlgili kişi [konuşma desteği](mailto:speechsupport@microsoft.com?subject=Request%20for%20higher%20concurrency%20for%20Speech-to-text) daha yüksek ölçeği ihtiyaç duyuyorsanız.

**S: Ben my modeli indirebilir ve yerel olarak çalıştırma?**

**A**: Modelleri indirilir ve yerel olarak yürütülür.

**S: İsteklerim günlüğe kaydediliyor?**

**A**: İzlemeyi devre dışı geçiş yapmak için bir dağıtım oluşturduğunuzda bir tercih yapabilirsiniz. Bu noktada, hiçbir ses ve dökümler günlüğe kaydedilir. Aksi takdirde, istekleri Azure'da genellikle güvenli depolama veritabanında kaydedilir.

**S: İsteklerim kısıtlanan?**

**A**: REST API istekleri 5 saniye başına 25 sınırlar. Ayrıntılar için sayfalarımızın bulunabilir [Konuşmayı metne dönüştürme](speech-to-text.md).

Daha fazla özel konuşma hizmeti kullanmaları yasaktır Gizlilik sorunları varsa, destek kanallarını başvurun.

## <a name="importing-data"></a>Verileri içeri aktarma

**S: Bir veri kümesi ve sınır neden olduğu boyutu sınırı nedir?**

**A**: Bir veri kümesi için geçerli sınırı 2 GB'dir. HTTP yüklemek için bir dosya boyutu kısıtlaması nedeniyle sınırlıdır. 

**S: Metin dosyalarımı, daha büyük bir metin dosyası karşıya yükleyebilir, böylece zip?** 

**A**: Hayır. Şu anda, yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**S: Veri raporu başarısız konuşma vardı diyor. Sorun nedir?**

**A**: Bir dosyada konuşma yüzde 100 karşıya yüklerken bir sorun değildir. Konuşma akustik veya dil bir veri kümesinde (için örnek, birden fazla yüzde 95 ') büyük çoğunluğu başarıyla içeri aktarıldı, veri kümesi kullanışlı olabilir. Ancak, konuşma başarısız olmasının anlamanıza ve sorunları gidermek aşağıdakileri öneririz. Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-an-acoustic-model"></a>Akustik model oluşturma

**S: Akustik elinizdeki verilere yüklemem gerekir?**

**A**: Akustik verilerin bir saat 30 dakika arasında ile başlamanızı öneririz.

**S: Hangi verilerin miyim toplamanız gerekir?**

**A**: Uygulama senaryosu gibi yakın olan verileri toplayın ve çalışması mümkün olduğunca kullanın. Veri toplama, hedef uygulamanın ve kullanıcıların cihaz veya cihazları, ortamlar ve konuşmacıları tür bakımından eşleşmelidir. Genel olarak, mümkün olduğunca konuşmacıları bir dizi geniş kapsamlı olarak verileri toplamanız gerekir. 

**S: Akustik verilerini nasıl toplamanız gerekir?**

**A**: Bir tek başına veri koleksiyonu uygulaması oluşturabilir veya hazır ses kayıt yazılımı kullanın. Ses verilerini günlüğe kaydeder ve sonra verileri kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**S: Kendim uyarlama veri konuşmaların gerekiyor mu?**

**A**: Evet! Kendiniz özelliği ya da professional transkripsiyonu hizmet kullanın. Bazı kullanıcılar profesyonel transcribers tercih eder ve diğerleri kitle kaynak kullanın ya da döküm yapın.

## <a name="accuracy-testing"></a>Doğruluk testi

**S: Çevrimdışı my özel akustik modelini, test bir özel dil modeli kullanarak gerçekleştirebilir miyim?**

**A**: Çevrimdışı test ayarladığınızda Evet, özel dil modeli açılan menüde seçmeniz yeterlidir.

**S: Çevrimdışı my özel dil modelini, test bir özel akustik model kullanarak gerçekleştirebilir miyim?**

**A**: Çevrimdışı test ayarladığınızda Evet, özel akustik model açılan menüde seçmeniz yeterlidir.

**S: Word hatası oranı (WER) nedir ve nasıl hesaplanır?**

**A**: WER'i konuşma tanıma için değerlendirme unsurdur. WER'i eklemeler, silme ve değişimler, bölü başvuru transkripsiyonu sözcükleri toplam sayısı içeren hatalarının toplam sayısını, kabul edilir. Daha fazla bilgi için [word hata oranı](https://en.wikipedia.org/wiki/Word_error_rate).

**S: Bir doğruluk testi sonuçlarını iyi olup olmadığını nasıl belirlerim?**

**A**: Sonuçları temel modeli ile özelleştirdiğiniz modeli arasında bir karşılaştırma gösterir. Özelleştirme faydalı hale getirmek için temel model beat için indirmeyi amaçlamanız gerekir.

**S: Bir geliştirme olup olmadığını görebilmeniz için temel bir modelin WER'e nasıl belirlerim?** 

**A**: Çevrimdışı test sonuçları taban çizgisi özel model ve geliştirme temel doğruluğunu gösterir.

## <a name="creating-a-language-model"></a>Dil modeli oluşturma

**S: Karşıya yüklemek metin veri miktarını gerekiyor?**

**A**: Üzerinde nasıl farklı sözlük bağlıdır ve uygulamanızda kullanılan ifadeler tarafından sağlanan başlangıç dil modellerini. Tüm yeni sözcükler için bu bir kelimelerin kullanımı mümkün olduğunca çok örnekleri sağlamak kullanışlıdır. Ayrıca bu koşulları dinlemek için sisteme söyler uygulamanızda kullanılan genel ifadeleri için ifadeleri dili verileri de dahil olmak üzere de kullanışlıdır. En az 100 ve yüzlerce ya da daha fazla genellikle birkaç konuşma, dil kümesinde çok yaygındır. Ayrıca, bazı sorgu türleri diğerlerinden daha sık olması bekleniyorsa, veri kümesinde birden çok kopyasını ortak sorgular ekleyebilirsiniz.

**S: Yalnızca de sözcüklerin listesini yükleyebilir miyim?**

**A**: Sözcüklerin listesini karşıya sözcükleri sözlüğü ekler, ancak sistem nasıl sözcükler genellikle kullanılan öğretin olmaz. Dil modeli tam veya kısmi konuşma (cümleler ya da tümcelere kullanıcılar söyleyin olasılığı yüksek olan şeyleri) sağlayarak yeni sözcükler ve nasıl kullanıldıkları öğrenebilirsiniz. Özel dil modeli, yalnızca yeni sözcükleri sisteme ekleme ancak uygulamanız için bilinen bir kelimelerin olasılığını ayarlamak için uygundur. Tam konuşma sağlayarak daha iyi bilgi sistemi yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
