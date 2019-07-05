---
title: Algoritma ve modül başvurusu
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning visual arabiriminde modülleri hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: e73d4ebd3eb05f7cf217573d8112e3dbbe6d3a37
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514069"
---
# <a name="algorithm--module-reference-overview"></a>Algoritma ve modül başvurusu genel bakış

Bu başvuru içeriği Teknik bilgiye her makine öğrenimi algoritmaları ve modülleri kullanılabilir Azure Machine Learning hizmeti (Önizleme) görsel arabirim sağlar.

Her modülün bağımsız olarak çalıştırılabilir ve bir makine gerekli girişlere görev, öğrenme gerçekleştirmek kodu kümesini temsil eder. Bir modül belirli algoritma içerebilir veya makine öğrenimi, eksik değer değiştirme ya da istatistiki analiz gibi önemli bir görevi gerçekleştirmek.

> [!TIP]
> Görsel arabirim içinde herhangi bir deneme içinde belirli bir modülle ilgili bilgileri alabilirsiniz. Modülü seçin ve ardından **daha fazla Yardım** bağlantısını **hızlı Yardım** bölmesi.

## <a name="modules"></a>Modüller

Modüller işlevselliğe göre düzenlenmiştir:

| İşlevi | Açıklama | Modül |
| --- |--- | ---- |
| Veri biçimi dönüştürmeleri | Verileri makine öğrenimi kullanılan çeşitli dosya biçimleri arasında dönüştürme, | [CSV'ye Dönüştür](convert-to-csv.md) |
| Veri giriş ve çıkış | Verileri bulut kaynaklardan denemenize taşıyın. Sonuçları veya Ara veriler Azure depolama, SQL veritabanı veya Hive, bir denemeyi çalıştırırken yazın veya denemeler arasında veri değişimi için bulut depolama kullanın.  | [Verileri İçeri Aktar](import-data.md)<br/>[Verileri dışarı aktarma](export-data.md)<br/>[Verileri el ile girin](enter-data-manually.md) |
| Veri dönüştürme | Machine learning, normalleştirme veya veri, özellik seçimi ve boyut düzeyi azaltma gruplama gibi benzersiz olan veriler üzerinde işlem.| [Veri kümesindeki sütunları Seç](select-columns-in-dataset.md) <br/> [Meta verilerini düzenleme](edit-metadata.md) <br/> [Eksik verileri temizleme](clean-missing-data.md) <br/> [Sütun Ekle](add-columns.md) <br/> [Satır ekleme](add-rows.md) <br/> [Yinelenen satırları Kaldır](remove-duplicate-rows.md) <br/> [Veri birleştirme](join-data.md) <br/> [Verileri bölme](split-data.md) <br/> [Veri normalleştirin](normalize-data.md) <br/> [Bölüm ve örnek](partition-and-sample.md) |
| Python ve R modüllerini | Kod yazma ve Python ve R ile denemenizi tümleştirmek için bir modüle ekleyin. | [Python betiği yürütme](execute-python-script.md)   <br/> [Python modeli oluşturma](create-python-model.md) <br/> [R betiği yürütmek](execute-r-script.md)
|  | **Makine öğrenimi algoritmaları**: | |
| Sınıflandırma | Bir sınıf tahmin edin.  İkili (iki sınıflı) seçin veya çok sınıflı algoritmaları.| [Veya çoklu sınıflar karar ormanı](multiclass-decision-forest.md) <br/> [Çok sınıflı Lojistik regresyon](multiclass-logistic-regression.md)  <br/> [Çok sınıflı sinir ağı](multiclass-neural-network.md)  <br/>  [İki sınıflı Lojistik regresyon](two-class-logistic-regression.md)  <br/>[İki sınıflı Perceptron ortalaması](two-class-averaged-perceptron.md) <br/> [İki sınıflı&nbsp;artırılmış&nbsp;karar&nbsp;ağacı](two-class-boosted-decision-tree.md)  <br/> [İki sınıflı karar ormanı](two-class-decision-forest.md)  <br/> [İki sınıflı sinir ağı](two-class-neural-network.md)  <br/> [İki&#8209;sınıfı&nbsp;Destek&nbsp;vektör&nbsp;makine](two-class-support-vector-machine.md) 
| Kümeleme | Veri gruplandırın.| [K-ortalamaları kümeleme](k-means-clustering.md)
| Regresyon | Bir değeri tahmin edin. | [Doğrusal regresyon](linear-regression.md)  <br/> [Sinir ağı regresyon](neural-network-regression.md)  <br/> [Karar ormanı regresyon](decision-forest-regression.md)  <br/> [Artırılmış&nbsp;karar&nbsp;ağaç&nbsp;regresyon](boosted-decision-tree-regression.md)
|  | **Yapı ve model değerlendirme**: | |
| Eğitim   | Veri algoritması üzerinden çalıştırın. | [Modeli eğitme](train-model.md)  <br/> [Kümeleme modeli eğitme](train-clustering-model.md)    |
| Modeli Değerlendirme | Eğitilen modelin doğruluğunu ölçer. |  [Modeli değerlendirme](evaluate-model.md)
| Puan | Yalnızca eğitilmiş modelden Öngörüler edinin. | [Dönüşüm Uygulama](apply-transformation.md)<br/>[Ata&nbsp;veri&nbsp;için&nbsp;kümeleri](assign-data-to-clusters.md) <br/>[Model Puanlama](score-model.md)

## <a name="error-messages"></a>Hata iletileri

Hakkında bilgi edinin [hata iletileri ve özel durum kodları](machine-learning-module-error-codes.md) modüllerini kullanarak Azure Machine Learning hizmeti visual arabiriminde karşılaşabilirsiniz.
