---
title: Azure üzerinde sık sorulan sorular konuşma metin hizmetine | Microsoft Docs
description: Burada, konuşma metin hakkında en popüler sorulan soruların yanıtları bulunur.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 64e505889ef9472603471d67a961985c1290663a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045852"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Özel konuşma hizmet sık sorulan sorular

Bu SSS sorularınızın yanıtlarını bulamazsanız, üzerinde özel konuşma hizmeti topluluğu isteyen deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ve [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Genel

**Soru**: metin modelleri için temel ve özel konuşma arasındaki fark nedir?

**Yanıt**: temel modelleri veri ait Microsoft ile eğitilmiş ve bulutta zaten dağıtılmış. Özel modelleri belirli ortam gürültüsünü veya dil olan belirli bir ortamı daha iyi uyacak şekilde bir model uyum izin verin. Fabrika Katlar, araba, Biyoloji, fizik, radyoloji, ürün adları ve özel kısaltmalar gibi belirli konular dil modeli gerektirir ancak gürültülü streets uyarlanmış akustik modeli duyar.

**Soru**: nereden başlamalıyım temel model kullanılacak isterseniz?

**Yanıt**: ilk almanız gereken bir [abonelik anahtarı](get-started.md). Predeployed temel modelleri REST çağrı yapmak istiyorsanız, başvurun [burada ayrıntıları](rest-apis.md). WebSockets indirme kullanmak istiyorsanız, [SDK](speech-sdk.md)

**Soru**: ı yapmak her zaman özel konuşma modelini oluşturmak için ihtiyacınız?

**Yanıt**: uygulamanızı özel sözlük veya o olabilir nadir ardından olmadan genel günlük dil kullanıyorsa, Hayır, size bir model özelleştirmek gerekmez. Ayrıca, uygulamanızın nerede çok az kayıpla veya hiç arka plan gürültü olduğundan sonra olmayan bir ortamda kullanılacak ya da özelleştirmeniz gerekiyorsa ise. Portal taban çizgisi ve özelleştirilmiş modelleri dağıtmak ve bunlara karşı doğruluğu testleri çalıştırmak kullanıcıların sağlar. Kullanıcıların özel bir model temel vs doğruluğunu ölçmek için bu özelliği kullanabilirsiniz.

**Soru**: nasıl bilebilirim my veri kümesi veya model işlenmesi tamamlandığında?

**Yanıt**: şu anda, model veya tablosu veri kümesinde yalnızca bilmek istiyorsanız durumudur.
İşlem tamamlandığında, durum "Hazır" olur.

**Soru**: aynı anda birden fazla model oluşturabilirim?

**Yanıt**: kaç modelleri, koleksiyonda yer alan bir sınır yoktur ancak tek zaman her sayfada oluşturulabilir.
Örneğin, varsa şu anda bir dil modeli işlem aşamasında bir dil modeli oluşturma işlemi başlatılamıyor.
Ancak, bir akustik modeli ve aynı anda işleme dil modeli olabilir. 

**Soru**: t hata yaptığım gerçekleşmiş. Nasıl veri içe aktarma işlemi iptal edin veya devam ediyor oluşturma model? 

**Yanıt**: şu anda bir kullanım veya dil uyarlama işlemi geri alamazsınız.
Alma işlemi tamamlandıktan sonra içeri aktarılan veriler silinebilir

**Soru**: arama & dikte modelleri ve konuşma modelleri arasındaki fark nedir?

**Yanıt**: iki temel akustik & dil modelleri özel konuşma hizmetinde seçilecek vardır.
sorgular veya dikte arayın. Microsoft Conversational AM konuşma stilde konuşulan konuşma tanıma için uygundur.
Bu tür bir konuşma genellikle başka bir kişinin, gibi çağrı merkezleri veya toplantılar yönlendirilir.

**Soru**: (modeli yığınlama) my varolan modeli güncelleştirmek?

**Yanıt**: Varolan modelleri güncelleştirilemez. Geçici bir çözüm olarak eski veri kümesinin yeni birleştirmek ve readapt.

Dil veri ise, eski ve yeni veri kümeleri (Akustik veri ise) tek bir .zip veya .txt dosyası birleştirilmiş gerekir. Yeni bir uç noktası edinmek için XML'deki dağıtılmış olması yeni güncelleştirilmiş model gereken uyarlama bir kez gerçekleştirilir

**Soru**: varsayılan değerinden daha yüksek eşzamanlılık ne ihtiyacım veya ne portalında sunulur. 

**Yanıt**: 20 eş zamanlı istek artışlarla modelinizi yukarı ölçeklendirebilirsiniz. 

Daha yüksek ölçek gerekiyorsa bizimle iletişime geçin.

**Soru**: t my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: modelleri indirilir ve yerel olarak yürütülür.

**Soru**: olan oturum isteklerim?

**Yanıt**: transcriptions oturum açmış olmanız ya da izleme devre dışı, bu noktada hiçbir ses geçiş yapmak için bir dağıtım oluşturulması sırasında seçenekleriniz vardır. Aksi takdirde istekleri Azure'da genellikle güvenli depolama kaydedilir. Daha fazla özel konuşma hizmeti kullanarak yasaktır Gizlilik sorunları varsa, destek kanallarını başvurun.

## <a name="importing-data"></a>Veri alma

**Soru**: veri kümesi boyutu sınırı nedir? Neden? 

**Yanıt**: geçerli bir veri kümesi için 2 GB ' HTTP yüklemek için bir dosya boyutu sınırlaması nedeniyle sınırlıdır. 

**Soru**: metin dosyalarımı ı daha büyük bir metin dosyasını karşıya yüklemek için ZIP? 

**Yanıt**: Hayır, şu anda yalnızca sıkıştırılmamış metin dosyalarını izin verilir.

**Soru**: başarısız utterances vardı veri raporu söyler. Sorun nedir?

**Yanıt**: % 100'dosyasında utterances karşıya yüklemek başarısız olan bir sorun değildir.
Bir kullanım veya dil veri utterances çoğunluğu ayarlarsanız (örneğin, > % 95) başarıyla içeri veri kümesi kullanışlı olabilir. Ancak, utterances neden geçemediğini anlamak ve sorunları düzeltmek deneyin önerilir. Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-am"></a>AM oluşturma

**Soru**: akustik ne kadar veri ihtiyacım var?

**Yanıt**: 30 dakika akustik verilerin bir saat ile başlayan öneririz

**Soru**: t hangi veri toplamanız gerekir?

**Yanıt**: uygulama senaryo olarak yakın olan verileri toplamak ve durumu mümkün olduğunca kullanın.
Veri toplama, cihaz veya cihazları, ortamları ve konuşmacılar türleri açısından kullanıcılar ve hedef uygulama eşleşmesi gerekir. Genel olarak, mümkün olduğunca konuşmacılar çeşitli geniş olarak verileri toplamanız gerekir. 

**Soru**: nasıl ı toplamak onu? 

**Yanıt**: bir tek başına veri toplama uygulaması oluşturabilir veya bazı raf ses kaydını yazılım kapalı kullanın.
Ses verilerini günlüğe kaydeder ve kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**Soru**: uyarlama veri kendim transcribe gerekiyor mu? 

**Yanıt**: veri transcribed gerekir. Kendiniz transcribe ya da professional transcription hizmeti kullanın. Bu kullanım profesyonel transcribers ve diğerleri bazıları kitle kaynak kullanın.

**Soru**: ne kadar özel akustik modeli oluşturmak için sürer?

**Yanıt**: akustik veri kümesi uzunluğu ile aynı özel akustik model oluşturma için işleme süresi hakkındadır.
Bu nedenle, bir saat beş veri kümesinden oluşturulan özelleştirilmiş bir akustik modeli yaklaşık beş saat arasında zaman alacak işleyemedi. 

## <a name="offline-testing"></a>Çevrimdışı Test

**Soru**: Çevrimdışı özel dil modeli kullanarak my özel akustik modelinin testi gerçekleştirebilirsiniz?

**Yanıt**: Evet, çevrimdışı testi ayarlama yalnızca özel dil modeli açılan seçin

**Soru**: Çevrimdışı özel akustik modelini kullanarak my özel dil modelinin testi gerçekleştirebilirsiniz?

**Yanıt**: Evet, çevrimdışı test ayarladığınızda açılır menüde yalnızca özel akustik modelini seçin.

**Soru**: Word hata oranı nedir ve nasıl, hesaplanan?

**Yanıt**: Word hata hızıdır konuşma tanıma için değerlendirme ölçüm. Eklemeler, silme ve kısaltmaları bölü başvuru transcription sözcükleri toplam sayısı, içeren hatalarının toplam sayısını, olarak sayılır.

**Soru**: doğruluğunu test sonuçlarını iyi olup olmadığını nasıl belirleyebilirim?

**Yanıt**: temel model ve, özelleştirilmiş bir arasında bir karşılaştırma sonuçları gösterir.
Özelleştirme faydalı yapmak için temel model uluslararası hedeflemeniz gerektiğini

**Soru**: nasıl ı şekil temel modelleri WER geliştirme olup olmadığını görebilmeniz için? 

**Yanıt**: Çevrimdışı test sonuçları temel doğruluğunu özel model ve geliştirme doğruluğunu temel gösterir.

## <a name="creating-lm"></a>LM oluşturma

**Soru**: ne kadar metin verileri karşıya yüklemek yapmalıyım?

**Yanıt**: üzerinde nasıl farklı sözlük bağlıdır ve uygulamanızda kullanılan tümcecikleri başlangıç dili modellerinden. Tüm yeni sözcükler için bu sözcükleri kullanımını mümkün olduğu kadar örnekler sağlamak kullanışlıdır. Bu koşulları için dinleme sisteme bildirdiğinde, uygulamanızda kullanılan ortak tümcecikleri tümcecikleri dil verileri de dahil olmak üzere de yararlıdır. En az bir 100 ve genellikle birkaç yüz utterances dil veri kümesi ya da daha fazla bilgi için yaygın bir durumdur. Ayrıca belirli sorgu türlerini diğerlerinden daha sık olması beklenmektedir, veri kümesinde Genel sorgular birden çok kopyasını ekleyebilirsiniz.

**Soru**: yalnızca sözcüklerin listesini karşıya yükleyebilir?

**Yanıt**: sözcüklerin listesini karşıya sözcüklere sözlük için alma ancak sistem nasıl sözcükler genellikle kullanılan öğretmek değildir.
Tam veya kısmi utterances (cümleleri veya kullanıcıların söylemek büyük olasılıkla şeyleri tümceleri) sağlayarak dil modeli yeni sözcükleri öğrenebilirsiniz ve nasıl kullanılır. Özel dil modeli sistemindeki yeni sözcükleri almak için yalnızca aynı zamanda, uygulamanız için bilinen sözcükler olasılığını ayarlamak için uygundur. Tam utterances sağlayarak daha iyi bilgi sistemi yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
