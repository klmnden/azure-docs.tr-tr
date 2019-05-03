---
title: 'Regresyon: Tahmin'
titleSuffix: Azure Machine Learning service
description: Bu görsel arabirim örnek deneyde otomobilin fiyatını tahmin etmek için bir regresyon modeli derler gösterilmektedir. İşlem, eğitim, test ve değerlendirme otomobil fiyat verileri (ham) veri modeli içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: fa9b9179cda767d69d08dcd357a03123bde901cb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028898"
---
# <a name="sample-1---regression-predict-price"></a>Örnek 1 - regresyon: Tahmin

Bu görsel arabirim örnek deneyde, otomobilin fiyatını tahmin etmek için regresyon modeli oluşturma işlemi gösterilmektedir. Kullanarak model değerlendirme eğitim ve sınama işlemi içerir **otomobil fiyat verileri (ham)** veri kümesi.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Seçin **açık** örnek 1 denemeyi düğmesi:

    ![Denemeyi açın](media/ui-sample-regression-predict-automobile-price-basic/open-sample1.png)

## <a name="related-sample"></a>İlgili örnek

[2 - regresyon. örnek: Otomobil fiyat tahmini (algoritmaları karşılaştırın)](ui-sample-regression-predict-automobile-price-compare-algorithms.md) iki farklı bir regresyon modeli kullanarak bu deney olarak aynı sorunu çözdü daha karmaşık bir örnek denemeyi sağlar. Bu hızlı bir şekilde farklı algoritmayı karşılaştırmak nasıl gösterir. Daha gelişmiş bir örnek arıyorsanız gözden geçirin.

## <a name="experiment-summary"></a>Deneme özeti

Bir deneme oluşturmak için aşağıdaki adımları kullanın:

1. Verileri elde edersiniz.
1. Verileri önceden işleme.
1. Modeli eğitme.
1. Test, değerlendirin ve modelleri karşılaştırın.

Denemeyi tam grafiği aşağıda verilmiştir:

![Denemeyi grafiği](media/ui-sample-regression-predict-automobile-price-basic/overall-graph.png)

## <a name="get-the-data"></a>Verileri alma

Bu deneyde kullandığımız **otomobil fiyat verileri (ham)** UCI Machine Learning depodan olan veri kümesi. Veri kümesi, otomobil üreticisi, modeli, fiyat, araç özellikleri (silindir sayısı gibi) MPG ve sigorta risk puanı da dahil olmak üzere ilgili bilgiler içeren 26 sütunlar içeriyor. Bu deneyde amaç, araba fiyatını tahmin etmektir.

## <a name="pre-process-the-data"></a>Verileri önceden işleme

Ana veri hazırlama görevleri, veri temizleme, tümleştirme, dönüştürme, azaltma ve ayrılma veya boyutlandırma içerir. Bu işlemleri ve diğer veri görevlerinde önceden işleme gerçekleştirmek için modülleri visual arabiriminde bulabilirsiniz **veri dönüştürme** sol bölmedeki grubu.

Kullandığımız **kümesindeki sütunları seçme** normalleştirilmiş birçok eksik değerleri olan kayıplar dışlanacak modülü. Ardından kullandığımız **eksik verileri temizleme** eksik değerler içeren satırları kaldırmak için. Bu, temiz bir eğitim veri kümesi oluşturmak için yardımcı olur.

![Veri ön işleme](./media/ui-sample-regression-predict-automobile-price-basic/data-processing.png)

## <a name="train-the-model"></a>Modeli eğitme
Machine learning sorunları farklılık gösterir. Sınıflandırma, kümeleme, regresyon ve her biri farklı bir algoritma gerektirebilir öneren sistemleri, ortak makine öğrenimi görevlerini içerir. Tercih ettiğiniz algoritması, genellikle kullanım örneği gereksinimlerine bağlıdır. Bir algoritma seçin sonra daha doğru bir model eğitip parametrelerini ayarlamak gerekir. Ardından tüm modellerin doğruluğu anlaşılabilirliği ve verimliliği gibi ölçümleri temel değerlendirmesi gerekir.

Bu deneyde amaç, otomobil fiyatlarını tahmin etmek için olduğundan ve gerçek sayılar (Fiyat) etiketi sütun içerdiği için bir regresyon modeli iyi bir seçimdir. Birçok özellik (daha az en az 100) görece küçük olduğunu ve bu özellikleri seyrek olduğunu dikkate alarak, karar sınır doğrusal olması olasıdır. Kullanacağız **karar ormanı regresyon** bu deneme için.

Kullandığımız **verileri bölme** modülü rastgele giriş verileri özgün veri 70 oranında eğitim veri kümesi içerir ve özgün veriler % 30'luk test veri kümesini içeren bölme.

## <a name="test-evaluate-and-compare"></a>Test, değerlendirin ve karşılaştırma

 Veri kümesi bölünemiyor ve farklı veri kümelerini eğitme ve değerlendirme modelin daha fazla hedefi bulunmak üzere modelinizi test etme için kullanırız.

Modeli eğitilir sonra kullandığımız **Score Model** ve **Evaluate Model** tahmini sonuçlar oluşturur ve modelleri değerlendirmek için modüller.

**Modeli Puanlama** eğitilen modeli kullanarak test veri kümesi için tahminler oluşturur. Sonuç denetlemek için çıkış bağlantı noktasına seçin **Score Model** seçip **Görselleştir**.

![Puan sonucu](./media/ui-sample-regression-predict-automobile-price-basic/score-result.png)

Biz sonra puanlar geçirin **Evaluate Model** değerlendirmesi oluşturmak için modül. Sonuç denetlemek için çıkış bağlantı noktasına seçin **Evaluate Model** seçip **Görselleştir**.

![Değerlendirme sonucu](./media/ui-sample-regression-predict-automobile-price-basic/evaluate-result.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Görsel bir arabirim için kullanılabilir diğer örneklerini keşfedin:

- [2 - regresyon. örnek: Otomobil fiyat tahmini için algoritmalar karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
- [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [5 - sınıflandırma. örnek: Dalgalanmasını tahmin](ui-sample-classification-predict-churn.md)
