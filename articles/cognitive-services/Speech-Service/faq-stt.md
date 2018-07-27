---
title: Azure üzerinde sık sorulan sorular için konuşma tanıma Hizmeti'ne metin
description: Konuşmayı metne dönüştürme ile ilgili en yaygın soruların yanıtları aşağıdadır.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: e5ba01c25646578da22f054659051be3515e9e4b
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281839"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Konuşmayı metne dönüştürme hakkında sık sorulan sorular

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, diğer destek seçenekleri denetleyin [burada](support.md).

## <a name="general"></a>Genel

**Soru**: metin modelleri için temel ve özel konuşma arasındaki fark nedir?

**Yanıt**: temel modelleri ile Microsoft Veri ait eğitilen ve bulut üzerinde zaten dağıtılmış. Özel modelleri, kullanıcının belirli bir ortam belirli ortam gürültü ya da dil daha iyi uyum sağlamak için bir model uyarlama izin verin. Yararlanarak, otomobiller, gürültülü sokaklar biyolojisi, fizik radyoloji, ürün adları ve özel kısaltmalar gibi belirli konular uyarlanmış dil modeli gerekir ancak uyarlanmış akustik model gerektirir.

**Soru**: nereden başlamalıyım temel model kullanmak isterseniz?

**Yanıt**: ilk almanız gereken bir [abonelik anahtarı](get-started.md). REST çağrılarını predeployed temel modellere yapmak istiyorsanız, başvurun [burada ayrıntıları](rest-apis.md). WebSockets indirme kullanmak istiyorsanız [SDK'sı](speech-sdk.md)

**Soru**: yaparım her zaman gerekecektir özel konuşma modeli oluşturmak?

**Yanıt**: Hayır, bir model özelleştirme gerekmez, uygulamanızın genel günlük dil kullanıyorsa. Ayrıca, uygulamanızın nerede çok az kayıpla veya hiç arka plan gürültüsü yoktur sonra ya da özelleştirme gerekmez bir ortamda kullanılıyorsa. Portal, kullanıcıların temel ve özelleştirilmiş modelleri dağıtma ve doğruluk testleri bu ortamlarda çalıştırmak olanak tanır. Kullanıcılar, bu özellik, özel bir model temel vs doğruluğunu ölçmek için kullanabilir.

**Soru**: olduğunu nasıl öğrenebilirim my veri kümesi veya model işlenmesi tamamlandığında?

**Yanıt**: şu anda modeli veya tabloya veri kümesinde durumunu öğrenmek için tek yoludur.
İşleme tamamlandığında, durum "başarılı olması".

**Soru**: birden fazla model oluşturabilir miyim?

**Yanıt**: kaç modelleri, koleksiyonda yer alan bir sınır yoktur.

**Soru**: Ben hata yaptığım gerçekleşmiş. Veri içe aktarma işlemi iptal etmek veya nasıl devam eden oluşturma modeli? 

**Yanıt**: şu anda bir akustik veya dil uyarlama işlemi geri alamazsınız. Bir terminal durumunda sonra içeri aktarılan veriler ve modelleri silinebilir.

**Soru**: arama & dikte modelleri ve konuşma modelleri arasındaki fark nedir?

**Yanıt**: birden fazla konuşma hizmeti seçilecek temel modeli vardır. Konuşma modeli, konuşma bir stile damıtarak konuşma bağlamında kullanılabilen konuşulan tanımayı için uygundur. Bu model arama aramaları fotoğrafını için uygun olan ve dikte sesle tetiklenen uygulamalar için idealdir. Evrensel hem senaryolara amaçlar yeni bir modeldir.

**Soru**: benim mevcut model (model yığınlama) güncelleştirebilirim?

**Yanıt**: var olan model güncelleştirilemiyor. Geçici bir çözüm olarak eski veri kümesinin yeni ile birleştirebilir ve readapt.

Dil veriler ise eski ve yeni veri kümeleri (Akustik verilerini ise) tek bir .zip veya bir .txt dosyasına birleştirilmelidir. Yeni bir uç noktası almak için XML'deki dağıtılmış olması için yeni güncelleştirilmiş model gereksinimlerine uyarlama bir kez gerçekleştirilir

**Soru**: ihtiyacım olursa ne yapabilirim daha yüksek Eş zamanlılık Portalı'nda sunulan daha my dağıtılan modeli. 

**Yanıt**: 20 eş zamanlı istek artışlarla modelinizde ölçekleme yapabilirsiniz. 

Daha yüksek ölçek ihtiyacınız varsa bizimle iletişim kurun.

**Soru**: Ben my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: modelleri indirilir ve yerel olarak yürütülür.

**Soru**: isteklerim oturum açmış olan?

**Yanıt**: izleme devre dışı, bu noktada hiçbir ses geçiş yapmak için bir dağıtımın oluşturulması sırasında vardır veya döküm kaydedilir. Aksi takdirde isteği Azure'da genellikle güvenli depolama veritabanında kaydedilir. Daha fazla özel konuşma hizmeti kullanmaları yasaktır Gizlilik sorunları varsa, destek kanallarını başvurun.

## <a name="importing-data"></a>Veri alma

**Soru**: veri kümesi boyutu sınırı nedir? Neden? 

**Yanıt**: geçerli bir veri kümesi için 2 GB, HTTP yüklemek için bir dosya boyutu kısıtlaması nedeniyle sınırlıdır. 

**Soru**: metin dosyalarımı miyim daha büyük bir metin dosyasını karşıya yükleme için zip? 

**Yanıt**: Hayır, şu anda yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**Soru**: başarısız konuşma vardı veri raporu söyler. Sorun nedir?

**Yanıt**: sesleri dosyasındaki %100 karşıya yüklerken sorun olmaz.
Konuşma akustik veya dil verilerde büyük çoğunluğu ayarlarsanız (örneğin, > % 95) başarıyla içeri aktarıldı, veri kümesi kullanışlı olabilir. Ancak, konuşma başarısız olmasının anlamanıza ve sorunları düzeltmek deneyin önerilir. Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-am"></a>AM oluşturma

**Soru**: akustik ne kadar veri yapmam gerekir mi?

**Yanıt**: akustik verilerin bir saat 30 dakika ile başlamanızı öneririz.

**Soru**: Ben hangi verileri toplamanız gerekir?

**Yanıt**: uygulama senaryosuna gibi yakın olan veri toplama ve kullanım örneği mümkün olduğunca.
Veri toplama, hedef uygulamanın ve kullanıcıların cihaz veya cihazları, ortamlar ve konuşmacıları tür bakımından eşleşmelidir. Genel olarak, mümkün olduğunca konuşmacıları bir dizi geniş kapsamlı olarak verileri toplamanız gerekir. 

**Soru**: nasıl miyim toplamak? 

**Yanıt**: bağımsız veri koleksiyonu uygulama oluşturma veya bazı raf ses kaydı yazılım kapalı kullanın.
Ses verilerini günlüğe kaydeder ve kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**Soru**: uyarlama veri kendim özelliği gerekiyor mu? 

**Yanıt**: Evet! Kendiniz özelliği ya da professional transkripsiyonu hizmet kullanın. Bazı kullanıcılar, diğerleri kitle kaynak kullanın ya da döküm yapmak profesyonel transcribers tercih eder.

## <a name="accuracy-testing"></a>Doğruluk testi

**Soru**: Çevrimdışı bir özel dil modeli kullanarak kendi özel akustik modelini, test yapabilir miyim?

**Yanıt**: Evet, çevrimdışı test ayarladığınızda açılan menü yalnızca özel dil modeli seçin

**Soru**: Çevrimdışı bir özel akustik model kullanarak kendi özel dil modelini, test yapabilir miyim?

**Yanıt**: Evet, çevrimdışı test ayarladığınızda özel akustik model açılan menüde seçmeniz yeterlidir.

**Soru**: sözcük hata oranı (WER) nedir ve nasıl bunu hesaplanır?

**Yanıt**: sözcük hata oranı (WER) olan değerlendirme ölçüm konuşma tanıma. Eklemeler, silme ve değişimler, bölü başvuru transkripsiyonu sözcükleri toplam sayısı içeren hatalarının toplam sayısını, olarak sayılır. Diğer ayrıntıları [burada](https://en.wikipedia.org/wiki/Word_error_rate) bulabilirsiniz.

**Soru**: bir doğruluk testi sonuçlarını iyi olup olmadığını nasıl belirleyebilirim?

**Yanıt**: temel model ve, özelleştirilmiş bir arasında bir karşılaştırma sonuçlarını göster.
Özelleştirmenin faydalı hale getirmek için temel model beat için indirmeyi amaçlamanız gerekir.

**Soru**: nasıl miyim şekil temel modelleri Word hata oranı geliştirme olup olmadığını görebilmeniz için? 

**Yanıt**: Çevrimdışı test sonuçlarını temel doğruluğunu özel model ve geliştirme doğruluğunu taban çizgisi göster.

## <a name="creating-lm"></a>LM oluşturma

**Soru**: metin veri miktarını karşıya yükleme için değiştirmem gerekiyor mu?

**Yanıt**: üzerinde nasıl farklı sözlük bağlıdır ve uygulamanızda kullanılan ifadeler tarafından sağlanan başlangıç dil modellerini. Tüm yeni sözcükler için bu bir kelimelerin kullanımı mümkün olduğunca çok örnekleri sağlamak kullanışlıdır. Bu terimler için dinlemek üzere sisteme söyler gibi uygulamanızda kullanılan ortak tümcecikleri tümcecikleri dil verileri de dahil olmak üzere de yararlıdır. En az yüz ve genellikle birkaç yüz konuşma dilini veri kümesi veya daha çok daha yaygındır. Ayrıca belirli sorgu türleri diğerlerinden daha sık olması bekleniyorsa, veri kümesinde birden çok kopyasını ortak sorgular ekleyebilirsiniz.

**Soru**: yalnızca sözcüklerin listesi karşıya yükleyebilirsiniz?

**Yanıt**: sözcüklerin listesini karşıya sözcüklere sözlük için alma ancak sistem nasıl sözcükler genellikle kullanılan öğretmek değildir.
Nasıl kullanılır ve tam veya kısmi konuşma (cümleler ya da tümcelere nesnelerin interneti kullanıcılar söyleyin doğan) sağlayarak dil modeli yeni sözcükleri edinebilirsiniz. Özel dil modeli sistemin yeni sözcükleri almak için yalnızca aynı zamanda uygulamanız için bilinen bir kelimelerin olasılığını ayarlamak için uygundur. Tam konuşma sağlayarak daha iyi bilgi sistemi yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
