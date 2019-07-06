---
title: Merkezi Transkripsiyonu - konuşma Hizmetleri çağırın
titleSuffix: Azure Cognitive Services
description: Konuşma metin için yaygın bir senaryo, büyük hacimli çeşitli sistemlerden etkileşimli sesli yanıt (IVR) gibi gelebilir telefon verileri fotoğrafını. Stereo ya da mono ses olabilir ve ham işlem yok az sonrası ile gerçekleştirilen sinyal. Konuşma Hizmetleri ve birleşik konuşma modeli kullanarak bir iş birçok ses yakalamayı sistemlerle yüksek kaliteli döküm alabilirsiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 37d68a4d2b7658542ebcfdb5d22a10676a8e4d52
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603298"
---
# <a name="speech-services-for-telephony-data"></a>Telefon veri konuşma Hizmetleri

Landlines, cep telefonları ve radyolara oluşturulan telefon genellikle düşük kalitede ve zorlukları Konuşmayı metne dönüştürme yaparken oluşturur 8 KHz aralığında dar bantlı verilerdir. Veriler bir insan anlaşılması zor olduğunda bile durumlarda bu telefon veri çoğaltmaya adresindeki Azure konuşma Hizmetleri'nden en son konuşma tanıma modelleri excel. Bu modeller, büyük hacimli telefon verileri eğitim ve en iyi gürültülü ortamlarda bile Pazar tanıma doğruluğunu sağlamak.

Konuşma metin için yaygın bir senaryo, büyük hacimli çeşitli sistemlerden etkileşimli sesli yanıt (IVR) gibi gelebilir telefon verileri fotoğrafını. Bu sistemler sağlamaya ses sinyale göre yapılan küçük Hayır post işlemeyle stereo veya mono ve ham olabilir. Konuşma Hizmetleri ve birleşik konuşma modeli kullanarak bir iş yüksek kaliteli döküm, hangi ses yakalamak için kullanılan sistemler elde edebilirsiniz.

Telefon veriler, daha iyi müşterilerinizin gereksinimlerini anlamak, yeni pazarlama fırsatları belirlemenize veya çağrı center aracıları performansını değerlendirmek için kullanılabilir. Veri transcribed sonra bir iş için Gelişmiş telemetri anahtar ifadeleri tanımlamak veya müşteri yaklaşımı analiz etme, çıkış kullanabilirsiniz.

Çeşitli çağrısı işlem Hizmetleri, hem gerçek zamanlı desteklemek için bu sayfada özetlenen Microsoft tarafından dahili olarak teknolojilerdir ve toplu iş modu.

Teknoloji ve ilgili özellikleri Azure konuşma Hizmetleri teklifi gözden geçirelim.

> [!IMPORTANT]
> Konuşma birleştirilmiş hizmetleri modeli farklı verilerle eğitilir ve tek bir model çözüm senaryosu için dikte telefon analytics'e sunar.

## <a name="azure-technology-for-call-centers"></a>Çağrı merkezi için bir Azure teknolojisi

İşlev, konuşma Hizmetleri birincil amaçları için çağrı merkezi – uygulandığında – müşteri deneyimini iyileştirmek üzere yönüdür. Bu konuda, üç düz etki alanı mevcuttur:

* Çağrı sonrası analiz çağrı kayıtları işlenmesini diğer bir deyişle, toplu iş
* Gerçek zamanlı analiz çağrı (tanınmış bir kullanım örneği olan yaklaşım ile) gerçekleşen olduğu gibi çeşitli bilgileri ayıklamak için ses sinyalini işlenmesini ve
* Sanal Yardımcıları (Botlar) aracısı yardımcı olmak için iletişim kutusu girişimi hiçbir agent katılımını ile müşterinin sorunu çözmek için müşteri ile bot arasında gidiş veya AI uygulama protokoller.

Bir batch senaryosu uygulanması tipik mimarisi diyagramı aşağıdaki resimde gösterilen ![çağrı merkezi transkripsiyonu mimarisi](media/scenarios/call-center-transcription-architecture.png)

## <a name="speech-analytics-technology-components"></a>Konuşma analizi teknoloji bileşenleri

Etki alanı veya gerçek zamanlı sonrası çağrı olsun, Azure olgun ve ortaya çıkan dizi müşteri deneyimini geliştirmek için teknoloji kümesi sunar.

### <a name="speech-to-text-stt"></a>Konuşmadan metne (STT)

