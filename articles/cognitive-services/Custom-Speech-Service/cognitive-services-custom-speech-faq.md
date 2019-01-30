---
title: Azure üzerinde sık sorulan sorular için özel konuşma hizmeti | Microsoft Docs
description: Özel konuşma hizmeti ile ilgili en yaygın soruların yanıtları aşağıdadır.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: 88486ec9d1ca11d25ca31ca0abb4a34509d19a27
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228045"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Özel konuşma hizmeti hakkında sık sorulan sorular

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)] 

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, üzerinde özel konuşma hizmeti Topluluğu'na sorun deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ve [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Genel

**Soru**: Kendi veri kümesi veya model işlenmesi tamamlandığında nasıl bilebilirim?

**Yanıt**: Şu anda modeli veya tabloya veri kümesinde durumunu öğrenmek için yalnızca istediğiniz ' dir.
İşleme tamamlandığında, durum "Hazır" olacaktır.
İletişim e-posta bildirimi gibi bir durum işleme için gelişmiş yöntemleri üzerinde çalışıyoruz.

**Soru**: Aynı anda birden fazla model oluşturabilir miyim?

**Yanıt**: Kaç tane modelleri, koleksiyonda yer alan bir sınır yoktur ancak tek bir zaman her sayfada oluşturulabilir.
Örneğin, varsa şu anda bir dil modeli işlem aşamasında bir dil modeli oluşturma işlemi başlatılamıyor.
Ancak, bir akustik model ve aynı anda işlenirken bir dil modeli olabilir. 

**Soru**: Ben bir hata yaptığım gerçekleşmiş. Veri içe aktarma işlemi iptal etmek veya nasıl devam eden oluşturma modeli? 

**Yanıt**: Şu anda bir akustik veya dil uyarlama işlemi geri alamazsınız.
İçeri aktarma tamamlandıktan sonra içeri aktarılan veriler silinebilir

**Soru**: Arama & dikte modelleri ve konuşma modelleri arasındaki fark nedir?

**Yanıt**: İki temel akustik ve dil modellerini özel konuşma hizmeti seçilecek vardır.
arama sorguları veya dikte. Microsoft Conversational AM, konuşma bir stile damıtarak konuşma bağlamında kullanılabilen konuşulan tanımayı için uygundur.
Bu tür bir konuşma genellikle başka bir kişinin, gibi çağrı merkezi veya toplantı yönlendirilir.

**Soru**: My var olan model (model yığın) güncelleştirebilirim?

**Yanıt**: Yeni veriler ile mevcut bir modeli güncelleştirme olanağı sunmuyoruz.
Yeni bir veri kümesine sahip olursunuz ve mevcut modelini özelleştirmek istiyorsanız, bunu yeni veriler ile kullanılan eski veri kümesini yeniden uyarlamanız gerekir.
(Akustik verilerini ise) eski ve yeni veri kümeleri tek bir .zip birleştirilmesini veya dil veri kez uyarlama ise bir .txt dosyasına yeni bir uç noktası almak için XML'deki dağıtılmış olması yeni güncelleştirilmiş model gereken gerçekleştirilir

**Soru**: İhtiyacım olursa ne yapabilirim varsayılan değerinden daha yüksek Eş zamanlılık. 

**Yanıt**: Modelinizi ölçek birimleri diyoruz 5 eşzamanlı istek artışlarla ölçeklendirme yapabilir. Her ölçek birimi, modelinizi 5 ses akışı aynı anda işleyebilir garanti eder. 100 ölçek birimi (veya 500 eş zamanlı istek) satın alabilirsiniz.

Daha yüksek ihtiyacınız varsa lütfen bizimle iletişime geçin.

**Soru**: Ben my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: İndirilebilen ve yerel olarak yürütüldüğü modellerini etkinleştirmeyin.

**Soru**: İsteklerim günlüğe kaydediliyor?

**Yanıt**: Bu noktada, hiçbir ses ve dökümler günlüğe kaydedilir, izlemeyi devre dışı geçiş yapmak için bir dağıtımın oluşturulması sırasında seçeneğiniz vardır. Aksi takdirde isteği Azure'da genellikle güvenli depolama veritabanında kaydedilir. Bizimle iletişime geçin, lütfen özel konuşma hizmeti kullanarak engelle Gizlilik sorunları başka varsa.

## <a name="importing-data"></a>Veri alma

**Soru**: Veri kümesi boyutu sınırı nedir? Neden? 

**Yanıt**: Bir veri kümesi için geçerli sınır 2 GB, boyutu HTTP için kısıtlama dosyasının nedeniyle karşıya yükleyin. 

**Soru**: Metin dosyalarımı miyim daha büyük bir metin dosyasını karşıya yükleme için zip? 

**Yanıt**: Hayır, şu anda yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**Soru**: Veri raporu başarısız konuşma vardı diyor. Bu, bir sorunla mı?

**Yanıt**: Bu bir sorun değil, yalnızca birkaç konuşma başarıyla içeri aktarılacak başarısız oldu.
Konuşma akustik veya dil verilerde büyük çoğunluğu ayarlarsanız (örneğin > % 95) başarıyla içeri aktarıldı, veri kümesi kullanışlı olabilir. Ancak, konuşma başarısız olmasının anlamanıza ve sorunları düzeltmek deneyin önerilir.
Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-am"></a>AM oluşturma

