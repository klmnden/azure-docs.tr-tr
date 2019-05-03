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
ms.openlocfilehash: 6602eb4bacdc3b6382c1b6873a465cdfc0632693
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029348"
---
# <a name="algorithm--module-reference-overview"></a>Algoritma ve modül başvurusu genel bakış

Bu başvuru içeriği Teknik bilgiye her makine öğrenimi algoritmaları ve modülleri kullanılabilir Azure Machine Learning hizmeti (Önizleme) görsel arabirim sağlar. 

Her modülün bağımsız olarak çalıştırılabilir ve bir makine gerekli girişlere görev, öğrenme gerçekleştirmek kodu kümesini temsil eder. Bir modül belirli algoritma içerebilir veya makine öğrenimi, eksik değer değiştirme ya da istatistiki analiz gibi önemli bir görevi gerçekleştirmek. 

> [!TIP]
> Görsel arabirim içinde herhangi bir deneme içinde belirli bir modülle ilgili bilgileri alabilirsiniz. Modülü seçin ve ardından **daha fazla Yardım** bağlantısını **hızlı Yardım** bölmesi.

Modüller işlevselliğe göre düzenlenmiştir:

**Veri biçimi dönüştürme**

  + [CSV'ye Dönüştür ](convert-to-csv.md)

**Giriş ve çıkış veri modülleri** denemenize bulut kaynaklardan veri taşıma işlemlerini yapın. Sonuçları veya Ara veriler Azure depolama, SQL veritabanı veya Hive, bir denemeyi çalıştırırken yazma ya da denemeler arasında veri değişimi için bulut depolama kullanın.  

  + [Verileri İçeri Aktar](import-data.md)

  + [Verileri dışarı aktarma](export-data.md)

  + [Verileri el ile girin](enter-data-manually.md)


**Veri dönüştürme modülleri** normalleştirme veya gruplama veri, özellik seçimi ve boyut düzeyi azaltma gibi makine öğrenimine benzersiz olan veriler üzerinde işlem desteği.

  + [Veri kümesindeki sütunları Seç](select-columns-in-dataset.md)

  + [Meta verilerini düzenleme](edit-metadata.md)

  + [Eksik verileri temizleme](clean-missing-data.md)

  + [Sütun Ekle](add-columns.md)

  + [Satır ekleme](add-rows.md)

  + [Yinelenen satırları Kaldır](remove-duplicate-rows.md)

  + [Verileri bölme](split-data.md)

  + [Veri normalleştirin](normalize-data.md)

  + [Bölüm ve örnek](partition-and-sample.md)


**Makine öğrenimi algoritmaları** kümeleme, Destekli vektör makinesi veya sinir ağları gibi uygun parametrelerle machine learning görev özelleştirmenize olanak tanıyan tek tek modüllerinin içinde kullanılabilir.  
  + [Model Puanlama](score-model.md)

  + [Veri kümelerine atama ](assign-data-to-clusters.md)

  + [Modeli eğitme](train-model.md)

  + [Kümeleme modeli eğitme](train-clustering-model.md)

  + [Modeli değerlendirme](evaluate-model.md)

  + [Dönüşüm Uygulama](apply-transformation.md)

  + [Doğrusal regresyon](linear-regression.md)

  + [Sinir ağı regresyon](neural-network-regression.md)

  + [Karar ormanı regresyon](decision-forest-regression.md)

  + [Artırmalı karar ağacı regresyonu](boosted-decision-tree-regression.md)

  + [İki sınıflı Artırmalı karar ağacı](two-class-boosted-decision-tree.md)

  + [İki sınıflı Lojistik regresyon](two-class-logistic-regression.md)

  + [Çok sınıflı Lojistik regresyon](multiclass-logistic-regression.md)

  + [Çok sınıflı sinir ağı](multiclass-neural-network.md)

  + [Veya çoklu sınıflar karar ormanı](multiclass-decision-forest.md)

  + [İki sınıflı Perceptron ortalaması](two-class-averaged-perceptron.md)

  + [İki sınıflı karar ormanı](two-class-decision-forest.md)

  + [İki sınıflı sinir ağı](two-class-neural-network.md)

  + [İki sınıflı destekli vektör makinesi](two-class-support-vector-machine.md)
  
  + [K-ortalamaları kümeleme](k-means-clustering.md)


**Python Modülü** özel bir işlev çalıştırmayı kolaylaştırır. Kod yazma ve Python bir deneme hizmeti ile tümleştirmek için bir modül ekleyin.
  + [Python betiği yürütme](execute-python-script.md)

  + [Python modeli oluşturma](create-python-model.md)


