---
title: Özel ses - konuşma Hizmetleri için veri hazırlama
titlesuffix: Azure Cognitive Services
description: Markanız için özel bir ses Azure konuşma Hizmetleri ile oluşturun. Studio kayıtları ve ilişkili betikler sağlarsanız, hizmete kayıtlı ses için ayarlanmış bir benzersiz ses modeli oluşturur. Bu ürünler, araçları ve uygulamaları konuşma sentezlemek için kullanın.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 18e1bb486c47baf7648a74e31451e2db73f72250
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65156866"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Özel ses oluşturma için verileri hazırlama

Uygulamanız için bir özel metin okuma sesli oluşturmaya hazır olduğunuzda, ilk adım sesli kayıtlar ve ilişkili betikler ses modeli eğitmek başlatmak için toplamaktır. Hizmet, benzersiz bir ses kayıtları, ses eşleşecek şekilde ayarlanan oluşturmak için bu verileri kullanır. Ses eğittiğimize sonra uygulamalarınızda konuşma synthesizing başlayabilirsiniz.

Küçük bir kavram kanıtı oluşturmak için veri miktarı ile başlayabilirsiniz. Ancak, sağladığınız veri,, özel sesli ses daha çok doğal. Kendi sesli metin okuma modeli eğitmek önce ses kayıtlarını ve ilgili metin döküm gerekir. Bu sayfada şu veri türlerini, bunların nasıl kullanıldığı ve her yönetme inceleyelim.

## <a name="data-types"></a>Veri türleri

Sesli kayıtlar ve bir metin dosyası ile ilişkili döküm bir ses eğitim veri kümesi içerir. Her ses dosyası (tek veya tek bir cümle etkinleştirmek için bir iletişim sistemi) tek bir utterance içerir ve 15 saniyeden uzun olması gerekir.