**Soru**: Akustik elinizdeki verilere yüklemem gerekir?

**Yanıt**: Akustik verilerin bir saat 30 dakika ile başlamanızı öneririz

**Soru**: Ne tür bir veri miyim toplamanız gerekir?

**Yanıt**: Uygulama senaryosu gibi yakın veri toplamak ve mümkün olduğunca kullanım örneği gerekir.
Başka bir deyişle, veri toplama, cihaz veya cihazları, ortamlar ve konuşmacıları türleri açısından kullanıcılar ve hedef uygulama eşleşmesi gerekir. Genel olarak, mümkün olduğunca konuşmacıları bir dizi geniş kapsamlı olarak verileri toplamanız gerekir. 

**Soru**: Bunu nasıl toplamanız gerekir? 

**Yanıt**: Bir tek başına veri koleksiyonu uygulaması oluşturabilir veya bazı raf ses kaydı yazılım kapalı kullanın.
Ses verilerini günlüğe kaydeder ve kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**Soru**: Kendim uyarlama veri konuşmaların gerekiyor mu? 

**Yanıt**: Veri transcribed gerekir. Kendiniz özelliği ya da professional transkripsiyonu hizmet kullanın. Bu kullanım profesyonel transcribers ve diğer bazı, kitle kaynak kullanır. Ayrıca bir döküm hizmet istek üzerine öneririz.

**Soru**: Ne kadar bir özel akustik model oluşturmak için sürer?

**Yanıt**: Bir özel akustik model oluşturmak için işleme süresi, uzunluğu akustik veri kümesi ile aynı hakkındadır.
Bu nedenle, beş saat veri kümesinden oluşturulan özelleştirilmiş bir akustik model yaklaşık beş saat sürer işlenecek. 

## <a name="offline-testing"></a>Çevrimdışı Test

**Soru**: Çevrimdışı bir özel dil modeli kullanarak kendi özel akustik modelini, test gerçekleştirebilir miyim?

**Yanıt**: Evet, özel dil modeli çevrimdışı test ayarlandığında aşağı açılan seçmeniz yeterlidir

**Soru**: Çevrimdışı bir özel akustik model kullanarak kendi özel dil modelini, test gerçekleştirebilir miyim?

**Yanıt**: Çevrimdışı test ayarladığınızda Evet, özel akustik model açılan menüde seçmeniz yeterlidir.

**Soru**: Word hata oranı nedir ve nasıl hesaplanır?

**Yanıt**: Word hata oranı, konuşma tanıma için değerlendirme unsurdur. Eklemeler, silme ve değişimler, bölü başvuru transkripsiyonu sözcükleri toplam sayısı içeren hatalarının toplam sayısını, olarak sayılır.

**Soru**: Ben artık benim özel modeli test sonuçlarını bilmeniz, iyi veya kötü birkaç budur?

**Yanıt**: Sonuçları, temel modeli ve, özelleştirilmiş bir arasında bir karşılaştırma gösterir.
Özelleştirmenin faydalı hale getirmek için temel model beat hedeflemeyi

**Soru**: Geliştirme olup olmadığını görebilmeniz için nasıl temel modellerin WER Şekil? 

**Yanıt**: Çevrimdışı test sonuçlarını temel doğruluğunu özel model ve geliştirme doğruluğunu taban çizgisi Göster

## <a name="creating-lm"></a>LM oluşturma

**Soru**: Karşıya yüklemek metin veri miktarını gerekiyor?

**Yanıt**: Bu zor soru kesin bir yanıt vermek için sağlanan başlangıç dil modellerini nasıl farklı sözlük ve uygulamanızda kullanılan ifadeleri bağımlı olduğundan. Tüm yeni sözcükler için bu bir kelimelerin kullanımı mümkün olduğunca çok örnekleri sağlamak kullanışlıdır. Bu terimler için dinlemek üzere sisteme söyler gibi uygulamanızda kullanılan genel ifadeleri için dil verileri de dahil olmak üzere de yararlıdır.
En az yüz ve genellikle birkaç yüz konuşma dilini veri kümesi veya daha çok daha yaygındır.
Ayrıca, bazı türleri *, sorguları diğerlerinden daha sık olması bekleniyor, veri kümesinde birden çok kopyasını ortak sorgular ekleyebilirsiniz.

**Soru**: Yalnızca de sözcüklerin listesini yükleyebilir miyim?

**Yanıt**: Sözcüklerin listesini karşıya sözlük içine sözcükleri Al ancak sistem nasıl sözcükler genellikle kullanılan öğretmek değildir.
Dil modeli tam veya kısmi konuşma (cümleler ya da tümcelere nesnelerin interneti kullanıcılar söyleyin doğan) sağlayarak yeni sözcükler ve nasıl kullanıldıkları öğrenebilirsiniz. Özel dil modeli sistemin yeni sözcükleri almak için yalnızca aynı zamanda uygulamanız için bilinen bir kelimelerin olasılığını ayarlamak için uygundur. Tam bir konuşma sağlayan, sistemin bunu öğrenmesine yardımcı olur. 

-----

 * [Genel Bakış](cognitive-services-custom-speech-home.md)
 * [Başlatıldı](cognitive-services-custom-speech-get-started.md)
 * [Sözlük](cognitive-services-custom-speech-glossary.md)
