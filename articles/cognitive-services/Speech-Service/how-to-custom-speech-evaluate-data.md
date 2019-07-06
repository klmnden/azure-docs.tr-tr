---
title: Özel konuşma tanıma - konuşma Hizmetleri doğruluğu değerlendir
titlesuffix: Azure Cognitive Services
description: Bu belgede, Microsoft'un Konuşmayı metne modeli veya özel modelinizin kalitesini quantitatively ölçmek öğreneceksiniz. Ses + insan etiketli transkripsiyonu veri doğruluğunu test etmek için gereklidir ve temsili bir ses 5 saat 30 dakika sağlanmalıdır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 2e9818fad9a0b5d04cc50a293b16d838c319dd86
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606563"
---
# <a name="evaluate-custom-speech-accuracy"></a>Özel konuşma doğruluğunu değerlendir

Bu belgede, Microsoft'un Konuşmayı metne modeli veya özel modelinizin kalitesini quantitatively ölçmek öğreneceksiniz. Ses + insan etiketli transkripsiyonu veri doğruluğunu test etmek için gereklidir ve temsili bir ses 5 saat 30 dakika sağlanmalıdır.

## <a name="what-is-word-error-rate-wer"></a>Word hata oranı (WER) nedir?

Model doğruluğu ölçmek için endüstri standardıdır *Word hata oranı* (WER). WER'i tanıma sırasında belirlenen yanlış sözcükleri sayar ve ardından bir kelimelerin İnsan etiketli transkripti sağlanan toplam sayı böler. Son olarak, bu sayıyı WER hesaplamak için % 100 ile çarpılır.

![WER'i formülü](./media/custom-speech/custom-speech-wer-formula.png)

Hatalı olarak üç kategoriye ayrılır sözcükleri tanımlanır:

* Ekleme (ı): Yanlış varsayım transkripti eklenmiş olan sözcükler
* Silme (D): Hipotezin transkripti algılanmayan sözcükler
* Değiştirme (S): Başvuru ile varsayım arasında yerine sözcükler

Bir örneği aşağıda verilmiştir:

![Örnek hatalı tanımlanmış bir kelimelerin](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Hataları giderin ve WER'e geliştirin

WER'i makine tanıma sonuçlarından, uygulama, aracı veya ürün kullanarak model kalitesini değerlendirmek için kullanabilirsiniz. WER'i bir % 5-%10, kaliteli olduğu kabul edilir ve kullanıma hazır. Ek eğitim isteyebilirsiniz ancak bir WER % 20 kullanılabilir. Bir WER % 30 veya daha düşük kaliteli bildirir ve özelleştirme ve eğitim gerektirir.

Hataları nasıl dağıtıldığını önemlidir. Birçok silme hatalarıyla karşılaşıldı, genellikle zayıf ses sinyal gücü nedeniyle olur. Bu sorunu çözmek için kaynak yaklaştırarak ses verilerini toplamak gerekir. Ekleme hataları ses gürültülü ortamında kaydedilmiştir ve söze tanıma sorunlarına neden olan mevcut olabilir anlamına gelir. Etki alanına özgü koşulları yetersiz bir örnek olarak ya da insan etiketli döküm sağlanan veya ilgili, değiştirme hatalarla karşılaştı genellikle metin.

Tek tek dosyaları analiz ederek, hangi türde hataları var ve hangi hataları belirli bir dosyaya benzersiz belirleyebilirsiniz. Dosya düzeyinde anlama sorunları size yardımcı olacak hedef geliştirmeleri.

## <a name="create-a-test"></a>Test oluşturma

Microsoft'un Konuşmayı metne temel modeli veya eğitim almış özel bir model kalitesini test etmek istiyorsanız, iki modeli doğruluğu değerlendirmek için yan yana karşılaştırabilirsiniz. Karşılaştırma WER ve tanıma sonuçları içerir. Genellikle, özel bir model, Microsoft'un temel modeli ile karşılaştırılır.

Modeli yan yana değerlendirmek için:

1. Gidin **konuşma metin > özel konuşma > test**.
2. Tıklayın **Test Ekle**.
3. Seçin **doğruluğu değerlendirmek**. Test açıklaması, bir ad verin ve ses + insan etiketli transkripsiyonu kümenizi seçin.
4. Test etmek istediğiniz en fazla iki modeli seçin.
5. **Oluştur**’a tıklayın.

Test başarılı bir şekilde oluşturulduktan sonra sonuçları yan yana karşılaştırabilirsiniz.

## <a name="side-by-side-comparison"></a>Yan yana karşılaştırma

Test tamamlandıktan sonra durum değişikliği için belirttiği *başarılı*, test çalıştırmanızda dahil her iki modelleri için WER birkaç bulabilirsiniz. Test ayrıntı sayfası görüntülemek için test adına tıklayın. Bu Ayrıntıları sayfası, tüm konuşma transkripsiyonu gönderilen veri kümesinden yanı sıra iki modeli tanıma sonuçları gösteren veri kümenizde, listeler. Yan yana karşılaştırmayı denetlemek amacıyla, ekleme, silme ve değiştirme gibi çeşitli hata türleri geçiş yapabilirsiniz. Ses için dinleme ve tanıma sonuçları her sütunda İnsan etiketli transkripsiyonu gösterir ve iki konuşma metin model için sonuçları karşılaştırma hangi modelle ihtiyaçlarınıza uygun ve ek eğitim ve geliştirmeleri olduğu karar verebilirsiniz Gerekli.

## <a name="next-steps"></a>Sonraki adımlar

* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Hazırlama ve test, verileri](how-to-custom-speech-test-data.md)
* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
