---
title: Özel konuşma tanıma - konuşma Hizmetleri test verilerini hazırladınız
titlesuffix: Azure Cognitive Services
description: Olup olmadığını nasıl doğru Microsoft konuşma tanıma görmek için test ettiğiniz olduğu veya kendi modellerin Eğitimi, verilerini (ses ve/veya metin biçiminde) gerekir. Bu sayfada, veri, nasıl kullanıldıkları ve bunların nasıl yönetileceğinin türlerini biz karşılarız.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: de2f1009c574d9768330d4e6a38a219ba1f81daa
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237948"
---
# <a name="prepare-data-for-custom-speech"></a>Özel konuşma için verileri hazırlama

Olup olmadığını nasıl doğru Microsoft konuşma tanıma görmek için test ettiğiniz olduğu veya kendi modellerin Eğitimi, ses ve metin biçiminde veri gerekir. Bu sayfada, veri, nasıl kullanıldıkları ve bunların nasıl yönetileceğinin türlerini biz karşılarız.

## <a name="data-types"></a>Veri türleri

Bu tablo, her veri türü kullanılmalıdır, kabul edilen veri türleri ve önerilen miktar listeler. Her veri türü, bir model oluşturmak için gereklidir. Veri gereksinimleri, test oluşturma veya modeli bağlı olarak değişir.

