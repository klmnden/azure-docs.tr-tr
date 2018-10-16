---
title: Azure üzerinde sık sorulan sorular için özel konuşma hizmeti | Microsoft Docs
description: Özel konuşma hizmeti ile ilgili en yaygın soruların yanıtları aşağıdadır.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: 8e3d5e0e2b70d8f97099103ed369e48dd74d56e2
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341370"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Özel konuşma hizmeti hakkında sık sorulan sorular

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)] 

Bu SSS'de sorularınızın yanıtlarını bulamazsanız, üzerinde özel konuşma hizmeti Topluluğu'na sorun deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ve [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Genel

**Soru**: olduğunu nasıl öğrenebilirim my veri kümesi veya model işlenmesi tamamlandığında?

**Yanıt**: şu anda durumu modeli veya tabloya veri kümesinde, yalnızca bilmek istiyorum değil.
İşleme tamamlandığında, durum "Hazır" olacaktır.
İletişim e-posta bildirimi gibi bir durum işleme için gelişmiş yöntemleri üzerinde çalışıyoruz.

**Soru**: aynı anda birden fazla model oluşturabilirsiniz?

**Yanıt**: kaç modelleri, koleksiyonda yer alan bir sınır yoktur ancak tek bir zaman her sayfada oluşturulabilir.
Örneğin, varsa şu anda bir dil modeli işlem aşamasında bir dil modeli oluşturma işlemi başlatılamıyor.
Ancak, bir akustik model ve aynı anda işlenirken bir dil modeli olabilir. 

**Soru**: Ben hata yaptığım gerçekleşmiş. Veri içe aktarma işlemi iptal etmek veya nasıl devam eden oluşturma modeli? 

**Yanıt**: şu anda bir akustik veya dil uyarlama işlemi geri alamazsınız.
İçeri aktarma tamamlandıktan sonra içeri aktarılan veriler silinebilir

**Soru**: arama & dikte modelleri ve konuşma modelleri arasındaki fark nedir?

**Yanıt**: iki temel akustik ve dil modellerini özel konuşma hizmeti seçilecek vardır.
arama sorguları veya dikte. Microsoft Conversational AM, konuşma bir stile damıtarak konuşma bağlamında kullanılabilen konuşulan tanımayı için uygundur.
Bu tür bir konuşma genellikle başka bir kişinin, gibi çağrı merkezi veya toplantı yönlendirilir.

**Soru**: benim mevcut model (model yığınlama) güncelleştirebilirim?

**Yanıt**: mevcut bir model yeni verilerle güncelleştirme olanağı sunmuyoruz.
Yeni bir veri kümesine sahip olursunuz ve mevcut modelini özelleştirmek istiyorsanız, bunu yeni veriler ile kullanılan eski veri kümesini yeniden uyarlamanız gerekir.
(Akustik verilerini ise) eski ve yeni veri kümeleri tek bir .zip birleştirilmesini veya dil veri kez uyarlama ise bir .txt dosyasına yeni bir uç noktası almak için XML'deki dağıtılmış olması yeni güncelleştirilmiş model gereken gerçekleştirilir

**Soru**: varsayılan değerinden daha yüksek Eş zamanlılık ihtiyacım olursa ne yapabilirim. 

**Yanıt**: modelinizi artışlarla ölçek birimleri diyoruz 5 eşzamanlı istek sayısı'kurmak ölçeklendirebilirsiniz. Her ölçek birimi, modelinizi 5 ses akışı aynı anda işleyebilir garanti eder. 100 ölçek birimi (veya 500 eş zamanlı istek) satın alabilirsiniz.

Daha yüksek ihtiyacınız varsa lütfen bizimle iletişime geçin.

**Soru**: Ben my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: indirilir ve yerel olarak yürütüldüğü modellerini etkinleştirmeyin.

**Soru**: isteklerim oturum açmış olan?

**Yanıt**: izleme devre dışı, bu noktada hiçbir ses geçiş yapmak için bir dağıtımın oluşturulması sırasında vardır veya döküm kaydedilir. Aksi takdirde isteği Azure'da genellikle güvenli depolama veritabanında kaydedilir. Bizimle iletişime geçin, lütfen özel konuşma hizmeti kullanarak engelle Gizlilik sorunları başka varsa.

## <a name="importing-data"></a>Veri alma

**Soru**: veri kümesi boyutu sınırı nedir? Neden? 

**Yanıt**: geçerli bir veri kümesi için 2 GB, HTTP yüklemek için bir dosya boyutu kısıtlaması nedeniyle sınırlıdır. 

**Soru**: metin dosyalarımı miyim daha büyük bir metin dosyasını karşıya yükleme için zip? 

**Yanıt**: Hayır, şu anda yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**Soru**: başarısız konuşma vardı veri raporu söyler. Bu, bir sorunla mı?

**Yanıt**: yalnızca birkaç konuşma başarıyla içeri aktarıldı başarısız oldu. Bu bir sorun değildir.
Konuşma akustik veya dil verilerde büyük çoğunluğu ayarlarsanız (örneğin > % 95) başarıyla içeri aktarıldı, veri kümesi kullanışlı olabilir. Ancak, konuşma başarısız olmasının anlamanıza ve sorunları düzeltmek deneyin önerilir.
Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-am"></a>AM oluşturma

**Soru**: akustik ne kadar veri yapmam gerekir mi?

**Yanıt**: akustik verilerin bir saat 30 dakika ile başlamanızı öneririz

**Soru**: Ben ne tür bir veri toplamanız gerekir?

**Yanıt**: uygulama senaryosuna gibi yakın olan veri toplama ve kullanım örneği mümkün olduğunca.
Başka bir deyişle, veri toplama, cihaz veya cihazları, ortamlar ve konuşmacıları türleri açısından kullanıcılar ve hedef uygulama eşleşmesi gerekir. Genel olarak, mümkün olduğunca konuşmacıları bir dizi geniş kapsamlı olarak verileri toplamanız gerekir. 

**Soru**: nasıl miyim toplamak? 

**Yanıt**: bağımsız veri koleksiyonu uygulama oluşturma veya bazı raf ses kaydı yazılım kapalı kullanın.
Ses verilerini günlüğe kaydeder ve kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**Soru**: uyarlama veri kendim özelliği gerekiyor mu? 

**Yanıt**: veri transcribed gerekir. Kendiniz özelliği ya da professional transkripsiyonu hizmet kullanın. Bu kullanım profesyonel transcribers ve diğer bazı, kitle kaynak kullanır. Ayrıca bir döküm hizmet istek üzerine öneririz.

**Soru**: ne kadar bir özel akustik model oluşturmak için sürer?

**Yanıt**: uzunluğu akustik veri kümesi ile aynı özel akustik model oluşturmak için işleme süresi hakkındadır.
Bu nedenle, beş saat veri kümesinden oluşturulan özelleştirilmiş bir akustik model yaklaşık beş saat sürer işlenecek. 

## <a name="offline-testing"></a>Çevrimdışı Test

**Soru**: Çevrimdışı bir özel dil modeli kullanarak kendi özel akustik modelini, test yapabilir miyim?

**Yanıt**: Evet, özel dil modeli çevrimdışı test ayarlandığında aşağı açılan seçmeniz yeterlidir

**Soru**: Çevrimdışı bir özel akustik model kullanarak kendi özel dil modelini, test yapabilir miyim?

**Yanıt**: Evet, çevrimdışı test ayarladığınızda özel akustik model açılan menüde seçmeniz yeterlidir.

**Soru**: sözcük hata oranı nedir ve nasıl bunu hesaplanır?

**Yanıt**: sözcük hata oranı, konuşma tanıma için değerlendirme ölçüm olur. Eklemeler, silme ve değişimler, bölü başvuru transkripsiyonu sözcükleri toplam sayısı içeren hatalarının toplam sayısını, olarak sayılır.

**Soru**: ben artık benim özel modeli test sonuçlarını bilmeniz, iyi veya kötü birkaç budur?

**Yanıt**: temel model ve, özelleştirilmiş bir arasında bir karşılaştırma sonuçlarını göster.
Özelleştirmenin faydalı hale getirmek için temel model beat hedeflemeyi

**Soru**: nasıl miyim şekil temel modellerin WER geliştirme olup olmadığını görebilmeniz için? 

**Yanıt**: Çevrimdışı test sonuçlarını temel doğruluğunu özel model ve geliştirme doğruluğunu taban çizgisi Göster

## <a name="creating-lm"></a>LM oluşturma

**Soru**: metin veri miktarını karşıya yükleme için değiştirmem gerekiyor mu?

**Yanıt**: Bu kesin bir yanıt vermek zor bir soru, bunu nasıl farklı olarak sözlük ve uygulamanızda kullanılan ifadeleri başlangıç dil modellerinin. Tüm yeni sözcükler için bu bir kelimelerin kullanımı mümkün olduğunca çok örnekleri sağlamak kullanışlıdır. Bu terimler için dinlemek üzere sisteme söyler gibi uygulamanızda kullanılan genel ifadeleri için dil verileri de dahil olmak üzere de yararlıdır.
En az yüz ve genellikle birkaç yüz konuşma dilini veri kümesi veya daha çok daha yaygındır.
Ayrıca, bazı türleri *, sorguları diğerlerinden daha sık olması bekleniyor, veri kümesinde birden çok kopyasını ortak sorgular ekleyebilirsiniz.

**Soru**: yalnızca sözcüklerin listesi karşıya yükleyebilirsiniz?

**Yanıt**: sözcüklerin listesini karşıya sözcüklere sözlük için alma ancak sistem nasıl sözcükler genellikle kullanılan öğretmek değildir.
Dil modeli tam veya kısmi konuşma (cümleler ya da tümcelere nesnelerin interneti kullanıcılar söyleyin doğan) sağlayarak yeni sözcükler ve nasıl kullanıldıkları öğrenebilirsiniz. Özel dil modeli sistemin yeni sözcükleri almak için yalnızca aynı zamanda uygulamanız için bilinen bir kelimelerin olasılığını ayarlamak için uygundur. Tam bir konuşma sağlayan, sistemin bunu öğrenmesine yardımcı olur. 

-----

 * [Genel Bakış](cognitive-services-custom-speech-home.md)
 * [Başlatıldı](cognitive-services-custom-speech-get-started.md)
 * [Sözlük](cognitive-services-custom-speech-glossary.md)
