---
title: Bir model eğitip özel konuşma tanıma - konuşma Hizmetleri
titlesuffix: Azure Cognitive Services
description: Eğitim bir konuşmayı metne dönüştürme hem Microsoft'un temel model veya oluşturmak için planlama özel bir model için tanıma doğruluğunu artırmak gereklidir. Bir model, İnsan etiketli döküm ve ilgili metin kullanılarak çalıştırılır. Daha önce yüklenen ses verileriyle birlikte bu veri kümeleri, geliştirin ve sözcükler, ifadeleri, kısaltmalar, adları ve diğer ürüne özgü koşulları tanımak için Konuşmayı metne modeli eğitmek için kullanılır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 49195efa54c8e6eca9186a9e2fc33da84ff34413
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625378"
---
# <a name="train-a-model-for-custom-speech"></a>Özel konuşma modeli eğitme

Eğitim bir konuşmayı metne dönüştürme hem Microsoft'un temel model veya oluşturmak için planlama özel bir model için tanıma doğruluğunu artırmak gereklidir. Bir model, İnsan etiketli döküm ve ilgili metin kullanılarak çalıştırılır. Daha önce yüklenen ses verileriyle birlikte bu veri kümeleri, geliştirin ve sözcükler, ifadeleri, kısaltmalar, adları ve diğer ürüne özgü koşulları tanımak için Konuşmayı metne modeli eğitmek için kullanılır. Geliştirilmiş tanıma sonuçları modelinizi olacak (hangi kullanıcıların kilit ekranında ve tanımak beklediğiniz ilgili veri) sağlayan etki alanındaki daha veri kümelerini daha doğru. Eğitim içine ilgisiz verilerin besleme tarafından azaltabilir veya model doğruluğunu zor olduğunu aklınızda bulundurun.

## <a name="use-training-to-resolve-accuracy-issues"></a>Doğruluk sorunları gidermek için eğitim kullanın

İnsan etiketli kullanarak modelinizi tanıma sorunları istemcilerinizle dökümleri ve ilgili verileri ek eğitim doğruluğunu artırmak için yardımcı olabilir. Hangi veri kümesinde, sorunu/sorunları ele almak için belirlemek için bu tabloyu kullanın:

| Kullanım örneği | Veri türü |
|----------|-----------|
| Sektöre özel sözlük ve tıbbi terminolojisi veya BT terminolojisinin gibi dil bilgisi tanıma doğruluğunu artırmak | İlgili metin (cümleler/konuşma) |
| Bir sözcük veya ürün adlarına veya kısaltmalar gibi standart olmayan telaffuz olan terim fonetik ve görüntülenen form tanımlayın. | İlgili metin (telaffuz) |
| Stiller, vurgular veya belirli bir arka plan gürültüleri Konuşmayı şirket tanıma doğruluğunu artırmak | Ses + insan etiketli dökümleri |
> [!IMPORTANT]
> Bir veri kümesi yüklemediyseniz, bkz. Lütfen [hazırlayın ve verilerinizi test](how-to-custom-speech-test-data.md). Bu belge, veri ve yüksek kaliteli veri kümeleri oluşturma yönergeleri için yönergeler sağlar.

## <a name="train-and-evaluate-a-model"></a>Eğitme ve modeli değerlendirme

Bir model eğitip ilk adımı, eğitim verilerini karşıya yükleme sağlamaktır. Kullanım [hazırlayın ve verilerinizi test](how-to-custom-speech-test-data.md) İnsan etiketli döküm ve ilgili metin (konuşma ve Söyleniş) hazırlamak adım adım yönergeler için. Eğitim verilerini yükledikten sonra modeli eğitimi başlatmak için bu yönergeleri izleyin:

1. Gidin **konuşma metin > özel konuşma > Eğitim**.
2. Tıklayın **modeli eğitme**.
3. Ardından, eğitim vermek bir **adı** ve **açıklama**.
4. Gelen **senaryo ve temel modeli** açılan menüsünde, etki alanınız için en uygun senaryoyu belirleyin. Hangi senaryo seçmek için emin değilseniz seçin **genel**. Eğitim için başlangıç noktası temel modelidir. Bir tercih yoksa, en son kullanabilirsiniz.
5. Gelen **seçin eğitim verilerini** eğitim için kullanmak istediğiniz bir veya birden çok ses + insan etiketli transkripsiyonu veri kümeleri seçin.
6. Alıştırma tamamlandıktan sonra doğruluk yeni eğitilen model üzerinde test gerçekleştirmeye seçebilirsiniz. Bu adım isteğe bağlıdır.
7. Seçin **Oluştur** özel modelinizi oluşturmak için.

Bu yeni modeli oluşturulan eğitim tablo karşılık gelen yeni bir giriş görüntüler. Tablo, ayrıca durumu görüntüler:  İşlem, başarılı, başarısız.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Eğitilen bir modelin doğruluğunu değerlendir

Verileri İnceleme ve bu belgeleri kullanarak doğruluğu değerlendirin:

* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)


Doğruluğunu test etmek seçerseniz, modelinizi modelin performans gerçekçi bir fikir almak için kullanılan olandan farklı bir akustik veri kümesi seçmeniz önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Modelinizi dağıtma](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Hazırlama ve test, verileri](how-to-custom-speech-test-data.md)
* [Verilerinizi denetleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
