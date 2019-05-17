---
title: 'Regresyon: Tahmin ve algoritmaları karşılaştırın'
titleSuffix: Azure Machine Learning service
description: Bu makalede tek satırlık bir görsel arabirim kullanarak kod yazmadan karmaşık machine learning denemesi oluşturma gösterilmektedir. Eğitim ve teknik özelliklerini temel alarak bir arabanın fiyatını tahmin etmek için birden çok regresyon modeli hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: c8c813a2304797e71499a916e29c18f8bec2b389
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787807"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>2 - regresyon. örnek: Tahmin ve algoritmaları karşılaştırın

Tek satırlık bir görsel arabirim kullanarak kod yazmadan karmaşık machine learning denemesi oluşturmayı öğrenin. Bu örnek eğitir ve teknik özelliklerini alarak bir arabanın fiyatını tahmin etmek için birden fazla regresyon modeli karşılaştırır. Bu deneme kullanarak kendi makine öğrenimi sorunlarını gidermek şekilde yapılan seçimleri için stratejinin sağlarız.

Yalnızca machine Learning'i kullanmaya başlıyorsanız, göz atın [temel sürümü](ui-sample-regression-predict-automobile-price-basic.md) deneme temel bir regresyon görmek için bu deneyde.

Bu deneme için tamamlanan grafiği aşağıda verilmiştir:

[![Denemeyi grafiği](media/ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** düğmesi için örnek 2 deneme:

    ![Denemeyi açın](media/ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="experiment-summary"></a>Deneme özeti

Bir deneme oluşturmak için aşağıdaki adımları kullanın:

1. Verileri elde edersiniz.
1. Verileri önceden işleme.
1. Modeli eğitme.
1. Test, değerlendirin ve modelleri karşılaştırın.

## <a name="get-the-data"></a>Verileri alma

Bu deneyde kullandığımız **otomobil fiyat verileri (ham)** UCI Machine Learning depodan olan veri kümesi. Bu veri kümesi, otomobil üreticisi, modeli, fiyat, araç özellikleri (silindir sayısı gibi) MPG ve sigorta risk puanı da dahil olmak üzere ilgili bilgiler içeren 26 sütunlar içeriyor. Bu deneyde amaç, bir araba fiyatını tahmin etmektir.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayrılma veya boyutlandırma içerir. Bu işlemleri ve diğer veri görevlerinde önceden işleme gerçekleştirmek için modülleri görsel arabirim içinde bulabilirsiniz **veri dönüştürme** sol bölmedeki grubu.

Bu deneyde kullandığımız **kümesindeki sütunları seçme** normalleştirilmiş birçok eksik değerleri olan kayıplar dışlanacak modülü. Ardından kullandığımız **eksik verileri temizleme** eksik değerler içeren satırları kaldırmak için. Bu, temiz bir eğitim veri kümesi oluşturmak için yardımcı olur.

![Veri ön işleme](media/ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme

Machine learning sorunları farklılık gösterir. Sınıflandırma, kümeleme, regresyon ve her biri farklı bir algoritma gerektirebilir öneren sistemleri, ortak makine öğrenimi görevlerini içerir. Tercih ettiğiniz algoritması, genellikle kullanım örneği gereksinimlerine bağlıdır. Bir algoritma seçin sonra daha doğru bir model eğitip parametrelerini ayarlamak gerekir. Ardından tüm modellerin doğruluğu anlaşılabilirliği ve verimliliği gibi ölçümleri temel değerlendirmesi gerekir.

Bu deneyde amaç, otomobil fiyatlarını tahmin etmek için olduğundan ve gerçek sayılar (Fiyat) etiketi sütun içerdiği için bir regresyon modeli iyi bir seçimdir. Birçok özellik (daha az en az 100) görece küçük olduğunu ve bu özellikleri seyrek olduğunu dikkate alarak, karar sınır doğrusal olması olasıdır.

İki doğrusal algoritması kullanırız farklı algoritmalar performansını karşılaştırmak için **artırılmış karar ağacı regresyonu** ve **karar ormanı regresyon**, modelleri oluşturabilir. Her iki algoritmaları değiştirebileceğiniz parametrelere sahip, ancak bu deneme için varsayılan değerler kullanıyoruz.

Kullandığımız **verileri bölme** modülü rastgele giriş verileri özgün veri 70 oranında eğitim veri kümesi içerir ve özgün veriler % 30'luk test veri kümesini içeren bölme.

## <a name="test-evaluate-and-compare-the-models"></a>Test, değerlendirin ve modelleri karşılaştırın

İki farklı rastgele seçilen veri kümesi eğitmek ve sonra modeli test etmek için önceki bölümde açıklandığı gibi kullanırız. Veri kümesi bölünemiyor ve farklı veri kümelerini eğitme ve değerlendirme modelin daha fazla hedefi bulunmak üzere modelinizi test etme için kullanırız.

Modeli eğitilir sonra kullandığımız **Score Model** ve **Evaluate Model** tahmini sonuçlar oluşturur ve modelleri değerlendirmek için modüller. **Modeli Puanlama** eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Biz sonra puanlar geçirin **Evaluate Model** değerlendirme ölçümleri oluşturulacak.

Bu deneyde iki örneklerini kullanırız **Evaluate Model** modelleri iki çiftlerini karşılaştırmak için.

İlk olarak, iki algoritması eğitim veri kümesi üzerinde karşılaştırın.
İkinci olarak, test veri kümesinde iki algoritması karşılaştırın.
Sonuçları şunlardır:

![Sonuçları karşılaştırma](media/ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Bu sonuçları ile derlenen modelini gösteren **artırılmış karar ağacı regresyonu** ortalama karesi alınmış hata üzerinde oluşturulan model değerinden daha düşük bir kök varsa **karar ormanı regresyon**.

Her iki algoritmaları daha düşük bir hata eğitim veri kümesi üzerinde görünmeyen sınama veri kümesi vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [Örnek 1 - regresyon: Otomobilin fiyatını tahmin edin](ui-sample-regression-predict-automobile-price-basic.md)
- [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
- [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [5 - sınıflandırma. örnek: Dalgalanmasını tahmin](ui-sample-classification-predict-churn.md)