| Veri türü | Test kullanılan | Miktar | Eğitim için kullanılan | Miktar |
|-----------|-----------------|----------|-------------------|----------|
| [Ses](#audio-data-for-testing) | Evet<br>Görsel denetim için kullanılan | 5 + ses dosyaları | Hayır | yok |
| [Ses + insan etiketli dökümleri](#audio--human-labeled-transcript-data-for-testingtraining) | Evet<br>Doğruluk değerlendirmek için kullanılan | 0,5 - 5 saatlik ses | Evet | 1 - 1000 saatlik ses |
| [İlgili metin](##related-text-data-for-training) | Hayır | yok | Evet | İlgili metin 1-200 MB |

Dosya türü bir veri kümesine göre gruplandırılmış ve karşıya bir zip dosyası. Her bir veri kümesi yalnızca bir tek veri türü içerebilir.

## <a name="upload-data"></a>Karşıya veri yükleme

Verilerinizi yüklemeye hazır olduğunuzda, tıklayın **verileri karşıya yükleme** sihirbazını başlatmak ve ilk kümenizi oluşturmak için. Verilerinizi karşıya izin vermeden önce veri kümesi için bir konuşma veri türü seçin istenir.

![Konuşma tanıma Portalı'ndan ses seçin](./media/custom-speech/custom-speech-select-audio.png)

Karşıya yüklediğiniz her bir veri kümesi, seçtiğiniz veri türü için gereksinimleri karşılaması gerekir. Intune'ye yüklenmeden önce verilerinizi doğru şekilde biçimlendirmek önemlidir. Bu verileri doğru bir şekilde özel konuşma hizmeti tarafından işlenecek sağlar. Gereksinimleri, aşağıdaki bölümlerde listelenmiştir.

Veri kümeniz karşıya yüklendikten sonra birkaç seçeneğiniz vardır:

* Gidebilirsiniz **test** sekmesini ve yalnızca ses veya ses + insan etiketli transkripsiyonu verileri görsel olarak inceleyin.
* Gidebilirsiniz **eğitim** sekmesi ve bize ses + insan transkripsiyonu veri ilgili metin verilerini veya özel bir modeli eğitmek için.

## <a name="audio-data-for-testing"></a>Test için ses verisi

Ses verisi, Microsoft'un temel konuşma metin model veya özel bir model doğruluğunu test etmek için idealdir. Göz önünde bulundurun, ses veri modelinin belirli performans bakımından konuşma doğruluğunu denetlemek için kullanılır. Bir model doğruluğunu ölçer arıyorsanız, kullanın [ses + insan etiketli transkripsiyonu veri](#audio--human-labeled-transcript-data-for-testingtraining).

Özel Konuşma ile kullanmak için ses dosyaları doğru biçimlendirildiğinden emin olmak için bu tabloyu kullanın:

| Özellik | Değer |
|----------|-------|
| Dosya biçimi | RIFF (WAV) |
| Örnek hızı | 8000 Hz veya 16.000 Hz |
| Kanal Sayısı | 1 (mono) |
| Ses başına en fazla uzunluk | 2 saat |
| Örnek Biçim | PCM, 16-bit |
| Arşiv biçimi | .zip |
| En uzun arşiv boyutu | 2 GB |

Bu özellikler, ses karşılamadığı ya da bunu desteklemediğini kontrol etmek istediğiniz indirme öneririz [sox](http://sox.sourceforge.net) denetleyin veya dönüştürün. Komut satırı aracılığıyla bu etkinliklerin her biri nasıl yapılabilir ilişkin bazı örnekler aşağıdadır:

| Etkinlik | Açıklama | SOX komutu |
|----------|-------------|-------------|
| Ses biçimini denetleyin | Ses dosyası biçimini denetlemek için bu komutu kullanın. | `sox --i <filename>` |
| Ses biçimine dönüştürün | Ses dosyası tek kanal, 16 bit dönüştürmek için bu komutu kullanmak 16 KHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Ses + insan etiketli transkript verileri test etme/eğitim

Ses dosyaları işlenirken Microsoft'un Konuşmayı metne doğruluğu doğruluğunu ölçmek için İnsan etiketli deşifre (word-tarafından-word) karşılaştırma için sağlamanız gerekir. İnsan etiketli transkripsiyonu genellikle zaman alıcı olsa da, doğruluk ve değerlendirmek için kullanım örnekleri modeli eğitmek için gereklidir. Unutmayın, Tanıma'daki geliştirmelerin yalnızca sağlanan verileri kadar iyi olur. Bu nedenle, yalnızca yüksek kaliteli dökümleri yüklenir önemlidir.  

| Özellik | Değer |
|----------|-------|
| Dosya biçimi | RIFF (WAV) |
| Örnek hızı | 8000 Hz veya 16.000 Hz |
| Kanal Sayısı | 1 (mono) |
| Ses başına en fazla uzunluk | 60 s |
| Örnek Biçim | PCM, 16-bit |
| Arşiv biçimi | .zip |
| En fazla zip boyutu | 2 GB |

Word silme veya değiştirme, önemli miktarda veri tanımayı geliştirmek için gereken gibi ilgili sorunları gidermek için. Genellikle, yaklaşık 10 ila 1.000 saatlik ses word sözcüklü döküm sağlamak için önerilir. Tüm WAV dosyalarının transkripsiyonları tek bir düz metin dosyasına yerleştirilmelidir. Transkripsiyon dosyasının her satırında ses dosyalarından birinin adı ve transkripsiyon bulunmalıdır. Dosya adı ve transkripsiyon sekme (\t) ile ayrılmalıdır.

  Örneğin:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> Transkripsiyon UTF-8 bayt sırası işareti (BOM) ile kodlanmış olmalıdır.

Transkripsiyon metinleri sistem tarafından işlenebilmesi için normalleştirilir. Ancak veriler Özel Konuşma Tanıma Hizmeti'ne yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Uygun dili, döküm hazırlarken kullanmak bkz. [İnsan etiketli bir döküm oluşturma](how-to-custom-speech-human-labeled-transcriptions.md)

Ses dosyalarını ve karşılık gelen döküm topladığınız sonra bunlar tek bir .zip dosyası olarak özel konuşma tanıma Portalı'na yüklemeden önce paketlenmesi. Bu üç ses dosyalarını ve İnsan etiketli döküm dosyası içeren bir örnek veri kümesi.

![Konuşma tanıma Portalı'ndan ses seçin](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Eğitim için ilgili metin verileri

Benzersiz özellik veya ürün adlarına sahip ve doğru tanınır emin olmak istiyorsanız, ilgili metin verileri eğitim eklemek önemlidir. İki tür ilgili metin veri tanımayı geliştirmek için sağlanabilir:

| Veri türü | Bu veri tanıma nasıl artırır? |
|-----------|------------------------------------|
| Konuşma ve/veya tümceleri | Ürün adları veya sektöre özel sözlük bir cümle bağlamında tanımayı olduğunda bunlar doğruluğunu geliştirebilir. |
| Söyleniş | Bu, tanımsız Söyleniş ile seyrek koşulları, kısaltmalar veya başka bir deyişle Söyleniş artırabilir. |

Konuşma, tek bir veya birden çok metin dosyaları sağlanabilir. Ne söylenir şekilde verilerdir yaklaştıkça metin, doğruluk artırıldı olasılığını büyük. Söyleniş tek bir metin dosyası olarak sağlanmalıdır. Her şey tek bir zip dosyası olarak paketlenmeli ve özel konuşma tanıma Portalı'na yüklenen.

### <a name="guidelines-to-create-an-utterances-file"></a>Bir konuşma dosyası oluşturmak için yönergeleri

İlgili metin kullanarak özel bir model oluşturmak için örnek konuşma listesini sağlamanız gerekir. Bu konuşma bakımından düzeltin veya tam cümlelerden olması gerekmez, ancak üretimde beklediğiniz okunan giriş doğru şekilde yansıtmalıdır. Belirli terimleri kalınlığı derecesini artırdığınıza isterseniz ilgili verileri dosyanızı bu belirli koşulları dahil birkaç cümle ekleyebilirsiniz.

İlgili verileri dosyanız konuşma için doğru şekilde biçimlendirildiğinden emin olmak için bu tabloyu kullanın:

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM |
| Satır başına konuşma sayısı | 1 |
| En büyük dosya boyutu | 200 MB |

Ayrıca, hesap için aşağıdaki kısıtlamaları isteyeceksiniz:

* Dört kattan fazla karakter yinelemekten kaçının. Örneğin: "aaaa" veya "uuuu".
* Özel karakterler veya UTF-8 karakter U + 00A1 yukarıda kullanmayın.
* URI reddedilir.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Söyleniş dosyası oluşturmak için yönergeleri

Kullanıcılarınızın kullanın veya düzenleyeceği karşılaştığınız standart Söyleniş olmadan seyrek koşulları varsa, tanımayı geliştirmek için özel telaffuz dosya sağlayabilir.

> [!IMPORTANT]
> Ortak bir kelimelerin telaffuz değiştirmek için bu özelliği kullanmak için önerilmez.

Bu konuşmada geçen bir utterance ve özel telaffuz örnekleri her biri için içerir:

| Konuşulan formu | Kabul edilen ve görüntülenen form |
|--------------|--------------------------|
| üç c p o | 3CPO |  
| c n t k | CNTK |
| i Üçlü e | IEEE |

İl fonetik sıra konuşulan biçimidir. Harf, sözcük, hece veya bir birleşiminde oluşabilir.

Özelleştirilmiş telaffuz Almanca (de-DE) ve İngilizce (en-US) ile kullanılabilir. Bu tabloda dili tarafından desteklenen karakter gösterilmektedir:

| Dil | Yerel ayar | Karakterler |
|----------|--------|------------|
| İngilizce | en-US | a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |
| Almanca  | de-DE | ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z |

İlgili verileri dosyanız Söyleniş doğru biçimlendirildiğinden emin olmak için bu tabloyu kullanın. Söyleniş dosyaları, küçük ve birkaç KB'leri aşmamalıdır.

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM (ANSI İngilizce için desteklenir) |
| Söyleniş başına satır sayısı | 1 |
| En büyük dosya boyutu | 1 MB (ücretsiz katmanı için 1 KB) |

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)