[Konuşmayı metne](speech-to-text.md) herhangi bir çağrı merkezi çözümüne özellik için en çok Aranan sonradır. Aşağı Akış analizi işlemlerini birçoğu transcribed metni kullanan, word hatası oranı (WER) dayanıklılığı olduğu. Çağrı merkezi transkripsiyonu en önemli zorluklardan çağrı merkezi (örneğin arka planda Konuşmayı diğer aracılar), birçok farklı dil yerel ayarlar ve diyalektler yanı sıra gerçek telefon sinyal düşük kalitesini yaygın gürültü biridir. WER'i akustik ve dil modellerini verili bir yerel ayar için ne kadar iyi eğitilir ile son derece bağıntılı, bu nedenle işaretleyebilmesine bölgeniz modele özelleştirmek için önemlidir. Bizim en son birleştirilmiş sürüm 4.x modelleri transkripsiyonu doğruluk ve gecikme süresi çözümdür. On ile eğitilmiş akustik verilerini saatlik binlerce ve sözcük bilgi birleşik milyarlarca çağrı merkezi veri özelliği piyasadaki en doğru modelleri modelleridir.

### <a name="sentiment"></a>Yaklaşım
İyi bir deneyim müşteri verip vermediğini ölçülebilir çağrı Merkezi alanına uygulandığında konuşma analizi en önemli alanları biridir. Bizim [Batch tanıma API'sini](batch-transcription.md) utterance başına yaklaşım analizi sunar. Hem aracılarınızın hem de müşteri çağrısı duyarlılığını belirlemek için bir çağrı döküm bir parçası olarak elde edilen değerleri kümesi toplayabilirsiniz.

### <a name="silence-non-talk"></a>Sessizlik (konuşma olmayan)
Konuşma olmayan süresi dediğimiz olması için bir destek çağrısının yüzde 35 seyrek değil. Konuşma olmayan hangi gerçekleşir bazı senaryolar şunlardır: önceki durumu geçmişi müşteriyle bakarak aracıları, vb. bir aktarım için bekleyen işlevlerini gerçekleştiren ve müşterinin erişmenizi sağlayan araçlar kullanarak aracılarını oturan müşteriler tutun. Son derece önemlidir bulunduğundan bir çağrıda sessizlik aşabilirler ölçer önemli müşteri sensitivities bu tür senaryolar oluşan ve sonuçları ortaya çıktıkları arama sayısı.

### <a name="translation"></a>Çeviri
Bazı şirketler, teslim yöneticileri müşterileri dünya çapında deneyimini anlayabilmeniz yabancı dilleri destek çağrılarının çevrilmiş dökümleri sağlamaya deneylerini. Bizim [çeviri](translation.md) eşsiz özellikleri. Biz ses için ses veya ses metin için çok sayıda yerel çevirebilir.

### <a name="text-to-speech"></a>Metin Okuma
[Metin okuma](text-to-speech.md) müşterilerle etkileşim kuran robotlar uygulamaya başka bir önemli alandır. Tipik müşteri konuşur, metin, ses transcribed, hedefleri için çözümlenmiş metin, tanınan amacına yanıt oluşturulan ve bir varlık için müşteri ya da sonra ortaya veya Sentezlenen sesli yanıt olduğunu yoludur oluşturuldu. Elbette bu tüm sahiptir hızlı bir şekilde – böylece gecikme süresi içinde bu sistemler başarısını önemli bileşenlerden biridir gerçekleşmesi.

Bizim uçtan uca gecikme süresi gibi dahil çeşitli teknolojileri de göz önünde bulundurur oldukça düşüktür [konuşma metin](speech-to-text.md), [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/), [Bot Framework](https://dev.botframework.com/), [ Metin okuma](text-to-speech.md).

Yeni sunduğumuz kişilerden daha fazlasını da insan ses arasından ayırt edilemiyor. Ses kendi benzersiz kişilik botunuzun vermek için kullanabilirsiniz.

### <a name="search"></a>Ara
Başka bir Zımbalama Analytics belirli bir olay olduğunda etkileşimleri tanımlamak için ya da deneyim oluştu. Bu genellikle iki yaklaşım, burada kullanıcının yalnızca bir ifade türleri ve sistemin yanıt verdiğini veya daha fazla yapılandırılmış sorgu, Analistin bir dizi mantıksal deyimleri, burada oluşturabilirsiniz çağrıda bir senaryo belirlemek ya da bir geçici arama biri ile yapılır , ve ardından her çağrı bu dizi sorgusuna göre sıralanabilir. Her yerde karşılaşılan uyumluluk deyimi iyi arama örnektir "Bu çağrı... kalite amacıyla kaydedilmesi "– birçok şirket çağrı gerçekten kaydedilir önce aracılarının bu sorumluluk reddi müşterilere sağladığından emin olmak istediğiniz kadar. Bu eğilimlerini raporlama sonuçta bir analytics sistem en önemli işlevlerinden biri olarak analiz sistemlerinin çoğu sorgu /search algoritmalarda – davranışları bulunan eğilim yeteneğine sahip. Aracılığıyla [Bilişsel hizmetler dizini](https://azure.microsoft.com/services/cognitive-services/directory/search/) uçtan uca çözümünüz dizin oluşturma ve arama özellikleriyle önemli ölçüde geliştirilebilir.

### <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama
Bu alan daha zorlu analiz uygulamaları ve yapay ZEKA ve ML uygulamadan yararlanmaya aşağıdakilerden biridir. Birincil burada müşteri hedefine çıkarsamak için bir senaryodur. Müşteri neden çağırma? Müşteri sorun nedir? Neden müşteri negatif bir deneyim ettiniz mi? Bizim [metin analiz hizmeti](https://azure.microsoft.com/services/cognitive-services/text-analytics/) bu önemli anahtar sözcüklere veya tümceciklere ayıklamak için uçtan uca çözümünüz yükseltmek için hızlı bir şekilde analiz edebilmesi sunmaktadır.

Şimdi artık toplu işlem ve gerçek zamanlı işlem hatları için konuşma tanıma göz biraz daha ayrıntılı olarak vardır.

## <a name="batch-transcription-of-call-center-data"></a>Çağrı merkezi veri batch transkripsiyon

Ses toplu fotoğrafını için biz geliştirilen [Batch tanıma API'sini](batch-transcription.md). Batch tanıma API'si, zaman uyumsuz olarak büyük miktarlarda veri ses özelliği geliştirilmiştir. Çağrı merkezi veri çoğaltmaya bakımından çözümümüz bu yapı taşları hakkında temel alır:

* **Doğruluk**: Dördüncü nesil birleşik modelleriyle eşsiz transkripsiyonu kalite sunuyoruz.
* **Gecikme süresi**: Toplu döküm yaparken döküm hızla gerekli olup olmadığını biliyoruz. Döküm işleri aracılığıyla başlatılan [Batch tanıma API'sini](batch-transcription.md) hemen sıraya alınır ve iş çalışmaya başladıktan sonra gerçek zamanlı döküm daha hızlı gerçekleştirilir.
* **Güvenlik**: Anlıyoruz çağrıları hassas verileri içerebilir. Güvenlik en önemli önceliklerimizden biri olduğunu içiniz rahat olsun. Hizmetimiz elde ISO, SOC, HIPAA, PCI sertifikaları.

Çağrı merkezi büyük hacimli ses verileri günlük olarak oluşturur. İşletmenizi Azure depolama gibi merkezi bir konumda, telefon veri depoluyorsa kullanabileceğiniz [Batch tanıma API'sini](batch-transcription.md) zaman uyumsuz olarak istemek ve döküm almak için.

Tipik bir çözüm, bu hizmetleri kullanır:

* Azure konuşma Hizmetleri, Konuşmayı metne özelliği kullanılır. Konuşma Hizmetleri standart aboneliği (vb.), Batch tanıma API'sini kullanmak için gereklidir. Ücretsiz abonelik (F0) işe yaramaz.
* [Azure depolama](https://azure.microsoft.com/services/storage/) telefon veri ve Batch tanıma API'si tarafından döndürülen dökümleri depolamak için kullanılır. Yeni dosyalar eklendiğinde bu depolama hesabı için özel bildirimleri kullanmanız gerekir. Bu bildirimler, döküm işlemi tetiklemek için kullanılır.
* [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/) her kayıt için paylaşılan erişim imzaları (SAS) URI oluşturmak ve bir döküm başlatmak için HTTP POST isteği tetiklemek için kullanılır. Ayrıca, Azure işlevleri, almak ve Batch tanıma API'sini kullanarak döküm silme istekleri oluşturmak için kullanılır.
* [Web kancaları](webhooks.md) döküm tamamlandığında bildirim almak için kullanılır.

Dahili olarak çağırır müşteri Microsoft toplu iş modunda desteklemek için yukarıdaki teknolojileri kullanıyoruz.
![Batch mimarisi](media/scenarios/call-center-batch-pipeline.png)

## <a name="real-time-transcription-for-call-center-data"></a>Çağrı merkezi veriler için gerçek zamanlı tanıma

Bazı şirketler, görüşmeler gerçek zamanlı konuşmaların için gereklidir. Gerçek zamanlı döküm, anahtar sözcükleri belirleyin ve içerik ve kaynakları ile ilgili yaklaşım, erişilebilirliği iyileştirmek üzere veya müşteriler ve yerel olmayan aracılar çevirileri sağlamak üzere izleme konuşmaya arar tetiklemek için kullanılabilir konuşmacılar.

Gerçek zamanlı döküm gerektiren senaryolar için kullanılması önerilir [Speech SDK'sı](speech-sdk.md). Şu anda, Konuşmayı metne kullanılabilir [20'den fazla dil](language-support.md), ve SDK'sı kullanılabilir C++, C#, Java, Python, Node.js, Objective-C ve JavaScript. Örnekleri olan her bir dilin kullanılabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk). En son haberler ve güncelleştirmeler için bkz. [sürüm notları](releasenotes.md).

Yukarıdaki teknolojileri gelişmelerden gerçek zamanlı Microsoft Müşteri çağrılarında analiz etmek için dahili olarak kullanıyoruz.

![Batch mimarisi](media/scenarios/call-center-reatime-pipeline.png)

## <a name="a-word-on-ivrs"></a>Bir sözcük IVRs üzerinde

Konuşma Hizmetleri kolayca tümleştirilebilir herhangi bir çözümü kullanarak [Speech SDK'sı](speech-sdk.md) veya [REST API](rest-apis.md). Ancak, çağrı merkezi transkripsiyonu ek teknolojileri gerektirebilir. Genellikle, bir IVR sistem ve Azure arasında bir bağlantı gereklidir. Bu bileşenler sunmuyoruz olsa da, ne bir IVR bağlantı kapsar açıklamak istiyoruz.

(Örneğin, Genesys veya AudioCodes) birkaç IVR veya telefon servis ürünü, bir Azure hizmetine gelen ve giden ses geçişi etkinleştirmek için yararlanılabilir tümleştirme özellikleri sağlar. Temel olarak, özel bir Azure hizmeti, telefon araması oturumlar (gibi çağrı başlangıç veya bitiş çağrısı) tanımlamak ve konuşma Hizmetleri ile kullanılan gelen akış ses almak için bir WebSocket API'yi kullanıma sunmak için belirli bir arabirim sağlayabilir. Konuşma transkripsiyonu veya bağlantıları ile Bot Framework gibi giden yanıtların Microsoft metin okuma service ile oluşturulan ve kayıttan yürütme için IVR döndürülen.

Başka bir senaryo, doğrudan SIP tümleştirmedir. Sunucuya SIP böylece gelen bir akış ve konuşma metin ve metin okuma aşamaları için kullanılan bir giden akış alma, bir Azure hizmetine bağlanır. SIP sunucu var olan Ozeki SDK gibi ticari yazılım teklifleri bağlanmak için veya [çağırma takımlar ve toplantı API](https://docs.microsoft.com/graph/api/resources/calls-api-overview?view=graph-rest-beta) (şu anda beta), ses çağrıları için bu tür bir senaryoyu desteklemek için tasarlanmıştır.

## <a name="customize-existing-experiences"></a>Var olan deneyimleri özelleştirme

Azure konuşma Hizmetleri yerleşik modelleri ile de çalışır, ancak daha fazla özelleştirme ve deneyimini ürün veya ortamı ayarlamak isteyebilirsiniz. Markanız için benzersiz bir ses tipleri için akustik model özelleştirme seçenekleri arasındadır. Özel bir model oluşturduktan sonra tüm Azure konuşma hizmetlerinin hem gerçek zamanlı hem de toplu iş modunda kullanabilirsiniz.

| Konuşma hizmeti | Model | Açıklama |
|----------------|-------|-------------|
| Konuşmayı Metne Dönüştürme | [Akustik model](how-to-customize-acoustic-models.md) | Uygulamalar, Araçlar, özel akustik model oluşturma veya cihazlar özellikle bir araba ya da bir Fabrika katı, her biri belirli bir kaydı koşullar ortamları gibi kullanılır. Vurgulu konuşma, belirli bir arka plan gürültüleri veya belirli bir mikrofon kaydı için kullanarak örnek verilebilir. |
| | [Dil modeli](how-to-customize-language-model.md) | Sektöre özel sözlük ve tıbbi terminolojisi ya da BT terminolojisinin gibi dil bilgisi dökümünün geliştirmek için bir özel dil modeli oluşturun. |
| | [Söyleniş modeli](how-to-customize-pronunciation.md) | Bir özel telaffuz modeliyle fonetik formu ve görüntüleme bir sözcük veya terimi tanımlayabilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası. |
| Metin okuma | [Ses tipi](how-to-customize-voice-font.md) | Özel ses tipi markanız için tanınan, tür, tek bir ses oluşturmanıza imkan tanır. Yalnızca veri kullanmaya başlamak için az miktarda alır. Daha fazla veri sağlarsanız, daha doğal ve İnsan benzeri, ses tipi duyulacaktır. |

## <a name="sample-code"></a>Örnek kod

Örnek kodu Github'da Azure konuşma hizmetlerinin her biri için kullanılabilir. Bu örnekler bir dosya veya akıştan, sürekli ve tek tanıma, ses okuma ve özel modelleriyle çalışma gibi yaygın senaryoları kapsar. Bu bağlantıları, görüntülemek için kullanın. SDK'sını ve REST örnekleri:

* [Konuşma metin ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Batch transkripsiyonu örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Metin okuma örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihaz SDK'sı](speech-devices-sdk.md)
* [REST API: Konuşma metin](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Batch tanıma ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