Bazı durumlarda, doğru veri kümesi hazır olmayabilir ve özel bir üslup eğitimi kısa veya uzun olan veya olmayan dökümleri kullanılabilir ses dosyaları test etmek istersiniz. Ses konuşma segmentlere ayırın ve dökümleri kullanarak Hazırlama yardımcı olacak araçlar (beta) sağladığımız [Batch tanıma API'sini](batch-transcription.md).

Bu tablo veri türleri ve her bir özel metin okuma sesli model oluşturmak için nasıl kullanıldığını listeler.

| Veri türü | Açıklama | Kullanılması gereken durumlar | Gerekli diğer hizmet | Modeli için bir miktar | Locale(s) |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Tek tek konuşma + eşleşen döküm** | Bir koleksiyonu (.zip) ses dosyaları (.wav) olarak tek tek konuşma. 15 saniye veya biçimlendirilmiş bir döküm (.txt) ile eşleştirilmiş daha az, her bir ses dosyası olmalıdır. | Dökümler eşleşen profesyonel kayıtları | Eğitim için hazır. | En-US ve zh-CN için sabit bir gereksinim. Birden fazla 2000'den fazla diğer yerel ayarlar için ayrı konuşma. | Tüm özel sesli yerel |
| **Uzun ses + deşifre metni (beta)** | Eşleştirilmiş tüm konuşulan kelimeleri içeren bir dökümü ile (.txt) uzun, kesimli ses dosyaları (20 saniyeden daha uzun), koleksiyonu (.zip). | Ses dosyalarını ve eşleşen dökümleri gibi sahip olduğunuz, ancak konuşma bölümlenmiş değil. | Segment (batch transkripsiyonu kullanarak).<br>Ses biçimi dönüştürme gerekli olduğu durumlarda. | En-US ve zh-CN için sabit bir gereksinim. | `en-US` ve `zh-CN` |
| **Yalnızca ses (beta)** | Bir koleksiyonu (.zip) bir döküm olmadan ses dosyaları. | Ses dosyaları dökümleri kullanılabilir yeterlidir. | Segment + döküm oluşturma (batch transkripsiyonu kullanarak).<br>Ses biçimi dönüştürme gerekli olduğu durumlarda.| İçin sabit bir gereksinim `en-US` ve `zh-CN`. | `en-US` ve `zh-CN` |

Dosya türü bir veri kümesine göre gruplandırılmış ve karşıya bir zip dosyası. Her bir veri kümesi yalnızca bir tek veri türü içerebilir.

> [!NOTE]
> Maksimum abonelik başına içeri aktarılacak izin verilen veri kümeleri (F0) aboneliği kullanıcıları ve 500 standart aboneliği (S0) kullanıcıları için ücretsiz 10 .zip dosya sayısıdır.

## <a name="individual-utterances--matching-transcript"></a>Tek tek konuşma + eşleşen döküm

Kayıtları tek tek konuşma ve iki yolla eşleşen döküm hazırlayabilirsiniz. Ya da bir betik yazabilir ve bir ses beceri tarafından okunur veya herkese ses kullanın ve metin özelliği vardır. İkincisini yapmak, disfluencies "um" ve diğer filler sesleri, atlar, mumbled sözcükleri veya mispronunciations gibi ses dosyalarını düzenleyin.

İyi ses tipi üretmek için yüksek kaliteli bir mikrofon sessiz bir odada kayıtları oluşturun. Tutarlı birim oranı, aralık ve konuşma ifadesel veren davranışların gibi konuşma gerekli.

> [!TIP]
> Üretim kullanımı için bir ses oluşturmak için profesyonel kaydı studio ve ses beceri kullanmanızı öneririz. Daha fazla bilgi için [ses kaydetmek nasıl özel bir ses için örnekleri](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/record-custom-voice-samples).

### <a name="audio-files"></a>Ses dosyaları

Her ses dosyası (tek bir cümle veya tek bir kapatma bir iletişim sistemi), tek bir utterance 15 saniyeden uzun içermelidir. Tüm dosyalar aynı konuşulan dili içinde olmalıdır. Çok dilli özel seslerle metin okuma, Çince-İngilizce çift dilli dışında desteklenmez. Her ses dosyasının .wav dosya adı uzantılı dosyalar benzersiz sayısal dosya adı olmalıdır.

Ses hazırlarken aşağıdaki yönergeleri izleyin.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | Bir .zip dosyasına gruplandırılmış RIFF (.wav) |
| Örnekleme hızı | en az 16. 000 Hz |
| Örnek Biçim | PCM, 16-bit |
| Dosya adı | Sayısal, .wav uzantısına sahip. Yinelenen dosya adlarına izin. |
| Ses uzunluğu | 15 saniyeden daha kısa |
| Arşiv biçimi | .zip |
| En uzun arşiv boyutu | 200 MB |

> [!NOTE]
> .wav dosyaları 16.000 Hz reddedilir daha düşük bir örnekleme hızı. Bir .zip dosyası farklı bir örnek hızlara .wav dosyaları içeriyorsa, yalnızca eşit veya daha yüksek 16.000 Hz aktarılır. Portal şu anda içeri aktarmalar .zip 200 MB'a kadar arşivler. Ancak, birden fazla arşivini karşıya yüklenebilir.

### <a name="transcripts"></a>Dökümleri

Düz metin dosyası döküm dosyasıdır. Döküm hazırlamak için aşağıdaki yönergeleri kullanın.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | Düz metin (.txt) |
| Kodlama biçimi | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE veya UTF-16 olabilir. ANSI/ASCII ve UTF-8 Kodlamalar, zh-CN, desteklenmiyor. |
| Satır başına konuşma sayısı | **Bir** -her satır döküm dosyasının karşılık gelen transkripsiyonu tarafından izlenen ses dosyaları, birinin adını içermelidir. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır. |
| En büyük dosya boyutu | 50 MB |

Dökümler utterance tarafından utterance bir .txt dosyasına nasıl düzenlenir, bir örnek aşağıda verilmiştir:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Dökümler ilgili ses dosyasının doğru döküm % 100 olduğunu önemlidir. Dökümler hataları kalite kaybı eğitim sırasında başlatacaktır.

> [!TIP]
> Üretim uygulamasına alan metin okuma sesleri, konuşma seçin (veya yazma betikleri) oluştururken fonetik kapsamı hem verimlilik hesap. Sonuçları aşmakta sorun istiyor musunuz? [Özel ses başvurun](mailto:speechsupport@microsoft.com) bize sahip hakkında daha fazla inceleyin çıkış bulmak için takım.

## <a name="long-audio--transcript-beta"></a>Uzun ses + deşifre metni (beta)

Bazı durumlarda, ses kullanılabilir segmentlere değil. Uzun ses dosyaları segmentlere ayırın ve döküm oluşturmanıza yardımcı olmak için özel sesli Portalı aracılığıyla (beta) bir hizmet sunuyoruz. Tutun göz önünde bulundurun konuşma metin abonelik kullanımınızı doğru bu hizmeti ücretlendirilecektir.

> [!NOTE]
> Uzun ses Segment hizmet konuşma-yalnızca standart aboneliği (S0) kullanıcıları destekleyen metin, batch tanıma özelliği özelliğinden yararlanır. Segmentlere ayırma işlemi sırasında dosyalarınızı ses ve dökümler ayrıca özel konuşma hizmeti için tanıma modeli doğruluğu, verileriniz için geliştirilebilir şekilde daraltmak için gönderilir. Veri yok, bu işlem sırasında korunur. Segment yapıldıktan sonra indirme ve eğitim için yalnızca bölümlenmiş konuşma ve kendi eşleme Dökümleri depolanır.

### <a name="audio-files"></a>Ses dosyaları

Ses kesimlemesi için hazırlarken aşağıdaki yönergeleri izleyin.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | En az 16 khz 16-bit PCM veya .mp3 örnekleme oranını en az 256 bir .zip dosyasına gruplandırılmış Kbps, bit hızı ile RIFF (.wav) |
| Dosya adı | Yalnızca ASCII karakterler. Adında Unicode karakterler başarısız olur (örneğin, Çince karakterler veya benzeri semboller gibi "-"). Yinelenen adlara izin verilir. |
| Ses uzunluğu | 20 daha uzun saniye |
| Arşiv biçimi | .zip |
| En uzun arşiv boyutu | 200 MB |

Tüm ses dosyaları bir zip dosyası olarak gruplandırılmalıdır. .Wav dosyaları ve .mp3 dosyalarını ses bir ZIP dosyasına koymak için bir sorun yoktur, ancak hiçbir alt klasör zip dosyasında izin verilir. Örneğin, 'kingstory.wav', 45 saniye sürdüğü ve başka bir adlandırılmış 'queenstory.mp3', 200-ikinci-herhangi bir alt klasör olmadan uzun, adlandırılmış bir ses dosyasını içeren bir zip dosyası karşıya yükleyebilirsiniz. Tüm .mp3 dosyalar .wav biçime işlendikten sonra dönüştürülür.

### <a name="transcripts"></a>Dökümleri

Bu tabloda listelenen belirtimlerine dökümleri hazırlanması gerekir. Her ses dosyası bir dökümü ile eşleşmelidir.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | Bir .zip gruplandırılmış düz metin (.txt) |
| Dosya adı | Eşleşen bir ses dosyası olarak aynı adı kullanın |
| Kodlama biçimi | UTF-8-BOM yalnızca |
| Satır başına konuşma sayısı | Sınırsız |
| En büyük dosya boyutu | 50 MİLYON |

Bu veri türü tüm dökümleri dosyaları bir zip dosyası olarak gruplandırılmalıdır. Hiçbir alt klasör zip dosyasında izin verilir. Örneğin, 45 saniye uzunluğundaki 'kingstory.wav' adlı bir ses dosyasını içeren bir zip dosyası karşıya yüklediğiniz ve başka bir adlandırılmış 'queenstory.mp3', 200 uzun saniye. Başka bir zip dosyası içeren iki dökümleri, bir adlandırılmış 'kingstory.txt', başka bir 'queenstory.txt' yüklemeniz gerekir. Her bir düz metin dosyası içinde doğru tam döküm eşleşen ses için sağlar.

Veri kümeniz başarıyla karşıya yüklendikten sonra ses dosyası konuşma sağlanan çeviri yazıya dayalı olarak segmentlere yardımcı olur. Veri kümesini yükleyerek segmentli konuşma ve eşleşen dökümleri kontrol edebilirsiniz. Benzersiz kimliklerinin segmentli konuşma otomatik olarak atanır. Sağladığınız dökümleri % 100 doğru olduğundan emin olmak önemlidir. Dökümleri hataları, ses ayrılmasını sırasında doğruluğu azaltmak ve daha fazla kalite kaybı daha sonra geldiğinde eğitim aşamasında tanıtır.

## <a name="audio-only-beta"></a>Yalnızca ses (beta)

Ses kayıtlarınızı döküm sahip değilseniz kullanmak **yalnızca ses** verilerinizi yüklemek için seçeneği. Sistemimiz, segmentlere ayırın ve ses dosyalarınızı konuşmaların yardımcı olabilir. Unutmayın, bu hizmet, Konuşmayı metne abonelik kullanımınızı doğru sayılır.

Ses hazırlarken aşağıdaki yönergeleri izleyin.

> [!NOTE]
> Uzun ses Segment hizmet konuşma-yalnızca standart aboneliği (S0) kullanıcıları destekleyen metin, batch tanıma özelliği özelliğinden yararlanır.

| Özellik | Değer |
| -------- | ----- |
| Dosya biçimi | En az 16 khz 16-bit PCM veya .mp3 örnekleme oranını en az 256 bir .zip dosyasına gruplandırılmış Kbps, bit hızı ile RIFF (.wav) |
| Dosya adı | Yalnızca ASCII karakterler. Adında Unicode karakterler başarısız olur (örneğin, Çince karakterler veya benzeri semboller gibi "-"). Yinelenen ad izin verilir. |
| Ses uzunluğu | 20 daha uzun saniye |
| Arşiv biçimi | .zip |
| En uzun arşiv boyutu | 200 MB |

Tüm ses dosyaları bir zip dosyası olarak gruplandırılmalıdır. Hiçbir alt klasör zip dosyasında izin verilir. Veri kümeniz başarıyla karşıya yüklendikten sonra ses dosyası konuşma konuşma batch transkripsiyonu hizmetimiz dayalı olarak segmentlere yardımcı olur. Benzersiz kimliklerinin segmentli konuşma otomatik olarak atanır. Dökümler eşleşen konuşma tanıma aracılığıyla oluşturulur. Tüm .mp3 dosyalar .wav biçime işlendikten sonra dönüştürülür. Veri kümesini yükleyerek segmentli konuşma ve eşleşen dökümleri kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel ses oluşturma](how-to-custom-voice-create-voice.md)
- [Kılavuzu: Ses örneklerinizi kaydedin](record-custom-voice-samples.md)
