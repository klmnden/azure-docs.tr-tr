---
title: Sık sorulan sorular özel konuşma hizmeti için Azure üzerinde | Microsoft Docs
description: Burada, özel konuşma hizmeti hakkında en popüler sorulan soruların yanıtları bulunur.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 11/21/2016
ms.author: panosper
ms.openlocfilehash: a929869b36387b3257b672308ceca36c84ff8cae
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351857"
---
# <a name="custom-speech-service-frequently-asked-questions"></a>Özel konuşma hizmet sık sorulan sorular

Bu SSS sorularınızın yanıtlarını bulamazsanız, üzerinde özel konuşma hizmeti topluluğu isteyen deneyin [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ve [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Genel

**Soru**: nasıl bilebilirim my veri kümesi veya model işlenmesi tamamlandığında?

**Yanıt**: şu anda, model veya tablosu veri kümesinde yalnızca bilmek istiyorsanız durumudur.
İşlem tamamlandığında, durum "Hazır" olur.
E-posta bildirimi gibi durumu işlenirken iletişimi için geliştirilmiş yöntemlerde çalışıyoruz.

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

**Yanıt**: yeni veriler ile varolan bir modeli güncelleştirme olanağı sunmaz.
Yeni bir veri kümesine sahip ve var olan bir model özelleştirmek istiyorsanız, bunu yeni verileri ve kullandığınız eski veri kümesinin yeniden uyarlamanız gerekir.
Eski ve yeni veri kümeleri (Akustik veri ise) tek bir .zip birleştirilmelidir veya .txt dosyası dil veri kez uyarlama ise yeni bir uç noktası edinmek için XML'deki dağıtılmış olması yeni güncelleştirilmiş model gereken yapılır

**Soru**: varsayılan değerinden daha yüksek eşzamanlılık ne ihtiyacım. 

**Yanıt**: modelinizi ölçek birimleri dediğimiz 5 eş zamanlı istek artışlarla yukarı ölçeklendirebilirsiniz. Her bir ölçek birimi modelinizi 5 ses akışı aynı anda işleyebilir güvence altına alır. 100 ölçek birimleri (veya 500 eşzamanlı istek) satın alabilirsiniz.

Daha yüksek gerekiyorsa lütfen bizimle iletişime geçin.

**Soru**: t my modeli indirebilir ve yerel olarak çalıştırma?

**Yanıt**: modellerini yüklenebilir ve yerel olarak yürütülen etkinleştirmeyin.

**Soru**: olan oturum isteklerim?

**Yanıt**: transcriptions oturum açmış olmanız ya da izleme devre dışı, bu noktada hiçbir ses geçiş yapmak için bir dağıtım oluşturulması sırasında seçenekleriniz vardır. Aksi takdirde istekleri Azure'da genellikle güvenli depolama kaydedilir. Daha fazla Bize Ulaşın özel konuşma hizmet Lütfen kullanarak yasaktır Gizlilik sorunları varsa.

## <a name="importing-data"></a>Veri alma

**Soru**: veri kümesi boyutu sınırı nedir? Neden? 

**Yanıt**: geçerli bir veri kümesi için 2 GB ' HTTP yüklemek için bir dosya boyutu sınırlaması nedeniyle sınırlıdır. 

**Soru**: metin dosyalarımı ı daha büyük bir metin dosyasını karşıya yüklemek için ZIP? 

**Yanıt**: Hayır, şu anda yalnızca sıkıştırılmamış metin dosyalarını izin verilir.

**Soru**: başarısız utterances vardı veri raporu söyler. Bu sorun mı?

**Yanıt**: yalnızca birkaç utterances başarıyla içeri aktarılacak başarısız oldu, bu bir sorun değildir.
Bir kullanım veya dil veri utterances çoğunluğu ayarlarsanız (örneğin > % 95) başarıyla içeri veri kümesi kullanışlı olabilir. Ancak, utterances neden geçemediğini anlamak ve sorunları düzeltmek deneyin önerilir.
Hataları, biçimlendirme gibi en sık karşılaşılan sorunları düzeltmek kolaydır. 

## <a name="creating-am"></a>AM oluşturma

**Soru**: akustik ne kadar veri ihtiyacım var?

**Yanıt**: 30 dakika akustik verilerin bir saat ile başlayan öneririz

**Soru**: t ne tür bir veri toplamanız gerekir?

**Yanıt**: uygulama senaryo olarak yakın olan verileri toplamak ve mümkün olduğunca çalışması kullanmanız gerekir.
Bu, veri toplama hedef uygulama ve kullanıcılara cihaz veya cihazları, ortamları ve konuşmacılar türleri açısından eşleşmesi gerektiği anlamına gelir. Genel olarak, mümkün olduğunca konuşmacılar çeşitli geniş olarak verileri toplamanız gerekir. 

**Soru**: nasıl ı toplamak onu? 

**Yanıt**: bir tek başına veri toplama uygulaması oluşturabilir veya bazı raf ses kaydını yazılım kapalı kullanın.
Ses verilerini günlüğe kaydeder ve kullanan uygulamanızın sürümünü de oluşturabilirsiniz. 

**Soru**: uyarlama veri kendim transcribe gerekiyor mu? 

**Yanıt**: veri transcribed gerekir. Kendiniz transcribe ya da professional transcription hizmeti kullanın. Bu kullanım profesyonel transcribers ve diğerleri bazıları kitle kaynak kullanın. Ayrıca bir transcription hizmet istek üzerine öneririz.

**Soru**: ne kadar özel akustik modeli oluşturmak için sürer?

**Yanıt**: akustik veri kümesi uzunluğu ile aynı özel akustik model oluşturma için işleme süresi hakkındadır.
Bu nedenle, bir beş saat veri kümesinden oluşturulan özelleştirilmiş bir akustik modelini yaklaşık beş saat arasında zaman alacak işleyemedi. 

## <a name="offline-testing"></a>Çevrimdışı Test

**Soru**: Çevrimdışı özel dil modeli kullanarak my özel akustik modelinin testi gerçekleştirebilirsiniz?

**Yanıt**: Evet, yalnızca çevrimdışı test ayarlandığında aşağı açılan özel dil modelini seçin

**Soru**: Çevrimdışı özel akustik modelini kullanarak my özel dil modelinin testi gerçekleştirebilirsiniz?

**Yanıt**: Evet, çevrimdışı test ayarladığınızda açılır menüde yalnızca özel akustik modelini seçin.

**Soru**: Word hata oranı nedir ve nasıl, hesaplanan?

**Yanıt**: Word hata hızıdır konuşma tanıma için değerlendirme ölçüm. Eklemeler, silme ve kısaltmaları bölü başvuru transcription sözcükleri toplam sayısı, içeren hatalarının toplam sayısını, olarak sayılır.

**Soru**: t şimdi my özel model test sonuçlarını bilmeniz, bu bir iyi ya da hatalı numarasıdır?

**Yanıt**: temel model ve, özelleştirilmiş bir arasında bir karşılaştırma sonuçları gösterir.
Özelleştirme faydalı yapmak için temel model uluslararası hedeflemeniz gerektiğini

**Soru**: nasıl ı şekil temel modelleri WER geliştirme olup olmadığını görebilmeniz için? 

**Yanıt**: Çevrimdışı test sonuçları temel doğruluğunu özel model ve geliştirme doğruluğunu temel gösterir.

## <a name="creating-lm"></a>LM oluşturma

**Soru**: ne kadar metin verileri karşıya yüklemek yapmalıyım?

**Yanıt**: Bu kesin bir yanıt vermek için zor bir soru, farklı nasıl bağlı olduğu gibi dağarcığı ve uygulamanızda kullanılan tümcecikleri başlangıç dili modellerinden. Tüm yeni sözcükler için bu sözcükleri kullanımını mümkün olduğu kadar örnekler sağlamak kullanışlıdır. Bu koşulları için dinleme sisteme bildirdiğinde uygulamanızda kullanılan ortak tümcecikleri dil verileri de dahil olmak üzere de yararlıdır.
En az yüz ve genellikle birkaç yüz utterances dil veri kümesi ya da daha fazla bilgi için yaygın bir durumdur.
Ayrıca, belirli türleri *, sorguları diğerlerinden daha sık olması bekleniyor, veri kümesinde Genel sorgular birden çok kopyasını ekleyebilirsiniz.

**Soru**: yalnızca sözcüklerin listesini karşıya yükleyebilir?

**Yanıt**: sözcüklerin listesini karşıya sözcüklere sözlük için alma ancak sistem nasıl sözcükler genellikle kullanılan öğretmek değildir.
Tam veya kısmi utterances (cümleleri veya kullanıcıların söylemek büyük olasılıkla şeyleri tümceleri) sağlayarak dil modeli yeni sözcükleri ve bunların nasıl kullanıldığı bilgi edinebilirsiniz. Özel dil modeli sistemindeki yeni sözcükleri almak için yalnızca aynı zamanda, uygulamanız için bilinen sözcükler olasılığını ayarlamak için uygundur. Tam utterances sağlayarak, bu bilgi sistem yardımcı olur. 

-----

 * [Genel Bakış](cognitive-services-custom-speech-home.md)
 * [Başlatıldı](cognitive-services-custom-speech-get-started.md)
 * [Sözlük](cognitive-services-custom-speech-glossary.md)
