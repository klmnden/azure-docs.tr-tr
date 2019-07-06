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
ms.openlocfilehash: 7bc1f2adf3d8c38845218681a55e3942aa809db5
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603197"
---
# <a name="train-a-model-for-custom-speech"></a>Özel konuşma modeli eğitme

Eğitim bir konuşmayı metne dönüştürme hem Microsoft'un temel model veya oluşturmak için planlama özel bir model için tanıma doğruluğunu artırmak gereklidir. Bir model, İnsan etiketli döküm ve ilgili metin kullanılarak çalıştırılır. Daha önce yüklenen ses verileriyle birlikte bu veri kümeleri, geliştirin ve sözcükler, ifadeleri, kısaltmalar, adları ve diğer ürüne özgü koşulları tanımak için Konuşmayı metne modeli eğitmek için kullanılır. Geliştirilmiş tanıma sonuçları modelinizi olacak (hangi kullanıcıların kilit ekranında ve tanımak beklediğiniz ilgili veri) sağlayan etki alanındaki daha veri kümelerini daha doğru. Eğitim içine ilgisiz verilerin besleme tarafından azaltabilir veya model doğruluğunu zor olduğunu aklınızda bulundurun.

## <a name="use-training-to-resolve-accuracy-issues"></a>Doğruluk sorunları gidermek için eğitim kullanın

İnsan etiketli kullanarak modelinizi tanıma sorunları istemcilerinizle dökümleri ve ilgili verileri ek eğitim doğruluğunu artırmak için yardımcı olabilir. Hangi veri kümesinde, sorunu/sorunları ele almak için belirlemek için bu tabloyu kullanın:

| Kullanım örneği | Veri türü | Veri miktarı |
|----------|-----------|---------------|
| Tam adlar ifadenin | İlgili metin (cümleler/konuşma) | 10 MB-500 MB |
| Sözcükleri nedeniyle bir Vurgu ifadenin | İlgili metin (telaffuz) | Misrecognized sözcükleri girin |
| Ortak kelimeler silindi veya ifadenin | Ses + insan etiketli dökümleri | 10 ila 1.000 transkripsiyonu saat |

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
